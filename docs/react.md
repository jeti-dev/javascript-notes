---
title: React
layout: default
---

# React

## React Compiler
TODO

## Components
- component names must start with a capital letter e.g. `export default function MyComp(){}`
- fragment: `<></>` OR `<Fragment>` if you have to add `key`
- rendering lists: use the `key` attribute
    - we can use the `key` to force a rerender on a component
- the `class` attribute is `className` because `class` is a reserved word
- attributes are written camelCase except `aria-*` and `data-*`
- `style={{backgroundColor: 'red'}}`
- props spreading `<Avatar {...props} />`
- nested child components are received in the `children` props
- props are immutable; don't change props in a component
- render nothing: return `null` (or `false` or `undefined`)
    - `0 && 'false` renders `0`!
- Strict Mode: in dev mode React calls each component's function twice -> finding impure calculations
    - wrap the root component into `<React.StrictMode>`
- local mutation: a pure function changes some variables that were created inside the function -> it is allowed
- event handlers don't have to be pure because they don't run during rendering
- render tree: tree of React components
- events: just pass an events handler from the parent as a prop
    - naming convention: `onSmash` and `handleSmash`
    - the capture events are defined too, e.g. `onClickCapture`
- hooks can only be called at the top level of components or my own hooks
- rendering steps: trigger -> render -> commit

### State
- state: `const [score, setScore] = useState(0)`
    - when we want to calculate the next state based on the current state, use an updater function: `setScore(s => s + 1)`
    - in case of objects or arrays, create a new reference when updating the state
- local variables don't persist between renders
- a state variable's value never changes within a render even if there is an async code in it (e.g. setTimeout)
    - in other words, inside `render` a state variable has always has its current snapshot value
- impossible state" e.g. if I have two state variables like 'isSending' and 'isSent', 'isSendining=true' and 'IsSent=true' at the same time is impossbile -> try to use 1 state variable to handle this e.g. status: 'typing' | 'sending'
- state variables are only initialized during the first render: so if we use a prop variable as the init value for a state variable, it won't be updated if the prop value changes the next time!
- uncontrolled component: it has no state, everything it needs come from the props
- The state of a component is tied to its position in the UI tree! So if you have a condition like `IF fancy <MyComp type={"fancy"}> ELSE <MyComp type={"normal"}>` then the state of the component is not reset if you keep chaging the value of fancy!
    - in other words: if we want to preserve the state between re-renders, we must keep the UI structure the same!
    - we have to use the `key` attribute if we don't need this behavior e.g. a chat input stays in the same UI structure but the recipient has changed

### Context
```js
import { createContext } from "react";

const LevelContext = createContext(1);

export default LevelContext;
```

```js
const level = useContext(LevelContext);
// level = 1
```

```js
// provide a different value for the child components
<LevelContext value={2}>
    <ChildComp></ChildComp>
</LevelContext>

// in ChildComp
const level = useContext(LevelContext);
// level = 2
```

### Reducer
```js
// Task reducer - defined the actions
function tasksReducer(tasks, action) {
  switch (action.type) {
    case 'added': {
      return [...tasks, {
        id: action.id,
        text: action.text,
        done: false
      }];
    }
    case 'changed': {
      return tasks.map(t => {
        if (t.id === action.task.id) {
          return action.task;
        } else {
          return t;
        }
      });
    }
  }
}
```

```js
const [tasks, dispatch] = useReducer(
    // our reducer
    tasksReducer,
    // initial value
    initialTasks
);
```

```js
  function handleAddTask(text) {
    // dispatch from useReducer()
    dispatch({
      type: 'added',
      id: nextId++,
      text: text,
    });
  }
```

### Context + Reducer
1. create a Context for the tasks and the dispatcher
2. create the tasks and the dispatcher with userReducer
3. put them in a component, and make your app its children

