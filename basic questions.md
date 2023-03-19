# `Basic questions`

## `What are react features?`
- JSX: file extension that allows both JS and HTML in a file
- Component: the building blocks of react app that construct smaller and reusable parts of the UI
- Virtual DOM: react keeps a representation of the real DOM in memory, when the state changes, the virtual DOM changes the only part of the real DOM where is affected
- One-way-data-binding: unidirectional is fast and modular

## `Can browser read JSX directly?`
- No, we need Babel to transpile JSX to HTML and JS

## `Advantages of React over Angular?`
- It's typically faster and has smaller bundle size
- Uses unidirectional data binding, so when we nested child components in parent components, it's easier to debug the data flow
- Has dedicated debugging tool

## `ES5 vs ES6`
- ES5 came out in 2009 while ES6 in 2015
- Beside string, number, boolean, null, undefined, ES6 has `symbol`
- ES5 can only defines variables using `var` while ES6 provides `const` and `let`
- ES6 is faster thanks to object destructuring assingment and spread operator
- ES5 can only defines a function using `function` and `return` while ES6 has arrow function
- ES6 also provide `for...of` loop
- For React, ES6 allows import jsx file in another jsx file while ES5 has to use require to include other components
- In ES6, we pass props explicitly in constructor and bind `this` to functions inside that constructor. When we bind `this` to a function, `this` inside that function will refer to the component. The alternative way is to use arrow functions but this could cost performance and testability

## `How to create react app?`
- Use npx create-react-app 

## `What is an event in React?`
- An event is an action from the user like mouse click, button pressed ...

## `How to handle an event in React?`
- For function based component:
  ```jsx
    function Form() {
        handleClick = (e) => {
            e.preventDefault();
            console.log("Submitted")
        }
        return (
            <form onSubmit={handleClick}>
            </form>
        )
    }
  ```
- For ES6 class based components:
  ```jsx
  class Form extends React.Component {
    constructor(props){
      super(props);
      //bind *this* of this method to the component
      this.handleClick.bind(this)
    }
    handleClick = () => {
      console.log("Click");
    }

    render() {
      return (
        <form onSubmit={this.handleClick}>
        </form>
      )
    }
  }
  ```
## `What is synthetic event?`
- Synthetic event is the event handling system of React that wraps around events of various browsers to ensure that they behave the same

## `How to traverse through list in React?`
- We use map()
  ```jsx
  <ul>
    {
      this.state.list.map(item => <li key={item.id}>{item.name}</li>)
    }
  </ul>
  ```
- Or use for loop
  ```jsx
  <ul>
    {
      (() => {
        let td = [];
        for (item of list){
          td.push(<li key={item.id}>{item.name}</li>)
        }
        return td
      })
    }
  </ul>
  ```
## `npm vs npx`
- npm is a package manager that installs and executes packages
- npx only executes without installing packages
## `Why key is needed?`
- Key is important to keep track which item has been changed to be re-rendered

## `How to create a form in React?`
  ```jsx
  function Form() {
    const [value, setValue] = useState("");
    const handleValueChange = (e) => {
      setValue({value: e.target.value})
    }
    const handleClick = (e) => {
      e.preventDefault();
      console.log("Submitted");
    }
    return (
      <form>
        <input value={value} onChange={handleValueChange}/>
        <button onClick={handleSubmit}>Submit</button>
      </form>
    )
  }
  ```
## `What is an arrow function?`
- Arrow function is a shorter way to write functions
- It's not necessary to bind arrow functions to the component because the `this` is always inherited from the parent scope

## `What is a state in React?`
- It's an object that contains data of a component
- State can change and trigger re-rendering of the component

## `How to update state?`
- We can't modify state directly but need to use set state method to create new state object from the last state
  ```jsx
  export default function User(){
    const [name, setName] = useState('');
    const updateName = (name) => {
      setName(name);
    }
  }
  ```
  ```jsx
  class User extends React.Component{
    constructor(props){
      super(props);
      this.state = {name: '', age: 20}
    }
    const updateName = (newName) => {
      this.setState({...this.state, name: newName})
    }
    const updateAge = (age) => {
      this.setState(prevState => {
        let newState = {...prevState};
        newState.age = age;
        return {newState}
      })
    }
  }
  ```
## `How to update nested properties in state?`
- Use spread operator
  ```jsx
  export default function User() {
    const [user, setUser] = useState({
      name: "Linh";
      address: {
        city: "Nam Dinh",
        country: "Vietnam"
      }
    });

    const handleUpdate = (country) => {
      setUser(prevUser => {
        ...prevUser, 
        address: {
          ...prevUser.address, 
          country: country
        }
      })
    }
  }
  ```
- To update array in state
  ```jsx
  export default function User(){
    const [arr, setArr] = useState([])
    const handUpdate = (newElement) => {
      setArr([...arr, newElement])
    }
    const remove = (removed) => {
      setArr(prevArr => {
        const newArr = [...prevArr];
        const index = newArr.indexOf(removed);
        newArr.splice(index, 1);
        return newArr;
      })
    }
  }
  ```
- Update an element in array
  ```jsx
  export default function User(){
    const [arr, setArr] = useState([]);
    const updateElement = (index, value) => {
      //shallow copy the array
      const newArr = [...arr];
      //shallow copy the element
      const element = {...newArr[index]};
      //modify the copy
      element.name = value;
      //put the copy back to the array
      newArr[index] = element;
      //set state
      setArr([...newArr]);
    }
  }
  ```
## `How to pass props in React?`
  ```jsx
  <Child name="Linh"/>
  ```
  ```jsx
  export default function Child(props) {
    const [name, setName] = useState(props.name)
  }
  ```
## `What is a higher order component?`
- Higher order components are components that wrap around other components that have similar pattern for reusability

## `How to embed 2 components in 1 file?`
  ```jsx
  export default function ComponentA(){
    return (
      <div>
        <ComponentB/>
      </div>
    )
  }
  function ComponentB(){
    return (
      <div>
        <h1>Component B</h1>
      </div>
    )
  }
  ```
## Class vs function components?
- As of 2023, function based components are recommended over class based component
- Function-based component can use `useEffect(fn)` to simulate `componentDidUpdate` and `useEffect(fn, [])` to simulate `componentDidMount`

## `Component lifecycle?`
- `getInitialState()`: executes before the component mount
- `componentDidMount()`: executes right after the first time the component mount
- `shouldComponentUpdate()`: returns true or false on whether a component should re-render
- `componentDidUpdate()`: executes every time the component re-render but doesn't execute on the first render
- `componentWillUnmount()`: executes after the component is destroyed and unmounted permanently

## `Components of Redux?`
- `Store`: object that stores the state of the application
- `Action`: the event types to invoke the change of state
- `Reducer`: defines how the state is changed according to the actions

## `How to implement React Router?`
- We need to wrap the entire application inside <Router> tags
  ```jsx
  <Router>
    <App>
      <Route path="/home" component="Home"/>
      <Route path="/contact" component="Contact"/>
      <Route path="/about" component="About"/>
    </App>
  </Router>
  ```
## `How to style a component?`
- We can use `style`
  ```jsx
  function ComponentA(){
    const style = {
      background-color: red
    }
    return (
      <div style={style}>
      </div>
    )
  }
  ```