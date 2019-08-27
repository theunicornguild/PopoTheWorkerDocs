So far our tasks are sorted into today and future tasks. What if our monkey likes to structure his life into Personal and Work? Let's give him the option to do so with labels!

Let's add labels to our default tasks in `TasksStore`:

```jsx
pendingTasks = [
  {
    [...],
    labels: ["Monkey Stuff"],
  },
  {
    [...],
    labels: [],
  }
];
todayTasks = [
  {
    [...],
    labels: ["Monkey Stuff"],
  },
  {
    [...],
    labels: [],
  }
];
```

To display a label for a task we're gonna use [Bootstrap's Pill Badges](https://mdbootstrap.com/docs/react/components/badges/#pills). In your `Task`'s render method, add the following code:

```jsx
import {
  [...],
  MDBBadge
} from "mdbreact";
render() {
  let labels;
  if (this.props.task.labels && this.props.task.labels.length > 0) {
    labels = this.props.task.labels.map(label => {
      return (
        <MDBBadge pill color="primary" className="mr-2">
          {label}
        </MDBBadge>
      );
    });
  }
  [...]
}
```

Then down in the return of the render method, update the JSX to include the labels in the right location:

```jsx
<div className="d-flex justify-content-between">
  <div className="d-flex align-items-start flex-column">
    <div className="flex-row">{labels}</div>
    <div className="d-flex justify-content-start">[...]</div>
  </div>
  <div>[...]</div>
</div>
```

Looks nice doesn't it? You can try adding multiple labels to a task and see how it looks!

---

Popo is complaining that he doesn't have a way for him to add labels when creating a new task. Let's add that to the task creation form!

Let's add in our store a list of label options that the user can choose from when creating a new task. In your `TasksStore`, add the following variable:

```jsx
labelOptions = ["Personal", "Work", "Monkey Stuff"];
```

There's a lovely react package we can use to allow users to choose from existing labels: [react-select](https://react-select.com/).

Install `react-select`:

```bash
$ yarn add react-select
```

In `CreateTaskForm.js`, import the `Select` component:

```jsx
import Select from "react-select";
```

Then down in the render's return, in the modal body we're gonna add the `Select` component as part of the task creation form:

```jsx
<MDBModalBody>
  [...]
  <Select
    options={options}
    isMulti
    value={this.state.labels}
    onChange={this.labelSelect.bind(this)}
  />
  <Datetime
    defaultValue="Optional Due Date"
    value={this.state.due}
    onChange={momentObj => {
      this.setState({ due: momentObj });
    }}
  />
</MDBModalBody>
```

You can see in the code above that the `Select` component needs `options`, `labels` in the state, and `labelSelect()` method. Let's add the `options` in the render method:

```jsx
let options = tasksStore.labelOptions.map(label => {
  return { value: label, label: label };
});
```

`react-select` handles labels by having each label be an object with the keys `value` and `label`. `value` is the actual value of the label, it doesn't get displayed on the page, and `label` is the text that appears on the page for the user to identify this label. Because of this, the way we're displaying the labels needs to be updated to reflect that. In your `Task.js`, where we're defining `labels`, change the following:

```jsx
<MDBBadge pill color="primary" className="mr-2">
  {label}
</MDBBadge>
```

to:

```jsx
<MDBBadge pill color="primary" className="mr-2">
  {label.label}
</MDBBadge>
```

Now the default tasks' labels need to be changed from this:

```jsx
["Monkey Stuff", "Banana"];
```

to:

```jsx
[
  {
    value: "Monkey Stuff",
    label: "Monkey Stuff"
  },
  {
    value: "Banana",
    label: "Banana"
  }
];
```

Back to `CreateTaskForm`. Let's add the `labels` in the state:

```jsx
state = {
  title: "",
  details: "",
  due: "",
  labels: [],
  modal: false
};
```

Lastly, let's add the `labelSelect()` method:

```jsx
labelSelect(value, action) {
  this.setState({ labels: value });
}
```

We need to update the `addTask()` method in `CreateTaskForm` so that it includes labels:

```jsx
addTask() {
  if (this.state.title) {
    tasksStore.addTask(
      this.state.title,
      this.state.details,
      this.state.due,
      this.state.labels
    );
    this.setState({ title: "", details: "", due: "", labels: [] });
    this.toggleModal();
  }
}
```

The `addTask()` method in `TasksStore` needs to handle labels as well:

```jsx
addTask(title, details, due, labels) {
  let newTask = {
    title: title,
    details: details,
    due: due,
    labels: labels,
    id: this.idCounter
  };
  [...]
}
```

---

### Local Storage

Our local storage needs to handle the label options we have. Update the `updateLocalStorage()` and `retrieveFromLocalStorage()` functions to:

```jsx
updateLocalStorage = () => {
  // This next line will stringify the tasks list
  let tasks = JSON.stringify({
    [...]
    labels: this.labelOptions
  });
  localStorage.setItem("tasks", tasks);
};
retrieveFromLocalStorage = () => {
  let tasks = JSON.parse(localStorage.getItem("tasks"));
  if (tasks) {
    [...]
    this.labelOptions = tasks.labels;
  }
};
```
