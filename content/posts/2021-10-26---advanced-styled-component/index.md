---
title: Advanced Styled-Component
date: "2021-10-26T12:40:32.169Z"
template: "post"
draft: false
slug: "advanced-styled-component"
category: "Frontend"
tags:
  - "Frontend"
  - "Web Development"
description: ""
# socialImage: "/media/42-line-bible.jpg"
socialImage: ""
---

There are many pros and cons to CSS-in-JS, but I believe it goes way past opinions and pros/cons, into neurodiversity territory.

CSS-in-JS is great at making CSS work for the JS-minded, and I would recommend it (preferably in some zero-runtime form) whenever the team is comprised purely by programmers in the traditional sense of that word (i.e. imperative programming CS background). Which is what most teams currently are, as there has been a ginormous shift from desktop and back end tasks into the front end, which made many of those devs switch as well.

Form my perspective, it just works great with component.

### File Structure

The styles can be placed:

1. Inside the component tsx file
2. In the same level of the component file
3. Common styles can be wrapped in the parent level

```JavaScript
//Yep
- FormContainer/
-- CommonStyled.ts
-- FormSection/
--- index.tsx
--- styled.ts

/**
  - FormSection/
  --- index.tsx
  For instance:
**/


const Headline = styled.h1`
  color: red;
`;

const Content = ({ title, children }) => {
  return (
    <section>
      <Headline>{title}</Headline>
      <span>{children}</span>
    </section>
  );
};


// Nope
- FormSection/
--- index.tsx
--- styled.ts
```

### Add style to an existing component

```JavaScript
import { Card } from 'component-library'

export const CardWrap = styled(Card)`
  margin: 40px 32px;
  padding: 24px 0;
  box-shadow: none;
  border: 1px solid ${colorGrayScaleNormal};

  .ant-card-body {
    padding: 0;
  }
`
```

### Multiple styled components over Single styled components

There are two ends of a spectrum for approaches when using Styled Components. It's important to know that this is a spectrum, and I will show two extreme versions of it, because after all there are many more liberal approaches in between.

On the left side of the spectrum, there is the approach when everything with style becomes a styled component. Thus every styled component is responsible for its style.

Usually this is the most popular approach and I think it's mostly because developers have a greater acceptance of JavaScript over CSS. Thus using only styled components without the need for CSS classes or CSS selectors keeps it simpler. In addition, it supports the mental mindset of "everything is a component".

Attention:

1. It would be fine to use single styled components and override ant design classes.
2. It would be fine to use single styled components and custom classes when it depends on the React refs.

```JavaScript
// Yep
const Section = styled.section`
  border-bottom: 1px solid grey;
  padding: 20px;
`;

const Headline = styled.h1`
  color: red;
`;

const Text = styled.span`
  padding: 10px;
`;

const Content = ({ title, children }) => {
  return (
    <Section>
      <Headline>{title}</Headline>

      <Text>{children}</Text>
    </Section>
  );
};

// Nope
const Container = styled.section`
  border-bottom: 1px solid grey;
  padding: 20px;

  h1 {
    color: red;
  }

  .text {
    padding: 10px;
  }
`;

const Content = ({ title, children }) => {
  return (
    <Container>
      <h1>{title}</h1>

      <span className="text">{children}</span>
    </Container>
  );
};
```

### CSS snippet

Avoid Semicolons when using the snippet.

```JavaScript
// Yep
import styled, { css } from 'styled-components';

const red = css`
  color: red;
`;

const Headline = styled.h1`
  ${red}

  font-size: 20px;
`;

const Text = styled.p`
  ${red}

  font-size: 16px;
`;


// Nope
import styled, { css } from 'styled-components';

const red = css`
  color: red;
`;

const Headline = styled.h1`
  ${red};

  font-size: 20px;
`;

const Text = styled.p`
  ${red};

  font-size: 16px;
`;

```

### Consume props

```JavaScript
export const LineCover = styled.img<{ src?: string; size?: string }>`
  width: ${props => (props.size ? props.size : '48px')};
  height: ${props => (props.size ? props.size : '48px')};
  border-radius: 8px;
  object-fit: cover;
`
```

### Return complex CSS snippet and inherit props

```JavaScript
const IsolatedItemWrapper = styled.div<{ showAsterisk?: boolean }>`
  ${props => {
    if (props.showAsterisk) {
      return css`
        .ant-form-item-label {
          &::before {
            display: inline-block;
            margin-right: 4px;
            color: #ff8b49;
            font-size: 12px;
            font-family: SimSun, sans-serif;
            line-height: 1;
            content: '*';
          }
        }
      `;
    }
  }}
`
```

### Inherit props via CSS snippet

```JavaScript
import { css } from 'styled-components'

export const BackgroundImage = css<{ src?: string }>`
  background-image: ${props =>
    props.src ? `url(${encodeURI(props.src)})` : 'none'};
`

export const MenuProductCover = styled.div<{ src?: string }>`
  width: 64px;
  height: 64px;
  border-radius: 8px;
  display: inline-block;
  cursor: pointer;
  ${BackgroundImage}
  background-size: cover;
  background-position: center;
`
```

### Transient props
If you want to prevent props meant to be consumed by styled components from being passed to the underlying React node or rendered to the DOM element, you can prefix the prop name with a dollar sign ($), turning it into a transient prop.

In this example, $draggable isn't rendered to the DOM like draggable is.

```JavaScript
const Comp = styled.div`
  color: ${props =>
    props.$draggable || 'black'};
`;

render(
  <Comp $draggable="red" draggable="true">
    Drag me!
  </Comp>
);
```
