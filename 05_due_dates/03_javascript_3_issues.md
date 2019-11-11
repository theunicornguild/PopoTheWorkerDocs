Implementing due dates caused some issues to pop up throughout our little site. Let's fix them one by one...

### Default tasks don't have a moment object due date

Give the default tasks in `App`'s state the current date and time as the default due date. First, import `moment` in your `App.js`:

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
      due: moment()
    },
    {
      title: "Tell The Monkey to get off his monkey butt and do something.",
      details: "",
      due: moment()
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
