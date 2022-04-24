---
title: React 18 New Features and life update
date: "2021-06-21T12:40:32.169Z"
template: "post"
draft: false
slug: "react-18-new-features-and-life-update"
category: "Frontend"
tags:
  - "Frontend"
  - "Life"
  - "Web Development"
description: ""
# socialImage: "/media/42-line-bible.jpg"
socialImage: ""
---

## Transition

Consider typing in an input field that filters a list of data. It needs to store the value of the field in state so that it can filter the data and control the value of that input field.

```JavaScript
setFilter(input);
```

It update the input value and use the new value to search the list and show the results whenever the user types a character. For large-screen updates, this can cause lag on the page while everything renders, making typing or other interactions feel slow and unresponsive. Even if the list is not too long, the list items themselves may be complex and different on every keystroke, and there may be no clear way to optimize their rendering.

Conceptually, the issue is that there are two different updates that need to happen. The first update is an urgent update, to change the value of the input field and, potentially, some UI around it. The second is a less urgent update to show the results of the search.

```JavaScript
// Urgent: Show what was typed
setInputValue(input);

// Not urgent: Show the results
setFilter(input);
```

In terms of the concept here, we use `debounce` to handle the search queries and separate the input text from the queries.

```JavaScript
 const onKeywordChange = useMemo(
    () =>
      debounce((keyword: string) => {
        setFilter(p => ({ ...p, keyword }))
      }, 500),
    []
  )

  const handleSearch = () => {
    onKeywordChange(rawKeywordInput)
    setRawKeywordInput(rawKeywordInput)
  }

  useEffect(() => {
    onKeywordChange(rawKeywordInput)
  }, [rawKeywordInput, onKeywordChange])
```

The new startTransition API solves this issue by giving you the ability to mark updates as “transitions”:

```JavaScript
import { startTransition } from 'react';
// Urgent: Show what was typed
setInputValue(input);
// Mark any state updates inside as transitions
startTransition(() => {
  // Transition: Show the results
  setFilter(input);
});
```

## Automatic batching for fewer renders

Batching is when React groups multiple state updates into a single re-render for better performance.

For example, if you have two state updates inside of the same click event, React has always batched these into one re-render. If you run the following code, you’ll see that every time you click, React only performs a single render although you set the state twice:

```JavaScript
const [loadingDisabled, setLoadingDisabled] = useState<boolean>(false)
const [roomNumber, setRoomNumber] = useState<RoomNumber>(defaultRoomNumber)

const clickHandler = (roomType: RoomType) => {
  setLoadingDisabled(disabled => !disabled)
  setRoomNumber(roomNumber => roomNumber + 1)
  // React would only re-render once in v18
}
```

This is great for performance because it avoids unnecessary re-renders.

React would not batch the updates if you need to fetch data, and then update the state in the `clickHandler` above, and perform two independent updates.

This is because React used to only batch updates during a browser event (like click), but here we’re updating the state after the event has already been handled (in fetch callback):

```JavaScript
const [loadingDisabled, setLoadingDisabled] = useState<boolean>(false)
const [roomNumber, setRoomNumber] = useState<RoomNumber>(defaultRoomNumber)

const clickHandler = (roomType: RoomType) => {
  fetchRoomNumber().then(() => {
    setLoadingDisabled(disabled => !disabled) // Before React 18: Re-render
    setRoomNumber(roomNumber => roomNumber + 1) // Before React 18: Re-render
    // React 18: Re-render once
  })
}
```

Manually re-render interface:

```JavaScript
import { flushSync } from 'react-dom'

const [loadingDisabled, setLoadingDisabled] = useState<boolean>(false)
const [roomNumber, setRoomNumber] = useState<RoomNumber>(defaultRoomNumber)

const clickHandler = (roomType: RoomType) => {
  flushSync(() => {
    setLoadingDisabled(disabled => !disabled)
  })
  // Re-render

  flushSync(() => {
    setRoomNumber(roomNumber => roomNumber + 1)
  })
  // Re-render
}
```

## SSR support for Suspense

Basically, it just shortens the time to interactive during the SSR process.

## Adoption

It is quite easy to upgrade React 18 as no breaking changes.

Well , that's great because Next.js would definitely adopt it every soon and I just need to keep an eye on the npm modules and next configs.

---

Half year has passed since React 17 is released.

I've stepped into another territory by learning new business, using new technical stack and collaborating with new tools.

It has been such a pleasure to work with my colleagues as everyone is really humble and proactive.

There are people love coding and engineering from the bottom of their hearts like I do.

And I made a timeline calendar from scratch recently.

It recalls me that my friends said someone would steal and copy my idea once but I'm the only person who has the actual creative sprit.

Innovation lives with my blood and ready to blossom any time.

To be humble and kind are much better than to be arrogant and cocky.
