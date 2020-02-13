---
templateKey: blog-post
title: Limit concurrency in fp-ts using TypeScript
date: 2020-02-13T15:20:59.866Z
description: ' '
featuredpost: false
featuredimage: /img/fp-ts-logo.png
tags:
  - fp-ts
  - typescript
  - promises
  - tasks
  - limit
  - concurrency
---
`fp-ts` is a great library in Typescript that helps writing functional programming code that helps in mitigating certain types of bugs and easier handling of concurrency, errors and side effects.

I encountered a problem which required limiting the concurrency of nodejs code, this is because of the endpoint I'm doing requests against starts to fail when a certain number of requests are done against it in short time. One of the solutions that came to my mind is limiting the number of promises that do requests for this endpoint.

`p-map` is an npm package that helps setting a concurrency limit on JS promises (something similar to using a thread pool in multithreaded languages). You can use it with `fp-ts` like in this snippet:

```typescript
import { Task } from 'fp-ts/lib/Task'
import pmap from 'p-map'

export function parallel<A>(tasks: Array<Task<A>>, limit: number): Task<Array<A>> {
  return () => pmap(tasks, t => t(), { concurrency: limit })
}
```
