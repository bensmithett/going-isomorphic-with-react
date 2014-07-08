# Going Isomorphic with React

Short intro to React
React is a JavaScript library for building user interfaces from the Facebook & Instagram teams.
If you think about MVC/MV* JS frameworks like Backbone, Angular & Ember, React is purely the V.

This is a React component
It's a button, when I click it it's going to say hello to whatever name I pass into its props.

And just like every other framework's Views I can give it some data & render it client side into my DOM
React.renderComponent
- calls the render function of the component I passed in (plus some other lifecycle methods)
- builds a DOM tree and appends it to the given mount node
- adds some event listeners

And you end up with a button that does what we expect it.
Nothing new here compared to other JS frameworks.

As well as renderComponent, React has a function called renderComponentToString which
instead creating a real DOM tree, just returns a HTML string representation.

And it returns a string that looks like this.

One place we like to generate HTML strings rather than real HTML elements is on the server
So (as you all have figured out by now) on we can render React components server side with node.js.
(diagram?)

The End (?)
Like most things with React, this is boringly simple concept to get your head around.
React has 2 functions
- renderComponent builds a real DOM tree, appends it to a given node & attaches event handlers
- renderComponentToString generates a string representation of that HTML tree in its initial state


Pause. Let's talk about Views.

Let's go back to the year 2000 and talk purely about server-rendered Views. No client-side rendering.
Tell you why I think React components are a better way to do server-rendered views than whatever you're currently using.

If you're building a web site today, you're probably doing a mixture of server-rendered HTML & client-rendered HTML.
How much of each you do probably depends where you sit on the web site to web app spectrum.
Assume most of us are doing at least a little of both.

I work in a Rails environment.
Server-rendered views in Rails means an ERB template.
[Server: ERB] ---- [Client: Handlebars]
If you work in PHP or .Net or Java this should look pretty familiar

Looks like this.

What is a template?
It's a thing that we give some data, and it gives us back some HTML.
If we give it the same data, it should always give us back the same HTML string.

Templates are functions.
This isn't news to anyone who's done client side rendering
If you compile a Handlebars template it literally gives you back a function
But I'd never really thought of server side templates as functions.

Early on, they were really big, complicated functions that did too much:
<div>
  <?php
  $con=mysqli_connect("example.com","peter","abc123","my_db");
  // Check connection
  if (mysqli_connect_errno()) {
    echo "Failed to connect to MySQL: " . mysqli_connect_error();
  }

  $result = mysqli_query($con,"SELECT * FROM Persons");

  while($row = mysqli_fetch_array($result)) {
    echo $row['FirstName'] . " " . $row['LastName'];
    echo "<br>";
  }

  mysqli_close($con);
  ?>
</div>
Source: w3schools

And we reacted to that unmaintainable spaghetti code
"no logic in templates!"

But it turns out we do need a bit of logic in our views.

"no business logic in templates!*"
*but the occasional loop or if statement is ok...

But it turns out sometimes we need more logic than a simple if-statement or loop

"View logic belongs in view-models, not templates!"

And that's kinda the current state of play for server-rendered HTML.
- Template 
  - as logic-less as possible, but probably has some loops & conditionals
  - it's a function, but we don't really treat it like one
  - it's a weird HTML document with implicit dependencies
- View model/presenter (for more complex view logic... sorting, string manipulation, etc)

I think React components are a better way to do server-side templating.

<script>
var React = require("react");
var _ = require("underscore");

var PizzaButton = React.createClass({
  shoutName: function (name) {
    return name.toUpperCase();
  },
  getFirstPizza: function (pizzas) {
    return _.first(pizzas);
  },
  render: function () {
    return(
      <div>
        <button>
          Say hi to {this.shoutName(this.props.name)}
        </button>

        The best pizza is {this.getFirstPizza(this.props.pizzas)}
      </div>
    );
  }
});

module.exports = PizzaButton;
</script>

http://georgebrock.github.io/talks/command-line-ruby/images/lex.jpg

What you get:
- Explicit dependencies
- View logic extracted into easily testable functions
- A HTML(ish) template
- Plain old real JavaScript instead of a hamstrung templating language
- All of this view's concerns in one place!

vs

- Implicit dependencies
- Logic lives off in another file somewhere
- Arbitrary rules about how much complexity is allowed

Ignoring the whole client side, I like React components for server-rendering HTML.

So... Isomorphic JavaScript
Means JS that you write once, runs on both client and server
First time I heard it Airbnb Rendr

Holy grail
Initial render happens on the server - serve real HTML up.
JS app bolts onto the server-rendered HTML, from that point on it's a single page JS app.

What's held us back until now is this
[ERB] ------ [Handlebars]
Making your client-side app play nice with server-rendered HTML is annoying.
Duplicate templates.
So we usually just do it client-side.

React makes the holy grail boringly simple.
- Initial render
  - React.renderComponentToString(<PizzaButton />)
- Client side app:
  - React.renderComponent(<PizzaButton />, mountNode)
  - If mountNode already contains a PizzaButton, React won't replace it. Just adds event handlers.

Same component used on client & server!

All well & good if your backend is node.js - how to do it in Rails/PHP/.NET/whatever?

View rendering as a service
Your app probably already talks to other services
- Search
- Authentication
- Third party APIs

Send JSON data + component name
Receive HTML string

https://github.com/facebook/react/tree/master/examples/server-rendering#architecture-overview

https://github.com/reactjs/react-rails
Doesn't need to be a service - if you can execute JS in your app, you can server-render a React component.

Progressive enhancement for free!

If JS breaks, no big deal. Your app can be rendered in any state on the server, just fall back to full page loads.

Example


