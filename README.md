# React To-do App

Following this tutorial you'll review these topics:

- Setup a React App using React Script
- Writing HTML with JSX
- Adding SCSS to React
- Using Font Awesome with React
- Creating React components
- Understanding the state
- Persisting data with localStorage
- Routing with React Router


## 1. Setting up our own App with `React Scripts`

[React Scripts](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md) is an npm package that is developed and maintained by the core React developers at Facebook. It helps to simplify the initial setup of React applications with Webpack, Babel and many more useful tools and features.

### 1.1 Creating the folder structure

The first thing we need to do is to create the folder structure, that our application will use. The folder structure of projects you will work on in your career as a developer might be different, but the following one is a common solution.

```markdown
- react-todo-list
  - public
  - src
    - components
    - modules
    - sass
```

### 2 Installing packages

We use [npm](https://www.npmjs.com/) to install all needed packages for our project.

```bash
npm install
```

## 3. Creating the first component

There are two types of components in React, that we will talk about in this tutorial. We will start with `class components`, to show many of the features that components can have.

**Class components** are extending the `React.component` class and offer a lot of features, like the `State` we will talk about later. They are the solution for more complex components.

**Stateless function components** are using a function to return the rendered JSX, do not use a state and are faster to load.

### 3.1 Hello World

[React Scripts](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md) will look for an `src/index.js` file by default, so let's create this and write our first component in there. We will move the component to its own file in another step, but keep it simple for now.

```markdown
src/index.js
```

```jsx
import React from 'react';
import { render } from 'react-dom';

render(<p>Hello World!</p>, document.querySelector('#main'));
```

Let's see what we got there.

1. We import React from its npm package.
1. We import ReactDOM to render our components to the HTML DOM.
1. We render a paragraph into our **#main** DIV element.

Now, that we've got something we want to render, let's start our React application with the following command.

```bash
npm start
```

The result should look like this:

![Screenshot: Hello World](./docs/images/hello-world.png)

If you get an error on Ubuntu, stating that there would not be enough disk space or the limit of inodes would be reached, run the following command in your terminal.

```bash
echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p
```

Okay, so we're able to render something into our DOM. The result should be a nice paragraph with the text "Hello World!".

### 3.2 A header component

Now, that we've seen that everything works in general, let's add the class for our first component. We will name it "Header" and use it to display the top part of our application.

Every class component that we create needs to have the `render()` method, that returns [JSX](https://reactjs.org/docs/introducing-jsx.html), which is the template language of React.

```markdown
src/index.js
```

```jsx
import React from 'react';
import { render } from 'react-dom';

class Header extends React.Component {
  render() {
    return <h1>What to do?</h1>;
  }
}

render(<Header />, document.querySelector('#main'));
```

With these changes we updated the code to

1. include our first class component.
1. render the component instead of plain HTML.

We should see the `h1` in the browser now.

![Screenshot: First Component](./docs/images/first-component.png)

### 3.2. Using the component directory

We don't want to put all our components into the `index.js` file, as this becomes quite messy over time. It would also be bad for the overall performance of our application.

We create the new file `Header.js` in the `src/components` folder. The `.js` file extension tells our code editor that we're working with React files, so that we get the best possible support.

```markdown
src/components/Header.js
```

```jsx
import React from 'react';

class Header extends React.Component {
  render() {
    return <h1>What to do?</h1>;
  }
}

export default Header;
```

To use the component, we have to update our `src/index.js` file.

```markdown
src/index.js
```

```jsx
import React from 'react';
import { render } from 'react-dom';
import Header from './components/Header';

render(<Header />, document.querySelector('#main'));
```

We import the component, so that we are able to use it in the template code that we render. Components are loaded in JSX by using the HTML tag syntax with the component name.

It is common to name all user defined React components with a capital letter, because they're easy to distinguish from regular HTML tags, that are all lowercase.

## 4. Writing HTML with JSX

### 4.1 Introduction

The template syntax of React has some quirks that we need to know, when we're working on the HTML code. An example is the attribute `class` that has to be replaced with `className` in JSX.

The most used attributes and there replacements:

| HTML Attribute | HTML Example       | JSX Example            |
| :------------- | :----------------- | :--------------------- |
| class          | class="row"        | className="row"        |
| for            | for="input-name"   | htmlFor="input-name"   |
| style          | style="color: red" | style={{color: "red"}} |
| tabindex       | tabindex="1"       | tabIndex="1"           |
| readonly       | readonly           | readOnly               |

**You should use Emmet:**

Enabling Emmet in Visual Studio Code by using the [TAB] key, can make your life a lot easier. It suggests the right usage of attribute names, while you're typing.

Add this to your settings:

```json
"emmet.triggerExpansionOnTab": true
```

### 4.2 Multi-line templates

Let's say we want to add a subtitle to our `Header` component and we update our render method. As you will see, we use parenthesis around our template now, as this is needed for all multi-line templates that we want to use.

```markdown
/src/components/Header.js
```

```jsx
import React from "react";

class Header extends React.Component {
  render() {
    return (
      <h1>What to do?</h1>
      <span className="tagline">This could be your bucket list.</span>
    );
  }
};

export default Header;
```

Your terminal or browser will immediately tell you that there's something wrong.

```bash
Syntax error: /src/components/Header.js: Adjacent JSX elements must be wrapped in an enclosing tag (6:4)

  4 |   return (
  5 |     <h1>What to do?</h1>
> 6 |     <span className="tagline">This could be your bucket list.</span>
    |     ^
  7 |   );
  8 | };
  9 |
```

This tells us, that there is a golden rule in JSX, that you can't break. **Every** render method is **always** allowed to return **one** single element only.

In project with older React versions you might find a lot of `DIV`elements that are used as extra wrapper, but there's a better way since the latest releases.

We can avoid to clutter up our markup with extra `DIV` elements by using a `React.Fragment` element. This element is a placeholder and does not render any extra elements into the DOM.

Update the code to the following and it will work.

```markdown
/src/components/Header.js
```

```jsx
import React from 'react';

class Header extends React.Component {
  render() {
    return (
      <React.Fragment>
        <h1>What to do?</h1>
        <span className="tagline">This could be your bucket list.</span>
      </React.Fragment>
    );
  }
}

export default Header;
```

As it doesn't look too nice, to write `React.Fragment` we could use [ES6 Destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) to get rid of the `React` part.

We change the import of React at the top of our file to the following.

```jsx
import React, { Fragment } from 'react';
```

Now we can use `Fragment` as element name.

```markdown
/src/components/Header.js
```

```jsx
import React, { Fragment } from 'react';

class Header extends React.Component {
  render() {
    return (
      <Fragment>
        <h1>What to do?</h1>
        <span className="tagline">This could be your bucket list.</span>
      </Fragment>
    );
  }
}

export default Header;
```

Our application should now look like this:

![Screenshot: Added tagline](./docs/images/added-tagline.png)

### 4.3 Embedded Expressions

We can embed any JavaScript expression in JSX by wrapping it in curly braces. This is also the way we use variables inside of JSX templates.

By default, React DOM will escape any values in JSX before rendering them. This helps to prevent [XSS attacks](https://developer.mozilla.org/en-US/docs/Glossary/Cross-site_scripting) and we don't have to sanitize user data in the frontend ourselves.

### 4.4 Comments

If you want to use comments in JSX, you have to use the JavaScript multi-line comment version in curly braces.

Remember that nothing can be outside of the single returned element. So no comment can be outside of our Fragment in the following example.

```jsx
import React, { Fragment } from 'react';

const Header = props => {
  return (
    <Fragment>
      {/* Our first comment */}
      <h1>What to do?</h1>
      <span className="tagline">This could be your bucket list.</span>
    </Fragment>
  );
};

export default Header;
```

## 5. Adding SCSS for styles

As we're going to use the Bootstrap framework in our demo application, it's a good idea to extend our setup with a SASS/SCSS loader. We will use the package `node-sass`.

### 5.1 Install additional package

We need some additional packages, that we install by running the following command. Bootstrap is used to create the basic layout of our application in the next steps.

```bash
npm install --save-dev node-sass bootstrap
```

### 5.2 Create the stylesheet file

With the SASS/SCSS loader in place, we can now add our SCSS file as `src/sass/style.scss` with some basic styling in it.

```markdown
src/sass/style.scss
```

```SCSS
@import "../../node_modules/bootstrap/scss/bootstrap";

body {
  margin: 1rem;
}

.header {
  margin-bottom: 1rem;
}
```

### 5.3 Import the stylesheet file

One final step is needed, before we can restart our application. We need to import the stylesheet into our `src/index.js` so that it's loaded by webpack.

```markdown
src/index.js
```

```jsx
import React from 'react';
import { render } from 'react-dom';
import Header from './components/Header';

import './sass/style.scss';

render(<Header />, document.querySelector('#main'));
```

### 5.4 Start the application

Now we can start the application again.

```bash
npm start
```

The result should now look like this:

![Screenshot: Bootstrap is there](./docs/images/added-bootstrap.png)

## 6. Create a typical app layout with components

Most React applications use a `<App>` component as their entry point for handling state, routing and more. So let's do this too and add our component for the todo list widget.

### 6.1 Create the `ToDoList` component

```markdown
src/components/ToDoList.js
```

```jsx
import React from 'react';

class ToDoList extends React.Component {
  render() {
    return (
      <div className="alert alert-info">
        This is where the ToDo items will be.
      </div>
    );
  }
}

export default ToDoList;
```

### 6.2 Create the `App` component

Now, as we have the components that we want to load on the page, let's create the App that includes them.

```markdown
src/components/App.js
```

```jsx
import React from 'react';
import Header from './Header';
import ToDoList from './ToDoList';

class App extends React.Component {
  render() {
    return (
      <div className="container">
        <Header />
        <ToDoList />
      </div>
    );
  }
}

export default App;
```

Next we need to use the new component in the `src/index.js` file.

```markdown
src/index.js
```

```jsx
import React from 'react';
import { render } from 'react-dom';
import App from './components/App';

import './sass/style.scss';

render(<App />, document.querySelector('#main'));
```

### 6.3 Update the `Header` component

We need a small update to the `Header` component, so that the styles are all applied and there's a distance to the new `ToDoList` component. We will replace the `<Fragment>` with a `<header>` element.

```markdown
src/components/Header.js
```

```jsx
import React from 'react';

class Header extends React.Component {
  render() {
    return (
      <header className="header">
        <h1>What to do?</h1>
        <span className="tagline">This could be your bucket list.</span>
      </header>
    );
  }
}

export default Header;
```

The result of our new app layout and the changes that we made should result in this:

![Screenshot: Added the app layout](./docs/images/added-app-layout.png)

## 7. Passing data with Props

When we are using components in React, the framework passes JSX attributes to this component as a single object called `props`.

### 7.1 Update the `App` component

Let's look at our `App` component and add a prop to the `Header` component, that we are using inside. The tagline below our `h1` element will be dynamic, when we are done.

```markdown
src/components/App.js
```

```jsx
import React from 'react';
import Header from './Header';
import ToDoList from './ToDoList';

class App extends React.Component {
  render() {
    return (
      <div className="container">
        <Header tagline="This could be your shopping list." />
        <ToDoList />
      </div>
    );
  }
}

export default App;
```

We are passing the `tagline` prop to the `Header` component. Great. So let us now use this prop inside of the component.

### 7.2 Update the `Header` component

All class components have lifecycle methods defined by React and can also contain user-defined methods. For things coming from React, the `this` keyword will always refer to a single instance of the component. For your own methods you have to care about the scope yourself and we will demonstrate this later.

The props of an instance are stored in the `this.props` property.

Whether you declare a component as a function or a class, it must never modify its own props.

Such functions are called “pure” because they do not attempt to change their inputs, and always return the same result for the same inputs.

```markdown
src/components/Header.js
```

```jsx
import React from 'react';

class Header extends React.Component {
  render() {
    return (
      <header className="header">
        <h1>What to do?</h1>
        <span className="tagline">{this.props.tagline}</span>
      </header>
    );
  }
}

export default Header;
```

The tagline of our application should now be changed from "This could be your bucket list." to "This could be your shopping list.". We can use the same component with different taglines now, so props are a good way to create dynamic components.

![Screenshot: Added a prop](./docs/images/added-a-prop.png)

## 8. Stateless functional components

As our header does not need any "special" behavior or methods, we can use the **stateless functional** way to write a component. This will not use the `React.component` class, load faster and use less memory.

Let's rewrite our `Header` component and check how that works. The result is an exported function that returns the JSX of our component.

Be aware that you need to use `props` instead of `this.props` as props are passed as a parameter of the function now.

```markdown
src/components/Header.js
```

```jsx
import React from 'react';

const Header = props => {
  return (
    <header className="header">
      <h1>What to do?</h1>
      <span className="tagline">{props.tagline}</span>
    </header>
  );
};

export default Header;
```

## 9. Routing with React Router

Every application should have some routes for showing a 404 page and more. In our case we will add a simple FAQ page as with the route `/help`.

### 9.1 Create `Help` component

Our `Help` component will be a template for a manual that helps users to use the application.

```markdown
src/components/Help.js`
```

```jsx
import React from 'react';
import Header from './Header';

class Help extends React.Component {
  componentDidMount() {
    document.title = 'Help | What to do?';
  }

  render() {
    return (
      <div className="container">
        <Header tagline="Your questions will be answered here." />
        <dl>
          <dt>Where is the data stored?</dt>
          <dd>We store all information in your browser's LocalStorage.</dd>
        </dl>
      </div>
    );
  }
}

export default Help;
```

Did you notice the new `componentDidMount()` method? This is one of a react component's [Lifecycle Methods](https://reactjs.org/docs/react-component.html#the-component-lifecycle). The `componentDidMount()` method is invoked immediately after a component is mounted. All initialization that requires DOM nodes should go here and it's also a good place to set our page title (defined by the &lt;title&gt; element).

### 9.2 Create `NotFound` component

For all routes that don't exist we will show a simple 404 page, telling the user that we can't show him the desired result.

```markdown
src/components/NotFound.js
```

```jsx
import React from 'react';
import Header from './Header';

class NotFound extends React.Component {
  componentDidMount() {
    document.title = 'Error 404 | What to do?';
  }

  render() {
    return (
      <div className="container">
        <Header tagline="404 -  Page not found!" />
        <div className="alert alert-warning">
          <strong>
            Oops .... sorry!
            <br />
          </strong>
          The requested page could not be found.
        </div>
      </div>
    );
  }
}

export default NotFound;
```

### 9.3 Create `Router` component

Now let's create the `Router` component that will load different parts of the application based on the URL that is used.

```markdown
src/component/Router.js
```

```jsx
import React from 'react';
import { BrowserRouter, Route, Switch } from 'react-router-dom';
import App from './App';
import Help from './Help';
import NotFound from './NotFound';

const Router = () => (
  <BrowserRouter>
    <Switch>
      <Route exact path="/" component={App} />
      <Route path="/help" component={Help} />
      <Route component={NotFound} />
    </Switch>
  </BrowserRouter>
);

export default Router;
```

We imported 3 new components from the `react-router-dom` package, that we need to built the simple routing for our application.

| Component       | Description                                                                                                                         |
| :-------------- | :---------------------------------------------------------------------------------------------------------------------------------- |
| `BrowserRouter` | A router that uses the HTML5 history API (`pushState`, `replaceState` and the `popState` event) to keep the UI in sync with the URL |
| `Switch`        | Renders the first matching route exclusively.                                                                                       |
| `Route`         | Renders some UI when a location matches the route's path.                                                                           |

The first route is used for our homepage, showing the main application (by using the `App` component).

The second route uses the `Help` component.

The third route shows the 404 page for all URL that didn't match any other route before.

### 9.4 Load the Router

Now that we've got created the needed `Route` component, let's load it in our `src/index.js` file and replace the `App` component there.

```markdown
src/index.js
```

```jsx
import React from 'react';
import { render } from 'react-dom';
import Router from './components/Router';

import './sass/style.scss';

render(<Router />, document.querySelector('#main'));
```

With our new routing setup, we should be able see the new pages, in our application, by changing the URL. Look at the 2 examples below and check your own results.

#### Help Page

![Screenshot: Help Page](./docs/images/added-help-page.png)

#### 404 Page

![Screenshot: 404 Page](./docs/images/added-404-page.png)

## 10. Helper Functions

There are functions from time to time that we would like to use in the full application and we will create a `src/helpers.js` file for this purpose.

Let's start with a simple function to return a random tagline for our header.

```markdown
src/helpers.js
```

```jsx
export function randomArrayItem(data) {
  return data[Math.floor(Math.random() * data.length)];
}

export function getRandomTagline() {
  const taglines = [
    'This could be your bucket list.',
    'This could be your shopping list.',
    'This could be your project milestone list.',
  ];

  return randomArrayItem(taglines);
}
```

We can use the new function to change our tagline on every page load. Therefore we need to import the `getRandomTagline()` function from our helper file.

As we learned how to create stateless functional components before, let's also implement this for the `App` component to optimize performance.

```markdown
src/components/App.js
```

```jsx
import React from 'react';
import Header from './Header';
import ToDoList from './ToDoList';
import { getRandomTagline } from '../helpers.js';

class App extends React.Component {
  render() {
    return (
      <div className="container">
        <Header tagline={getRandomTagline()} />
        <ToDoList />
      </div>
    );
  }
}

export default App;
```

As a result you should see a different tagline every time you reload the application in your browser.

Now that we know it works, let's remove this funny feature which could become a bit annoying, when you use the todo list every day. 😉

```markdown
src/components/App.js
```

```jsx
import React from 'react';
import Header from './Header';
import ToDoList from './ToDoList';

class App extends React.Component {
  render() {
    return (
      <div className="container">
        <Header tagline="Here are all the next tasks." />
        <ToDoList />
      </div>
    );
  }
}

export default App;
```

## 11. Events and Binding

Events in React are very similar to the Vanilla JS or jQuery events. The biggest difference is that React wraps events in something called a Synthetic Event that ensures it works on all browsers and all devices, so that the developer doesn’t have to care about that.

All React events are camelCase, rather than lowercase.

[Event Documentation on reactjs.org](https://reactjs.org/docs/events.html)

### 11.1 Create a `ToDoForm` component

Let's create a `ToDoForm` component that shows and handles the form we need to add new todo items to our list. The form will check if the input is empty and add the new item otherwise.

```markdown
src/components/ToDoForm.js
```

```jsx
import React from 'react';

class ToDoForm extends React.Component {
  render() {
    return (
      <form className="input-group my-3">
        <input
          className="form-control"
          type="text"
          placeholder="Add a new to-do item ..."
        />
        <div className="input-group-append">
          <button className="btn btn-outline-secondary" type="submit">
            <i className="fas fa-plus" aria-hidden="true" />
            &nbsp;Add item
          </button>
        </div>
      </form>
    );
  }
}

export default ToDoForm;
```

Next we'll add the form to our `App` component.

```markdown
src/components/App.js
```

```jsx
import React from 'react';
import Header from './Header';
import ToDoForm from './ToDoForm';
import ToDoList from './ToDoList';

class App extends React.Component {
  render() {
    return (
      <div className="container">
        <Header tagline="Here are all the next tasks." />
        <ToDoForm />
        <ToDoList />
      </div>
    );
  }
}

export default App;
```

The result should look like this:

![Screenshot: Added a form](./docs/images/added-a-form.png)

### 11.2 Add an event handler

Now, that we've got the form in our application, we will ad an event handler to react on its submit. Remember that a form submit can be fired in 2 different ways. First by clicking on a submit button (in our case "Add item") and second by pressing [Enter] in any input field (in our case the item text).

```markdown
src/components/ToDoForm.js
```

```jsx
import React from 'react';

class ToDoForm extends React.Component {
  handleSubmit(e) {
    e.preventDefault();
    console.log(`Create new item`);
  }

  render() {
    return (
      <form className="input-group my-3" onSubmit={this.handleSubmit}>
        <input
          className="form-control"
          name="name"
          type="text"
          placeholder="Add a new to-do item ..."
        />
        <div className="input-group-append">
          <button className="btn btn-outline-secondary" type="submit">
            <i className="fas fa-plus" aria-hidden="true" />
            &nbsp;Add item
          </button>
        </div>
      </form>
    );
  }
}

export default ToDoForm;
```

What did we change?

1. We added a method `handleSubmit` to our class, that retrieves the event we catch as the first argument. When this method is called, we prevent the submit from doing the default (sending the form and reloading the page) and log a simple message to the console.
1. We added the `onSubmit` event to the `<form>` element and told React to call our `handleSubmit` as a handler, when it is fired.

Give it a try! Click on the "Add item" button and check your e developer console. You should see the message "Create new item".

### 11.3 Add a reference

If we want to get the value of input elements when submitting a form, we need to create a reference first. Each input gets its own reference, which is only one in our case, as there's only the text input.

References are created as properties of our class, by using the `React.createRef()` method.

An important thing to note is that we need to change the binding of `this` for our event handler, so that we can access the instance of our component. Therefore we will extend the constructor of our component class.

Let's add the reference in our `ToDoForm` component.

```markdown
src/components/ToDoForm.js
```

```jsx
import React from 'react';

class ToDoForm extends React.Component {
  constructor(props) {
    super(props);

    this.handleSubmit = this.handleSubmit.bind(this);
  }

  textInput = React.createRef();

  handleSubmit(e) {
    e.preventDefault();
    console.log(`Create new item: ${this.textInput.current.value}`);
  }

  render() {
    return (
      <form className="input-group my-3" onSubmit={this.handleSubmit}>
        <input
          className="form-control"
          name="name"
          type="text"
          placeholder="Add a new to-do item ..."
          ref={this.textInput}
        />
        <div className="input-group-append">
          <button className="btn btn-outline-secondary" type="submit">
            <i className="fas fa-plus" aria-hidden="true" />
            &nbsp;Add item
          </button>
        </div>
      </form>
    );
  }
}

export default ToDoForm;
```

What did we change?

1. We extended the constructor to set the binding of `this` for our event handler.
1. We created the reference `textInput` with `React.createRef()`.
1. We added the reference to the input field, by using the `ref` attribute.
1. We added the value of the input reference to our console message in the event handler.

When you open the Chrome dev console, insert a text in the input field and press the button, you should see the following:

![Screenshot: Added reference and binding](./docs/images/added-a-reference.png)

Great! But let's improve our code style a bit and use an alternative to the old school binding that we have right now. As we're using the `react scripts` module, which has babel inside, we are able to use an easier ES6 syntax without modifying the constructor.

```markdown
src/components/ToDoForm.js
```

```jsx
handleSubmit = e => {
  e.preventDefault();
  console.log(`Create new item: ${this.textInput.current.value}`);
};
```

## 12. Understanding the state

### 12.1 What is it?

The state is an object storing data that is required by a component or its children.

Neither parent nor child components can know if a certain component is stateful or stateless, and they shouldn’t care whether it is defined as a function or a class.

This is why state is often called local or encapsulated. It is not accessible to any component other than the one that owns and sets it.

A component may choose to pass its state down as props to its child components, which can be especially useful if multiple components need to have a shared state with the same data.

This is commonly called a “top-down” or “unidirectional” data flow. Any state is always owned by some specific component, and any data or UI derived from that state can only affect components “below” them in the tree.

The state is a single source of truth, so we don’t manually update multiple places using the same data, but use the shared state of a parent component instead.

We will use our `App` component to hold the shared state for the `ToDoForm` and the `ToDoList` components, that will both modify the data.

### 12.2 Adding state to the `App`

As described above, we need to have a state for our `App` component, that is shared with its child components, that can update the data.

Let's create the state with our list of todo items. We use an object for the todo items, because the unique identifier of an item will be used as the property key. This way it's very easy to target one specific item that we want to change later.

The official way to set the initial state of a component is to use a `constructor` method, that gets the props as argument and calls the parent constructor by using `super`.

```markdown
src/components/App.js
```

```jsx
import React from 'react';
import Header from './Header';
import ToDoForm from './ToDoForm';
import ToDoList from './ToDoList';

class App extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      toDoItems: {},
    };
  }

  render() {
    return (
      <div className="container">
        <Header tagline="Here are all the next tasks." />
        <ToDoForm />
        <ToDoList />
      </div>
    );
  }
}

