---
title: How to generate CSV and preserve leading
date: "2022-06-22T12:40:32.169Z"
template: "post"
draft: true
slug: "how-to-set-up-storybook-with-next-js-styled-components-and-i18next"
category: "Frontend"
tags:
  - "Frontend"
  - "Web Development"
  - "Storybook"
  - "Styled components"
  - "Next.js"
  - "i18next"
description: ""
# socialImage: "/media/42-line-bible.jpg"
socialImage: ""
---

A good design system would align the same gene code across the product line.
It help resolve the communication gaps between designers and developers. 
And it is part of code reuse and decoupled, therefor it can indeed speed up the development progress.

I would introduce how to set up Storybook with Next.js,  styled-components and I18next.
- Storybook is an open source tool for building UI components and pages in isolation. It streamlines UI development, testing, and documentation. It is a great tool for Component libraries and Design System.
- Next.js is a React framework that gives you building blocks to create web applications. 
- Styled-component is one of the most popular CSS-In-JS library for theming and modularized styles.
- I18next is an internationalization-framework written in and for JavaScript.


### Install dependencies

```bash
npx sb init --builder webpack5
```

This would add `@storybook/addon-essentials` and `@storybook/react` to package.json.

`@storybook/addon-essentials` provides core features like Docs, Controls, Actions, Viewport, Backgrounds, Toolbars & globals, Measure & outline.

### Config `.storybook/main.js`

- `addons`: Config which addons would be used. The backgrounds feature is disabled for our use case.
- `babel`: styled-components plugin is added into it.
- `framework`: declares the targeting library is React.
- `stories`: stories paths. Generally, the naming convention would be like ComponentName.stories.(js|jsx|ts|tsx).
- `webpackFinal`: Custom Webpack config. For instance, I added a path alias '@' here for representing the repository root path. Secondly, I also add `fs: 'empty'` to avoid errors while using Storybook with Webpack 5.

```JavaScript
const path = require('path')

module.exports = {
  addons: [
    {
      name: '@storybook/addon-essentials',
      options: {
        backgrounds: false,
      },
    },
    '@storybook/addon-actions',
  ],
  babel: async options => ({
    ...options,
    presets: ['next/babel'],
    plugins: [
      ['styled-components'],
    ],
  }),
  framework: '@storybook/react',
  stories: [
    '../ui/**/*.stories.@(js|jsx|ts|tsx)',
    '../components/**/*.stories.@(js|jsx|ts|tsx)',
  ],
  webpackFinal: async config => {
    // alias
    Object.assign(config.resolve.alias, {
      '@': path.resolve(__dirname, '../'),
    })

    return {
      ...config,
      node: {
        ...config.node,
        fs: 'empty',
      },
    }
  },
}
```

### Create custom `.storybook/viewports.js`

The Viewport toolbar item allows adjusting the dimensions of the iframe your story is rendered in. It makes it easy to develop responsive UIs.
`@storybook/addon-essentials` offers a standard set of viewports by default.
But we would like to custom it according to support specific viewports.


```JavaScript
export const CUSTOM_VIEWPORTS = {
  iphone5: {
    name: 'iPhone 5',
    styles: {
      height: '568px',
      width: '320px',
    },
    type: 'mobile',
  },
  iphonex: {
    name: 'iPhone X',
    styles: {
      height: '812px',
      width: '375px',
    },
    type: 'mobile',
  },
  iphonexr: {
    name: 'iPhone XR',
    styles: {
      height: '896px',
      width: '414px',
    },
    type: 'mobile',
  },
  iphone12: {
    name: 'iPhone 12',
    styles: {
      height: '844px',
      width: '390px',
    },
    type: 'mobile',
  },
  iphone12promax: {
    name: 'iPhone 12 Pro Max',
    styles: {
      height: '926px',
      width: '428px',
    },
    type: 'mobile',
  },
  ipad: {
    name: 'iPad',
    styles: {
      height: '1024px',
      width: '768px',
    },
    type: 'tablet',
  },
  ipad10p: {
    name: 'iPad Pro 10.5-in',
    styles: {
      height: '1112px',
      width: '834px',
    },
    type: 'tablet',
  },
  ipad12p: {
    name: 'iPad Pro 12.9-in',
    styles: {
      height: '1366px',
      width: '1024px',
    },
    type: 'tablet',
  },
  galaxys9: {
    name: 'Galaxy S9',
    styles: {
      height: '740px',
      width: '360px',
    },
    type: 'mobile',
  },
  nexus6p: {
    name: 'Nexus 6P',
    styles: {
      height: '732px',
      width: '412px',
    },
    type: 'mobile',
  },
  pixel: {
    name: 'Pixel',
    styles: {
      height: '960px',
      width: '540px',
    },
    type: 'mobile',
  },
  pixelxl: {
    name: 'Pixel XL',
    styles: {
      height: '1280px',
      width: '720px',
    },
    type: 'mobile',
  },
};
```

