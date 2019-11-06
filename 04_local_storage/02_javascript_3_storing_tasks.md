In your `App.js`, add the following method:

```jsx
updateLocalStorage = () => {
  // This next line will stringify the tasks list
  let tasks = JSON.stringify({
    tasks: this.state.tasks
  });
  localStorage.setItem("tasks", tasks);
};
```

This method stringifies the list of tasks, then uses `localStorage` to store them. Let's use this method appropriately, in the `App`'s `addTask()` method, add this line at the bottom:

```jsx
this.updateLocalStorage();
```

Now everytime a new task is added, local storage is updated to reflect the current list of tasks.
