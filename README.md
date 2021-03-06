# React Class State (react-class-state)

Very small, fast, and unopinionated. You can use just like you want, state-rerenders are minimum especially if you use state.watchState(). Everything is type supported and smooth!

---

### Usage

First, create a React app, then paste this to your console:

```
npm install react-class-state
//OR
yarn add react-class-state
```

#### Creating State

```TSX
import ClassState from "react-class-state"
import { ITodo } from "./types/ITodo"

class TodoState extends ClassState {
  todos: ITodo[] = []

  // If you want, you can use actions inside the class, if you want you can also follow the next usages
  async fetchTodos() {
    const response = await fetch("https://jsonplaceholder.typicode.com/todos")
    const data = await response.json()
    this.setState((state) => (state.todos = data))
  }
}
const todoState = new TodoState()
// You can call this in react components, too.
// If you do this process on server or outside of component, you can use this as SSR with NextJS
todoState.fetchTodos()
```

#### Other Way To Change State

```TSX
// You can change the state from anywhere, in regular files or inside class components/function components, no matter whether it is async or not,
const response = await fetch("https://jsonplaceholder.typicode.com/todos")
todoState.setState(async (state) => (state.todos = await response.json()))  */
```

#### Subscribe State

```TS
todoState.subscribeState((currentState, previousState) => {
  console.log({ currentState })
  console.log({ previousState })
  // This will re-render only once and whatever you change here will also change the React Component State
  // Don't use setState here if you don't want a stackoverflow
  todoState.push({ id:101, title:"New todo from subscribe!" })
})
```

#### Usage

```TSX
const App = () => {
  const { todos } = todoState.getState()
  return (
    <div>
      {todos.map((todo) => (
        <div key={todo.id}>{todo.title}</div>
      ))}
    </div>
  )
}
```

#### You can use the system however you want.

```TSX
// If you use 'todoState.watchState()' in your parent, you don't need to use it in another file.
const App = () => {
  todoState.watchState()
  // Rest of the App
}

```

#### Changing state inside components

```TSX
const App = () => {
  const { setState } = todoState.getState()

  useEffect(() => {
    const fetchTodos = async () => {
      const response = await fetch("https://jsonplaceholder.typicode.com/todos")
      const data = await response.json()
      setState((state) => (state.todos = data))
    }
    fetchTodos()
  }, [])

  // Rest of the App
}
```