### Customize `.storybook/preview.js` and enable i18next

`.storybook/preview.js` configure actions, controls, viewports, parameters etc.. for all stories.

The entire file would look like this.

And I will explain the code snippets and their usages below it.

```JavaScript
import React, { Suspense } from 'react'
import { initReactI18next, I18nextProvider } from 'react-i18next'
import HttpApi from 'i18next-http-backend'
import i18next from 'i18next'
import { useGlobals } from '@storybook/addons'
import { CUSTOM_VIEWPORTS } from './customViewports'

export const parameters = {
  actions: { argTypesRegex: '^on[A-Z].*' },
  controls: { expanded: true },
  viewport: {
    viewports: CUSTOM_VIEWPORTS,
  },
  storySort: {
    method: 'alphabetical',
    order: ['Components', 'Icons', 'Others'],
  },
}

export const globalTypes = {
  locale: {
    name: 'Locale',
    description: 'Internationalization locale',
    defaultValue: 'en',
    toolbar: {
      icon: 'globe',
      items: [
        { value: 'en', title: 'English' },
        { value: 'ja', title: '日本語' },
        { value: 'zh', title: '繁體中文' },
        { value: 'zh-CN', title: '简体中文' },
      ],
    },
  },
}

export const decorators = [
  (Story, Context) => {
    const [{ locale }] = useGlobals()

    i18next
      .use(HttpApi)
      .use(initReactI18next)
      .init({
        whitelist: ['en', 'ja', 'zh', 'zh-CN'],
        lng: locale,
        fallbackLng: 'en',
        ns: 'common',
        defaultNS: 'common',
        backend: {
          loadPath: '/static/locales/{{lng}}/{{ns}}.json',
        },
        // debug: true,
      })

    return (
      <Suspense fallback="Loading...">
        <I18nextProvider i18n={i18next}>
          <Story />
        </I18nextProvider>
      </Suspense>
    )
  },
]

```

#### I18next - internationalization
As `Next.js` brings SSR feature and `next-i18next` only support loading locale files via SSR.
We are not going to use `next-i18next` as it is not suitable.

Instead, it requires 'react-i18next', 'i18next-http-backend', 'i18next' for Storybook to load the locales via CSR.

And to connect the current locale with Storybook, `useGlobals` is also imported from '@storybook/addons'

The `debug` property can be used for logging current locale information.

```JavaScript
import { initReactI18next, I18nextProvider } from 'react-i18next'
import HttpApi from 'i18next-http-backend'
import i18next from 'i18next'
import { useGlobals } from '@storybook/addons'

const [{ locale }] = useGlobals()

i18next
  .use(HttpApi)
  .use(initReactI18next)
  .init({
    whitelist: ['en', 'ja', 'zh', 'zh-CN'],
    lng: locale,
    fallbackLng: 'en',
    ns: 'common',
    defaultNS: 'common',
    backend: {
      loadPath: '/static/locales/{{lng}}/{{ns}}.json',
    },
    // debug: true,
  })
```

#### Use custom viewports
Import the viewports set we just created.

```JavaScript
import { CUSTOM_VIEWPORTS } from './customViewports'

export const parameters = {
  viewport: {
    viewports: CUSTOM_VIEWPORTS,
  },
}
```

#### Config global parameters

- `actions`: Register actions starts with 'on'.
- `controls`: Expanded controls by default.
- `storySort`: sort stories properly.
**The most important thing is, `export` it.**

```Javascript
export const parameters = {
  actions: { argTypesRegex: '^on[A-Z].*' },
  controls: { expanded: true },
  storySort: {
    method: 'alphabetical',
    order: ['Components', 'Icons', 'Others'],
  },
}
```

#### Add a custom toolbar - locale in the toolbar

Storybook has a simple, declarative syntax for configuring toolbar menus. 
We can add custom toolbar by creating globalTypes with a toolbar annotation.

```JavaScript
export const globalTypes = {
  locale: {
    name: 'Locale',
    description: 'Internationalization locale',
    defaultValue: 'en',
    toolbar: {
      icon: 'globe',
      items: [
        { value: 'en', title: 'English' },
        { value: 'ja', title: '日本語' },
        { value: 'zh', title: '繁體中文' },
        { value: 'zh-CN', title: '简体中文' },
      ],
    },
  },
}
```

### Start the storybook

Just run below command and see a design systems/component libraries support internationalization and css-in-js using Next.js.

```bash
yarn storybook
```

And we all set and happy coding.

### Advanced References
- [@storybook/addon-essentials](https://storybook.js.org/docs/react/essentials/introduction)
- [Toolbar and Globals](https://storybook.js.org/docs/react/essentials/toolbars-and-globals)
- [Structuring your storybook](https://storybook.js.org/blog/structuring-your-storybook/)