export default App;
```

### 12.3 Updating state with mutations

In React and most other JS frontend frameworks, you should never manipulate the state directly. Handle it as if it would be an immutable object, that you never touch.

Instead, we are using mutations, that do the job and are added as functions to our class component. As the state lives in our `App` component, so will the mutations to work with it.

Let's add a mutation to add a new item to the todo list and pass this mutation to the `ToDoForm` component, that will use it when the form is submitted.

As we will use unique identifiers (UUID) for our todo items, let's add a new npm package first, that does the magic for us.

```bash
npm install --save-dev uuid
```

Now we can use that in our component.

```markdown
src/components/App.js
```

```jsx
import React from 'react';
import Header from './Header';
import ToDoForm from './ToDoForm';
import ToDoList from './ToDoList';
import uuid from 'uuid/v4';

class App extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      toDoItems: {},
    };
  }

  addToDo = text => {
    const todo = {
      uuid: uuid(),
      text: text,
      done: false,
    };

    this.setState(state => {
      state.toDoItems[todo.uuid] = todo;
      return state;
    });
  };

  render() {
    return (
      <div className="container">
        <Header tagline="Here are all the next tasks." />
        <ToDoForm addToDo={this.addToDo} />
        <ToDoList />
      </div>
    );
  }
}

export default App;
```

What are we doing there?

1. We import the uuid module.
1. We add the mutation addToDo.
1. The mutation creates a new todo object and uses `setState()` to add the item to the state.
1. We pass the mutation down to the `ToDoForm` component.
1. React will check which parts of the application need to be re-rendered because of the state change. As we don't have the list of items yet, this is nothing.

The next thing we have to do is to update our `ToDoForm` component, so that it uses the mutation from its parent. Let's upgrade the `handleSubmit()` function.

```markdown
src/components/ToDoForm.js
```

```jsx
handleSubmit = e => {
  e.preventDefault();
  const text = this.textInput.current.value.trim();
  this.props.addToDo(text);
  e.currentTarget.reset();
};
```

Now the `handleSubmit()` function will pass the input to the `addToDo()` method of our `App` component class, because we passed that to our `ToDoForm` component as prop with the name `addToDo`. It's good practice to name the prop the same as the function, because it's easier to figure out the relation this way.

We can check if the mutation works by using the React Dev Tools in Chrome. Open the Chrome dev console and switch to the `React` tab. Inside of the `React` tab use the search field to look for the `App` component, where our state for the todo items lives.

You will see an empty `toDoItems` property of the state when the page is initially loaded. Now add a new item, using the form, and check if the state got updated.

If everything works it should look like this, after you added a new item:

![Screenshot: Using the first mutation](./docs/images/added-a-mutation.png)

## 13. Displaying state with JSX

Now that we will store some data in our state, let's display the list of items on the screen.

We will create a `ToDoItem` component, that we will use in our already existing `ToDoList` component then. But let's pass the data from our `App` component to the `ToDoList` component first.

```markdown
src/components/App.js
```

```jsx
  render() {
    return (
      <div className="container">
        <Header tagline="Here are all the next tasks." />
        <ToDoForm addToDo={this.addToDo} />
        <ToDoList items={this.state.toDoItems} />
      </div>
    );
  }
