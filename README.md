## Introduction

## Table of Contents
1. Lifecycle Event
2. Introduction of React Router
3. The BrowserRouter Component
4. The Link Component
5. The Route Component
6. Additional useful component
  - form-serialize
## Content
### 1. Lifecycle Event
Life cycle events are special methods each component can have that allow us to hook into the views when specific conditions happened.
There are four main life cycle event should remember:
  - <strong>componentWillMount</strong>
  invoked immediately before the component is inserted in the DOM
  - <strong>componentDidMount</strong>
  invoked immediately after the component is inserted in the DOM
  - <strong>componentWillUnmount</strong>
  invoked immediately before a component is removed from the DOM
  - <strong>componentWillReceiveProps</strong>
  invoked whenever the component is about to receive brand new props

Just realize they existed and look documentation if needed.

If we want to fetch external data from an API - use componentDidMount lifecyele event.

Given an example:
```jsx
ComponentDidMount() {
  fetchUser().then(user => this.setState({
    name: user.name,
    age: user.age
  }))
}
render() {
  return (
    <div>
      <p>Name: {this.state.name}</p>
      <p>age: {this.state.age}</p>
    </div>
  )
}
/*
1. Initially, the render method called with empty this.state.name and this.state.age
2. Once the component has been mounted, comes to ComponentDidMount - fetchUser request from the UserAPI to database and when data comes from database, setState called and update name and age.
3. state has changed, render() called again.
*/
```
<strong>Recap</strong>
#### Adding to the DOM
These lifecycle events are called when a component is being added to the DOM:
- constructor()
- componentWillMount()
- render()
- componentDidMount()

#### Re-rendering
These lifecycle events are called when a component is re-rendered to the DOM
- componentWillReceiveProps()
- shouldComponentUpdate()
- componentWillUpdate()
- render()
- componentDidUpdate()

### 2. Introduction of React Router
React Router allow us to build a single page app. React router is a collection of <strong>navigational components</strong> that compose declarative with your application
- small trick of short-circuit evaluation syntax
```jsx
/* If screen equals to list it will show list screen otherwise show create screen */
{this.state.screen === 'list' && (
  <ListContacts
    contacts={this.state.contacts}
    onDeleteContact={this.removeContact}
  />
)}
{this.state.screen === 'create' && (
  <CreateContact />
)}
```
- small trick on pass parameters between differenct component
Given an example:
```jsx
/*If we want to pass state - screen from ListContacts to appjs
first we create a function to setState in appjs like that
*/
onNavigate={() => {
  this.setState({ screen: 'create' })
}}
/* then onNavigate becomes a props then we can pass this props to another component. For example, ListContacts.js like this*/
<a
  href='#create'
  onClick={this.props.onNavigate}
  className="add-contact"
>Add Contact</a>
/* Then whenever a user click it, the state will update then list screen will switch to create contact */
```
### 3. The BrowserRouter Component
However, in previous example, when user have entered createContact screen but want to go back to list contacts screen, the browser wont response correctly. So in this case, we have to use browser router component from react-router-dom.
1. First to import react-router, nomarlly it will be imported in index.js
```jsx
import { BrowerRouter } from 'react-router-dom'
/* Wrap the <App> component with BrowserRouter */
ReactDOM.render(
  <BrowserRouter><App /><BrowserRouter>,
  document.getElementById('root')
)
```
2. Explore what happened inside Router
```jsx
class BrowserRouter extends React.Component {
  static propTypes = {
    basename: PropTypes.string,
    forceRefresh: PropTypes.bool,
    getUserConfirmation: PropTypes.func,
    keyLength: PropTypes.number,
    children: PropTypes.node
  }

  history = createHistory(this.props)

  render() {
    return <Router history={this.history} children={this.props.children}  />
  }
}
/* BrowerRouter component will render a router component and passing it to a history prop. This history prop will listen to the changes in URL*/
```
<strong>Recap</strong>
In conclusion, we have to wrap whole app with BrowerRouter component and then BrowserRouter will be aware of any URL changes to make cooresponding changes.
### 4. The Link Component
The Link componenet can provide a declarative, accessible navigation around your application.
Given serveral examples
```jsx
/*1. Intuitive examples*/
<link to="/about">About</Link>
/*2. More complex example with query*/
<Link to={{
  pathname: '/courses',
  search:'?sort=name',
  hash: '#the-hash',
  state: { fromDashboard: true }
}}
Course
</link>
```
<strong>Recap</strong>
A Link compoenent will allow you to add declarative, accessible navigation around your applicaiton.
### 5. The Route Component
A router component will help react to decide which components are rendered based on the current URL path. Typically, it will mean the backwards button in the browswer will work.
Given an example
```jsx
import { Route } from 'react-route-dom'
<Route
  path='/create'
  render={ui}
/>
```
It is extremely useful when you want to go to different page according to URL
```jsx
/*Previously we need those steps to decide which page should render*/
{this.state.screen === 'list' && (
  <ListContacts
    contacts={this.state.contacts}
    onDeleteContact={this.removeContact}
  />
)}
{this.state.screen === 'create' && (
  <CreateContact />
)}
/*Now with Router Component, we can simple let route component to decide which page to render according to URL*/
  <Route exact path="/" render={() => (
    <ListContacts
    onDeleteContact={this.removeContact}
    contacts={this.state.contacts}
    />)}/>
  <Route path='/create' component={CreateContact}/>
```
<strong>Recap</strong>
So with route component, it is easier to render pages.
### 6. Additional useful component
Form-serialize can output form information as a regular javascript
```jsx
import serializeForm from 'form-serialize'
/*Usually we have to prevent browser default form submition and serlize form ourselves*/
handleSubmit = (e) => {
  e.preventDefault()
  const values = serializeForm(e.target, { hash: true });
  if (this.porps.onCreateContact) {
    this.props.onCreateContact(values)
  }
}
```
## Conclusion
In this Note, we learned what is lifecycle event, how react router works and some useful npm tools.
## Code Reference
https://github.com/BridgeNB/React-Learning-Note-3.git
