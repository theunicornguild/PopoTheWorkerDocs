Our monkey, Popo, is such a dedicated workaholic that he'd like to be able to set due date for his task to increase his productivity. To do that, we're gonna use a library called [Moment](https://momentjs.com/), and another library called [react-datetime](https://github.com/YouCanBookMe/react-datetime). `react-datetime` uses `Moment.js` and `React` to wor. So this is a nice harmonious melting pot of JavaScript libraries.

Install MomentJS with the following command:

```bash
$ yarn add moment
```

Moment will allow us to store datetime objects, compare datetime objects, and generally be able to process the dimension of time. Our little monkey is evolving his perception of time, aww.

Install `react-datetime`:

```bash
$ yarn add react-datetime
```

Now let's add the datetime picker in the task creation form. In your `CreateTaskForm`, add this import: `import Datetime from "react-datetime";`, and then add the following above the submit button:

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
addTask() {
  this.props.addTask(this.state.title, this.state.details, this.state.due);
  this.setState({ title: "", details: "", due: "" });
}
```

Update the `addTask()` method in `App` to include the due date:

```jsx
addTask(title, details, due) {
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

### Default tasks don't have a moment object due date

Give the default tasks in `App`'s state the current date and time as the default due date. First, import moment:

```jsx
import moment from "moment";
```

Then add the current date and time to the tasks in the state. The state will be:

```jsx
state = {
  tasks: [
    {
      title: "Eat a banana",
      details: "Find a banana. Eat it.",
      due: moment(),
      id: 1
    },
    {
      title: "Tell The Monkey to get off his monkey butt and do something.",
      details: "",
      due: moment(),
      id: 2
    }
  ]
};
```

### Old tasks stored in Local Storage don't have a default due date

Let's clear out all our tasks from Local Storage. In the browser, either Google Chrome or Safari, open up developer tools (`CMD-ALT-I` on Mac), open the Console, and type "`localStorage.removeItem("tasks")`", and hit Enter. Then close up the developer tools and refresh the page. Your old tasks will be gone, and your new tasks will have a default due date.

### New tasks without a due date cause the application to crash

The application crashes because in `Task.js` we wrote `{this.props.task.due.fromNow()}` on tasks that don't have a `.due`. So this crashes. To fix this, we'd only render `{this.props.task.due.fromNow()}` if `.due` exists. Here's how... Change the render method of `Task` to:

```jsx
render() {
  let dueDate;
  if (this.props.task.due)
    dueDate = <span>- {this.props.task.due.fromNow()}</span>;
  return (
    <p>
      {this.props.task.title} - {this.props.task.details} {dueDate}
    </p>
  );
}
```

This won't call `.fromNow()` on the due date unless there's a moment object due date.

### It still crashes when I refresh!

Yup, this is because the due date is a moment object. It's stringified and stored in local storage, then when it's being retrieved when reloading the page, it's loaded back as a string, not a moment object. Let's fix that!

In your `App.js`, update the `retrieveFromLocalStorage` method to:

```jsx
retrieveFromLocalStorage = () => {
  let tasks = JSON.parse(localStorage.getItem("tasks"));
  if (tasks) {
    // The following iteration converts a stringified due date to a moment object.
    tasks.tasks.forEach(task => {
      if (task.due) task.due = moment(task.due);
    });
    this.setState({ tasks: tasks.tasks });
  }
};
```

Ta da! Problems solved. Now due dates work and are displayed with a nice human-readable message. Just like the monkey wanted.
