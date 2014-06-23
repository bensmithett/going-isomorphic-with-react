# Intro
---------------------
- CSS?

- Preprocessor?

- BEM, SMACSS, OOCSS


# Components vs page design/build
---------------------
- We used to design & build pages.

- Homepage

- Products page
  - Do things page by page

- We don't design pages any more.
  - We still end up with pages

- Now we design & build components
  - What do I mean by component?

- A little independent chunk of a web page
  - It's a piece of HTML
  - Has CSS styles associated with it
  - Might have some JavaScript behaviour associated with it as well

- Example: FB Like button
  - Self-contained little chunk of web page made of HTML, CSS & JS

- Building blocks of our web pages
  - Design entire libraries of components
  - Keep them in living style guides

- Building a page: grab pre-existing components you need & assemble a page out of them


# Responsive components
---------------------
- I think these components should be responsive.
  - Page : Device == Component : Container
  - Where our pages should work no matter what device you view them in
  - Our components should work no matter what container you put them in
  - Those little isolated chunks of web page should be flexible
  - Should't know or care where you put them
  - They should just work in the available space

- Example component
  - In sidebar it's green
  - In main column
    - when there's more space available it should adjust itself to fit that space
  
- Component should be flexible enough to work in any sized container
  - We'll want to make styling changes at certain breakpoints

- How do we do that? How do we make a component adjust itself based on where we put it?

- When you're dealing with the whole page, you use Media Queries
  - Entire browser window is the viewport
  - With Media Queries we measure the width of that viewport to change styles at different breakpoints
  - For a responsive component, the component itself is the viewport.
  - Remember a component is just a HTML element
  - Media Queries only tell us the width of the whole browser window, not about specific elements.

- Instead of window-based MQs we need element-based MQs

# Element Queries
---------------------
- Been floating around for a while:
  - Heaps of interest about 9 months ago
  - http://ianstormtaylor.com/media-queries-are-a-hack/
  - Explains the things I just talked about

- syntax example
  - We don't care how big the window is, we only care about how big the element itself is.

- Element Queries don't exist!
  - Why?
    - Circular dependencies

- code example
  - the big reason we can't have Element Queries

- Post by Tab Atkins explains it

- But just because we can't have Element Queries doesn't mean we need to give up on responsive components.


# How to fake it!
---------------------
- iframes!

- Facebook Like button
  - & most third party components

- Perfect if you think about it
  - Establish new, isolated viewport
  - Regular Media Queries work
  - Not affected by child elements
  - No circular dependencies

- Animation
  - Put an iframe-based component in any sized container & it just works

- Obvious downsides
  - Big performance impact
    - Every Facebook Like button is a whole new web page loading in a little iframey window
    - For that reason they usually show up half an hour after the rest of the page
  - Because they're in this whole new isolated document context
    - They don't inherit styles from outer window
      - If I've set base font styles, components inside an iframe won't inherit those styles
    - It's a pain to make JS from your window talk to the document inside the iframe
- Verdict: out


- Fake Element Queries with JS
  - Watch an element
  - detect when it resizes
  - Update a class on the element

- Animation
  - Add class once component reaches certain width

- CSS for that

- Downsides
  - Detecting resize difficult
  - Hacky solutions
  - Verdict: ehhhhhhh
    - Maybe for an experiment, but not something I'd do on a production website tomorrow
  - Links


- My non-solution solution for the moment for responsive components

- It'd be nice if our responsive components looked amazing in an infinite number of situations
  - That's not reaaaaally the case with our day-to-day work

- Most components we build will only ever be rendered in a 2 or 3 different places
  - Sidebar
  - Main content area

- You probably already know the Media Query breakpoints
  - Because we know where our components will live (even though we're not supposed to, components meant to be independent & isolated)
    - We can kinda cheat
    - Just use classes & normal Media Queries!
  - Just normal Responsive Web Design
  - But with BEM syntax & goodies from Sass it really starts to look like a proper isolated component
  - Not as nice as a pure solution

- What is a Responsive Component
  - Works where you need it to
  - Really easy to go overboard


# The future
---------------------
- Web Components / Shadow DOM
  - Awesome! I can't wait!
  - Only in Chrome
  - Polymer alpha polyfill
    - W3C starting to see they need to be responsive
    - Driving renewed push for...
  

- Element Queries! 
  - <viewport> solves circular depency issue
    - Creates independent viewport like iframe
    - Without iframes other baggage
  - They might exist some day

- Thanks!


