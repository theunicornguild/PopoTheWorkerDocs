Our hardworking monkey forgets everything once you refresh the page. Let's have our Popo evolve long-term memory! This way the user can come back to the tasks at any time. You can try this out in the Live Demo. Add some tasks, refresh the page, the tasks you added will still be there.

There's many ways we can make this happen. We could have the user register an account and store the tasks in the backend using an API to send the tasks data to be stored and retrieved later. But we're gonna use something simpler: [Local Storage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage). Local Storage will store the tasks data in the browser's session cookies.

Move the trello card "User can go back to their old tasks when reloading the page" from "`Backlog`" to "`Doing`".

Using Local Storage to store our tasks can be split into two parts: 1. Storing new tasks, and 2. Retrieving tasks when the page is loaded.
