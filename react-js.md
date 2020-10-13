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
```java
//Source Class
export class App extends React.Component
//Importing Class
import { App } from './App';
``` 

* Default Export
```java
//Source Class
export default class App extends React.Component
//Importing Class
import App from './App';
```
### Functions vs React Component Class
* Function are stateless as constructor not allowed. 
* Only props sent and React elements are returned. 
* Lifecycle hook methods cannot be used.
```java
function App() 
{ 
  return(
      <p>Hello Anupama<p>
  ); 
} 
```  
* React Components are stateful as constructors can have states. 
* Lifecycle hook methods can be used.
```java
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

### Nodemon 
* Automatically Refreshes code 
