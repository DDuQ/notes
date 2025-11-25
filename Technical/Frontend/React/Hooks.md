# Definition
Hooks let us interact with a component’s lifecycle without using classes. They enable logic reuse across different parts of the application through functional components.

> [!IMPORTANT] 
> Hooks must be called at the top level of a component.  
> The only exception is inside custom hooks.

# Common used hooks
## UseState
Provides reactive state:  
```jsx
const [value, setValue] = useState(initialValue)
```

Each state update triggers a re-render and updates the DOM accordingly.

## UseContext
`useContext()` allows consuming the value provided by a parent context provider.

```jsx
const { user } = useContext(AuthContext);
```

## UseEffect
`useEffect()` receives:

- A function (the effect)
- A dependency array

The effect runs:

- **After every render**, if no dependency array is provided
- **Only once**, if the dependency array is empty
- **Whenever dependencies change**, if dependencies are provided

It can mimic lifecycle phases by returning a cleanup function.

### Example (subscription pattern)

```jsx
useEffect(() => {
  const animation = new Animation();
  animation.start();

  return () => animation.stop(); // cleanup on unmount
}, []);
```
### Render timing in React 18

In React 18, effects run **after the commit phase**, once the DOM has been updated.

## UseReducer

`useReducer()` provides state management based on reducer functions, useful for complex state transitions.

```jsx
const [state, dispatch] = useReducer(reducer, initialState);
```

It resembles Redux: state updates occur through dispatched actions rather than direct mutation.

## UseMemo

Memoizes the result of expensive functions.

```jsx
const result = useMemo(() => heavyCalculation(), [value]);
```

Benefits:
- Avoids recomputing when dependencies haven't changed
- Helps maintain referential equality (stable object/array references)

> [!WARNING] 
> Overusing `useMemo()` can hurt performance due to memory overhead.