```

Now we create the `ToDoItem` component that is very basic for now and will be extended in the next few steps. Let's keep it simple to see if the data is there.

```markdown
src/components/ToDoItem.js
```

```jsx
import React from 'react';

class ToDoItem extends React.Component {
  render() {
    return (
      <div className="todo-item">
        {this.props.data.uuid} | {this.props.data.text}
      </div>
    );
  }
}

export default ToDoItem;
```

To loop over our object of items we will use a combination of Object.keys() and Array.map().

With Object.keys() we get all the property names of our object, which are equal and with Array.map() we create a new array of JSX templates that get rendered into our `ToDoItem` component.

```markdown
src/components/ToDoList.js
```

```jsx
import React from 'react';
import ToDoItem from './ToDoItem';

class ToDoList extends React.Component {
  render() {
    return (
      <div className="todo-list">
        <ul className="todo-items">
          {Object.keys(this.props.items).map(uuid => (
            <ToDoItem key={`todo-item-${uuid}`} data={this.props.items[uuid]} />
          ))}
        </ul>
      </div>
    );
  }
}

export default ToDoList;
```

Great! So we should see all items that we add to our list below the form. Try to add a few entries, when the page is loaded with your latest changes.

The result should look like this:

![Screenshot: Using the first mutation](./docs/images/using-a-first-mutation.png)

## 14. Updating and deleting items

Cool! Now we can add and display items and in the next step we deal with editing and deleting. We will also add a checkbox to mark items as done.

### 14.1 Loading additional styles

Before we start to modify all of our components, we will load some more styles, to enhance our application layout. We want to display the items in a nice list with features to modify and delete them.

As a first step we install [Font Awesome](https://fontawesome.com/) using npm.

```bash
npm install --save-dev @fortawesome/fontawesome-free
```

To load Font Awesome, we need to add it to the `index.js` file.

```markdown
src/index.js
```

```jsx
import React from 'react';
import { render } from 'react-dom';
import Router from './components/Router';

