To keep our monkey focused, let's separate the tasks for today from the tasks for the future.

To tell apart the tasks for today and the tasks for the future, change the state of your `App` to:

```jsx
state = {
  todayTasks: [
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
  ],
  futureTasks: []
};
```

Now they're in two separate lists. Since we just changed the name in the state for the tasks list, we need to change references to it. Change it's name in the `addTask`, `updateLocalStorage`, `retrieveFromLocalStorage`, and `render` functions.

When we're adding a new task, we need to check if the task's due date is in the future. If it is, then we add it to `futureTasks`. So, change your `App`'s `addTask()` method to:

```jsx
addTask(title, details, due) {
  let newTask = { title: title, details: details, due: due };
  if (due && due.isAfter(moment(), "day")) {
    let tasks = this.state.futureTasks;
    tasks.push(newTask);
    this.setState({ futureTasks: tasks });
  } else {
    let tasks = this.state.todayTasks;
    tasks.push(newTask);
    this.setState({ todayTasks: tasks });
  }
  this.updateLocalStorage();
}
```

The main addition that needs explaining here is this: `if (due && due.isSameOrBefore(moment(), "day"))`. Here we're checking if `due` is defined (so if there's a due date set), and if there is we're checking if the due date is today or in the past by `due.isSameOrBefore(moment(), "day")`. This method is built-into the `MomentJS` library. You can read more about it [here](https://momentjs.com/docs/#/query/is-after/).

In your `TodayList`'s render method, change the return to:

```jsx
return (
  <div>
    <h3>Today</h3>
    {tasks}
  </div>
);
```

Next, copy everything in the "`TodayList.js`" file, and paste it into a new file called "`FutureList.js`" in your "`src/`". Then change any mentions of "Today" to "Future": The class name, the export at the bottom of the file, and the heading in the render.

In your `App.js`, import and render the `FutureList`:

```jsx
[...]
import FutureList from "./Components/FutureList";

class App extends Component {
  [...]
  render() {
    return (
      <div className="App">
        <CreateTaskForm addTask={this.addTask.bind(this)} />
        <TodayList tasks={this.state.todayTasks} />
        <FutureList tasks={this.state.futureTasks} />
      </div>
    );
  }
}

export default App;
```

Lastly, let's not forget Local Storage. Update your `updateLocalStorage()` method to include future tasks:

```jsx
updateLocalStorage = () => {
  // This next line will stringify the tasks list
  let tasks = JSON.stringify({
    todayTasks: this.state.todayTasks,
    futureTasks: this.state.futureTasks
  });
  localStorage.setItem("tasks", tasks);
};
```

Then update the `retrieveFromLocalStorage()` method to include future tasks as well:

```jsx
retrieveFromLocalStorage = () => {
  let tasks = JSON.parse(localStorage.getItem("tasks"));
  if (tasks) {
    // The following iteration converts a stringified due date to a moment object.
    tasks.todayTasks.forEach(task => {
      if (task.due) task.due = moment(task.due);
    });
    tasks.futureTasks.forEach(task => {
      if (task.due) task.due = moment(task.due);
    });
    this.setState({
      todayTasks: tasks.todayTasks,
      futureTasks: tasks.futureTasks
    });
  }
};
```

And there you have it! Our monkey is maturing and growing a human-like perception of time. He can now see the future.

TODO: INSERT MEME-Y GIF OF MONKEY SEEING THE FUTURE
