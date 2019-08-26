Comparing what we've built so far with the Live Demo, you can see our working monkey needs a makeover, with lots of makeup. Like, so much makeup that he looks like a doll. We want our monkey to look like a doll. So fake people would think he's mass produced, at a factory with so much chemicals that workers there wear gas masks.

In walks Mr. Material Design Bootstrap! Let's install [MDBootstrap](https://mdbootstrap.com/docs/react/) in our project:

```bash
$ yarn add mdbreact
```

Add the following imports in your `index.js` file for the CSS styling to work with `MDBReact`:

```jsx
import "@fortawesome/fontawesome-free/css/all.min.css";
import "bootstrap-css-only/css/bootstrap.min.css";
import "mdbreact/dist/css/mdb.css";
```

First thing to do is to use a container. In `App.js`, import this: `import { MDBContainer } from "mdbreact";` and in the render's return, change this:

```jsx
<div className="App">[...]</div>
```

to:

```jsx
<MDBContainer>[...]</MDBContainer>
```

We're gonna completely rework our task creation form. We'll have a button that once clicked will display a modal. On that modal will be the form with the input fields and submit button. We're gonna use a modified version of [this modal from MDBootstrap](https://mdbootstrap.com/docs/react/modals/basic/#simple-contact).

```jsx
<MDBBtn ...>
  // (1) button to bring up the modal here
</MDBBtn>
<MDBModal ...>
  <MDBModalHeader ...>
    // (2) header here
  </MDBModalHeader>
  <MDBModalBody>
    // (3) form here
  </MDBModalBody>
  <MDBModalFooter ...>
    // (4) footer here
  </MDBModalFooter>
</MDBModal>
```

The code for this modal consists of four main parts:

1. The button that when clicked would bring up the modal.
2. The modal header.
3. The modal body, which is where we'll display the form.
4. The modal footer, which is where we'll display the Cancel and Add Task buttons.

We're gonna need a method to toggle the modal on/off, so add this to your `CreateTaskForm` component:

```jsx
toggleModal() {
  this.setState({ modal: !this.state.modal });
}
```

A method to cancel creating a task:

```jsx
cancelTask() {
  this.setState({ taskText: "", taskDetails: "", due: "", labels: [] });
  this.toggleModal();
}
```

And lastly, a method to add a task:

```jsx
addTask() {
  if (this.state.title) {
    tasksStore.addTask(this.state.title, this.state.details, this.state.due);
    this.setState({ taskText: "", taskDetails: "", due: "" });
    this.toggleModal();
  }
}
```

We also need to add a variable to our state. Update `CreateTaskForm`'s state to:

```jsx
state = {
  title: "",
  details: "",
  due: "",
  modal: false
};
```

These will be used throughout the modal code. We'll be using MDBootstrap's `MDBInput` in place of our old `input` tag.

Change your render method's return to this:

```jsx
<div>
  <MDBBtn outline color="primary" onClick={this.toggleModal.bind(this)}>
    New Task
  </MDBBtn>
  <MDBModal
    isOpen={this.state.modal}
    toggle={this.toggleModal.bind(this)}
    size="sm"
  >
    <MDBModalHeader>Add A New Task</MDBModalHeader>
    <MDBModalBody>
      <MDBInput
        type="text"
        label="Task"
        onChange={e => this.setState({ title: e.target.value })}
        value={this.state.title}
        placeholder="Task"
      />
      <MDBInput
        type="textarea"
        label="Details (Optional)"
        onChange={e => this.setState({ details: e.target.value })}
        placeholder="Optional details"
        value={this.state.details}
      />
      <Datetime
        defaultValue="Optional Due Date"
        value={this.state.due}
        onChange={momentObj => {
          this.setState({ due: momentObj });
        }}
      />
    </MDBModalBody>
    <MDBModalFooter>
      <MDBBtn color="secondary" size="sm" onClick={this.cancelTask.bind(this)}>
        Cancel
      </MDBBtn>
      <MDBBtn color="primary" size="sm" onClick={this.addTask.bind(this)}>
        Add Task
      </MDBBtn>
    </MDBModalFooter>
  </MDBModal>
</div>
```

Most of this code is self-explanatory. For the bits that aren't, go [here](https://mdbootstrap.com/docs/react/modals/basic/#docsTabsAPI) and read about the API for the modal to learn more about what they do.

---

The way our tasks are displayed currently is super ugly and messy. To beautify the list of tasks, we're gonna use [MDBootstrap's List Group](https://mdbootstrap.com/docs/react/components/list-group/#custom-content). So to facilitate it, in your `FutureList` and `TodayList` change the following:

```jsx
{
  tasks;
}
```

to:

```jsx
<MDBListGroup>{list}</MDBListGroup>
```

and import the component: `import { MDBListGroup } from "mdbreact";`

In your `Task`'s render, change:

```jsx
<p>
  {this.props.task.title} - {this.props.task.details} {dueDate}
</p>
```

to:

```jsx
<MDBListGroupItem>
  {this.props.task.title} - {this.props.task.details} {dueDate}
</MDBListGroupItem>
```

And import this: `import { MDBListGroupItem } from "mdbreact"`. Now this alone makes our list much nicer and more organized. But we're not gonna stop here. Oh no. Our monkey can be more beautiful than this.

Change your `Task` component to this:

```jsx
class Task extends Component {
  render() {
    let dueDate;
    if (this.props.task.due)
      dueDate = <small>{this.props.task.due.fromNow()}</small>;
    return (
      <MDBListGroupItem>
        <div className="d-flex justify-content-between">
          <div className="flex-grow-1 p-3 text-wrap">
            <h5 className="mb-1">{this.props.task.title}</h5>
            <p className="mb-1">{this.props.task.details}</p>
          </div>
        </div>
        {dueDate}
      </MDBListGroupItem>
    );
  }
}
```

This code uses Bootstrap's [Flexbox](https://mdbootstrap.com/docs/react/utilities/flexbox/). Read more about it to better understand how this task UI is laid out.