import '@fortawesome/fontawesome-free/css/all.css';
import './sass/style.scss';

render(<Router />, document.querySelector('#main'));
```

Now we will extend the stylesheets for our todo list.

```markdown
src/sass/style.scss
```

```SCSS
@import "../../node_modules/bootstrap/scss/bootstrap";

body {
  margin: 1rem;
}

.header {
  margin-bottom: 1rem;
}

.table-borderless > tbody > tr > td,
.table-borderless > tbody > tr > th,
.table-borderless > tfoot > tr > td,
.table-borderless > tfoot > tr > th,
.table-borderless > thead > tr > td,
.table-borderless > thead > tr > th {
  border: none;
}

.todo-items {
  margin-top: 1rem;
}

.todo-item {
  &:hover {
    background-color: #efefef;
  }

  td {
    padding: 0.25rem 0;

    &:first-child {
      padding-left: 0.5rem;
      border-top-left-radius: 0.25rem;
      border-bottom-left-radius: 0.25rem;
    }

    &:last-child {
      padding-right: 0.5rem;
      border-top-right-radius: 0.25rem;
      border-bottom-right-radius: 0.25rem;
    }
  }

  td.col-action {
    text-align: right;
    padding-right: 0.25rem;

    .icon-remove {
      color: #c0c0c0;
      cursor: pointer;
      padding: 0 0.25rem;

      &:hover {
        color: #bd2d2d;
      }
    }
  }

  input[type="text"] {
    border: 1px solid transparent;
    padding: 0 0.25rem;
    width: 98%;
    height: auto;
  }
}
```

### 14.2 Updating the `App`

We add methods to update the state and pass them as props to the `ToDoList` component.

```markdown
src/components/App.js
```

```jsx
import React from 'react';
import Header from './Header';
import ToDoForm from './ToDoForm';
import ToDoList from './ToDoList';
import uuid from 'uuid/v4';

