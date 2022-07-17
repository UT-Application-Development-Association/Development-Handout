# Function Components and Hooks

> Relevant courses: CSC309, CSC324

## Function Component

```js
const App = (props) => {
  return (
    <div>
      MyApp
    </div>
  )
}
export default App
```

## Hooks

Don't use hooks like regular functions:

- ✅ Call Hooks from React function components. (NOT in a nested function in the function component! It must be at the top level function)
- ✅ Call Hooks from custom Hooks 

### `useState`

A ***state*** is a private property of a component and is fully controlled by the component. **When the value of a state changes, the component will be re-rendered** - this is how a state is different from an ordinary variable.

State value must be changed by the `setState` function to trigger re-render. Generally, if setState does not assign a new value, then re-render may not be triggered.

```js
const [state, setState] = useState(initialState)

setState(newValue)
setState(prevState => { return getNewState(prevState) }) // If you need access to previous state
```

### `useEffect`

Allows you to perform ***side effects*** in your components, such that the side effects do not interrupt rendering immediately. `useEffect` acts like `componentDidMount`.

#### What are the Side Effects

Side effects generally include those which **affect rendering**, such as fetching data, or directly assigning states before rendering. 

Anything that affects rendering should be put into a `useEffect`.

```js
useEffect(() => {
  fetchData()
  return () => cleanup() 
}, [...deps]}
```

#### Dependencies

The effect function is **triggered after every render** unless dependencies is provided in the list. ***dependencies*** should include **every variable that is used in the effect function*** so that the function is updated on time. 

Providing an empty list would make the effect only run once when the component renders the first time. This can replace `componentDidUpdate`.

#### Clean up

If the side effect needs things to be cleaned up (e.g., clear a timer), then you need to return a function for that. This acts like `componentWillUnmount`.

```js
useEffect(() => {
  const timer = setTimeout(...)
  return () => clearTimer(timer) 
})
```

### `useRef`

Create a reference for a **DOM element**. Performs better than `document.querySelector`.

```js
export default App = () => {
  const headingRef = useRef()   // It does not re-create a new instance when the element re-render.
  headingRef.current.innerText = "HEADING"

  return <h1 ref={headingRef}>Heading</h1>
}
```

### `useContext`

#### What is Context

***Context*** provides a way to pass data through the component tree **without having to pass `props` down manually at every level**.

```js
// stores/MyContext.js
export default MyContext = React.createContext({...initialValues}) 
// This is not an appropriate place to assign values, but a good place to define types.

// App.js
import ...

export default App = () => {
  const [initialValues, setValues] = useState(...)
  return (
    <MyContext.Provider 
       value={{
               ...initialValues,  // This is where you should assign initial values
               setValues,
       }}
    >
      <Content />
    </MyContext.Provider>
  )
}

// Content.js
import ...

export defualt Content = () => {
  const ctx = useContext(MyContext)
  return <h1>{ctx.value}</h1>
}
```

### `useReducer`

#### Why use reducer

`useReducer` is usually preferable to `useState` when you have **complex state logic** that involves multiple sub-values or when the next state depends on the previous one (when the state can change in multiple ways). 

The `dispatch` function allows a uniform way to set state.

```js
const [state, dispatch] = useReducer(reducer, initialValues, init)

// @param state Previous state.
// @param action Contains { type, payload }.
function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return {count: state.count + action.payload}
    case 'decrement':
      return {count: state.count - action.payload}
    default:
      throw new Error('Invalid action type')
  }
}
```

#### Integrate with Context

```js
const MyContext = React.createContext({
  state: initialState
,
  dispatch: () => void,
})

const reducer = (state, action) => {
switch (action.type) {
    case 'ACTION_1':
      return getNewState(state, action.payload)
    ...
    default:
      throw new Error(`Unhandled action type: ${action.type}`)
  }
}

export const MyContextProvider = ({ children }) => {
  const [state, dispatch] = React.useReducer(reducer, initialState)

  return (
    <MyContext.Provider value={{ state, dispatch }}>
      {children}
    </MyContext.Provider>
  )
}

// App.js > App()
return <MyContextProvider><App /></MyContextProvider>

// Content.js > Content()
const ctx = useContext(MyContext)
ctx.dispatch({ type, payload })
```

### `useMemo`

Returns a **memorized value**. Pass a “create” function and an array of dependencies. useMemo will only recompute the memoized value when one of the dependencies has changed. This optimization helps to avoid expensive calculations on every render.

```js
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b])
```

### `useCallback`

Return a **memoized version of the callback function** that only changes if one of the dependencies has changed.

```js
const fn = useCallback(cb, [...deps])
```

## The Real Advantage: Custom Hooks

A custom hook is really a function, as long as you call it `**use**WhatEverNameYouLike()` instead of `whatEverNameYouLike()`.

But it allows using all the hooks above! Therefore, you can extract common behaviors in your components to a custom hook to better the your states.

```js
const useFetch(route, method) {
  const [error, setError] = useState(null)
  const [loading, setLoading] = useState(false)
  
  const [data, setData] = useState(null)
  
  useEffect(async () => {
    setError(null)
    setLoading(true)
    
    try {
      const response = await fetch({...})
      ...
      setData(await response.json())
    } catch (err) {
      setError(err.message)
    } finally {
      setLoading(false)
    }   
  }, [route, method])
  
  return {error, loading, data}

}
```



