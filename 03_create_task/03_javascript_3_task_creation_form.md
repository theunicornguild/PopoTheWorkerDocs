### Event Handlers

Event handlers are functions attached to certain events that might happen. One event is when the user clicks a button, for example. So an event handler is a function that runs when the user clicks a certain button. To learn more, go [here](https://reactjs.org/docs/handling-events.html) and read about event handlers. You'll need to know about event handlers to build this form.

Make a new file in `Components/` called "`CreateTaskForm.js`". This component's state must store the title and details of the task the user is creating. So, write the following in the file:

```jsx
import React, { Component } from "react";

class CreateTaskForm extends Component {
  state = {
    title: "",
    details: ""
  };
  render() {
    return (

    );
  }
}

export default CreateTaskForm;
```

Let's add an input field for the title of the task:

```jsx
render() {
  return (
    <input
      type="text"
      placeholder="Title"
      onChange={e => {
        this.setState({ title: e.target.value });
      }}
    />
  );
}
```

This element is an input field where once the user types anything in it, what the user types will be stored in the state of this component. ([read more about `this.setState()` here](https://reactjs.org/docs/react-component.html#setstate).) An input field for the details:

```jsx
render() {
  return (
    <input
      type="text"
      placeholder="Title"
      onChange={e => {
        this.setState({ title: e.target.value });
      }}
    />
    <textarea
      placeholder="Details"
      onChange={e => {
        this.setState({ details: e.target.value });
      }}
    />
  );
}
```

Same thing here, except this is a textarea. Which means it can have multiple lines. In React, you can't have sibling elements/components in the render's return. So in this case, we need to wrap them in a `<div>` element so that we're returning only a single element.

After adding a submit button, the return becomes:

```jsx
return (
  <div>
    <input
      type="text"
      placeholder="Title"
      onChange={e => {
        this.setState({ title: e.target.value });
      }}
    />
    <textarea
      placeholder="Details"
      onChange={e => {
        this.setState({ details: e.target.value });
      }}
    />
    <input type="submit" value="Create Task" />
  </div>
);
```

Let's display this form... In your `App.js`, add the import `import CreateTaskForm from "./Components/CreateTaskForm";`, then add `<CreateTaskForm />` in the return to render the form. The return will be:

```jsx
return (
  <div className="App">
    <CreateTaskForm />
    <TodayList tasks={this.state.tasks} />
  </div>
);
```