class App extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      toDoItems: {},
    };
  }

  addToDo = text => {
    const todo = {
      uuid: uuid(),
      text: text,
      done: false,
    };

    this.setState(state => {
      state.toDoItems[todo.uuid] = todo;
      return state;
    });
  };

  updateToDoText = (uuid, text) => {
    this.setState(state => {
      state.toDoItems[uuid].text = text;
      return state;
    });
  };

  toggleToDoDone = event => {
    const checkbox = event.target;

    this.setState(state => {
      state.toDoItems[checkbox.value].done = checkbox.checked;
      return state;
    });
  };

  removeToDo = uuid => {
    this.setState(state => {
      delete state.toDoItems[uuid];
      return state;
    });
  };

  render() {
    return (
      <div className="container">
        <Header tagline="Here are all the next tasks." />
        <ToDoForm addToDo={this.addToDo} />
        <ToDoList
          items={this.state.toDoItems}
          updateToDoText={this.updateToDoText}
          toggleToDoDone={this.toggleToDoDone}
          removeToDo={this.removeToDo}
        />
      </div>
    );
  }
}

export default App;
```

As you can see we added 3 new methods.

| Method         | Description                                                                                 |
| :------------- | :------------------------------------------------------------------------------------------ |
| updateToDoText | Updates the text of a todo item and will be used for the input field of each item.          |
| toggleToDoDone | (Un)checks a todo item and will be used on click on the checkbox in front of each item.     |
| removeToDo     | Removes a todo item and will be used on click on the remove icon, we will add to each item. |

### 14.3 Updating the `ToDoList`

We take the mutations and pass them to the `ToDoItem`, where they will be used for different events. We will also change from a `ul` to a `table`, to achieve the layout we want to have.

```markdown
src/components/ToDoList.js
```

```jsx
import React from 'react';
import ToDoItem from './ToDoItem';

