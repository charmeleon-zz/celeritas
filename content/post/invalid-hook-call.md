---
title: "Invalid Hook Call"
date: 2019-08-10T08:11:18-05:00
tags: react, javascript, warning, yarn
draft: false
---

The "Invalid Hook Call Warning" sometimes pops up when developing with React.
If you follow React's [docs](https://reactjs.org/warnings/invalid-hook-call-warning.html)
on the subject and it isn't resolved and you're seeing this warning when
running your tests.

First verify that `react` and `react-dom` are the same version and that there
is only one version installed as the docs say. If you still see this during
your tests, check the version of `react-test-renderer` and make sure it matches
your version of `react`. It's easy to do this with yarn resolutions:

![yarn resolutions](/invalid_hooks_warning/react_resolutions.png)

Alternatively, you can try uninstalling and then reinstalling the
`enzyme-adapter-react-16` package which is where the dependency is likely coming
from. But if you don't control the version of this package installed in your
project then a resolution is the way to go. 