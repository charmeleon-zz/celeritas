---
title: "Organizing Frontend Projects"
date: 2019-08-11T12:46:18-05:00
draft: false
tags: react, javascript, architecture, frontend
---

A frequent question with frontend projects is how they should be structured.
Not too long ago a single PHP file was enough for backend, HTML and JS/CSS
logic; but with increasing complexity the need to organize in sensible fashion
has become obvious. What is less obvious is your options, so let's go over
some.

Starting a new `React` project, the scaffolding provided by `create-react-app`
is simple enough for a UI project:

```bash
├── package.json
├── package-lock.json
├── public
│   ├── favicon.ico
│   ├── index.html
│   ├── logo192.png
│   ├── logo512.png
│   ├── manifest.json
│   └── robots.txt
├── README.md
└── src
    ├── App.css
    ├── App.js
    ├── App.test.js
    ├── index.css
    ├── index.js
    └── logo.svg
```

The `public` directory can remain untouched even for very large projects. With
smaller projects in mind, let's review what the `src` directory should
look like.

## A Flat Project
The first option is to use a completely flat structure, sticking with what's
provided out of the box by `create-react-app`.

```bash
src/
├── App.css
├── App.js
├── App.test.js
├── Dropdown.css
├── Dropdown.jsx
├── Dropdown.test.js
├── index.css
├── index.js
├── Input.css
├── Input.jsx
├── Input.test.js
└── logo.svg
```

This can be alright for prototypes and toy projects.

## Logical Grouping
Another option is to logically group slices of functionality. This isolates
application responsibilities and can reduce the cognitive load for finding
a piece of functionality even as the project grows in size. It makes sense
to consider this if your application is using (or will use) `redux` since
it's used in several popular tutorials, increasing the chance that new members
will be able to find their way around your codebase relatively quickly.  

```bash
src/
├── App.css
├── App.js
├── App.test.js
├── index.css
├── index.js
├── logo.svg
├── actions
│   └── index.js
├── components
│   ├── Dropdown.css
│   ├── Dropdown.jsx
│   ├── Dropdown.test.js
│   ├── index.js
│   ├── Input.css
│   ├── Input.jsx
│   └── Input.test.js
├── containers
│   ├── index.js
│   ├── OptionChangedReducer.js
│   └── OptionChangedReducer.test.js
├── reducers
│   ├── index.js
│   └── InputActions.js
└── selectors
    ├── index.js
    ├── InputActionsSelector.js
    └── InputActionsSelector.test.js
```

Note that each directory under `src` now has an `index.js`; they are now modules
and the `index.js` file can be used to define the public functionality exposed
by each of them. In the case of the `components` directory you can expect to
see the `Dropdown` and `Input` components exported; this also makes it so
your bundler (`webpack` or `rollup` or `parcel` or `bazel`) doesn't pickup your
test files or dependencies since they are not imported via your entry points: 

```javascript
export * from './Dropdown';
export * from './Input';
```

## Group by Feature

A third option is to group files such that the contents of the module
directories represent a complete feature. This is less common but can be
effective if your project is growing large and it'll be difficult to locate
files when grouped logically (imagine having 50+ components and finding a file
through that!).

This structure also lends itself to nesting for sub-features in future
improvements, such as contexts, higher-order components, etc.

```bash
src/
├── App.css
├── App.js
├── App.test.js
├── index.css
├── index.js
├── logo.svg
├── Dashboard
│   ├── Dashboard.css
│   ├── Dashboard.jsx
│   ├── dashboardReducer.js
│   ├── dashboardReducer.test.js
│   ├── dashboardSelectors.js
│   ├── dashboardSelectors.test.js
│   ├── Dashboard.test.jsx
│   └── index.js
├── Feed
│   ├── Entry.css
│   ├── Entry.jsx
│   ├── Entry.test.jsx
│   ├── Feed.css
│   ├── Feed.jsx
│   ├── Feed.test.jsx
│   └── index.js
└── Header
    ├── Header.css
    ├── Header.jsx
    ├── headerReducer.js
    ├── headerReducer.test.js
    ├── headerSelectors.js
    ├── headerSelectors.test.js
    ├── Header.test.jsx
    └── index.js
```

## Isolating tests
But it isn't necessary to have your tests alongside your code; it's common to
dedicate a directory solely to contain your tests. This can be an improvement
if your IDE allows you to mark a directory as a test directory.

Here's what the Functional Grouping would look like with isolated tests:
```bash
src/
├── App.css
├── App.js
├── App.test.js
├── index.css
├── index.js
├── logo.svg
├── actions
│   └── index.js
├── components
│   ├── __tests__
│   │   ├── Dropdown.test.jsx
│   │   └── Input.test.jsx
│   ├── Dropdown.css
│   ├── Dropdown.jsx
│   ├── index.js
│   ├── Input.css
│   ├── Input.jsx
├── containers
│   ├── __tests__
│   │   └── OptionChangedReducer.test.js
│   ├── index.js
│   └── OptionChangedReducer.js
├── reducers
│   ├── index.js
│   └── InputActions.js
└── selectors
    ├── __tests__
    │   └── InputActionsSelector.test.js
    ├── index.js
    └── InputActionsSelector.js
```

This can be done similarly for a project grouped by features.

## Summary
As frontend projects continue to grow in complexity (especially true of single
page applications) having a few go-to strategies for organizing your code will
prove a time-saver in the long run. Make sure you recognize the trade-offs of
different approaches and choose carefully!

Be on the lookout for the next installment where I will discuss large frontend
projects.