class ToDoList extends React.Component {
  render() {
    return (
      <div className="todo-list">
        <table className="todo-items table table-borderless">
          <tbody>
            {Object.keys(this.props.items).map(uuid => (
              <ToDoItem
                key={`todo-item-${uuid}`}
                data={this.props.items[uuid]}
                updateToDoText={this.props.updateToDoText}
                toggleToDoDone={this.props.toggleToDoDone}
                removeToDo={this.props.removeToDo}
              />
            ))}
          </tbody>
        </table>
      </div>
    );
  }
}

export default ToDoList;
```

### 14.4 Updating the `ToDoItem`

Now let's use the mutations to lift up the state changes that each item can trigger. We can change the text of an item, mark it as done or remove it from the list. Keep care to change the list item to a table row, to match the table of the `ToDoList` component.

We will also add a method to handle every `keyup` event of the text input field, to check if the `keyCode` matches the \[Enter\] key. If the \[Enter\] key is pressed, the `blur()` method of the input element is called, to remove the focus.

```markdown
src/components/ToDoItem.js
```

```jsx
import React from 'react';

class ToDoItem extends React.Component {
  handleInputKeyUp(e) {
    // Remove focus, when the [Enter] key is pressed
    if (e.keyCode === 13) {
      e.target.blur();
    }
  }

