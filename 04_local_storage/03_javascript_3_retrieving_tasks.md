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

`componentDidMount()` is a built-in function that runs only once when the component is being "_mounted_" on the page.

(To learn about `componentDidMount()` properly, you should read and learn from [the source](https://reactjs.org/docs/state-and-lifecycle.html) about lifecycle methods. Here's a cheatsheet diagram offered to us by the generous React developers: [cheat sheet diagram](http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/).)

This'll call the `retrieveFromLocalStorage()` method once the page is loaded.

Ta da!
