---
title: Redux
layout: default
---

# React Tool Kit

## Basics

1 Create the slice and export the reducer

```ts
const postsSlice = createSlice({
  name: "posts",
  initialState, // array of Posts
  reducers: {}, // it is currently empty
});

export default postsSlice.reducer;
```

2 Add the reducer to the store

```ts
export const store = configureStore({
  reducer: {
    posts: postsReducer,
  },
});
```

3 Select the data in a component

```ts
const posts = useAppSelector((state) => state.posts);
```

Where `useAppSelector` is defined in your store file like this:

```ts
export type AppStore = typeof store; // your store
export type RootState = ReturnType<AppStore["getState"]>;

export const useAppSelector = useSelector.withTypes<RootState>();
```

4 At this point, you have access to the `initialState` but you have no actions to dispatch

5 Add a `postAdded` reducer and export the automatically generated action

- You can mutate the state because RTK uses Immer

```ts
const postsSlice = createSlice({
  name: "posts",
  initialState,
  reducers: {
    postAdded(state, action: PayloadAction<Post>) {
      state.push(action.payload);
    },
  },
});

export const { postAdded } = postsSlice.actions;
```

6 Import `dispatch` and dispatch the `postAdded()` event

```ts
const dispatch = useAppDispatch();

const newPost: Post = {
  id,
  title,
  content,
};
dispatch(postAdded(newPost));
```

Where `dispatch` is defined like this

```ts
export type AppStore = typeof store; // your store
export type AppDispatch = AppStore["dispatch"];

export const useAppDispatch = useDispatch.withTypes<AppDispatch>();
```

## Advanced

1 **Action creator**: Let's imagine you have to generate an `id` for your new Post and you want to do this from several different components. In that case, you'd need to create the `id` and pass that together with the other properties to your `dispatch`.
In a case like this you should use the `reducer + prepare` reducer pattern where you can create your `id` in the `prepare` callback. Redux will call your `prepare` first where you generate the `id` then Redux will call your `reducer` with the payload you created in `prepare`.

```ts
const postsSlice = createSlice({
  name: "posts",
  initialState,
  reducers: {
    postAdded: {
      // action.payload is the result of prepare()
      reducer(state, action: PayloadAction<Post>) {
        state.push(action.payload);
      },
      // it can take any number of params and must return the correct shape of your payload
      // basically, this is your acton creator
      prepare(title: string, content: string) {
        return {
          payload: { id: nanoid(), title, content },
        };
      },
    },
  },
});
```

Dispatch your action

```ts
dispatch(postAdded(title, content));
```

2 **Selector functions**: Let's imagine you want to select a specific post by id in many components. You don't want to repeat your search logic in every component so you can create a selector function.

```ts
// your selector
export const selectPostById = (state: RootState, postId: string) =>
  state.posts.find((post) => post.id === postId);

// using your selector
const post = useAppSelector((state) => selectPostById(state, postId!));
```

**Alternative**: Write your selectors as part of your `createSlice()`. This way the `state` in your selector is not the root state but the state of your slice.

```ts
const postsSlice = createSlice({
  name: "posts",
  initialState,
  reducers: {},
  selectors: {
    // state is just the posts and not the root state
    selectAllPosts: (postsState) => postsState,
    selectPostById: (postsState, postId: string) => {
      return postsState.find((post) => post.id === postId);
    },
  },
});
```

3 **`extraReducers`**: In a slice, listen for actions that happened elsewhere in the app. e.g. When the user logs out, you want to (1) delete the cookies and (2) delete the posts of the user. So you need to do 2 things for 1 event.

- `builder.addCase(actionCreator, caseReducer)`: listen for a specific action
- `builder.addMatcher(matcherFunction, caseReducer)`: listen for matching actions
- `builder.addDefaultCase(caseReducer)`: when no other reducers are triggered ('default case')

```ts
// the action you are listening to
import { userLoggedOut } from "@/features/auth/authSlice";

const postsSlice = createSlice({
  name: "posts",
  initialState,
  extraReducers: (builder) => {
    // react to the action
    // state = posts
    builder.addCase(userLoggedOut, (state) => {
      return [];
    });
  },
});
```

4 **Thunks**: Do stuff before and/or after a `dispatch`. Usually used for async code.

```ts
// define parameters if needed
const logAndAdd = (amount: number) => {
  // return a function that accepts the dispatch and a getState function
  return (dispatch: AppDispatch, getState: () => RootState) => {
    // runs BEFORE dispatch
    const stateBefore = getState();
    console.log(`Counter before: ${stateBefore.counter}`);
    dispatch(incrementByAmount(amount));
    // runs AFTER dispatch
    const stateAfter = getState();
    console.log(`Counter after: ${stateAfter.counter}`);
  };
};
```