  render() {
    const todo = this.props.data;

    return (
      <tr className="todo-item" data-id={todo.uuid}>
        <td>
          <div className="custom-control custom-checkbox">
            <input
              className="custom-control-input"
              value={todo.uuid}
              id={`todo-done-${todo.uuid}`}
              type="checkbox"
              checked={todo.done}
              onChange={this.props.toggleToDoDone}
            />
            <label
              className="custom-control-label"
              htmlFor={`todo-done-${todo.uuid}`}
            >
              &nbsp;
            </label>
          </div>
        </td>
        <td className="col-1">
          <input
            type="text"
            className="form-control"
            value={todo.text}
            onChange={e => {
              this.props.updateToDoText(todo.uuid, e.target.value);
            }}
            onKeyUp={e => {
              this.handleInputKeyUp(e);
            }}
          />
        </td>
        <td className="col-action">
          <i
            className="icon-remove far fa-trash-alt"
            onClick={e => this.props.removeToDo(todo.uuid)}
          />
        </td>
      </tr>
    );
  }
}

export default ToDoItem;
```

The result should look like this:

![Screenshot: New Layout](./docs/images/new-layout.png)

## 15 Persisting state with localStorage

After adding all the mutations to our todo list, we want to store the data in the localStorage, so that we don't loose the data if we leave the page and come back later.

Keep in mind that the localStorage might not be stored forever, depending on your browser settings.

### 15.1 Adding a storage class

As we want to keep our application flexible and we might want to change it to a database or something else later, we will create a class to handle the data storage.

```markdown
src/modules/Storage.js
```

```jsx
class Storage {
  constructor() {
    if (!this.canUseLocalStorage()) {
      throw Error('The local storage is disabled or full!');
    }
  }