```js
import { createContext, useContext, useReducer } from 'react';

const TasksContext = createContext(null);
const TasksDispatchContext = createContext(null);

export function TasksProvider({ children }) {
  const [tasks, dispatch] = useReducer(
    tasksReducer,
    initialTasks
  );

  return (
    <TasksContext value={tasks}>
      <TasksDispatchContext value={dispatch}>
      // your app will be here
        {children}
      </TasksDispatchContext>
    </TasksContext>
  );
}

// custom hooks
export function useTasks() {
  return useContext(TasksContext);
}

export function useTasksDispatch() {
  return useContext(TasksDispatchContext);
}

function tasksReducer(tasks, action) {
  switch (action.type) {
    case 'added': {
      return [...tasks, {
        id: action.id,
        text: action.text,
        done: false
      }];
    }
    case 'changed': {
      return tasks.map(t => {
        if (t.id === action.task.id) {
          return action.task;
        } else {
          return t;
        }
      });
    }
  }
}

const initialTasks = [];
```

## Escape hatches
### Ref
- preserving a data but not triggering a rerender: `const ref = useRef(0); console.log(ref.current);`
- e.g. timeout ids, DOM elements
- DOM ref: `<input ref={ref} />`
- ref.current is mutable
- handling a dynamic number of refs: use a ref callback

```js
 ref={(node) => {
    // save node e.g. to  an array which is stored in yourRef.current
}}
```

```js
// accessing another component's DOM nodes

function MyInput({ ref }) {
  return <input ref={ref} />;
}

function MyForm() {
  const inputRef = useRef(null);
  // inputRef = MyInput's input DOM node -> MyForm can call e.g. focus
  return <MyInput ref={inputRef} />
}
```

### useEffect
- to sync with an external system *after rendering* (or to specify side effects that are caused by the rendering itself and not by a particular event)
    - some stuff should only be triggered by a user event (e.g. buying an item) so don't put these type of events in an Effect!
```js
      useEffect(() => {
        const connection = createConnection();
        connection.connect();
        return () => connection.disconnect();
    }, []);
```

    - return a cleanup function: it is called before every new trigger of the Effect and on unmount
    - the second parameter is a reactive property that triggers a re-init of the effect
- think about Effects like when to sync and when to unsync
- if you don't have a dependency for the Effect, it runs on every render -> don't set state in the Effect because it causes an infinite loop
    - how to make it run only once: add an empty array as the dependency
- a variable declared in the component body is also considered reactive -> if it's a dependecy of an Effect and or an object, array or a function, it can re-trigger the Effect
  - so if you need a variable like that, you can put it outside of the component or if it uses a reactive value, put it inside the Effect
- `useEffectEvent`: we can't use logStuff below because it might not have the latest num2 value AND num2 should be a dependency of our Effect but we don't want that, we only want to depend on num1
  - in other words: use it when you want to use a reactive value in an Effect but you don't want your Effect to re-run on that value change
- every reactive value inside an Effect is a dependency of the Effect -> use useEffectEvent when needed
```js
  const [num1, setNum1] = useState(0);
  const [num2, setNum2] = useState(0);

  function logStuff() {
    console.log('logStuff: ', num2);
  }

  const logStuffEff = useEffectEvent(() => {
    logStuff();
  });

  useEffect(() => {
    console.log('num1 in effect: ', num1)
    // logStuff(); -> won't work!
    logStuffEff();
  }, [num1])
```

### useMemo
- to not recaulculate expensive stuff on each render
```js
// getFilteredTodos only runs if the todos or the filter variables have changed, not if any other reactive variables changed
const visibleTodos = useMemo(() => getFilteredTodos(todos, filter), [todos, filter]);
```

### Other
- `flushSync`: force a React update so before your next step, the UI is up to date
```js
flushSync(() => {
  setTodos([ ...todos, newTodo]);
});
// Otherwise the scroll would happen before setTodos updates the UI
listRef.current.lastChild.scrollIntoView();
```

- when we update a component during rendering, React throws aways the current JSX and retries rendering with the new values
- custom hook for data fethcing:

```js
function useData(url) {
  const [data, setData] = useState(null);
  useEffect(() => {
    let ignore = false;
    fetch(url)
      .then(response => response.json())
      .then(json => {
        if (!ignore) {
          setData(json);
        }
      });
    return () => {
      ignore = true;
    };
  }, [url]);
  return data;
}

const results = useData(`/api/search?${params}`);
```

### Custom Hooks
- if they are used at 2 places, then those are 2 different variables! they are not shared = custom Hooks let us share stateful logic but not the state itself

## Built-in components
- `Suspense`: show thee fallback content while the children are loading
  - child must be loading with e.g. `lazy` or `use`
```js
<Suspense fallback={<Loading />}>
  <SomeComponent />
</Suspense>
```

- `Activity`: hide a component but keep its state and update it in the background
```js
<Activity mode={visibility}>
  <Sidebar />
</Activity>
```

- `Profiler`: check performance of a subtree
```js
<Profiler id="App" onRender={onRender}>
  <App />
</Profiler>
```

- `<form>`: the action attribute can take a function too
- `<input>` (and other input-like stuff): use `value` and `checked` to have a controlled component and `defaultValue` and `defaultChecked` for an uncontrolled component

## Built-in hooks
- `useCallback`: cache a function definition between re-renders
```js
  const handleSubmit = useCallback((orderDetails) => {
    post('/product/' + productId + '/buy', {
      referrer,
      orderDetails,
    });
  }, [productId, referrer]);
```

- `useDebugValue`: add a debug label to my custom hook
- `useDeferredValue`: 'show the old list of items while loading the new list instead of showing "loading..." again'
  - must use `lazy` or `use`
```js
  const [query, setQuery] = useState('');
  const deferredQuery = useDeferredValue(query);
```

- `useId`: to generate ids for accessiblity stuff
- `useImperativeHandle`: expose only a subset of APIs of a DOM node e.g.
```js
function MyInput({ ref }) {
  const realInputRef = useRef(null);
  useImperativeHandle(ref, () => ({
    // Only expose focus and nothing else
    focus() {
      realInputRef.current.focus();
    },
  }));
  return <input ref={realInputRef} />;
};
```

- `useMemo`: cache the results of a calculation between rerenders
```js
const cachedValue = useMemo(calculateValue, dependencies)
```

- `useSyncExternalStore`: subscribe to an external store e.g. DOM APIs
  - `subscribe`: has the event listener, calls a callback, return an unsub function
  - `getSnapshot`: called by React when the callback is called
``` js
 const isOnline = useSyncExternalStore(subscribe, getSnapshot);
```

- `useTransition`: ?
- `useFormStatus`: the component which uses this hook must be in a `<form>`
```js
const { pending, data, method, action } = useFormStatus();
```

- `useActionState`: allows you to update state based on the result of a form action

## Built-in APIs
- `lazy`: load a component lazily, loaded only when first displayed
```js
const MarkdownPreview = lazy(() => import('./MarkdownPreview.js'));
// instead of import MarkdownPreview from './MarkdownPreview.js';
```

- `memo`: when a parent component rerenders, React rerenders the child components too -> you might only want to rerender a component when its props changed!
```js
const MemoizedComponent = memo(SomeComponent, arePropsEqual?)
```

- `use`: read the value of a resource like a Pomise or context ?
- `createPortal`: render some children into a different part of the DOM
```js
<div>
  <p>This child is placed in the parent div.</p>
  {createPortal(
    <p>This child is placed in the document body.</p>,
    document.body
  )}
</div>
```

- `preconnect`: eagerly connect to a server
- `prefetchDNS` 
- `preinit`: eagerly fetch and evaluate a stylesheet or external script
- `preinitModule`: eagerly fetch and evaluate an ESM module
- `preload`: eagerly fetch a resource such as a stylesheet, font, or external script 
- `preloadModule`: eagerly fetch an ESM module 