---
author: Andrew Xia
pubDatetime: 2024-02-10T14:44:04.400Z
modDatetime: 2024-02-10T14:44:04.400Z
title: The first step into React, class component and lifecycles
slug:  The first step into React, class component and lifecycles
featured: true
draft: false
tags:
  - docs
description: React class component and its lifecycles
---

# React introduction No.1

## What is React？

- Official: [https://react.dev/](https://react.dev/)
- React is a popular JavaScript library for building user interfaces, particularly for single-page applications where you need a fast, interactive user experience. Developed and maintained by Facebook (now Meta), React was first released in 2013 and has since become one of the most widely used front-end development tools.
- react basic ecosystem:
    
    **React:** This is the core React library that provides the necessary functionality to define React components and manage their lifecycle, state, and props.
    
    **React-DOM:** This package provides DOM-specific methods that can be used at the top level of a web app to enable efficient updates of the web page. It serves as the glue between React components and the DOM.
    
- **Declarative: r**efer to a style of UI (User Interface) development where you describe **what** you want to render, rather than **how** to render it
    
    examples:
    
    ```tsx
    // Imperative
    const list = document.createElement('ul');
    for (let i = 0; i < items.length; i++) {
      let item = document.createElement('li');
      item.appendChild(document.createTextNode(items[i]));
      list.appendChild(item);
    }
    document.body.appendChild(list);
    
    // declarative
    function List({ items }) {
      return (
        <ul>
          {items.map(item => <li key={item}>{item}</li>)}
        </ul>
      );
    }
    ```
    

## Class Component (Not Recommanded)

```tsx
import React from 'react';

class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}!</h1>;
  }
}

export default Welcome;
```

## Functional Component (Recommended)

```tsx
import React from 'react';

function Greeting(props) {
  return <h1>Hello, {props.name}!</h1>;
}

export default Greeting;
```

## JSX

- There is no difference between using **Class Component** and **Functional Component**
    
    ```tsx
    <Welcome name="Hello World"/>
    
    <Greeting name="Hello World"/>
    ```
    
- React uses JSX for templating instead of regular JavaScript. **JSX** is a syntax extension that looks similar to HTML. It is optional but recommended by the React team as it produces "elements" that React’s rendering system can understand and work with more efficiently.
- React use babel to transpile `JSX` into regular javascript (we can play around babel react transpiler in [this link](https://babeljs.io/repl#?browsers=defaults%2C%20not%20ie%2011%2C%20not%20ie_mob%2011&build=&builtIns=false&corejs=3.21&spec=false&loose=false&code_lz=Q&debug=false&forceAllTransforms=false&modules=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&timeTravel=false&sourceType=module&lineWrap=true&presets=env%2Creact%2Cstage-2&prettier=false&targets=&version=7.23.10&externalPlugins=&assumptions=%7B%7D))
    
    ```tsx
    // jsx
    <div>hello world</div>
    
    // after transpiling
    import { jsx as _jsx } from "react/jsx-runtime";
    /*#__PURE__*/_jsx("div", {
      children: "hello world"
    });
    
    // or we can use createElement from React
    import { createElement } from "react";
    
    export default () => {
      return (
        <div>
          <button className="welcome-words">This is from jsx</button>
          {createElement(
            "div", // element type
            {
              className: "welcome-words",
            }, // options (props) of the element
            "This is from createElement", // children
            createElement("div", {
              children: 123,
              className:'welcome-words'
            }) // children
          )}
        </div>
      );
    };
    ```
    

## [DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model) vs Virtual DOM

- DOM: [https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction)
- Virtual DOM:  `DOM tree representation in JavaScript` Fiber model since React v16
- Check the DOM data structure in Devtools

![fiber.png](@assets/images/fiber.png)


## Why Virtual DOM?

### **1. Efficient Diffing Algorithm**

The Virtual DOM implements a diffing algorithm that identifies changes made to the VDOM and efficiently updates only those parts of the real DOM that need to change. When the state of an object changes, React first reflects this change in the Virtual DOM. Then, it compares the updated Virtual DOM with a pre-update version and calculates the minimum number of operations required to update the real DOM. This process is known as "reconciliation."

### **2. Minimizes Direct DOM Manipulation**
> (Really? Please check solid)

Direct manipulation of the real DOM is slow and inefficient because it involves a lot of computation and rendering effort that can significantly degrade performance, especially in complex applications. By abstracting these manipulations to the Virtual DOM, React minimizes direct interactions with the real DOM, thus avoiding costly operations and reflows.

### **3. Batch Updates**

Instead of updating the real DOM in real-time with every single change, the Virtual DOM allows React to batch updates. This batching process means that multiple changes can be applied to the Virtual DOM at once, and a single update operation can be performed on the real DOM. This reduces the number of reflows and repaints, leading to better performance.

### **4. Simplifies Event Handling**

React abstracts away the complexity of direct DOM manipulation and event handling by providing its own event handling system through the Virtual DOM. This simplification allows developers to write cleaner, more declarative code without worrying about cross-browser compatibility issues.

### **5. Facilitates Stateless Components**

### **6. Enables Seamless Integration of Updates**

## Component lifecycle

The methods listed below are from class component. We have to use hooks to achieve these effect in functional component

[https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)

### Mounting (to the real DOM)

<aside>
✅ These methods are called in the following order when an instance of a component is being created and inserted into the DOM:

1. **constructor(props)**
    - The constructor for a React component is called before it is mounted. It's where you initialize state and bind event handler methods.
2. **static getDerivedStateFromProps(props, state)**
    - Called right before rendering, both on the initial mount and on subsequent updates. It should return an object to update the state, or null to update nothing.
3. **render()**
    - The only required method in a class component. It examines **`this.props`** and **`this.state`** and returns React elements, arrays and fragments, portals, string or number, Booleans, or null.
4. **componentDidMount()**
    - Invoked immediately after a component is mounted (inserted into the tree)
</aside>

### Updating

<aside>
✅ An update can be caused by changes to props or state. These methods are called in the following order when a component is being re-rendered:

1. **static getDerivedStateFromProps(props, state)**
    - Called before the render method and used to update the state based on changes in props over time.
2. **shouldComponentUpdate(nextProps, nextState)**
    - Allows you to cancel the updating process. Return false to prevent the rendering process.
3. **render()**
4. **getSnapshotBeforeUpdate(prevProps, prevState)**
    - Called right before the most recently rendered output is committed to the DOM. It returns a snapshot value or null.
5. **componentDidUpdate(prevProps, prevState, snapshot)**
    - Invoked immediately after updating occurs. This method is not called for the initial render.
</aside>

### Unmounting

<aside>
✅ This method is called when a component is being removed from the DOM:

1. **componentWillUnmount()**
    - Perform any necessary cleanup in this method, such as invalidating timers, canceling network requests, or cleaning up any subscriptions that were created in **`componentDidMount`**.
</aside>

A simple example:

```tsx
import React from "react";

interface UserProfileProps {
  name: string;
  bio: string;
}

class UserProfile extends React.Component<
  UserProfileProps,
  { name: string; bio: string }
> {
  constructor(props: UserProfileProps) {
    super(props); // Must call the base constructor with props

    // Initializing state
    this.state = { ...props }
    
    // Binding the handleUpdateBio method to this component instance
    this.handleUpdateBio = this.handleUpdateBio.bind(this);
  }

  componentDidMount(): void {
    console.log("User profile mount: ", this.state);
  }

  componentDidUpdate(
    prevProps: Readonly<UserProfileProps>,
    prevState: Readonly<{ name: string; bio: string }>,
    snapshot?: any
  ): void {
    console.log("User profile before update: ", prevState);
    console.log("User profile after update: ", this.state);
  }

  componentWillUnmount(): void {
    console.log("User profile unmounted");
  }

  // Event handler for updating the bio
  handleUpdateBio(newBio: string) {
    this.setState({ bio: newBio });
  }

  render() {
    return (
      <div>
        <h1>{this.state.name}</h1>
        <p>{this.state.bio}</p>
        {/* Example button to demonstrate updating state. 
            In a real app, the newBio would come from user input */}
        <button
          onClick={() => this.handleUpdateBio("Updated bio information.")}
        >
          Update Bio
        </button>
      </div>
    );
  }
}

export default UserProfile;
```

## What are props and states?

### **States**

The **state** of a React component refers to an object that holds some information that may change over the lifetime of the component. It is managed within the component (similar to variables declared within a function). Changes to the state trigger a re-render of the component, allowing it to reflect the new state in the UI. State is particularly useful for handling user inputs, data retrieved from network requests, and other dynamic information that changes over time.

Key characteristics of state:

- **Mutable**: State can be changed using the **`setState`** method in class components or the **`useState`** hook in functional components.
- **Local**: State is local to the component it is defined in, though it can be passed down to child components via props.
- **Asynchronous**: State updates may be asynchronous for performance reasons.

### **Props**

**Props** (short for properties) are read-only components that are passed from parent to child component(s) as an object. They are similar to function parameters and allow for data to flow down the component tree. Props are immutable from within the receiving component, which means that a component cannot change its props, but it can pass callback functions through props to allow changes to be communicated back to parent components.

Key characteristics of props:

- **Read-only**: Props cannot be modified by the component that receives them.
- **Immutable**: Trying to change a prop from within a component will not work and will result in a React error.
- **Reusable**: Components can be reused by passing different data as props.
