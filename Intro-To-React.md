# Welcome to React

* React is a Javascript library, not a framework.

* A library provides methods and classes that will help you with your application design and control flow.
* A framework provides an application design and a control flow which will take your code and call it for you.

* React is a library, so it gives you the control and provides classes and methods that will help you.

## Component-based Development

* React excels at creating small components, or pieces, that fit together to make something bigger.

* Building with components makes it easier to test and find problems, easier to work with a group on the same project, and easier to make changes later.

* Other methods of building applications are MVC, etc.

### Architecture

* Component based apps are comprised of components, obviously, with React, all components are rendered starting with something called the 'root component' from there components will branch out to other 'sub-components' i.e. header component, body component, and footer component.

```
                     root           - Root (Parent Components)
                /     |     \
             header  body  footer   - Level 1 (Child Components to Root)
             /       /  \
            X       X    X          - Level 2 (Child Components to Level 2)
```

* Sub-components can also spawn other sub-components, quickly increasing the complexity of a project.

## The Virtual DOM

* The DOM, or Document Object Model, is the interface for HTML and XML documents. It allows us to provide the structure, style, and content of a webpage.

  * The DOM decomposes the HTML elements into nodes and objects that allow programs written in JavaScript and other languages to interact with the webpage. The DOM provides an API interface that allows developers to query and modify objects and nodes. For example `.getElementByID()` is a method that the DOM extends to Javascipt.

  * ["The DOM was designed to be independent of any particular programming language, making the structural representation of the document available from a single, consistent API. Though we focus exclusively on JavaScript in this reference documentation, implementations of the DOM can be built for any language..."](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction)

  * [More information on the DOM](https://www.w3schools.com/js/js_htmldom.asp)

* React stands between you and the DOM to make rendering simpler and to make performance better. React does this by working with a virtual DOM.

* The Virtual DOM allows React to do its computations within this abstract world and skip the 'real' DOM operations, often slow and browser-specific. When a change is made the old Vitual DOM is destroyed, a new one is created containing the updated components, then a comparison is algorithmically made between the Virtual DOM and the actual DOM. Any differences between the two are updated in the browser.

* [There's no big difference between the 'regular' DOM and the virtual DOM. This is why the JSX parts of the React code can look almost like pure HTML.]('http://reactkungfu.com/2015/10/the-difference-between-virtual-dom-and-dom/')

* To understand the details of how the DOM is updated we must first understand two data types in React; ReactElement and ReactComponent.
  * ReactElement's can be thought of as visual representations of the virtual DOM that are stateless, almost all HTML tags are ReactElements. These elements are rendered into the actual DOM and are no longer controlled by React.
  * ReactComponent's are the same as ReactElements with the only exception being that they are stateful and they do not have access to the Virtual DOM.
  * Whenever the state changes the component is re-rendered. ReactComponent's can be converted to a ReactElement's, the new ReactElement's are re-rendered in the Virtual DOM. Any changes between the virtual and actual DOM are converted to HTML DOM code that is executed in the actual DOM resulting in an updated web application.

## Diving into JSX

* JSX is slang for Javascript-XML, JSX is a preprocessor (transpiler) step that adds XML syntax to Javascript code. It is syntactic sugar that allows us to write JS and HTML within the same file.

  * Im sure this raises several questions:
    * What is a transpiler? ["Transpilers, or source-to-source compilers, are tools that read source code written in one programming language, and produce the equivalent code in another language."](https://scotch.io/tutorials/javascript-transpilers-what-they-are-why-we-need-them)
    * What is XML? XML is a data format similar to JSON,["it was designed to describe data."](https://www.xmlfiles.com/xml/xml-intro/)

* When the compiler looks at our code it will read the JSX and convert it to native JS.

## Understanding Import/ Export

* In React we can share functionality between files by importing and exporting functions, variables, arrays, etc.

* This is done by creating functionality in one file and declaring `export default ExampleComponent`, this means that any file that imports the file containing the `ExampleComponent` functionality, make the the function available by default.

* If you decline to use `export default` you must destructure the imported component i.e. `import {<exportedComponent>} from component.file`

* Additonally we can place export in front of any functions that we want to share:
  `export function add(guest) { }`

## Uni-Directional Data-Flow

* Unlike its predecessors React manages data flow by allowing it to move in one direction. This eliminates or mitigates the possibility of unintentional changes in state occuring throughout an application.

* Data tends to flow as follows in react: App Component --> Parent Component --> Child Component
  * If any data changes in a child component that change will not necessarily result in a change at the parent level, unless that is what the developer wanted.

### State

* State is simply data that is contained within a particular component that is subject to change. If an application has any interactivity the data keeping track of changes is considered the state of the component i.e. input fields on a sign-up form, data fetched from a server, notifications for updates in a facebook activity feed, a clock that updates every minute, etc.

* [State is considered local or encapsulated (private) to a component and is not accessible by other components regardless of the relationship between components.](https://reactjs.org/docs/state-and-lifecycle.html#the-data-flows-down) Although components do not share their state they can be imported/ exported as we saw earlier. For example you may have a parent component that renders a page with many child components that are imported to the parent.

* State allows Developers to manage changes at the component level but it also allows the DOM to re-render only the components that have state changes rather than the entire application. We no longer need to continually search for changes because we explicitly state what changes occur in the application using the this.setState() method.

* There are situations where managing state in a component is not necessary these components are stateless.

* Example of a stateful component that updates its header text when a user enters numbers into the input field.

![Alt Text](https://github.com/ScottWorks/React-Notes/blob/master/num-render-demo.gif)

```JS
/*Stateful component Classes in React follow a very
similar pattern as Javascript*/
class Counter extends React.Component {
  /*State stores the data specific to that component.
  We must first initialize state, it must be create
  before we can change/ edit it*/

  constructor() {
    /*the super() method gives the constructor access
    to the parent class, in this case the parent class
    is the Component*/

    super();

    /*state should contain anything that the application
    needs to keep track of that will change over time,
    i.e. input values, arrays that will be displayed, etc...*/

    this.state = {
      count: ''
    };

    // See notes below regarding binding methods
    // this.storeInput = this.storeInput.bind(this);
  }

  storeInput(event) {
    /*if we console.log(event), the DOM will return an object
    containing many properties about the action that caused
    it to appear. Most commonly we will use the target
    property. In this case the target property is returning
    what is being typed in our input to the storeInput() method.*/

    // returns the value present in the input element
    // console.log(event.target.value)

    // returns the input element from the DOM
    // console.log(event.target)

    // returns the entire object from the DOM
    // console.log(event)

    /*Correct way to change state, use this.setState() to update
    state. This merges the setState object with the current
    state and gets rendered once every second.

    setState() enqueues changes to the component state and
    tells React that this component and its children need to
    be re-rendered with the updated state. This is the primary
    method you use to update the user interface in response
    to event handlers and server responses.*/

    this.setState({
      count: event.target.value
    });
  }

  render() {
    return (
      <div>
        <input
          type="text"
          placeholder="Enter a number"
          onChange={(event) => this.storeInput(event)}
          /*Because we made onChange a callback with the event
          arguement we are effectively passing the event parameter
          to the storeInput method. If we wanted to we could also
          forego the callback method and simply bind storeInput as
          we did above with:

          "this.storeInput = this.storeInput.bind(this);"

          Then we can write onChange as:

          "onChange={this.storeInput}"*/
        />
        <h1>Rendered Number: {this.state.count}</h1>
      </div>
    );
  }
}

export default Counter;
```
