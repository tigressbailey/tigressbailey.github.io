---
layout: post
title: "Reset Redux State"
date: 2019-10-11T11:01:23+08:00
author: tigressbailey
sitemap: false
keywords: ""
description: ""
---

Once the user exits the system, do multiple clear functions called still?

For example, the state of the user and his/hers cats needs to be flushed during the logout process.
The code would be:
```
const usersDefaultState = [];
const users = (state = usersDefaultState, { type, payload }) => {
  switch (type) {
    case "RESET_USER":
      return usersDefaultState;
    case "ADD_USER":
      return [...state, payload];
    default:
      return state;
  }
};

const catsDefaultState = [];
const cats = (state = catsDefaultState, { type, payload }) => {
  switch (type) {
    case "RESET_CATS":
      return catsDefaultState;
    case "ADD_CATS":
      return [...state, payload];
    default:
      return state;
  }
};
```

Regarding DRY principle, is it possible to reset the redux state using the root reducer?
The answer is yes.
Let's checkout the code:
```
// configureStore.js
const appReducer = createAppReducer();

const rootReducer = (state, action) => {
  let currentState = state;

  if (action.type === 'LOGOUT_SUCCEEDED') {
    currentState = undefined;
  }

  return appReducer(currentState, action);
};

// Execute it in saga
yield put({ type: 'LOGOUT_SUCCEEDED' });
```

Additionally, the isolation clear actions can still exit in each reducer.
The Redux state can be cleaned up partially and entirely.
<!--more-->
