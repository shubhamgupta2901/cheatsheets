### Introduction

* React is a javascript library to build user-interfaces. 
* React apps run on browsers, they don't run on servers. And since we don't have to wait for server responses, rendering of new pages happen quite fast.
* React is a component driven framework, i.e. a we can split a random webpage into components. By doing so we can build these components as contained pices of code, and we don't have to build the web page as one big thing. We can change independent components and reuse these components.
* So React solves the problem of having to build complex user interfaces which we had to do with HTML and Javascript.

### Single and Multipage applications
* In a single page application like React, we only get back one single HTML file by the server. We get this file the first time the user visits the page. Thereafter everything is managed with javascript. The entire page consists of components which are rendered and handled by javascript.
* On the other hand in a multi-page application, we get back multiple HTML pages, where each page has a content for a given route or URL we visited.For example: for example.com and example.com/users we get two different pages.
* On multipage applications, we might also use React, but only to create little widgets; i.e. the whole page is managed by React. 
* The more popular approach is to use a single page application, because we get to manage the entire page with javascript. We don't have to go to the server and reload the page. This gives us a much better user experience.
* 
  