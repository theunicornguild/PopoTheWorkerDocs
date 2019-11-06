Before we can continue, we need to organize our code and files properly.

Instead of displaying the list of tasks in `App.js`, we'll make another component responsible for the list. We'll call this component `TodayList`. Add the following import in `App.js`:

```jsx
import TodayList from "./Components/TodayList";
```

Then in the return of the `render()`, remove the definition of `tasks_list` inside the render method, and replace `{tasks_list}` in the return with:

```jsx
<TodayList tasks={this.state.tasks} />
```

In the import we just added, we're importing a React component called "`TodayList`", and in the return we're _rendering_ it, and giving it a prop called `tasks`, its value is the list of tasks we made in the state above. ([read more](https://reactjs.org/docs/components-and-props.html) about components and props.)

In your `src/` folder, create a folder called "`Components/`". This is where we'll keep our React Components. Create a file called "`TodayList.js`" and another file called "`Task.js`". In your `TodayList.js`, write the following boilerplate code:

```jsx
import React, { Component } from "react";

class TodayList extends Component {
  render() {
    return (

    );
  }
}

export default TodayList;
```

And in the `Task.js`, write the following boilerplate code also:

```jsx
import React, { Component } from "react";

class Task extends Component {
  render() {
    return (

    );
  }
}

export default Task;
```

These blocks of code make React class components called `TodayList` and `Task`. These components alone by this point don't display anything. Let's make them display the list of tasks in a rudimentary way. Change the render method of `TodayList` to this:

```jsx
render() {
  let tasks = this.props.tasks.map(task => <Task task={task} />);
  return <div>{tasks}</div>;
}
```

and add the import `import Task from "./Task";` Then in the `Task` component, change the render to:

```jsx
render() {
  return (
    <p>
      {this.props.task.title} - {this.props.task.details}
    </p>
  );
}
```

Now if you save the files and look at the webpage, you'll see nothing has changed. Add tasks to the list of tasks in `App.js`, you'll see they appear on the webpage just like before. That's the expectation. We just cleaned up the code a bit, we didn't add anything.

This is a good point to create a checkpoint in our git repository. Let's commit and push our changes:

```bash
$ git add .
$ git commit -m "Cleaned up and organized the code"
$ git push
```