**createAsyncThunk**

```ts
import { createAsyncThunk } from "@reduxjs/toolkit";

export const fetchItemById = createAsyncThunk(
  "items/fetchItemById",
  async (itemId: string) => {
    const item = await someHttpRequest(itemId);
    return item;
  },
);
```

In order to give it proper types, use:

```ts
export const createAppAsyncThunk = createAsyncThunk.withTypes<{
  state: RootState;
  dispatch: AppDispatch;
}>();
```

Use your thunk
These actions will be dispatched automatically: `posts/fetchPosts/pending`, `posts/fetchPosts/fulfilled`, `posts/fetchPosts/failed`.
The second argument `thunkApi` has special methods e.g. `dispatch()`, `getState()`, `signal` (to abort a request).

```ts
export const fetchPosts = createAppAsyncThunk(
  "posts/fetchPosts",
  async (type: string, thunkApi) => {
    const response = await client.get<Post[]>("/fakeApi/posts?type=" + type);
    return response.data;
  },
);
```

Modeling API calls: add an `error` and a `status` property to your slice

```ts
interface PostsState {
  posts: Post[];
  status: "idle" | "pending" | "succeeded" | "failed";
  error: string | null;
}
```

React to your thunk actions
You can have a reference to the thunk actions with `fetchPost.`

```ts
const postsSlice = createSlice({
  name: "posts",
  initialState,
  reducers: {
    // omit existing reducers here
  },

  extraReducers: (builder) => {
    builder
      .addCase(fetchPosts.pending, (state, action) => {
        state.status = "pending";
      })
      .addCase(fetchPosts.fulfilled, (state, action) => {
        state.status = "succeeded";
        state.posts.push(...action.payload);
      })
      .addCase(fetchPosts.rejected, (state, action) => {
        state.status = "failed";
        state.error = action.error.message ?? "Unknown Error";
      });
  },
});
```

Dispatch your thunk

```ts
dispatch(fetchPosts("Bob"));
```

Wait for your thunk in a component
`createAsyncThunk` handles errors internally and returns the "fulfilled" or the "rejected" action - so it is never fails basically. We can use `unwrap()` to get the `action.paylod` or catch the thrown error.

```ts
try {
  setAddRequestStatus("pending");
  await dispatch(fetchPosts("Bob")).unwrap();

  form.reset();
} catch (err) {
  console.error("Failed to save the post: ", err);
} finally {
  setAddRequestStatus("idle");
}
```

Thunk condition
Let's say you have an ongoing API call and something triggers the same event again. You don't want to call the API again so you need a mechanism to skip the second action. If the `condition` returns `false` then it won't trigger the thunk.

```ts
export const fetchPosts = createAppAsyncThunk(
  "posts/fetchPosts",
  async (name: string) => {
    const response = await client.get<Post[]>("/fakeApi/posts?name=" + name);
    return response.data;
  },
  {
    condition(arg, thunkApi) {
      const postsStatus = selectPostsStatus(thunkApi.getState());
      if (postsStatus !== "idle") {
        return false;
      }
    },
  },
);
```

