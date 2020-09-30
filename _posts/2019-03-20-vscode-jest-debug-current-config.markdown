---
layout: post
title: "VSCode Jest Debug Current Config"
date: 2019-03-20T21:58:19+08:00
author: tigressbailey
sitemap: false
keywords: "vscode debug jest config"
description: "VSCode Jest Debug Current Config"
---

```
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

```
{
  // Use IntelliSense to learn about possible attributes.
  // Hover to view descriptions of existing attributes.
  // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
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
<!--more-->
