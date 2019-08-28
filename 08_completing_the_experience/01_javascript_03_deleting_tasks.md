Let's allow our users to delete a task. We can add an `X` icon to the task that when clicked would delete the task.

Move the trello card "User can delete a task" to "`Doing`".

Every task needs to have a unique ID to it. Using this ID we can delete the task that the user wants to delete. In the store, let's add an ID counter and change the default tasks to the following:

```jsx
idCounter = 3;
todayTasks = [
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
```

Now when adding a new task, we need to set its ID and increment our `idCounter`. In the store, update the `addTask()` method to:

```jsx
addTask(title, details, due) {
  let newTask = {
    title: title,
    details: details,
    due: due,
    id: this.idCounter
  };
  this.idCounter++;
  [...]
}
```

MDBootstrap comes with an icon for this: `MDBCloseIcon`. Update your `Task`'s render to:

```jsx
<MDBListGroupItem>
  <div className="d-flex justify-content-between">
    <div className="d-flex align-items-start flex-column">
      <div className="d-flex justify-content-start">
        <div className="flex-grow-1 p-3 text-wrap">
          <h5 className="mb-1">{this.props.task.title}</h5>
        </div>
      </div>
    </div>
    <div>
      <MDBCloseIcon className="ml-auto" />
    </div>
  </div>
  <p className="mb-1">{this.props.task.details}</p>
  {dueDate}
</MDBListGroupItem>
```

When the user clicks on this icon, we'll use [this modal]() to confirm the delete. Once confirmed, the task will be deleted. So we're gonna need a function that'll trigger the modal, and another to delete the task. Add these in your `Task` component:

```jsx
state = {
  modal: false
};
deleteTask() {
  this.toggleDeleteModal();
  tasksStore.deleteTask(this.props.task.id);
}
toggleDeleteModal() {
  this.setState({ modal: !this.state.modal });
}
```

The `deleteTask()` method here will call the store's `deleteTask()` method. Let's create that method! In your `TasksStore` add the following method:

```jsx
deleteTask = taskId => {
  // Remove task from today and future tasks.
  this.todayTasks = this.todayTasks.filter(item => item.id !== taskId);
  this.futureTasks = this.futureTasks.filter(item => item.id !== taskId);

  // Update local storage.
  this.updateLocalStorage();
};
```

Add the following modal to the `Task` render's return:

```jsx
<MDBListGroupItem>
  <MDBModal
    modalStyle="danger"
    className="text-white"
    size="sm"
    position="top"
    isOpen={this.state.modal}
    toggle={this.toggleDeleteModal.bind(this)}
  >
    <MDBModalHeader
      className="text-center"
      titleClass="w-100"
      tag="p"
      toggle={this.toggleDeleteModal.bind(this)}
    >
      Are you sure?
    </MDBModalHeader>
    <MDBModalBody className="text-center">
      <MDBIcon icon="times" size="4x" pulse className="animated tada" />
    </MDBModalBody>
    <MDBModalFooter className="justify-content-center">
      <MDBBtn color="danger" onClick={this.deleteTask.bind(this)}>
        Yes
      </MDBBtn>
      <MDBBtn
        color="danger"
        outline
        onClick={this.toggleDeleteModal.bind(this)}
      >
        No
      </MDBBtn>
    </MDBModalFooter>
  </MDBModal>
  [...]
</MDBListGroupItem>
```

Don't forget the imports:

```jsx
import {
  MDBListGroupItem,
  MDBCloseIcon,
  MDBBtn,
  MDBModal,
  MDBModalBody,
  MDBModalHeader,
  MDBModalFooter,
  MDBIcon
} from "mdbreact";
```

Lastly, to trigger the modal when the `X` icon is clicked, change the `<MDBCloseIcon ... />` to this:

```jsx
<MDBCloseIcon className="ml-auto" onClick={this.toggleDeleteModal.bind(this)} />
```

---

### Local Storage

We need to handle the `idCounter` along with the tasks in local storage. In your `TasksStore`, update `updateLocalStorage()` and
`retrieveFromLocalStorage()` to:

```jsx
updateLocalStorage = () => {
  // This next line will stringify the tasks list
  let tasks = JSON.stringify({
    todayTasks: this.todayTasks,
    futureTasks: this.futureTasks,
    idCounter: this.idCounter
  });
  localStorage.setItem("tasks", tasks);
};
retrieveFromLocalStorage = () => {
  let tasks = JSON.parse(localStorage.getItem("tasks"));
  if (tasks) {
    [...]
    this.idCounter = tasks.idCounter;
  }
};
```

Move the trello card to "`Done`", and commit change to git.
