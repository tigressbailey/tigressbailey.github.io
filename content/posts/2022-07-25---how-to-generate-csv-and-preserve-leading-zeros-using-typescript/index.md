---
title: How to generate CSV without garbled characters and preserve leading zeros using TypeScript
date: "2022-07-22T12:40:32.169Z"
template: "post"
draft: false
slug: "how-to-generate-CSV-without-garbled-characters-and-preserve-leading-zeros-using-typescript"
category: "Frontend"
tags:
  - "Frontend"
  - "TypeScript"
  - "CSV"  
description: ""
# socialImage: "/media/42-line-bible.jpg"
socialImage: ""
---

I'm going to share how to Generate an elegant CSV file from the Frontend side.

Below use cases will be covered in this article:
1. CSV file in Microsoft Excel and are seeing incorrect, or garbled characters, such a é, ü, Å, etc. 
2. Leading zeros are cut off after the CSV is generated, such as 001 to 1.
3. TypeScript


First of all, kudos to [react-csv](https://github.com/react-csv/react-csv).
It supports 3 data source type: array of literal objects, array of arrays and strings
It is using JavaScript.

I'm going to transfer it to TypeScript and only support array of literal objects.

### Data and Columns
These will be used for CSV's data source and headers.
```TypeScript
const Data: Datum[] = [
    {
        "id": "cc7ae7fb-4b9a-412c-97e9-6cb122374203",
        "type": "001",
        "percent": "1.6 %",
        "deposit": "$293.50 SGD",
    },
    {
        "id": "9f5fcdba-8963-4a08-b1d1-f37c04715c5b",
        "type": "002",
        "deposit": "$130.50 SGD",
    }
]

const Columns: Column[] = [
    {
        "title": "Type",
        "dataIndex": "type"
    },
    {
        "title": "percent",
        "dataIndex": "percent"
    },
    {
        "title": "deposit",
        "dataIndex": "deposit"
    },
]
```

### TypeScript version here we go

```TypeScript
interface Header {
  [key: string]: string
}

export const isJsons = <T>(array: T[]): boolean =>
  Array.isArray(array) &&
  array.every(row => typeof row === 'object' && !(row instanceof Array))

export const getHeaderValue = <T>(property: string, obj: T): string => {
  const foundValue = property
    .replace(/\[([^\]]+)]/g, '.$1')
    .split('.')
    .reduce(function (o, p, i, arr) {
      // if at any point the nested keys passed do not exist, splice the array so it doesnt keep reducing
      const value = o[p]
      if (value === undefined || value === null) {
        arr.splice(1)
      } else {
        return value
      }
    }, obj)
  // if at any point the nested keys passed do not exist then looks for key `property` in object obj
  return foundValue === undefined
    ? property in obj
      ? obj[property]
      : ''
    : foundValue
}

export const jsons2arrays = <T>(
  jsons: T[],
  originalHeaders: Header[]
): Array<Header[] | string[]> => {
  const headers = originalHeaders

  // allow headers to have custom labels, defaulting to having the header data key be the label
  let headerLabels: string[]
  let headerKeys: string[]
  if (isJsons(headers)) {
    headerLabels = headers.map(header => header.title)
    headerKeys = headers.map(header => header.dataIndex)
  }

  const data = jsons.map(object =>
    headerKeys.map(header => getHeaderValue(header, object))
  )

  return [headerLabels, ...data]
}

export const elementOrEmpty = <T>(element: undefined | null | T): string | T =>
  typeof element === 'undefined' || element === null ? '' : element

interface JoinerData {
  [key: number]: string
}

export const joiner = (
  data: Array<JoinerData[]>,
  separator = ',',
  enclosingCharacter = '"'
): string => {
  return data
    .filter(e => e)
    .map(row =>
      row
        .map(element => elementOrEmpty(element))
        .map(column => {
          if (Number.isNaN(parseInt(column as string, 10))) {
            return `${enclosingCharacter}${column}${enclosingCharacter}`
          }

          return `=${enclosingCharacter}${column}${enclosingCharacter}`
        })
        .join(separator)
    )
    .join(`\n`)
}

export const jsons2csv = <T>(
  data: T[],
  headers: Header[],
  separator = ',',
  enclosingCharacter = '"'
): string => joiner(jsons2arrays(data, headers), separator, enclosingCharacter)

export const toCSV = <T>(
  data: T[],
  headers: Header[],
  separator = ',',
  enclosingCharacter = '"'
): string => {
  if (isJsons(data)) {
    return jsons2csv(data, headers, separator, enclosingCharacter)
  }

  throw new Error()
}

export const buildURI = <T>(
  data: T[],
  headers: Header[],
  uFEFF = true,
  separator = ',',
  enclosingCharacter = '"'
): string => {
  const csv = toCSV(data, headers, separator, enclosingCharacter)
  const type = 'text/csv'
  const blob = new Blob([uFEFF ? '\uFEFF' : '', csv], { type })
  const dataURI = `data:${type};charset=utf-8,${uFEFF ? '\uFEFF' : ''}${csv}`

  const URL = window.URL || window.webkitURL

  return typeof URL.createObjectURL === 'undefined'
    ? dataURI
    : URL.createObjectURL(blob)
}

```

### Key points
1. Resolve garbled characters issue. 
  ```TypeScript
  // In buildURI function
  const blob = new Blob([uFEFF ? '\uFEFF' : '', csv], { type })
  ```

2. Resolve leading zeros are cut off issue.
  ```TypeScript
  // joiner
  return `=${enclosingCharacter}${column}${enclosingCharacter}`
  ```
### Generate CSV action

`clickHandler` should be tied to a button's click event or other actions.
It would generate a CSV file and download it in a new tab.
`message` is not required here. It could be replaced with other component library.

```TypeScript
  const clickHandler = (data: Datum[], columns: Column[], filename: string) => {
    try {
      const href = buildURI(data, columns)
      const downloadLink = document.createElement('a')

      downloadLink.download = filename
      downloadLink.href = href
      downloadLink.target = '_blank'
      downloadLink.style.display = 'none'

      document.body.appendChild(downloadLink)
      downloadLink.click()
      document.body.removeChild(downloadLink)

      message.success('Successfully generate CSV file.')
    } catch (e) {
      message.error(e.message)
    }
  }
```


### Advanced References
- [react-csv](https://github.com/react-csv/react-csv).
