So far our monkey has no means to finish a task. Popo can only create tasks and delete them. Let's add a checkbox on the task item that when clicked will mark a task as done/finished.

Move the trello card "User can check a task after finishing it" to "`Doing`".

To do this, we need to store the finished tasks separately from the due/unfinished tasks. In your `TasksStore` set the following variables:

```jsx
pendingTasks = [
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
];
doneTasks = [];
```

Update the `addTask()` method to push the new task to the pending tasks as well:

```jsx
addTask(title, details, due) {
  let newTask = {
    title: title,
    details: details,
    due: due,
    id: this.idCounter
  };
  this.idCounter++;
  this.pendingTasks.push(newTask);
  [...]
}
```

We need a method that'll check a task as finished, removing it from the pending and today/future lists and adding it to the done list. Add this method to your `TasksStore`:

```jsx
checkTask = taskId => {
  // Find/Get task
  let task = this.pendingTasks.find(item => item.id === taskId);

  // Remove task from pending, today, and future tasks.
  this.pendingTasks = this.pendingTasks.filter(item => item.id !== taskId);
  this.todayTasks = this.todayTasks.filter(item => item.id !== taskId);
  this.futureTasks = this.futureTasks.filter(item => item.id !== taskId);

  // Add task to done tasks.
  this.doneTasks.push(task);

  // Update local storage.
  this.updateLocalStorage();
};
```

Moving over to `Task`, add this method:

```jsx
checkTask() {
  tasksStore.checkTask(this.props.task.id);
}
```

This method will be triggered when the user clicks on the checkbox. Next, let's add the checkbox! Update the render's return to include the checkbox icon:

```jsx
<MDBListGroupItem>
  [...]
  <div className="d-flex justify-content-between">
    <div className="d-flex align-items-start flex-column">
      <div className="d-flex justify-content-start">
        <div className="align-self-center">
          <MDBIcon
            far
            icon="square"
            size="2x"
            onClick={this.checkTask.bind(this)}
          />
        </div>
        <div className="flex-grow-1 p-3 text-wrap">
          <h5 className="mb-1">{this.props.task.title}</h5>
        </div>
      </div>
    </div>
    <div>
      <MDBCloseIcon
        className="ml-auto"
        onClick={this.toggleDeleteModal.bind(this)}
      />
    </div>
  </div>
  <p className="mb-1">{this.props.task.details}</p>
  {dueDate}
</MDBListGroupItem>
```

Voilà! Now you can check a task as done.

---

### Task Deletion

Now that we have our tasks that are not yet finished in a list of pending tasks, when deleting a task we need to remove it from this list as well. So in your store, update the `deleteTask()` method to:

```javascript
deleteTask = taskId => {
  // Remove task from today and future tasks.
  this.todayTasks = this.todayTasks.filter(item => item.id !== taskId);
  this.futureTasks = this.futureTasks.filter(item => item.id !== taskId);
  this.pendingTasks = this.pendingTasks.filter(item => item.id !== taskId);

  // Update local storage.
  this.updateLocalStorage();
};
```

Removing the task from future and today tasks but not removing it from pending will cause problems and errors.

---

### Local Storage

Now that we've added `pendingTasks` and `doneTasks`, we need to handle them in local storage. Update your `TasksStore`'s `updateLocalStorage()` and `retrieveFromLocalStorage()` to:

```jsx
updateLocalStorage = () => {
  // This next line will stringify the tasks list
  let tasks = JSON.stringify({
    pendingTasks: this.pendingTasks,
    todayTasks: this.todayTasks,
    futureTasks: this.futureTasks,
    done: this.doneTasks,
    idCounter: this.idCounter
  });
  localStorage.setItem("tasks", tasks);
};
retrieveFromLocalStorage = () => {
  let tasks = JSON.parse(localStorage.getItem("tasks"));
  if (tasks) {
    // The following iterations converts a stringified due date to a moment object.
    tasks.pendingTasks.forEach(task => {
      if (task.due) task.due = moment(task.due);
    });
    tasks.todayTasks.forEach(task => {
      if (task.due) task.due = moment(task.due);
    });
    tasks.futureTasks.forEach(task => {
      if (task.due) task.due = moment(task.due);
    });
    this.pendingTasks = tasks.pendingTasks;
    this.todayTasks = tasks.todayTasks;
    this.futureTasks = tasks.futureTasks;
    this.doneTasks = tasks.done;
    this.idCounter = tasks.idCounter;
  }
};
```

Move the trello card to "`Done`", and commit change to git.
