Install MomentJS with the following command:

```bash
$ yarn add moment
```

Moment will allow us to store datetime objects, compare datetime objects, and generally be able to process the dimension of time. Our little monkey is evolving his perception of time, aww.

Install `react-datetime`:

```bash
$ yarn add react-datetime
```

Let's add the datetime picker in the task creation form. In your `CreateTaskForm`, add this import: `import Datetime from "react-datetime";`, and then add the following above the submit button:

```jsx
<Datetime
  defaultValue="Optional Due Date"
  value={this.state.due}
  onChange={momentObj => {
    this.setState({ due: momentObj });
  }}
/>
```

Also add `due` to the state. So the state becomes:

```jsx
state = {
  title: "",
  details: "",
  due: ""
};
```

Running this now will show you something that looks like it just escaped from a mental asylum. Let's fix that! For `react-datetime` to work and look proper out-of-the-box we need to get the CSS stylesheet it comes with. Create a file "`DatetimePicker.css`" inside `src/`. Copy the contents of [this file](https://github.com/YouCanBookMe/react-datetime/blob/master/css/react-datetime.css) and paste it into `DatetimePicker.css`. Then add the following import in `CreateTaskForm.js`: `import "../DatetimePicker.css";`.

Next, update the `addTask()` method in `CreateTaskForm` to:

```jsx
addTask = () => {
  if (this.state.title) {
    this.props.addTask(this.state.title, this.state.details, this.state.due);
    this.setState({ title: "", details: "", due: "" });
  }
};
```

Update the `addTask()` method in `App` to include the due date:

```jsx
addTask = (title, details, due) => {
  let newTask = { title: title, details: details, due: due };
  [...]
}
```

To display the due date, we don't want to display the actual date and time, we'd rather display the time left until the due date. Like "in an hour", or "in 4 hours". Take a peak into [MomentJS's documentation on this](https://momentjs.com/docs/#/displaying/fromnow/).

In your `Task.js`, change the render's return to:

```jsx
return (
  <p>
    {this.props.task.title} - {this.props.task.details} -{" "}
    {this.props.task.due.fromNow()}
  </p>
);
```

There's a few problems that'll pop up at this point. I'm gonna address them and fix them one by one...
