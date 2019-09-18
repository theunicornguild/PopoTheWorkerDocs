Let's create our hard working Popo app! `cd` into wherever you'd like to store the project's code, and run the following command:

```bash
$ yarn create react-app popotheworker
```

Once that's done, run the project website with the following command:

```bash
$ cd popotheworker
$ yarn start
```

This should automatically open up the following page in your default browser:

![New React App](https://i.imgur.com/xtR6USh.png)

## Project Files

Let's walk through the important files that we get out-of-the-box when creating a React app...

```
package.json
src/
    App.js
    index.js
```

- `package.json` contains the packages, and their versions, that our app needs to run.
- `src/App.js` is the main ReactJS file that displays the webpage itself.
- `src/index.js` renders `App.js` to the browser.

There's plenty other files, but they're not important to us.

---

### Hello, React World!

Let's do the obligatory introductory "Hello World!" app that you do when you're learning a new techology/language.

In your `src/App.js`, look for and change the following:

```
Edit <code>src/App.js</code> and save to reload.
```

to:

```js
Hello, React World!
```

Save the file and go to the webpage again. Your webpage should refresh automagically and you should see what you wrote appear on the page.

Voilà!
