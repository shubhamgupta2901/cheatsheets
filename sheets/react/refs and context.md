#### Refs


* ReactNative supports a special attribute that we can attach to any component. The ref attribute can be a callback function, and this callback will be executed immediately after the component is mounted. The referenced component will be passed in as a parameter, and the callback function may use the component immediately, or save the reference for future use (or both).

```js
<ActionSheet ref={(component) => (this._actionSheetRef = component)}>
	<View style={styles.container} onLayout={this.onMainViewLayout}>
		{this.renderMessages()}
		{this.renderInputToolbar()}
	</View>
</ActionSheet>
```

* Here we have attached ref to component ActionSheet, It is a callback function which is executed immediately after ActionSheet component will be mounted. This component itself will be passed as a parameter to the callback function of ref. And we have saved the reference of this component for later use.

* For this specific case, we have passed the reference of this component to the child components(using context) of GiftedChat and they can use this component to create an action sheet by themselves.

#### Context
* ```context``` is similar to ```props``` in react , the only difference is that context lets you pass some data through all nodes in component tree.```Props``` are passed explicitly by parent to child, and they do not propagate down the hierarchy, while ```context``` can be requested by children component implicitly.
* Consider a following component tree:
				GrandparentComponent
								|
						ParentA
						 /    \
				 ChildA   ChildB
* ```GrandparentComponent``` renders ```ParentA``` component that renders two child components: ```ChildA``` and ```ChildB```. Lets say ```Grandparent``` knows some data Xdata that ```ChildA``` and ```ChildB``` wants to know as well, but ```ParentA``` doesn’t care about. How would ```GrandparentComponent``` give ```ChildA``` and ```ChildB``` access to Xdata?  
	* IF, using Flux architecture we can store Xdata inside a store and let Grandparent, Child A and Child B subscribe to that store.
	* we can also pass Xdata as prop to ```ChildA``` and ```ChildB```. But ```GrandparentComponent``` can not pass props to its grand children without going through ```ParentA```.So ```GrandparentComponent``` passes props to ```ParentA``` and ```ParentA``` passes them to child.
	* Or we can use ```context```. ```context`` allows children component to request some data to arrive from component that is located higher in the hierarchy.

Example:

```js
class Grandparent extends React.Component{  

	childContextTypes: {
    name: React.PropTypes.string.isRequired
  },

  getChildContext: function() {
    return {name: 'Jim'};
  },

  render() {
    return <Parent/>;
  }
}
```

```js
class Parent extends React.Component{
 render() {
   return <Child/>;
 }
}
```

```js
class Child extends React.Component{
 contextTypes: {
   name: React.PropTypes.string.isRequired
 },
 render() {
  return('My name is' + {this.context.name});
 }
}
```

* Our ```GrandparentComponent```  says two things:
	1.I provide my children with context type named ```'name'``` of type string. This is what happening in ```childContextTypes``` declaration.
	2.The value of context type named ```'name'``` is ```Jim```. This is what happening in ```getChildContext``` function.

* And our child component just says “Hey I expect to have context type named name!” and it gets it.
As far as I understand (and I’m far from being expert in react.js internals) when react renders children components it checks which components want to have context and those that want context are provided with context if parent supplies them.


### Spread attribute:
* It's called spread attributes and its aim is to make the passing of props easier.Not a react feature but that of ES6.

* Let us imagine that you have a component that accepts N number of properties. Passing these down can be tedious and unwieldy if the number grows.

```
<Component x={} y={} z={} />
```

* Thus instead you do this, wrap them up in an object and use the spread notation

```var props = { x: 1, y: 1, z:1 };
<Component {...props} />
```
* this will unpack it into the props on your component, i.e., you "never" use {... props} inside your render() function, only when you pass the props down to another component. Use your unpacked props as normal this.props.x.
