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
### State
- state: `const [score, setScore] = useState(0)`
    - when we want to calculate the next state based on the current state, use an updater function: `setScore(s => s + 1)`
    - in case of objects or arrays, create a new reference when updating the state
- local variables don't persist between renders
- a state variable's value never changes within a render even if there is an async code in it (e.g. setTimeout)
    - in other words, inside `render` a state variable has always has its current snapshot value



- events: just pass an events handler from the parent as a prop
    - naming convention: `onSmash` and `handleSmash`
    - the capture events are defined too, e.g. `onClickCapture`
- hooks can only be called at the top level of components or my own hooks
- rendering steps: trigger -> render -> commit