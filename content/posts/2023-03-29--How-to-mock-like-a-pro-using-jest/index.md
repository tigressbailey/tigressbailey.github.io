---
title: How to mock like a Pro using Jest
date: "2023-03-29T12:40:32.169Z"
template: "post"
draft: false
slug: "how-to-mock-like-a-pro-using-jest"
category: "Frontend"
tags:
  - "Frontend"
  - "Jest"
description: ""
# socialImage: "/media/42-line-bible.jpg"
socialImage: ""
---

React Testing has been changed from avoiding side effects to embracing side effects.
Previously, we were using Enzyme to test an isolated component by modifying the component's props and states.
Now we simulate the user interactions and the reflections on the screen.

But there is always tons of questions regarding how to mock `navigator`, `window`, `internal library` and `functions of internal libraries`.
Let's dive into it and see how to mock them like a Pro.

### Mock `navigator`

```Javascript
describe('utils/handleCopyText', () => {
  Object.assign(navigator, () => {
    clipboard: {
      writeText: () => {},
    }
  });

  jest.spyOn(navigator.clipboard, 'writeText');

  it('should get correct copy', () => {
    handleCopyText('test');

    expect(navigator.clipboard.writeText).toHaveBeenCalledWith('test');
  })
});
```

### Mock `window`

```Javascript
describe('utils/getRouteStatus', () => {
  it('should get active status', () => {
    const windowSpy = jest.spyOn(global, 'window', 'get');
    // @ts-ignore
    windowSpy.mockImplementation(() => {
      return {
        location: {
          pathname: '/test',
          hash: '',
        },
      };
    });

    const status = getRouteStatus({
      layout: MENU_LAYOUT.Even,
      main: [
        {
          id: 'text id',
          type: MENU_TYPE.Text,
          title: 'text title',
          href: '/test',
        },
      ],
    });

    expect(status).toBeTruthy();
  });
});

```

### Clean up

It is important to clean up the mocks.


```Javascript
afterEach(() => {
  jest.clearAllMocks();
});
```
