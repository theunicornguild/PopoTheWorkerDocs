To help us organize our data, install MobX:

```bash
$ yarn add mobx mobx-react
```

Create a folder inside `src/` called "`Stores`". In `Stores/`, create a file called "`TasksStore.js`". Everything related to data, we're gonna move to this store. Write the following boilerplate code in this new file:

```jsx
class TasksStore {}

const tasksStore = new TasksStore();

export default tasksStore;
```

The powerful thing about React is how the UI updates whenever the state of a component is updated. MobX allows us to extend that so that all components using certain data get re-render when that data source is updated. The store is that data source. ([read more about MobX here](https://mobx.js.org/README.html#core-concepts).)

Right now, the data is stored in the `App`'s state. Let's move it to the `TasksStore.js` file. Change the class definition to:

```jsx
class TasksStore {
  todayTasks = [
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
  ];
  futureTasks = [];
}
```

Now we have the tasks lists in the MobX store. Now delete the state from `App`, we don't need it anymore. Next, move the `addTask`, `updateLocalStorage`, and `retrieveFromLocalStorage` methods from `App` to `TasksStore`, then delete them from `App`. In those methods, remove any reference to state. For example, change `this.state.todayTasks` to `this.todayTasks`, `this.setState({ futureTasks: tasks })` should be changed to `this.futureTasks = tasks`.

Next, to make the UI re-render when the data is modified, add the following below the class definition:

```jsx
decorate(TasksStore, {
  todayTasks: observable,
  futureTasks: observable
});
```

Since we're using moment and other stuff from MobX, you need to add these imports in `TasksStore.js` on top:

```jsx
import { decorate, observable } from "mobx";
import moment from "moment";
```

Lastly, our components used to receive everything they needed as props from `App`. Now, with MobX, they're interact directly with the store. So remove all props sent to our components in `App`'s render method. Your `App`'s render method should look like this:

```jsx
render() {
  return (
    <div className="App">
      <CreateTaskForm />
      <TodayList />
      <FutureList />
    </div>
  );
}
```

Let's make our components use the store...

Add the following imports in `Task.js`, `TodayList.js`, `FutureList.js`, and `CreateTaskForm.js`:

```jsx
import tasksStore from "../Stores/TasksStore";
import { observer } from "mobx-react";
```

Then, in `TodayList.js`, change the export line:

```jsx
export default TodayList;
```

to:

```jsx
export default observer(TodayList);
```

Do the same for the other components. This change makes it so that if the data in the store changes the component `TodayList` will re-render to display the new data.

In `TodayList`, change `this.props.tasks` in the render method to `tasksStore.todayTasks`. In `FutureList` change `this.props.tasks` to `tasksStore.futureTasks`. In your `App`'s `componentDidMount` method, change `this.retrieveFromLocalStorage()` to `tasksStore.retrieveFromLocalStorage()`, and import the store `import tasksStore from "./Stores/TasksStore"`.

Next, in the `CreateTaskForm`, in the `addTask()` method change `this.props.addTask(...)` to `tasksStore.addTask(...)`.

That's it! Now your project's data have been completely migrated to using MobX.

Don't forget to commit changes and push to git.
