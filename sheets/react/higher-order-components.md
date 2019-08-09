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

* Lets look at an example to understand the concept: Consider a blogging app in react where we may have to show a blogPost and the comments in the blogpost. 

This is how our ```BlogPost``` looks like:
```javascript
class BlogPost extends Component {
    
    constructor(props){
        super(props);
        this.state = {
            blogPost: {
                title: '',
                body: '',
            },
        }
    }

    async componentDidMount(){
        const blogPost = await DataSource.getBlogPost(this.props.id);
        this.setState({blogPost});
    }


    render () {
        return (
            <div className={styles.BlogPost}>
                <h1>{this.state.blogPost.title}</h1>
                <p>{this.state.blogPost.body}</p>
            </div>

        );
    }
}
```

And this is how our ```Comments``` component looks like:
```javascript
class Comments extends React.Component {
  
  constructor(props){
    super(props);
    this.state = {
      comments: [],
    }
  }

  async componentDidMount(){
    const comments  = await DataSource.getComments();
    if(comments){
      this.setState({comments});
    }
  }
  
  renderComments(){
    return this.state.comments.slice(0,5).map(comment=> <Comment  comment = {comment}key = {comment.id}/>);
  }

  render(){
    return (
        <div className={styles.Comments}>
            {this.renderComments()}
        </div>
        
    );
  }
}
```

```Comments``` component is composed of a stateless react component ```Comment```, which looks like: 

```javascript
const comment = (props) => (
    <article className={styles.Post}>
        <h4>{props.comment.email}</h4>
        <div className={styles.Info}>
            <div className={styles.Author}>{props.comment.body}</div>
        </div>
    </article>
);
```

* Depending on the app route, we may need to render ```BlogPost``` or ```Comments``` in our app.

* ```Comment``` and ```BlogPost``` aren’t identical — they call different methods on ```DataSource```, and they render different output. But much of their implementation is the same:
  * on mount, they call a method in ```DataSource``` to recieve some data.
  * when they recieve response from the method, they set their state for data.
  * accordingly the component is rendered with data provided by ```DataSource```.

* You can imagine that in a large app, this same pattern of subscribing to ```DataSource``` and calling setState will occur over and over again. We want an abstraction that allows us to define this logic in a single place and share it across many components. This is where higher-order components excel.






