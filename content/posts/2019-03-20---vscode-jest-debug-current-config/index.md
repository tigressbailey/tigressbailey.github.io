---
title: VSCode Jest Debug Current Config
date: "2019-03-20T12:40:32.169Z"
template: "post"
draft: false
slug: "vscode-jest-debug-current-config"
category: "Frontend"
tags:
  - "Frontend"
  - "Web Development"
description: "Introduce vscode jest debug config technique."
# socialImage: "/media/42-line-bible.jpg"
socialImage: ""
---

### Jest

```JSON
{
  "version": "0.2.0",
  "configurations": \[
    {
      "type": "node",
      "name": "vscode-jest-tests",
      "request": "launch",
      "args": ["${relativeFile}", "--codeCoverage=false"],
      // "args": [
      //   "test",
      //   "lib-name",
      //   "--runInBand=true",
      //   "--codeCoverage=false"
      // ],
      "disableOptimisticBPs": true,
      "cwd": "${workspaceFolder}",
      "console": "integratedTerminal",
      "internalConsoleOptions": "neverOpen",
      "program": "${workspaceFolder}/node_modules/jest/bin/jest",
      "windows": {
        "program": "${workspaceFolder}/node_modules/jest-cli/bin/jest"
      },
      "trace": "all"
    }
  ]
}

```

### Chrome

```JSON
{
  "version": "0.2.0",
  "configurations": [
      {
          "type": "chrome",
          "request": "launch",
          "name": "Launch Chrome against localhost",
          "url": "http://localhost:3001",
          "webRoot": "${workspaceFolder}/src",
          "userDataDir": false
      }
  ]
}
```