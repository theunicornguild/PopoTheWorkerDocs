Before starting this, go [here](https://reactjs.org/docs/handling-events.html) and read about event handlers. You'll need to know about event handlers to build this form.

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
<input
  type="text"
  placeholder="Title"
  onChange={e => {
    this.setState({ title: e.target.value });
  }}
/>
```

This element is an input field where once the user types anything in it, what the user types will be stored in the state of this component. An input field for the details:

```jsx
<textarea
  placeholder="Details"
  onChange={e => {
    this.setState({ details: e.target.value });
  }}
/>
```

Same thing here, except this is a textarea. Which means it can have multiple lines. After adding a submit button, the return becomes:

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

If you use this form and hit the submit button, it won't add anything to the list of tasks. Let's make it work!

In your `App.js`, add the following method above the render method:

```jsx
addTask(title, details) {
  let newTask = { title: title, details: details };
  let tasks = this.state.tasks;
  tasks.push(newTask);
  this.setState({ tasks: tasks });
}
```

([read more](https://reactjs.org/docs/state-and-lifecycle.html) about state and component lifecycle.)

Then down in the render's return, change `<CreateTaskForm />` to `<CreateTaskForm addTask={this.addTask.bind(this)}/>`. Next, in the `CreateTaskForm` component, add the following method:

```jsx
addTask() {
  this.props.addTask(this.state.title, this.state.details);
}
```

Then down in the render's return, change:

```jsx
<input type="submit" value="Create Task" />
```

to:

```jsx
<input type="submit" value="Create Task" onClick={this.addTask.bind(this)} />
```

Voilà! Now you can add tasks with the form. However, both the title and details are optional at this point. Let's make the title required. Update the `addTask()` method in `CreateTaskForm` to the following:

```jsx
addTask() {
  if (this.state.title) {
    this.props.addTask(this.state.title, this.state.details);
  }
}
```

Now if you add a task without entering a title, it won't do anything.

The way all of this is working is when the user clicks the submit button in the form it calls `App`'s `addTask()` method, which was passed down to `CreateTaskForm` as a prop, and send it the title and details that the user typed in the form. The `addTask()` method in `App.js` uses `this.setState()` to update the list of tasks in the state. This particular method will trigger the component `App`, and all the components it renders, to re-render. Since the list of tasks were updated to include the new task the user wrote in the form, the list of tasks that are displayed will include this new task. This automatic re-rendering is React's magical secret weapon that puts it above other libraries.

### Clearing the fields

You might've noticed how when you create the task, the title and details you wrote in the form remain there. Don't you think it feels more natural to erase them once the task is created? Well, Popo seems to agree with me.

In your `CreateTaskForm`, update the `addTask()` method to:

```jsx
addTask() {
  if (this.state.title) {
    this.props.addTask(this.state.title, this.state.details);
    this.setState({ title: "", details: "" });
  }
}
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
