---
title: How to customize Typescript types from a design system
date: "2022-05-02T12:40:32.169Z"
template: "post"
draft: false
slug: how-to-customize-typescript-types-from-a-design-system"
category: "Frontend"
tags:
  - "Frontend"
  - "Web Development"
  - "Typescript"
  - "Ant Design"
description: ""
# socialImage: "/media/42-line-bible.jpg"
socialImage: ""
---

Typescript is a powerful tool for building large-scale, complex, and maintainable applications. It is a superset of JavaScript, and is used to build both client-side and server-side applications.
It is quite popular in the industry, and is used by our team as well.Although the runtime bugs are still depending on the developer skill set, two major benefits of using Typescript are:
 1. It reduces the compile time bug rate by defining the types of variables and functions explicitly.
 1. It speeds up the development process by defining the types of variables and functions explicitly.

And yes, define the types of variables and functions explicitly is indeed a good practice.

For a React Project, generally, the types of Props and Components are defined in the following way:

```typescript
import { FC, Dispatch, SetStateAction} from 'react;

interface DateRangeData {
  [key: string]: {
    month: string
    year: string
  }
}

interface Props {
  name: string;
  age: number;
  dateRangeData: DateRangeData;
  setDateRange?: Dispatch<SetStateAction<PatchApiDateRange.Data>>
}

const MyComponent:FC<Props> = ({name, age, dateRangeData}) => {
  const { month, year } = dateRangeData;
  return (
    <>
      <h1>{name}</h1>
      <h2>{age}</h2>
      <h3>{year}</h3>
      <h3>{month}</h3>
    </>
  );
};
```

A quick question, how to customize the types of variables and functions for an existing design system?

I'll use [Ant Design](https://ant.design/components/overview/) as an example.

#### Use Case: Extends the Type

We are going to extend the Props to add a new type: `suffix`.

```typescript
import { FC } from 'react'
import { ButtonProps } from 'antd/lib/button'
import { StyledButton, Suffix } from './styled'

interface Props extends ButtonProps {
  suffix?: string
}

const Button: FC<Props> = ({
  suffix,
  children,
  ...props
}) => {
  return (
    <StyledButton
      {...props}
      $type={props.type || 'default'}
      $size={props.size || 'middle'}
    >
      {children}
      {suffix && <Suffix>{suffix}</Suffix>}
    </StyledButton>
  )
}

export default Button
```

It is quite easy.

And for `$type` and `$size`, they are [Transient props](https://styled-components.com/docs/api#transient-props).
It prevents props meant to be consumed by styled components from being passed to the underlying React node or rendered to the DOM element, you can prefix the prop name with a dollar sign ($), turning it into a transient prop.

#### Use Case: Override the Type
The button component has a `type` type and it contains 6 values: primary, ghost, dashed, link, text and default.
We are going to override the type of the button component to add a new type: `secondary`.
  

```typescript
// Button/types.ts
import { ButtonType as OriginalButtonType } from 'antd/lib/button'

export enum ButtonType {
  primary = 'primary',
  secondary = 'secondary',
  outline = 'outline',
  text = 'text',
  link = 'link',
  dashed = 'dashed',
}

export interface TypeAlias {
  [key: string]: OriginalButtonType
}
```

```typescript
// Button/index.tsx
import { FC } from 'react'
import { ButtonProps } from 'antd/lib/button'
import { StyledButton, Suffix } from './styled'
import { ButtonType, ButtonSize, TypeAlias } from './types'

interface Props {
  type?: ButtonType
  suffix?: string
}

const Button: FC<Omit<ButtonProps, keyof Props> & Props> = ({
  type = ButtonType.secondary,
  suffix,
  children,
  ...props
}) => {
  /**
  *   Below alias is not required for a fully custom style component.
  *   It is only applicable when we want to reuse the original styles according to the type.
  **/
  const typeAlias: TypeAlias = {
    [ButtonType.primary]: 'primary',
    [ButtonType.secondary]: 'default',
    [ButtonType.outline]: 'default',
    [ButtonType.text]: 'text',
    [ButtonType.link]: 'link',
    [ButtonType.dashed]: 'dashed',
  }

  return (
    <StyledButton
      {...props}
      type={typeAlias[type]}
      $type={type}
      $size={ButtonSize.middle}
    >
      {children}
      {suffix && <Suffix>{suffix}</Suffix>}
    </StyledButton>
  )
}

export default Button
```

In this case, we shall just focus on the `Omit` utility function.
`Omit` removes the keys of Props from the ButtonProps.
Lexically, it shall remove `type` and `suffix` from the ButtonProps.
But `suffix` is not existed at all so that only `type` would be removed.

This rule can be simply applied to any types when we want to customize them.

#### Use Case: Override Components as TypeScript Generic Functions

The select component has a `mode` type and it contains 2 values: multiple and tags.
We are going to override the type of the select component to only support `tags` mode.

```typescript
import React, { FC } from 'react'
import { SelectValue, SelectProps } from 'antd/lib/select'
import { SizeType } from 'antd/lib/config-provider/SizeContext'
import { StyledSelect, GlobalSelectStyle } from './styled'

interface Props {
  size?: SizeType
  status?: 'error'
  mode?: 'tags'
}

type CustomSelectProps<VT> = Omit<SelectProps<VT>, keyof Props> & Props

const Select: FC<CustomSelectProps<SelectValue>> = <
  VT extends SelectValue = SelectValue
>({
  size = 'middle',
  status,
  mode,
  ...props
}: CustomSelectProps<VT>) => {
  return (
    <>
      <GlobalSelectStyle $size={size} />
      <StyledSelect
        {...props}
        size={size}
        $size={size}
        $status={status}
        mode={mode}
      />
    </>
  )
}

export default Select
```

VT stands for Value Type and it is consistent with the definition of the SelectValue in Ant Design.
`GlobalSelectStyle` is used for overriding the dropdown menu style.

Happy coding!