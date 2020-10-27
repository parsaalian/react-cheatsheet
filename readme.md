# React

## Table of content
1. [Start Project](#start-project)
2. [Project Structure](#project-structure)
3. [Explanations](#explanations)
4. [JSX](#jsx)
5. [Components](#components)
6. [Forms](#forms)
7. [Code Splitting](#code-splitting)
8. [Context](#context)

## Start Project
Creation of project is done by create-react-app:
```bash
create-react-app your-project-name
```
if you want to create your own toolchain for creating react apps, check [this](https://blog.usejournal.com/creating-a-react-app-from-scratch-f3c693b84658) guide out.

You can install and learn to use this cli from [this link](https://github.com/facebook/create-react-app).

## Project Structure
The initial project structure which is created by `create-react-app` is like this (after installing dependencies with `npm i`):
```
your-project-name
├── public
|    ├── favicon.ico
|    ├── index.html
|    └── manifest.json
├── src
|   ├── App.js
|   ├── App.css
|   ├── App.test.js
|   ├── index.js
|   ├── index.css
|   └── serviceWorker.js
├── .env
├── .gitignore
├── package.json
└── package.lock.json
```
Create the files that are not created by default. You should do some changes in src folder structure to make your application better for scaling. make src look like this:
```
src
├── components
├── assets
|   ├── images
|   ├── audios
|   └── videos
├── lib
|   ├── api
|   ├── utils
|   └── validation
├── views
├── App.js
├── App.css
├── App.test.js
├── index.js
├── index.css
└── serviceWorker.js
```
I start explaining each folder.
1. components:    All global components that are used more than once in the pages are placed inside this directory. Each component should have its own folder and in each folder, lies a `.js` and a `.css` file.
2. assets:    All of the static assets should be placed in this directory. Each type of asset in different folder. api and validation folder will be explained later more in-depth.
3. lib:    All functions that are not written in the component, should be placed inside this directory for separation of concerns.
    - api:    Requests from the server are inside in this directory.
    - utils:    All the helper functions should be placed inside this directory.
    - validation:    Functions for validating users and ... are in this folder.
4. views:    each page of the website should be putted inside a separate folder inside this directory. Every component that is used just inside that page, should be placed inside that page's directory to (for locality of reference).

## Explanations
### index.html
There isn't much things that should be changed in this file. You should only change the title of the website and then change the responsivity meta tag to this:
```html
<meta id="viewport" name="viewport" content="width=device-width, initial-scale=1, minimum-scale=1, maximum-scale=1, viewport-fit=cover">
```
also if you are using a ui framework, the link to the stylesheet of that framework should be added to the html to (in case it is not included inside index.js).

### favicon.ico
To make your favicon.ico icon for the top of the site, you can visit [here](http://icoconvert.com/).

### App.js
Router will be placed in this file. It will be explained later.

### index.js
This is the place where app is rendered to the htmlDOM. There isn't much things that you should change in this directory. Some libraries that you want to use in your project globally should be included here.

### .env
This place is for configurations of the project. For example if you want your addresses to be started from `src` directory ( believe me you want:)) ) you can use line below:
```
NODE_PATH=src/
```
or if you want to use preprocessors like `sass`, you should first install it:
```bash
npm i --save node-sass
```
and then add this line to `.env` folder:
```
REACT_APP_CSS_MODULES=true
```

### .gitignore
this template for gitignore should do well for you:
```
# dependencies
/node_modules

# testing
/coverage

# production
/build

# misc
.DS_Store
.env.local
.env.development.local
.env.test.local
.env.production.local

npm-debug.log*
yarn-debug.log*
yarn-error.log*
```

## JSX
The react pages [jsx docs](https://reactjs.org/docs/introducing-jsx.html) is a good start for learning this syntax. Also after reading the basic `jsx` docs, [this](https://reactjs.org/docs/jsx-in-depth.html) will help you master this syntax.

## Components
This section contains multiple parts:
1. [The template of a component](#the-template-of-a-component)
2. [Props in components](#props-in-components)
3. [States in components](#states-in-components)
4. [Functions in components](#functions-in-components)
5. [Lifecycle functions](#lifecycle-functions)
6. [Conditional rendering](#conditional-rendering)
7. [Updating components from children or siblings](#updating-components-from-children-or-siblings)
8. [Lists and keys](#lists-and-keys)

### The template of a component
Your example component should look like this at creation:
```javascript
import React, { Component } from 'react';
import './example.scss';

export default Example extends Component {
    render() {
        return(

        );
    }
}
```
then you can add and edit things inside this template.
### Props in components
You can always refer to component properties by calling `this.props.propName`. There are some other things that are better to be careful of them. First of them is propTypes. You can define the type of property which is inputed from upper component. First you should import `prop-types`:
```javascript
import PropTypes from 'prop-types';
```
and then to specify the type:
```javascript
Example.propTypes = {
  // You can declare that a prop is a specific JS type. By default, these
  // are all optional.
  optionalArray: PropTypes.array,
  optionalBool: PropTypes.bool,
  optionalFunc: PropTypes.func,
  optionalNumber: PropTypes.number,
  optionalObject: PropTypes.object,
  optionalString: PropTypes.string,
  optionalSymbol: PropTypes.symbol,

  // Anything that can be rendered: numbers, strings, elements or an array
  // (or fragment) containing these types.
  optionalNode: PropTypes.node,

  // A React element.
  optionalElement: PropTypes.element,

  // You can also declare that a prop is an instance of a class. This uses
  // JS's instanceof operator.
  optionalMessage: PropTypes.instanceOf(Message),

  // You can ensure that your prop is limited to specific values by treating
  // it as an enum.
  optionalEnum: PropTypes.oneOf(['News', 'Photos']),

  // An object that could be one of many types
  optionalUnion: PropTypes.oneOfType([
    PropTypes.string,
    PropTypes.number,
    PropTypes.instanceOf(Message)
  ]),

  // An array of a certain type
  optionalArrayOf: PropTypes.arrayOf(PropTypes.number),

  // An object with property values of a certain type
  optionalObjectOf: PropTypes.objectOf(PropTypes.number),

  // An object taking on a particular shape
  optionalObjectWithShape: PropTypes.shape({
    color: PropTypes.string,
    fontSize: PropTypes.number
  }),

  // You can chain any of the above with `isRequired` to make sure a warning
  // is shown if the prop isn't provided.
  requiredFunc: PropTypes.func.isRequired,

  // A value of any data type
  requiredAny: PropTypes.any.isRequired,

  // You can also specify a custom validator. It should return an Error
  // object if the validation fails. Don't `console.warn` or throw, as this
  // won't work inside `oneOfType`.
  customProp: function(props, propName, componentName) {
    if (!/matchme/.test(props[propName])) {
      return new Error(
        'Invalid prop `' + propName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  },

  // You can also supply a custom validator to `arrayOf` and `objectOf`.
  // It should return an Error object if the validation fails. The validator
  // will be called for each key in the array or object. The first two
  // arguments of the validator are the array or object itself, and the
  // current item's key.
  customArrayProp: PropTypes.arrayOf(function(propValue, key, componentName, location, propFullName) {
    if (!/matchme/.test(propValue[key])) {
      return new Error(
        'Invalid prop `' + propFullName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  })
};
```
You also can create default values for properties so if they are not specified, you wouldn't be in any trouble in two ways:
```javascript
// after the property decleration
Example.defaultProps = {
    propName: propValue
};
export defaule Example;
```
or:
```javascript
// inside the component
static defaultProps = {
    propName: propValue
}
```
### States in components
States should be defined in the constructor (in my convention):
```javascript
constructor(props) {
  super(props);
  this.state = {
    stateName: stateInitialValue
  }
}
```
states are the props that change. changing states should happen like this:
```javascript
this.setState({
  stateName: newStateValue
})
```

### Functions in components
Functions can be called in two different ways: 1. with arguments passed and with parentheses - 2. without any arguments passed and without parentheses. If parentheses and arguments are included, there is no need for binding. If no parentheses are included, the class behave to function like its properties. So it should be binded as a property to the class. Binding can happen inside the constructor like this:
```javascript
this.functionName = this.functionName.bind(this);
```
when functions are binded and called as mentioned, there is an event that is passing to the function.

### Lifecycle functions
There are three stages for each component.
1. mounting
2. updating
3. unmounting
You can find a good explanation of these methods on [react website](https://reactjs.org/docs/react-component.html). But this diagram will help you understand these functions better:
![diagram](https://cdn-images-1.medium.com/max/1600/0*OoDfQ7pzAqg6yETH.)

### Conditional rendering
There are varies of ways that can be used to render a component or a part of the component conditionally:
```javascript
render() {
  [...]
  if (condition) {
    return_this_component
  } else {
    return_that_component
  }
}
```
```javascript
render() {
  return(
    [...]
    {condition && component_to_render_on_condition}
    [...]
  )
}
```
```javascript
render() {
  return(
    [...]
    {condition ? (
      component_to_render_on_contition
    ) : (
      component_to_render_on_not_contition
    )}
    [...]
  )
}
```

### Updating components from children of siblings
For updating components from children, you should pass a function and an state to the child. The function has the semantics for changing the state. Whenever the child decides to change the parent's state, it calls the parent's function.

Updating components from siblings is pretty much the same. You should pass the state to two children and a function for changing the state to one of them. Whenever the child decides to change the state, it calls the function and the sibling is updated to.

### Lists and keys
You may want to render components from a list. You can use the `map` function for this use:
```javascript
function convert_list_to_component_set(list) {
  return list.map((listItem) => {
    return <ComponentToConvertToFromListItem />
  })
}
```
but this example is wrong. Each component in the returning list should have a unique key. If `listItem` contains a key:
```javascript
function convert_list_to_component_set(list) {
  return list.map((listItem) => {
    return <ComponentToConvertToFromListItem key={listItem.key} />
  })
}
```
else you can use counter in the map function to use them as key:
```javascript
function convert_list_to_component_set(list) {
  return list.map((listItem, i) => {
    return <ComponentToConvertToFromListItem key={i} />
  })
}
```

## Forms
There are two type of forms in react. Controlled and uncontrolled. Controlled form inputs are the inputs that react know the value of them every moment. Uncontrolled components are ordinary inputs like `html` inputs and so on. For the controlled components there are two approaches:
1. To define all inputs inside one react component. In this case, it is easier to define the name of the inputs same as valueState of them. like this:
```javascript
class Reservation extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      isGoing: true,
      numberOfGuests: 2
    };

    this.handleInputChange = this.handleInputChange.bind(this);
  }

  handleInputChange(event) {
    const target = event.target;
    const value = target.type === 'checkbox' ? target.checked : target.value;
    const name = target.name;

    this.setState({
      [name]: value
    });
  }

  render() {
    return (
      <form>
        <label>
          Is going:
          <input
            name="isGoing"
            type="checkbox"
            checked={this.state.isGoing}
            onChange={this.handleInputChange} />
        </label>
        <br />
        <label>
          Number of guests:
          <input
            name="numberOfGuests"
            type="number"
            value={this.state.numberOfGuests}
            onChange={this.handleInputChange} />
        </label>
      </form>
    );
  }
}
```
2. You can define a separate component for each input and use it in your form:
```javascript
class ControlledInput extends Component {
  constructor(props) {
    super(props);
    this.state = { value: '' };
    this.handleChange = this.handleChange.bind(this);
  }

  handleChange(event) {
    event.preventDefault();
    this.setState({
      value: event.target.value
    })
  }

  render() {
    return(
      <input name='blah' type='text' value={this.state.value} onChange={this.handleChange} />
    )
  }
}
```
`preventDefault()` will stop your function from running when component is mounted and before any action is dispatched. To choose between controlled and uncontrolled components, you can read [this](https://goshakkk.name/controlled-vs-uncontrolled-inputs-react/) article.

## Code Splitting
The webpack bundle's all of the codes into one single `js` module. So if the page grows, loading a single page may take longer than necessary. You should use code splitting to prevent loading the entire page data everytime user takes an action and load only the necessary. I think the best solution to this problem are these ways:
1. Using `React.lazy` and `Suspense`:
```javascript
const OtherComponent = React.lazy(() => import('./OtherComponent'));

function MyComponent() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <OtherComponent />
      </Suspense>
    </div>
  );
}
```
this code will first render `MyComponent` to the page and wait until the data from `OtherComponent` are ready to render. Until then, it shows the `fallback` instead of `OtherComponent`. If any errors occured, we can use `ErrorBoundaries` to wrap the `Suspense` module and show proper message to the user. Error boundaries will be introduces later.
2. Also code splitting might be route based like this:
```javascript
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import React, { Suspense, lazy } from 'react';

const Home = lazy(() => import('./routes/Home'));
const About = lazy(() => import('./routes/About'));

const App = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Switch>
        <Route exact path="/" component={Home}/>
        <Route path="/about" component={About}/>
      </Switch>
    </Suspense>
  </Router>
);
```
splitting only works on `default` exports.

The second solution to this problem is the `react-loadable` library. It can be installed with the command below:
```bash
npm i --save react-loadable
```
then for each item that you want to be loaded to the page you can use:
```javascript
import Loadable from 'react-loadable';
import Loading from './my-loading-component';

const LoadableComponent = Loadable({
  loader: () => import('./my-component'),
  loading: Loading,
});

export default class App extends React.Component {
  render() {
    return <LoadableComponent/>;
  }
}
```
the `<LoadabelComponent/>` automatically split the code and lazy loads the `my-component`. For customizing your loadable module for errors or timeouts, I suggest you read the code below:
```javascript
function Loading(props) {
  if (props.error) {
    return <div>Error! <button onClick={ props.retry }>Retry</button></div>;
  } else if (props.pastDelay) {
    return <div>Loading...</div>;
  } else {
    return null;
  }
}

Loadable({
  loader: () => import('./component'),
  loading: Loading,
  delay: 300, // 0.3 seconds
});
```
it will use the `Loading` function while loading, if importing component faces an error, it will pass an error prop to the `Loading` (same when it pass the delay time). For more details you can read [react-loadable](https://github.com/jamiebuilds/react-loadable).

## Context
Context is a method for passing global data without need to pass it as a prop. It means that context is available for every node in the tree of components, no matter how deep it is. It may be used for the user authentication information, color theme, language or ... in your website.