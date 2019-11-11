If you use this form and hit the submit button, it won't add anything to the list of tasks. Let's make it work!

In your `App.js`, add the following method above the render method:

```jsx
addTask = (title, details) => {
  let newTask = { title: title, details: details };
  let tasks = this.state.tasks;
  tasks.push(newTask);
  this.setState({ tasks: tasks });
};
```

([read more](https://reactjs.org/docs/state-and-lifecycle.html) about state and component lifecycle.)

Then down in the render's return, change `<CreateTaskForm />` to `<CreateTaskForm addTask={this.addTask}/>`. Next, in the `CreateTaskForm` component, add the following method above the render method:

```jsx
addTask = () => {
  this.props.addTask(this.state.title, this.state.details);
};
```

Then down in the render's return, change:

```jsx
<input type="submit" value="Create Task" />
```

to:

```jsx
<input type="submit" value="Create Task" onClick={this.addTask} />
```

Voilà! Now you can add tasks with the form. However, both the title and details are optional at this point. Let's make the title required. Update the `addTask()` method in `CreateTaskForm` to the following:

```jsx
addTask = () => {
  if (this.state.title) {
    this.props.addTask(this.state.title, this.state.details);
  }
};
```

Now if you add a task without entering a title, it won't do anything.

The way all of this is working is when the user clicks the submit button in the form it calls `App`'s `addTask()` method, which was passed down to `CreateTaskForm` as a prop, and send it the title and details that the user typed in the form. The `addTask()` method in `App.js` uses `this.setState()` to update the list of tasks in the state. This particular method will trigger the component `App`, and all the components it renders, to re-render. Since the list of tasks were updated to include the new task the user wrote in the form, the list of tasks that are displayed will include this new task. This automatic re-rendering is React's magical secret weapon that puts it above other libraries.
