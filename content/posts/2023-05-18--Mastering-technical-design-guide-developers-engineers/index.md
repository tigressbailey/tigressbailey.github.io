---
title: Mastering the Art of Technical Design - A Comprehensive Guide 
date: "2023-05-18T12:40:32.169Z"
template: "post"
draft: false
slug: "mastering-technical-design-guide-developers-engineers"
category: "Frontend"
tags:
  - "Technical design"
  - "Development process"
  - "Risk reduction"
  - "Software engineering"
  - "Feature implementation"
description: ""
# socialImage: "/media/42-line-bible.jpg"
socialImage: ""
---

## Introduction
Technical designs play a crucial role in the development process by reducing risks and providing a structured approach. This guideline aims to assist developers and engineers in creating effective technical designs. By following these essentials, teams can better understand requirements, identify critical paths, break down large features into manageable tasks, anticipate future requirements and technical challenges, mitigate risks, and facilitate task estimations and code reviews.

## Target Audience
This guideline is intended for engineers and developers involved in the software development process.

## Essential Elements
### Where
- Entrance: Clearly define the entry point or access method for the feature.
- Routes: Identify the routes that will be affected or created as part of the feature implementation.
- Redirection Targets: Specify any redirections that need to be implemented as part of the feature.

### Who
- Roles: Determine which roles are allowed or restricted from accessing the feature.
- Permissions: Define the specific permissions required or denied for accessing the feature.
- Subscriptions: Identify any subscriptions that may have access or restrictions related to the feature.

### When
- Feature Flag: Specify the feature flag that activates or deactivates the feature.
- Related Settings: Identify any additional settings that control the activation or deactivation of the feature.

### Which
- Containers: Determine which containers will be affected or created by the feature.
- Components: Identify the components and nested components that need to be assembled for different containers.
- Container/Component Changes: Consider how containers and components may need to adapt to different responsive breakpoints.
- Currency and Time Format: Address any requirements related to currency and time formatting.
- i18n Support: Determine the internationalization (i18n) requirements and how they will be supported.

### What
- Synchronous Actions: Specify the actions that occur synchronously and their success or failure outcomes.
- Asynchronous Actions: Identify any asynchronous actions, including loading status and success or failure states.
- API Integrations: Describe the necessary API integrations, such as GET, POST, PATCH, and DELETE requests.
- Websocket: Determine if the feature requires WebSocket integration for real-time communication.

By considering these essential elements and incorporating them into the technical design, developers and engineers can ensure a well-thought-out approach that reduces risks and enhances the development process.