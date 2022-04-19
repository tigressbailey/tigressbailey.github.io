---
layout: post
title: "Get Image/file Upload Progress Using Axios"
date: 2022-04-19T16:13:29+08:00
author: tigressbailey
sitemap: false
keywords: ""
description: ""
---

Axios is a library that makes it easy to make HTTP requests in JavaScript.

We've got used to it for handling all kinds of HTTP requests.

But Let me ask a question:
Do you know how to get the progress of an image/file upload using Axios?

Here is an example.

Assuming Ant Design's upload component is used.

```js
import { Upload } from 'antd'

<Upload
  accept="image/png,image/jpeg"
  onChange={changeHandler}
  customRequest={customRequest}
  fileList={fileList}
  multiple={false}
  showUploadList={false}
  beforeUpload={beforeUpload}
>
  <Label htmlFor="uploader">{icon}</Label>
</Upload>
```

The key method for the progress is `customRequest`.
Take a look at it.

```js
const customRequest = async options => {
  const { onSuccess, onError, file, onProgress } = options
  const offlineId = `${uuidv4()}`

  submitMessage(
    { type: SPINNER_ACTION, comment: getProgressText(0) },
    offlineId
  )
  const config = {
    onUploadProgress: ({ loaded, total }) => {
      const percent = Math.round((loaded * 100) / total)
      if (percent !== 100) {
        onProgress({ percent })
        submitMessage(
          { type: SPINNER_ACTION, comment: getProgressText(percent) },
          offlineId
        )
      }
    },
  }

  try {
    const result = await fileUploadRequest(file, config)

    submitMessage(
      { type: SPINNER_ACTION, comment: getProgressText(100) },
      offlineId
    )
    onSuccess('Ok')
    result && uploadSuccess(result, offlineId)
  } catch (err) {
    removeMessage(offlineId)
    onError({ err })
  }
}
```

The config object defines the progress callback and it would be used inside the Axios `PUT` request.

```js
export const fileUploadRequest = async (
  file: File,
  config: { onUploadProgress: (ProgressEvent) => void }
): Promise<ResponseData> => {
  const res = await post<ResponseData>(
    `/upload_api`,
    {
      folder: 'example',
      filename: file.name,
      contentType: file.type,
    }
  ).then(res => res.data)

  await Axios.put(res.url, file, {
    headers: res.headers,
    ...config,
  })

  return res
}
```

In this function, we create a file record first.

Then we upload the file to the server.

Done and happy coding.

---

Spring is officially arrived.

Since the weather is nice, I am going to enjoy it with my Family..
<!--more-->