  set(key, value) {
    localStorage.setItem(key, value);
  }

  get(key, defaultValue = null) {
    const value = localStorage.getItem(key);

    if (value !== null) {
      return value;
    }

    return defaultValue;
  }

  canUseLocalStorage() {
    var test = 'test';

    try {
      localStorage.setItem(test, test);
      localStorage.removeItem(test);
    } catch (e) {
      return false;
    }

    return true;
  }
}

export default new Storage();
```

### 15.2 Updating the `App`

We will load our new `Storage` class and get the stored data in our constructor, before the component is rendered. As the localStorage can only store strings, we need to use the `JSON.stringify()` and `JSON.parse()` methods.

Every time that the state gets changed, we will update the data stored in localStorage, using the `componentDidUpdate()` lifecycle method.

```markdown
src/components/App.js
```

```jsx
import React from 'react';
import Header from './Header';
import ToDoForm from './ToDoForm';
import ToDoList from './ToDoList';
import uuid from 'uuid/v4';
import Storage from '../modules/Storage';

class App extends React.Component {
  constructor(props) {
    super(props);

    this.storageKey = 'react-todo';
    const old = Storage.get(this.storageKey);

    if (old) {
      this.state = JSON.parse(old);
    } else {
      this.state = {
        toDoItems: {},
      };

      Storage.set(this.storageKey, JSON.stringify(this.state));
    }
  }

  componentDidUpdate() {
    Storage.set(this.storageKey, JSON.stringify(this.state));
  }

  addToDo = text => {
    const todo = {
      uuid: uuid(),
      text: text,
      done: false,
    };

    this.setState(state => {
      state.toDoItems[todo.uuid] = todo;
      return state;
    });
  };

  updateToDoText = (uuid, text) => {
    this.setState(state => {
      state.toDoItems[uuid].text = text;
      return state;
    });
  };

  toggleToDoDone = event => {
    const checkbox = event.target;

    this.setState(state => {
      state.toDoItems[checkbox.value].done = checkbox.checked;
      return state;
    });
  };

  removeToDo = uuid => {
    this.setState(state => {
      delete state.toDoItems[uuid];
      return state;
    });
  };

  render() {
    return (
      <div className="container">
        <Header tagline="Here are all the next tasks." />
        <ToDoForm addToDo={this.addToDo} />
        <ToDoList
          items={this.state.toDoItems}
          updateToDoText={this.updateToDoText}
          toggleToDoDone={this.toggleToDoDone}
          removeToDo={this.removeToDo}
        />
      </div>
    );
  }
}

export default App;
```

That's it! Now the state of our application is always synced to localStorage, when it's changed and the user will see the same items as before, when he reloads the page or visits the same page again later.



