### Basics
* Uses Javascript 
* Developed by Facebook
* [Download & Install](https://nodejs.org/en/download/)
* Installation gets node and npm 
* Node is the runtime execution enviroment to run our code

### NPM(Node Package Manager)
* Helps in downloading Dependencies from 3rd Party Repositories and integrate in our project.
* All versions and name details can be searched from [NPM Page](https://www.npmjs.com/)

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

### Nodemon 
* Automatically Refreshes code 
* Used as devDependency as above

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
### Redux State Management
* Coming up

### Webpack
* Coming up
