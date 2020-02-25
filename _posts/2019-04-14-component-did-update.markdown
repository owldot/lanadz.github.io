---
layout: post
title: 'React: Change state in componentDidUpdate()'
date: 2019-04-14
tags:
  - javascript
keywords:
  - react
  - change state in componentDidUpdate
  - Maximum update depth exceeded.
---

## componentDidUpdate()

I've recieved new props and needed to update a state.
Legit place to do that, as I thoght, was `componentDidUpdate`.

<!--more-->

So, you go to `componentDidUpdate` you set state as usual and you encouner nice error..
caused by infinite loop of setting state which triggers `componentDidUpdate`..

{% highlight err %}
Maximum update depth exceeded. This can happen when a component
repeatedly calls setState inside componentWillUpdate
or componentDidUpdate.
React limits the number of nested updates to prevent infinite loops.
{% endhighlight %}

The rest is from [official doc](https://reactjs.org/docs/react-component.html#componentdidupdate)
{% highlight js t%}
componentDidUpdate(prevProps, prevState, snapshot)
{% endhighlight %}

componentDidUpdate() is invoked immediately after updating occurs. This method is not called for the initial render.

Use this as an opportunity to operate on the DOM when the component has been updated. This is also a good place to do network requests as long as you compare the current props to previous props (e.g. a network request may not be necessary if the props have not changed).
{% highlight js t%}
componentDidUpdate(prevProps) {
// Typical usage (don't forget to compare props):
if (this.props.userID !== prevProps.userID) {
this.fetchData(this.props.userID);
}
}
{% endhighlight %}
You may call `setState()` immediately in componentDidUpdate() but note that it must be wrapped in a condition like in the example above, or you’ll cause an **infinite loop**. It would also cause an extra re-rendering which, while not visible to the user, can affect the component performance. If you’re trying to “mirror” some state to a prop coming from above, consider using the prop directly instead. Read more about why copying props into state causes bugs.

Cheers!