Defining thunks in createSlice: [link](https://redux.js.org/tutorials/essentials/part-5-async-logic#optional-defining-thunks-inside-of-createslice)

## createListenerMiddleware

Run a logic in response to specific actions. Previously we saw how reducers can respond to other actions but those responses can't be async.

Initial setup

```ts
// import the special functions
import { createListenerMiddleware, addListener } from "@reduxjs/toolkit";
// import your project specific stuff
import type { RootState, AppDispatch } from "./store";

export const listenerMiddleware = createListenerMiddleware();

// set your project specific types
export const startAppListening = listenerMiddleware.startListening.withTypes<
  RootState,
  AppDispatch
>();
export type AppStartListening = typeof startAppListening;

// create a listener with project specific types
export const addAppListener = addListener.withTypes<RootState, AppDispatch>();
export type AppAddListener = typeof addAppListener;
```

Store config

```ts
export const store = configureStore({
  reducer: {},
  middleware: (getDefaultMiddleware) =>
    // prepend = start of the array
    getDefaultMiddleware().prepend(listenerMiddleware.middleware),
});
```

Create your listener in your slice file

```ts
// startAppListening: your project specific stuff
export const addPostsListeners = (startAppListening: AppStartListening) => {
  startAppListening({
    // WHEN this action is dispatched
    actionCreator: addNewPost.fulfilled,
    // THEN do this async logic
    effect: async (action, listenerApi) => {
      const { toast } = await import("react-tiny-toast");

      const toastId = toast.show("New post added!", {
        variant: "success",
        position: "bottom-right",
        pause: true,
      });

      // listenerApi also has: dispatch, getState etc

      await listenerApi.delay(5000);
      toast.remove(toastId);
    },
    // matcher: match some actions
    // predicate: a more general matching function with access to currState and prevState
  });
};
```

Register your listener in your middleware file

```ts
addPostsListeners(startAppListening);
```

## Tips

1 You can use useAppSelector to select calculated data

```ts
const post = useAppSelector((state) =>
  state.posts.find((post) => post.id === postId),
);
```

2 You can combine selectors

```ts
export const selectCurrentUser = (state: RootState) => {
  // 1st selector
  const currentUsername = selectCurrentUsername(state);
  // 2nd selector
  return selectUserById(state, currentUsername);
};
```

3 Memoizing selectors
Returning new reference for the same input
There are some cases when a reducer is dispatched and it returns a new reference even though the inputs didn't change e.g. filter the posts by user id.

```ts
export const selectPostsByUser = (state: RootState, userId: string) => {
  const allPosts = selectAllPosts(state);
  // ❌ WRONG - this _always_ creates a new array reference!
  return allPosts.filter((post) => post.user === userId);
};
```

So we have to memoize a selector like that:

```ts
export const selectAllPosts = (state: RootState) => state.posts.posts;

export const selectPostsByUser = createSelector(
  // input selectors in an array, they prepare the parameters for the output function
  // called one by one with the parameters you provide in your select
  [
    // 1st input function, returns the posts
    selectAllPosts,
    // 2nd input function, returns the userId
    (state: RootState, userId: string) => userId,
  ],
  // output function, it get the posts and userId from the 2 input functions
  (posts, userId) => posts.filter((post) => post.user === userId),
);

// use the memoized selector
selectPostsByUser(state1, "user1");
```

5 Normalizing Data
Data is not just an array, it is in a special format. It lets us reference the object by id so we don't have to loop through everything all the time.

```ts
{
  users: {
    ids: ["user1", "user2", "user3"],
    // get the items by id
    entities: {
      "user1": {id: "user1", firstName, lastName},
      "user2": {id: "user2", firstName, lastName},
      "user3": {id: "user3", firstName, lastName},
    }
  }
}
```

Example

```ts
interface PostsState extends EntityState<Post, string> {
  // our extra fields
  status: "idle" | "pending" | "succeeded" | "rejected";
  error: string | null;
}

const postsAdapter = createEntityAdapter<Post>({
  // Sort in descending date order
  sortComparer: (a, b) => b.date.localeCompare(a.date),
});

const initialState: PostsState = postsAdapter.getInitialState({
  // setting the extra fields, the entity related fields ({ids: [], entities: {}}) are generated automatically
  status: "idle",
  error: null,
});

 postUpdated(state, action: PayloadAction<PostUpdate>) {
      const { id, title, content } = action.payload

      // reference entities by id
      const existingPost = state.entities[id]

      if (existingPost) {
        existingPost.title = title
        existingPost.content = content
      }
    },
```

The adapter has special helper methods:

```ts
  extraReducers(builder) {
    builder
      .addCase(fetchPosts.fulfilled, (state, action) => {
        state.status = 'succeeded'
        // use the adapter's special method
        postsAdapter.setAll(state, action.payload)
      })
      // use the adapter's addOne method
      .addCase(addNewPost.fulfilled, postsAdapter.addOne)
  }

  export const {
    selectAll: selectAllPosts,
    selectById: selectPostById,
    selectIds: selectPostIds
    // Pass in a selector that returns the posts slice of state
  } = postsAdapter.getSelectors((state: RootState) => state.posts)
```

## RTK Query

A data fetching and caching tool, recommended as the default approach for data fetching in Redux apps. For each unique endpoint + argument combination, there is going to be a cache key.

```ts
import { createApi, fetchBaseQuery } from "@reduxjs/toolkit/query/react";

// Use the `Post` type we've already defined in `postsSlice`
import type { Post } from "@/features/posts/postsSlice";

// Define our single API slice object
export const apiSlice = createApi({
  // The cache reducer expects to be added at `state.api` (already default - this is optional)
  reducerPath: "api",
  // tags are used to invalidate cache this reloading data
  tagTypes: ["Post"],
  // All of our requests will have URLs starting with '/fakeApi'
  baseQuery: fetchBaseQuery({ baseUrl: "/fakeApi" }),
  // The "endpoints" represent operations and requests for this server
  endpoints: (builder) => ({
    // The `getPosts` endpoint is a "query" operation that returns data.
    // The return value is a `Post[]` array, and it takes no arguments.
    getPosts: builder.query<Post[], void>({
      // The URL for the request is '/fakeApi/posts'
      query: () => "/posts",
      // the cacheed result of this query belongs to the Post tag
      providesTags: (result = [], error, arg) => [
        // a simple string tag
        "Post",
        // tags for each individual Posts
        ...result.map(({ id }) => ({ type: "Post", id }) as const),
      ],
      // transformResponse: transform the response if needed
    }),
    getPost: builder.query<Post, string>({
      query: (postId) => `/posts/${postId}`,
      providesTags: (result, error, arg) => [{ type: "Post", id: arg }],
    }),
    // calling .mutation() and not .query()
    addNewPost: builder.mutation<Post, NewPost>({
      query: (initialPost) => ({
        // The HTTP URL will be '/fakeApi/posts'
        url: "/posts",
        // This is an HTTP POST request, sending an update
        method: "POST",
        // Include the entire post object as the body of the request
        body: initialPost,
      }),
      // after this mutation ran, invalidate the cache of the queries that belong to this tag
      invalidatesTags: ["Post"],
    }),
    editPost: builder.mutation<Post, PostUpdate>({
      query: (post) => ({
        url: `posts/${post.id}`,
        method: "PATCH",
        body: post,
      }),
      invalidatesTags: (result, error, arg) => [{ type: "Post", id: arg.id }],
    }),
  }),
});

// Export the auto-generated hook for the `getPosts` query endpoint
export const {
  useGetPostsQuery,
  useGetPostQuery,
  useAddNewPostMutation,
  useEditPostMutation,
} = apiSlice;
```

Add it to the store

```ts
export const store = configureStore({
  reducer: {
    [apiSlice.reducerPath]: apiSlice.reducer,
  },
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware().concat(apiSlice.middleware),
});
```

Use it in a component

```ts
// Calling the `useGetPostsQuery()` hook automatically fetches data!
const {
  data: posts = [],
  currentData, // the data for the current query arguments
  isLoading, // true only for the first loading
  isFetching, // true for any loading
  isSuccess,
  isError,
  error,
  refetch, // used rarely
} = useGetPostsQuery();

const { data: post } = useGetPostQuery(postId!);

// POST
const [addNewPost, { isLoading: isPostLoading }] = useAddNewPostMutation();

try {
  await addNewPost({ title, content, user: userId }).unwrap();
} catch (err) {
  console.error("Failed to save the post: ", err);
}

// PATCH
const [updatePost, { isLoading: isEditPostLoading }] = useEditPostMutation();
```

When the number of data subscriptions goes down to 0, RTK Query starts a timer (60s default). If the timer expires before any new subscriptions are added, the data is removed from the cache.

Update the event listeners to listen to apiSlice events

```ts
export const addPostsListeners = (startAppListening: AppStartListening) => {
  startAppListening({
    matcher: apiSlice.endpoints.addNewPost.matchFulfilled,
    effect: async (action, listenerApi) => {
      //...
    })
}
```

Split up apiSlice

```ts
// This is the _same_ reference as `apiSlice`, but this has
// the TS types updated to include the injected endpoints
export const apiSliceWithUsers = apiSlice.injectEndpoints({
  endpoints: (builder) => ({
    getUsers: builder.query<User[], void>({
      query: () => "/users",
    }),
  }),
});

export const { useGetUsersQuery } = apiSliceWithUsers;

export const selectUsersResult = apiSliceWithUsers.endpoints.getUsers.select();
```

Filtering query result in component (?)

```ts
// Create a TS type that represents "the result value passed
// into the `selectFromResult` function for this hook"
type GetPostSelectFromResultArg = TypedUseQueryStateResult<Post[], any, any>;

const selectPostsForUser = createSelector(
  (res: GetPostSelectFromResultArg) => res.data,
  (res: GetPostSelectFromResultArg, userId: string) => userId,
  (data, userId) => data?.filter((post) => post.user === userId),
);

// Use the same posts query, but extract only part of its data
const { postsForUser } = useGetPostsQuery(undefined, {
  selectFromResult: (result) => ({
    // Optional: Include all of the existing result fields like `isFetching`
    ...result,
    // Include a field called `postsForUser` in the result object,
    // which will be a filtered list of posts
    postsForUser: selectPostsForUser(result, userId!),
  }),
});
```

Updating just the cache

```ts
dispatch(
  // queryArg = cache key
  apiSlice.util.updateQueryData(endpointName, queryArg, (draft) => {
    // mutate `draft` here like you would in a reducer
    draft.value = 123;
  }),
);
```

`onQueryStarted`: called every time a new request goes out

Optimistic updates and other advanced stuff [link](https://redux.js.org/tutorials/essentials/part-8-rtk-query-advanced#implementing-optimistic-updates)

`onCacheEntryAdded`: called when a new entry is added to the cache
