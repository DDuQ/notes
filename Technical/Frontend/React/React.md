# Definition



# Context API

The Context API provides a way to share global state without manually passing props down the component tree.

Useful for scenarios like:

- Theme management
- Authentication state
- User settings
- Multi-level shared dependencies

## CreateContext
`createContext()` creates a context object whose value is made available to all components wrapped by its provider.

```jsx
const AuthContext = createContext();

function App() {
  return (
    <AuthContext.Provider value={{ user }}>
      <Dashboard />
    </AuthContext.Provider>
  );
}
```

# Component Lifecycle
Functional components don’t have lifecycle methods like classes do. Instead, lifecycle phases are modeled using **hooks**, mainly `useEffect`.
## **Mounting**
Runs once when the component is added to the DOM.

```jsx
useEffect(() => {
  // componentDidMount
}, []);
```

## **Updating**
Runs when **state**, **props**, or **context** change — depending on the dependency array.

```jsx
useEffect(() => {
  // componentDidUpdate (for these dependencies)
}, [dependencies]);
```

## **Unmounting**
Runs when the component is removed from the DOM.  
Implemented through the cleanup function returned from an effect.

```jsx
useEffect(() => {
  return () => {
    // componentWillUnmount
  };
}, []);
```

# Relevant Questions
## What is the use of  `<StrictMode></StrictMode>`
This is essential for React to check on components as a measure of assurance for them to build correctly.

>[!IMPORTANT]
>The [[React#Component lifecycle|component lifecycle]] gets called 2 times in order to do this check.
>
>THIS TAG **MUST BE REMOVED** ON PRODUCTION ENVIRONMENTS.
## What is the use of `createRoot`?

This method allows us to materialize our react components into the selected identifier, normally `'root'`.

```JSX
createRoot(document.getElementById('root')!).render(
	//React components go here.
)
```