### Basics
* Uses Javascript 
* Developed by Facebook
* [Download & Install](https://nodejs.org/en/download/)
* Installation gets node and npm 
* Node is the runtime execution enviroment to run our code

### NPM(Node Package Manager)
* Helps in downloading Dependencies from 3rd Party Repositories and integrate in our project.
* All versions and name details can be searched from [NPM Page](https://www.npmjs.com/)

### Browser Components
* User interface : Relevant IP Address searched for DNS name(Eg. wwww.google.com) to get the resource(HTML,CSS, Javascript, Images, etc) 
* Browser engine : Takes commands and informs rendering engine to perform action(Eg. Refresh F5)
* Rendering engine : Parses unconventionally/context free grammar(HTML) & conventionally/context sensitive(CSS & Javascript) -> Render Tree -> Layout creation(Calculates position & size) -> Rendered on webpage
* Networking : Resources are handled
* Javascipt Interpreter : Handles Javascript
* UI Backend : Backend for UI
* Data Persistence : Eg. Cookies, LocalStorage

### DOM vs Virtual DOM
* DOM(Document Object Model): Tree like structure of HTML elements present in webpage. The entire DOM used to be rendered in ancient webpage design
* Virtual DOM: Lightweight copy of the DOM which tracks any change to JSX element and renders only change
* Virtual DOM is quite faster than entire DOM rendering

### Default vs Namespaced Import
* Each React Component can have utmost one default export. WHile it can export in non-default way multiple components 
* Namespaced Import
```javascript
//Source Class
export class App extends React.Component
//Importing Class
import { App } from './App';
``` 

* Default Export
```javascript
//Source Class
export default class App extends React.Component
//Importing Class
import App from './App';
```
### Functions vs React Component Class
* Function are stateless as constructor not allowed. 
* Only props sent and React elements are returned. 
* Lifecycle hook methods cannot be used.

```javascript
function App() 
{ 
  return(
      <p>Hello Anupama<p>
  ); 
} 
```  
* React Components are stateful as constructors can have states. 
* Lifecycle hook methods can be used.
```javascript
export class App extends React.Component {
    render(){
         return(
            <p>Hello Anupama<p>
        ); 
    }
} 
```
### Constructor and Event Binding
* Event methods must be binded to constructor. Since otherwise it will try to check 'window' states and method for Javascript

### LifeCycle Hooks
* Lifecycle methods can be leveraged for invoking business logics before or after rendering,mounting or updating

### Ecmascipt(ES6/ES7)
* JSX Uses Ecmascript standard of Coding

### Babel
* Transpiler which helps in transpiling newer future browser versions(ES2017,etc) to older browser version(ES5)
* Its Not a compiler(Java -> Bytecode) as it converts one language to another
* Used as devDependency since we don't need this in Production environment. Since webpack will help us in create bundle.js
* Try your hands at [Babel TryOut](https://babeljs.io/) and write latest Javascript version codes and see how it's converted to brower-compatible Javascript
* Babel 7(const arr = [4,5,56]) & es2015(var arr = [4, 5, 56];)


### Nodemon 
* Automatically Refreshes code 
* Used as devDependency as above


### Webpack
* Bundles all files together(bundle.js,bundle.css)


### Arrow Function Components/Lambda Function Component
* Introduced with ES6/ES2015(Year 2015)

```javascript
<button onClick={() => this.setState({count: this.state.count+1})}>Increase</button>
```

```javascript
increase = () => {
    this.setState({count : this.state.count +1})
}

<button onClick={this.increase}>Increase</button>
```

### Hooks in Functional Components 
* Introduced in Oct 2018 with React 16.8 
* Functions which help in hooking state and lifecycle in Functional components
* Examples - useEffect,useState,useContext,useReducer,etc
* Custom hooks can also be created

#### useState Hook
* Makes functional components stateful same as React Class Component 
* No need to change function to components any more
* Prefixed with name use as state variables declared at Function Component rendering. But when useState is used, it just return current value
* Takes input as value(current state)
* First array element is state variable name
* Second array element is state handler methodname

#### useEffect Hook 
* Helps in handling lifecycle(componentDidMount,componentDidUpdate & componentWillUnmount in React classes, but unified into a single API)
 
```javascript
import React, { useEffect, useState } from 'react';

export default function AppFunction() {
    const [count, setCount] = useState(0);

    useEffect(() => {
        console.log(`Count ::  ${count}`);
        document.title=`Clicked ${count} times`;
    })

	  //Array Destructuring
    // var countStateVariable = useState(0); // Returns a pair
    // var count = countStateVariable[0]; // First item in a pair
    // var setCount = countStateVariable[1]; // Second item in a pair

    return (
        <div>
            <p>Hello Anupama</p>
            <p>Let's Learn React.js now - AppFunction</p>
            <button onClick={() => setCount(count + 1)}>Increase</button>
            <p>Hi, My friend. U have increased the count : {count}</p>
        </div>
    );
}
```
### Reducers
* Pure functions which gives same output for the same given input
* Immutable state : Reducer creates a new state object for output. Input state remains as-is
* Accepts state and action and returns the new state 
> (state,action) => newState

* Option 1 : Separate initial state
```javascript
  const count = 0;
  const initialState = { count: 7 };
  const [state, dispatch] = useReducer(reducer, initialState);
```

* Option 2 : Initial State combined in reducer 
```javascript
  const [state, dispatch] = useReducer(reducer, { count: 7 });
``` 

* Option 3 : Lazy initialization of inital state
```javascript
  const initialCount=40;
  const [state, dispatch] = useReducer(reducer, initialCount, init);

function init(initialCount) {
  return { count: initialCount };
}
```

* Option 4 : Arrow Function
```javascript
  const initialCount = 40;
  const [state, dispatch] = useReducer(countReducer, initialCount, init);
```

* Reducer
```javascript
function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'reset':
      return init(action.payload);
    default:
      throw new Error();
  }
}
```

* Arrow Function reducer
```javascript
const countReducer = (state, action) => {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'reset':
      return init(action.payload);
    default:
      throw new Error();
  }
}
```

* Invoking dispatch during element rendering
```html
    <button onClick={() => dispatch({ type: 'increment' })}>Increase</button><br /><br />
    <button onClick={() => dispatch({ type: 'reset', payload: initialCount })}>Reset</button>
    <p>Hi, My friend. U have increase the count : {state.count}</p>
```

### Need of useContext
* React useReducer hook creates colocated state container for each functional component
* useContext enables state containers created by useReducers and the dispatch function to be passed to any lower level component(Uni Directional flow of React)
* If used at topmost level, state management can become "global".Thereby, props dont need to be passed to each component in the tree

### useContext Working
* React.createContext -> Returns the context
* Value can be changed by top component(MyContext.Provider)
* useContext only reads value

```javascript
const Mycontext = React.createContext("red");
  const value = useContext(Mycontext);

  useEffect(() => {
    console.log(`Context is :: ${value}`);
  })
```

### Redux State Mangement vs React State Management(useReducer+useContext hooks)
* All component reducer can't be combined to single reducer concept as in Redux
* One reducer provides one dispatch function which is then tightly coupled with one action in React State Management. While in Redux, global dispatch function can be used with any function across any reducer
* Rich middleware ecosystem in Redux(Eg. Action logger)

### Redux
* Library for managing global application state
* Uses uni-direction data flow from parent to child component
* Store : Storage for common data
* Actions : Simple JS Object(Key-Value) and has type. Helps in sending data to Redux Store
* Dispatch : Redux API to invoke actions
* Reducers :   JS Function accepting oldState and action. Performs the logic(frontend or business) when action is formed
* Component -> Creates action -> Dispatch Action -> Store -> Use reducer to create newState -> Store updates component with newState

### Best Practices for Redux Statment Management
* Side effect operations not allowed in reducer function(Eg. Fetching data using API)
* Better to have separate reducers based on functionalities and use combineReducers API to merge to one as Redux accepts one reducer only

### Redux State Management Working

index.js
```javascript
import { Provider } from 'react-redux'
import { createStore } from 'redux'
import CountReducers from './reducers/CountReducers';

const store = createStore(CountReducers)
ReactDOM.render(
  <Provider store={store}> {/* Required for redux store */}
    <React.StrictMode>
      <ReduxApp>
    <React.StrictMode>
  <Provider>    
```

CountReducer.js
```javascript
const initialState = {
    count: 0
}

export default function CountReducers (state=initialState, action){
    switch (action.type) {
        case 'INCREASE':
            return {
                ...state,
                count: action.payload + 1
            }
        default:
            return state
    }
}
```

CountAction.js
```javascript
export const incrementAction = (count) => {
  return {
    type: 'INCREASE',
    payload: count
  }
}
```

ReduxApp.js
```javascript
class ReduxApp extends Component {
    constructor(props) {
        super(props);
    }
    handleIncrementAction = () => {
        const {incrementAction,count} = this.props
        incrementAction(count)
    }

    render() {
        return (
            <div>
                <button onClick={this.handleIncrementAction}>Increase</button>
            </div>
        );
    }

    function mapStateToProps(state){
    return{
        count: state.count
    }
}

const mapDispatchToProps={
    incrementAction:incrementAction,
}

export default connect(mapStateToProps,mapDispatchToProps)(ReduxApp);
```

### Redux Middleware Concepts
* Middleware is the block of code which acts as a mediator while receiving or generating response
* There are 2 popular ones - Thunk & Saga(The latest one)

### Redux Thunk
* Thunk is a function wrapper to delay the evalutation using async-wait
* Allows to write action creators returning functions instead of typical action object

### Redux Thunk Working
* In progress

### Redux Saga
* State management library to make application side effects(Eg. Asynchronous Actions such as fetching data) easier to execute and handle
* Uses functions of generator for dispatching and listenening to actions
* Complex logic function expressed as pure functions(Sagas). Pure functions are predictable and repeatable

### Redux Saga Working

index.js
```javascript
import { store } from './stores/store'
import ReduxSagaApp from './components/ReduxSagaApp'

ReactDOM.render(
  <Provider store={store}>
    <React.StrictMode>
      <ReduxSagaApp>
    <React.StrictMode>
  <Provider> 
```

store.js
```javascript
import { createStore, applyMiddleware } from 'redux';
import createSagaMiddleware from 'redux-saga';
import CountReducers from '../reducers/CountReducers';
import { watchCountIncrease } from '../sagas/saga'

const sagaMiddleware = createSagaMiddleware();

export const store = createStore(CountReducers, applyMiddleware(sagaMiddleware));

sagaMiddleware.run(watchCountIncrease);
```

CountAction.js
```javascript
export const incrementActionAsyncSaga  = (countSaga) => {
  return {
    type: 'INCREASE_ASYNC_WATCH',
    payload: countSaga
  }
}
```

CountReducer.js
```javascript
case 'INCREASE_ASYNC_WATCH':
            console.log("Action.payload",action)
            return {
                ...state,
                countSaga: action.payload + 1
            }
```

ReduxSagaApp.js
```javascript
<button onClick={this.props.onCountIncrease}>IncreaseAsync</button>

function mapStateToProps(state) {
    return {
        countSaga: state.countSaga
    }
}

const mapDispatchToProps = dispatch => {
    return {
        onCountIncrease: (countSaga) => dispatch({ type: 'INCREASE_ASYNC_WATCH', payload: countSaga})
    };
};

```

### Invoking API Endpoints using asynchronous AJAX request
* AJAX(Asynchronous JavaScript And XML) : Uses XMLHttpRequest object to communicate with servers
* Supports all file formats(JSON,XML,HTML & Text files)
* fetch() : Takes mandatory arg and returns response as promise
* Promise : Object stating eventual completion(fulfilled,rejected & pending)

```javascript
componentDidMount(){
        fetch('https://restcountries.eu/rest/v2/capital/delhi')
        .then(response => response.json())
        .then(responseJSON => {
            this.setState({
                items: responseJSON
                });
            })
        .catch(error => {
            console.log("Fetch errored out",error)
        });
    }
```

### Invoking API Endpoints using Axios
* Axios : Promise(async & await) based HTTP client
* Used for larger applications
* Request and response interception
* Streamlined error handling
* Protection against XSRF
* Support for upload progress
* Response timeout
* Ability to cancel requests
* Support for older browsers
* Automatic JSON data transformation
* https://www.smashingmagazine.com/2020/06/rest-api-react-fetch-axios/

Please refer my Github learning code [here](https://github.com/anupama-sinha/reactjs-redux-project) for complete details

### References
* https://reactjs.org/tutorial/tutorial.html
* https://www.robinwieruch.de/redux-vs-usereducer
* https://medium.com/@lavitr01051977/make-your-first-call-to-api-using-redux-saga-15aa995df5b6
* https://redux-saga.js.org/docs/introduction/BeginnerTutorial.html
* [Babel](https://youtu.be/yLrNwo4wXOs) 
* [Webpack](https://youtu.be/lFjinlwpcHY)