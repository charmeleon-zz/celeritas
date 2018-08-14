---
title: "Beyond Todo Apps"
date: 2018-08-11T10:28:01-05:00
draft: false
---

Developers implement toy projects from scratch to understand new technology.
These often implement simple functionality, and none is more pervasive than
the "Todo" app.

The web has too many Todo tutorials, Hello World tutorials, introductions
to new frameworks by showing how you handle a click, and it's time to slow these
down. 

Instead we need to tackle more complex problems to demo the more powerful
features when writing sample projects.

This is a small catalog of projects I find useful when I need to come up to
speed with X technology in a short period of time. The TODO app is not found
here, as I have never implemented a TODO app.

Frontend Projects
==

## [Tic Tac Toe](#tic-tac-toe)

Tic tac toe is useful for learning a new language or frontend framework.
It's simple and has a definite end, and has no database or network interactions.
None of us are beyond implementing tic tac toe in a new language The following
two examples implement it using web frontend frameworks, it can just as easily
be built on a terminal.

* [React Tic Tac Toe](https://reactjs.org/tutorial/tutorial.html)
* [React TypeScript Tic Tac Toe](https://charmeleon.github.io/post/react-typescript-tic-tac-toe/)

## [Yelp clone](#yelp-clone)

A Yelp clone is no easy feat, but there is an extremely detailed tutorial for it
that includes setting up and configuring Babel, Webpack, Karma with React. If
you are looking to learn these technologies, it is well worth your time. There's
no reason why it can't be reproduced in Vue/Flutter/Angular, or why your can't
later on swap out your bundler or test framework.

* [React Tutorial: Cloning Yelp](https://www.fullstackreact.com/articles/react-tutorial-cloning-yelp/)

Backend/Full Stack Projects
==

## [Reddit clone](#reddit-clone)
Makeschool hosts an excellent walkthrough on the creation of a reddit clone. It
suggests NodeJS but there's no starter code; it can be built in any
stack, multiple times even.

* [Reddit Clone](https://www.makeschool.com/academy/track/reddit-clone-in-node-js)


## [CRUD](#crud)
A large number of useful applications are driven by simple CRUD
(create/read/update/delete) operations. The TODO app is often used to
demonstrate the concept, but in evaluating new tech it's simplistic and likely
doesn't hit the sore spots. There are better options: 

* [Microsoft's modeling of University classes](https://docs.microsoft.com/en-us/aspnet/core/data/ef-mvc/complex-data-model?view=aspnetcore-2.0)
  * Can be extracted to model, build and test RESTful API in your language /
  framework of choice
* Inspirational quotes service: Build a site that shows the user a random
 inspirational quote from a database.
  * Easily extracted to build just a tested backend RESTful API for reading
  from a database, with seed data inserted directly through the database via
  the framework or SQL statements.

## [Real Time](#real-time)
Increasingly, applications need near-real time interactions. Here's a small
collection of projects (these are more web specific):

* [Real-time Chat with React and GraphQL](https://scotch.io/tutorials/how-to-build-a-realtime-chat-app-with-react-and-graphql)
* [Real time communication with WebRTC](https://codelabs.developers.google.com/codelabs/webrtc-web/#0)