---
layout: post
title: 'Things I Bookmarked When Learning React And ES6'
date: 2019-04-12
tags:
  - javascript
keywords:
  - react
  - arrow functions
  - default props react
  - setting state react
  - handling events in react
  - preventing components from rendering
---

Things I found important, strange or just interesting about react and es6 (from official tutorial). Article is for people who start learning react

## Arrow Functions

Parenthesize the body of function to return an object literal expression:
{% highlight js %}
params => ({foo: bar})
{% endhighlight %}
Rest params and default params
{% highlight js %}
(param1, param2, ...rest) => { statements }
{% endhighlight %}

<!--more-->

## Default Props

We can make them conditional when rendering, something like `number || 0` or ternary operator or you can use `static`
{% highlight js %}
static defaultProps = {
center: {
lat: 1,
lng: 100
},
zoom: 1
};
{% endhighlight %}

## Setting State

If we have functional component we need to convert it to a class and set initial state in constructor:
{% highlight js %}
class Clock extends React.Component {
constructor(props) {
super(props);
this.state = {date: new Date()};
}
{% endhighlight %}

Never modify state directly, right way is to use `setState`. If state updates may be asynchronous - we use `setState` with callback

{% highlight js %}
this.setState((state, props) => ({
counter: state.counter + props.increment
}));
{% endhighlight %}

Setting state with `setState` is shallow, so `this.setState({comments})` leaves `this.state.posts` intact, but completely replaces `this.state.comments`.

## Handling Events

Bind methods in constructor
{% highlight js %}
constructor(props) {
super(props);
this.state = {isToggleOn: true};

    // This binding is necessary to make `this` work in the callback
    this.handleClick = this.handleClick.bind(this);

}
{% endhighlight %}

Otherwise you will need to do callback in rendering, like:

{% highlight react %}
render() {
// This syntax ensures `this` is bound within handleClick
return (
<button onClick={(e) => this.handleClick(e)}>
Click me
</button>
);
{% endhighlight %}
In most cases, this is fine. However, if this callback is passed as a prop to lower components, those components might do an extra re-rendering. We generally recommend binding in the constructor or using the class fields syntax, to avoid this sort of performance problem.

Passing Arguments to Event Handlers
Inside a loop it is common to want to pass an extra parameter to an event handler. For example, if `id` is the row ID, either of the following would work:
{% highlight react %}

<div>
  <button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
  <button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
</div>
{% endhighlight %}

## Preventing Component from Rendering

In rare cases you might want a component to hide itself even though it was rendered by another component. To do this return null instead of its render output.

## Keys

Do not handle key attribute in extracted child component. Handle key in parent component

{% highlight react %}
function ListItem(props) {
// Correct! There is no need to specify the key here:
return <li>{props.value}</li>;
}

function NumberList(props) {
const numbers = props.numbers;
const listItems = numbers.map((number) =>
// Correct! Key should be specified inside the array.
<ListItem key={number.toString()}
              value={number} />

);
return (
<ul>
{listItems}
</ul>
);
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
<NumberList numbers={numbers} />,
document.getElementById('root')
);
{% endhighlight %}

_Keys Must Only Be Unique Among Siblings_

Embedding `map` in jsx
{% highlight react %}
function NumberList(props) {
const numbers = props.numbers;
return (
<ul>
{numbers.map((number) =>
<ListItem key={number.toString()}
                  value={number} />

      )}
    </ul>

);
}
{% endhighlight %}

## React router

How to pass props to component? You need to use `render` instead `component` property
{% highlight react %}
<Route
path='/dashboard'
render={(props) => <Dashboard {...props} isAuthed={true} />}
/>
{% endhighlight %}
