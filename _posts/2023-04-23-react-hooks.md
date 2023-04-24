---
layout: post
title:  "Mastering React's Basic Hooks: A Comprehensive Guide to useState, useEffect, and useContext"
author: Mayuresh
categories: [ React, JS ]
image: assets/images/hooks.png
---

React hooks have become an essential part of building modern React applications. They provide a simple and elegant way to manage state and lifecycle events within functional components.

Hooks were introduced in React 16.8 as a way to reuse stateful logic between components, without needing to use class components. They work by allowing developers to extract stateful logic into reusable functions called "hooks". These hooks can then be used in any functional component, providing a clean and simple way to manage state and lifecycle events.

The first hook we will cover is useState. This hook allows us to add state to functional components. It works by providing a way to declare a state variable and a function to update that variable. For example:

```jsx
import React, { useState } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

In this example, we declare a state variable called "count" and a function to update it called "setCount". We start the count at 0 and display it in a paragraph element. When the button is clicked, we call the setCount function with the new count value.

The next hook we will cover is useEffect. This hook allows us to perform side effects in functional components. It works by providing a way to declare a function that runs after the component has been rendered. For example:

```jsx
import React, { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `You clicked ${count} times`;
  }, [count]);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

In this example, we declare a useEffect function that updates the document title with the current count value. We use the second parameter of useEffect to tell React to only update the title when the count variable changes.

Finally, we will cover useContext. This hook allows us to share data between components without needing to pass props down through every level of the component tree. It works by providing a way to declare a context and a provider component that wraps the component tree. For example:

```jsx
import React, { useContext } from 'react';

const ThemeContext = React.createContext('light');

function Button() {
  const theme = useContext(ThemeContext);

  return (
    <button style={{ backgroundColor: theme }}>
      I am a {theme} button
    </button>
  );
}

function Example() {
  return (
    <ThemeContext.Provider value="dark">
      <Button />
    </ThemeContext.Provider>
  );
}
```

In this example, we declare a ThemeContext and a Button component that uses the useContext hook to access the current theme. We then wrap the Button component in a ThemeContext.Provider component and set the value to "dark".

In conclusion, React hooks provide a powerful way to manage state and lifecycle events in functional components. By using hooks like useState, useEffect, and useContext, we can build complex applications with clean and easy-to-read code. While there are many hooks available, these basic hooks provide a solid foundation for building React applications.
