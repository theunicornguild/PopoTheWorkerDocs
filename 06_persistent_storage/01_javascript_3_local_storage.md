Our hardworking monkey forgets everything once you refresh the page. Let's have our Popo evolve long-term memory! This way the user can come back to the tasks at any time. You can try this out in the Live Demo. Add some tasks, refresh the page, the tasks you added will still be there.

There's many ways we can make this happen. We could have the user register an account and store the tasks in the backend using an API to send the tasks data to be stored and retrieved later. But we're gonna use something simpler: [Local Storage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage). Local Storage will store the tasks data in the browser's session cookies.

Move the trello card "User can go back to their old tasks when reloading the page" from "`Backlog`" to "`Doing`".

Using Local Storage to store our tasks can be split into two parts: 1. Storing new tasks, and 2. Retrieving tasks when the page is loaded.

### Storing tasks

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

### Retrieving tasks when loading the page

Add the following method to `App.js`:

```jsx
retrieveFromLocalStorage = () => {
  let tasks = JSON.parse(localStorage.getItem("tasks"));
  if (tasks) {
    this.setState({ tasks: tasks.tasks });
  }
};
```

The `updateLocalStorage()` method stringifies the tasks list, meaning it converts the list of objects into a string. When retrieving this string in the `retrieveFromLocalStorage()` method, we need to parse it in order to convert the string back to a list of objects. If there isn't any, it won't do anything.

Let's call the `updateLocalStorage()` method. Add the following method to `App.js`:

```jsx
componentDidMount() {
  this.retrieveFromLocalStorage();
}
```

(To learn about `componentDidMount()` properly, you should read and learn from [the source](https://reactjs.org/docs/state-and-lifecycle.html) about lifecycle methods. Here's a cheatsheet diagram offered to us by the generous React developers: [cheat sheet diagram](http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/).)

This'll call the `retrieveFromLocalStorage()` method once the page is loaded.

Ta da! Try it! Add some dummy tasks, something you think a workaholic monkey who thinks he's human would do. Refresh the page and see the tasks remain.

Move the trello card to "`Done`", and commit changes to git.
