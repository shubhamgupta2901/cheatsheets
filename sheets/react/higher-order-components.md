### Higher Order Components 

* A higher-order component (HOC) is an advanced technique in React for reusing component logic. 
* A Higher-order component is a function that takes a component and returns a new component.
```javascript const EnhancedComponent = higherOrderComponent(WrappedComponent);```
* HOCs are not part of the React API, per se. They are a pattern that emerges from React’s compositional nature.

#### Functional Programming

* The concept of HOCs must have been derived from functional programming.  Just like functional programming pattern, the goal of using HOCs is to decompose the logic into simpler and smaller components that can be reused. This  avoids side effects and makes debugging and maintenance a whole lot easier.

* Consider ```map()``` function in javascript - It takes an array, for each element it performs the task provided by the callback function, and returns a new *enhanced* array:

```javascript
var numbers = [1, 4, 9];
var doubles = numbers.map(num => num * 2);

// doubles is now [2, 8, 18]
// numbers is still [1, 4, 9]
```

* HOCs can also be used for similar purposes. Two things happen with HOCs. They take a component as an argument and return some JSX code. This JSX Code is generally a component which takes the component provided in argument and does something to enhance it.

* Lets look at an example to understand the concept:



