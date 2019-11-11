You might've noticed how when you create the task, the title and details you wrote in the form remain there. Don't you think it feels more natural to erase them once the task is created? Well, Popo seems to agree with me.

In your `CreateTaskForm`, update the `addTask()` method to:

```jsx
addTask = () => {
  if (this.state.title) {
    this.props.addTask(this.state.title, this.state.details);
    this.setState({ title: "", details: "" });
  }
};
```

Then update the input fields to:

```jsx
<input
  type="text"
  placeholder="Title"
  value={this.state.title}
  onChange={e => {
    this.setState({ title: e.target.value });
  }}
/>
<textarea
  placeholder="Details"
  value={this.state.details}
  onChange={e => {
    this.setState({ details: e.target.value });
  }}
/>
```

When adding the task, the title and details in the state will be reset, triggering the re-render, and the value in the input fields will take the values in the state, which have been reset. Voilà! Simple and clean.

---

Your resulting `CreateTaskForm.js` file should look like this:

```jsx
import React, { Component } from "react";

class CreateTaskForm extends Component {
  state = {
    title: "",
    details: ""
  };
  addTask = () => {
    if (this.state.title) {
      this.props.addTask(this.state.title, this.state.details);
      this.setState({ title: "", details: "" });
    }
  };
  render() {
    return (
      <div>
        <input
          type="text"
          placeholder="Title"
          value={this.state.title}
          onChange={e => {
            this.setState({ title: e.target.value });
          }}
        />
        <textarea
          placeholder="Details"
          value={this.state.details}
          onChange={e => {
            this.setState({ details: e.target.value });
          }}
        />
        <input type="submit" value="Create Task" onClick={this.addTask} />
      </div>
    );
  }
}

export default CreateTaskForm;
```
