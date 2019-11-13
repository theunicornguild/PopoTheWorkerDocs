In your `App.js`, remove the following imports:

```jsx
import logo from "./logo.svg";
import "./App.css";
```

Then replace the following import:

```jsx
import React from "react";
```

with:

```jsx
import React, { Component } from "react";
```

Then change the following definition:

```jsx
function App() {...}
```

to:

```jsx
class App extends Component {...}
```

Then wrap the return of the `App` component in a `render() {...}` method:

```jsx
render() {
  return (
    [...]
  );
}
```

This changes the App component from a function component to a class component ([read more](https://reactjs.org/docs/components-and-props.html#function-and-class-components)). These changes to the `App` component are necessary for the next step: State. ([read more about State](https://reactjs.org/docs/state-and-lifecycle.html).)

In your `App.js`, add the following state above the `render()` method:

```jsx
class App extends Component {
  state = {
    tasks: [
      {
        title: "Eat a banana",
        details: "Find a banana. Eat it."
      },
      {
        title: "Tell The Monkey to get off his monkey butt and do something.",
        details: ""
      }
    ]
  }
  [...]
}
```

This is a list of two tasks. Later the tasks will be dynamic, but for now we're working with default static data. To display this data, change the render method of `App.js` to:

```jsx
render() {
  let tasks_list = this.state.tasks.map(task => (
    <p key={task.title}>
      {task.title} - {task.details}
    </p>
  ));
  return <div className="App">{tasks_list}</div>;
}
```

In the definition of `tasks_list` we're iterating through the `this.state.tasks` list using the `.map()`. We're passing it the unique `key` prop that it requires. Since our tasks don't have unique IDs, we're gonna use the task's title. I won't explain everything about the `.map()` method, so [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map), read up.

In the return, we wrote `{tasks_list}`. This is how we render the list of tasks as paragraphs (`<p>` tags) that we defined in the lines above.
