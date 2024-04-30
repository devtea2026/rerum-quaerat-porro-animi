<p align="center">
  <img src="https://github.com/devtea2026/rerum-quaerat-porro-animi/blob/master/logo.svg" width="300px"/>
</p>

# Resreq

[![version](https://img.shields.io/github/v/release/molvqingtai/@devtea2026/rerum-quaerat-porro-animi)](https://www.npmjs.com/package/@devtea2026/rerum-quaerat-porro-animi) [![deno land](http://img.shields.io/badge/available%20on-deno.land/x-lightgrey.svg?logo=deno&labelColor=black&color=blue)](https://deno.land/x/@devtea2026/rerum-quaerat-porro-animi) [![workflow](https://github.com/devtea2026/rerum-quaerat-porro-animi/actions/workflows/ci.yml/badge.svg)](https://github.com/devtea2026/rerum-quaerat-porro-animi/actions) [![download](https://img.shields.io/npm/dt/@devtea2026/rerum-quaerat-porro-animi)](https://www.npmjs.com/package/@devtea2026/rerum-quaerat-porro-animi) [![JavaScript Style Guide](https://img.shields.io/badge/code_style-standard-brightgreen.svg)](https://standardjs.com)

## What is @devtea2026/rerum-quaerat-porro-animi?

It is a modern http client, based on [fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch), because it is implemented internally using the onion model, so you can use middleware to intercept requests and responses elegantly.

[Learn more](https://dev.to/molvqingtai/applying-koas-onion-model-to-front-end-requests-356p)

## Install

Resreq targets modern browsers and [Deno](https://github.com/denoland/deno)

```shell
pnpm install @devtea2026/rerum-quaerat-porro-animi
```

**or**

```typescript
import Resreq from 'https://esm.sh/@devtea2026/rerum-quaerat-porro-animi'
```

If you are using a lower version of [node](https://nodejs.org/en/), you may need to add some polyfill.

```typescript
import fetch, { Headers, Request, Response } from 'node-fetch'
import AbortController from 'abort-controller'

globalThis.fetch = fetch
globalThis.Headers = Headers
globalThis.Request = Request
globalThis.Response = Response
globalThis.AbortController = AbortController
```

## Documentation

### Get Started

```typescript
import Resreq from '@devtea2026/rerum-quaerat-porro-animi'

const @devtea2026/rerum-quaerat-porro-animi = new Resreq({
  baseURL: 'https://example.com',
  responseType: 'json'
})

const res = await @devtea2026/rerum-quaerat-porro-animi.request({
  url: '/api/user',
  method: 'GET',
  params: { foo: 'bar' }
})
console.log(res) // Object

const res = await @devtea2026/rerum-quaerat-porro-animi.get('/api/download', {
  responseType: 'blob'
})
console.log(res) // Blob
```

**Cancel request**

```typescript
const @devtea2026/rerum-quaerat-porro-animi = new Resreq()

const abortController = new AbortController()

@devtea2026/rerum-quaerat-porro-animi.get('https://example.com/api', {
    signal: abortController.signal
  })
  .catch((error) => {
    console.log(error) // Abort error
  })

abortController.abort() // request cancel
```

**Use Middlewares**

```typescript
const @devtea2026/rerum-quaerat-porro-animi = new Resreq({
  baseURL: 'https://example.com'
})

// Intercepting responses and requests using middleware
@devtea2026/rerum-quaerat-porro-animi.use((next) => async (req) => {
  try {
    console.log(req) // Request can be changed here
    const res = await next(req)
    console.log(res) // Response can be changed here
    return res
  } catch (error) {
    console.log(error) // Catch errors here
    throw error
  }
})

const res = await @devtea2026/rerum-quaerat-porro-animi.get('/api', {
  params: { foo: 'bar' }
})

console.log(res)
```

### API

**new Resreq(options?:Options)**

Create a @devtea2026/rerum-quaerat-porro-animi instance and configure the global options.

```typescript
const @devtea2026/rerum-quaerat-porro-animi = new Resreq({
  baseURL: 'https://example.com',
  timeout: 10000,
  responseType: 'json',
  throwHttpError: true,
  onDownloadProgress(progress, chunk) {
    console.log(progress, chunk)
  }
})
```

**@devtea2026/rerum-quaerat-porro-animi.request(options?:Options)**

Use ''request'' to send the request and configure the options.

```typescript
const @devtea2026/rerum-quaerat-porro-animi = new Resreq({
  baseURL: 'https://example.com'
})

const res = await @devtea2026/rerum-quaerat-porro-animi.request({
  url: '/api',
  method: 'GET',
  params: { foo: 'bar' },
  throwHttpError: true,
  onDownloadProgress(progress, chunk) {
    console.log(progress, chunk)
  }
})

console.log(res)
```

**@devtea2026/rerum-quaerat-porro-animi\[method\](options?:Options)**

Use ''method'' to send the request and configure the options

```typescript
const @devtea2026/rerum-quaerat-porro-animi = new Resreq({
  baseURL: 'https://example.com'
})

const res = await @devtea2026/rerum-quaerat-porro-animi.get('/api', {
  params: { foo: 'bar' },
  throwHttpError: true,
  onDownloadProgress(progress, chunk) {
    console.log(progress, chunk)
  }
})

console.log(res)
```

**@devtea2026/rerum-quaerat-porro-animi.use(middleware:Middleware)**

Add  headers using middleware.

```typescript
import Resreq, { Middleware } from '@devtea2026/rerum-quaerat-porro-animi'

const @devtea2026/rerum-quaerat-porro-animi = new Resreq({
  baseURL: 'https://example.com'
})

// Implement a middleware that set token
const setAccessToken: Middleware = (next) => (req) => {
  req.headers.set('Access-Token', 'foo-bar')
  return next(req)
}

// Register middleware
@devtea2026/rerum-quaerat-porro-animi.use(setAccessToken)

await @devtea2026/rerum-quaerat-porro-animi.get('/api')
```

Rewriting  headers using middleware.

```typescript
import Resreq, { Req } from '@devtea2026/rerum-quaerat-porro-animi'

const @devtea2026/rerum-quaerat-porro-animi = new Resreq({
  baseURL: 'https://example.com'
})

@devtea2026/rerum-quaerat-porro-animi.use((next) => async (req) => {
  
  // Create a new request with Req
  const _req = new Req(req, {
    headers: {
      'X-Custom-Header': 'bar'
    }
  })
  
  console.log(_req.headers.get('Content-Type')) // null
  console.log(_req.headers.get('X-Custom-Header')) // bar

  return await next(_req)
})

await @devtea2026/rerum-quaerat-porro-animi.get('/api', {
  headers: {
    'Content-Type': 'application/json',
    'X-Custom-Header': 'foo'
  }
})
```

**Req(request: Req, init: ReqInit | Request)**

**Res(response: Res, init: ResInit | Response)**

In the middleware, use `new Req()` and `new Res()` to rewrite the request and response.

```typescript
import Resreq, { Req, Res } from '@devtea2026/rerum-quaerat-porro-animi'

const @devtea2026/rerum-quaerat-porro-animi = new Resreq({
  baseURL: 'https://example.com',
})

@devtea2026/rerum-quaerat-porro-animi.use((next) => async (req) => {
  
  const _req = new Req(req, {
    url: 'https://example.com/mock'
  })

  const res = await next(_req)
  return new Res(res, {
    body: { foo: 'bar' }
    status: 200,
  })
})

const res: Response = await @devtea2026/rerum-quaerat-porro-animi.get('/api')

console.log(res.status) // 200
console.log(await res.json()) // { foo: 'bar' }
```

> **Warning**
> Req & Res extends from [Request](https://developer.mozilla.org/en-US/docs/Web/API/Request/Request) and [Response](https://developer.mozilla.org/en-US/docs/Web/API/Response); to create a new request and response in the middleware, use `new Req()` and `new Res()`.
>
> To access the contents of the `body` in the current middleware, please use `Res.clone()` to ensure that the res received by the next middleware are unlocked.

### Interfaces

**Options**

Options extends from the [RequestInit](https://developer.mozilla.org/en-US/docs/Web/API/Request/Request) type with some additional properties

```typescript
interface Options extends Omit<RequestInit, 'body'> {
  baseURL?: string
  url?: string
  method?: 'GET' | 'POST' | 'PUT' | 'DELETE' | 'HEAD' | 'PATCH'
  params?: Record<string, any>
  body?: BodyInit | Record<string, any>
  meta?: Record<string, any>
  timeout?: number
  responseType?: 'json' | 'arrayBuffer' | 'blob' | 'formData' | 'text' | null | false
  throwHttpError?: boolean
  onDownloadProgress?: ProgressCallback
}
```

- **baseURL**: The url prefix of the request will be concatenated with the url in `@devtea2026/rerum-quaerat-porro-animi[method]()` to form a complete request address, the default value is ' '.
- **url**: Request url, the default value is ' '.
- **method**：Request method, the default value is 'GET'.
- **params**: The params of a `@devtea2026/rerum-quaerat-porro-animi.get` request are automatically added to the url via the [new URLSearchParams](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams) method.
- **body**: Based on [BodyInit](https://developer.mozilla.org/en-US/docs/Web/API/Request/Request), and adding `Record<string, any>`, which means you can pass `object` directly, instead of `JSON.stringify(object)`, which will automatically add `Content-Type: application/json` request headers.
- **meta**: The extra information that needs to be carried in the request is not really sent to the server, but it can be obtained in  `Res.meta` and `Req.meta`.
- **timeout**: Specify the number of milliseconds of time before the request, if the time is exceeded the request will be aborted, the default value is 1000ms.
- **responseType**: Set how the response will be parsed, if not set or set to false, the [Response](https://developer.mozilla.org/en-US/docs/Web/API/Response) instance will be returned, the default value is undefined.
- **throwHttpError**: If true, a status code outside of 200-299 will throw an error, the default value is false.
- **onDownloadProgress**: The download progress hook, which depends on the [ReadableStream API](https://developer.mozilla.org/en-US/docs/Web/API/ReadableStream), polyfill is required in lower versions of node.

To avoid adding complexity, `new Resreq(options)` and `@devtea2026/rerum-quaerat-porro-animi[method](options)`, in which 'options' are of the same type.

The options defined in `new Resreq(options)` will take effect globally, `@devtea2026/rerum-quaerat-porro-animi[method](options)`, will override the "global options", except for the following options which will not be overridden.

- **meta**: The meta within the method will be shallow merged with the global meta.

- **headers**: The headers defined in the method are merged into the global headers.

- **onDownloadProgress**: Defining onDownloadProgress in a method and the global onDownloadProgress are both retained.

**ReqInit**

Options extends from the [RequestInit](https://developer.mozilla.org/en-US/docs/Web/API/Request/Request) type with some additional properties.

```typescript
interface ReqInit extends Omit<RequestInit, 'body'> {
  url?: string
  method?: 'GET' | 'POST' | 'PUT' | 'DELETE' | 'HEAD' | 'PATCH'
  meta?: Record<string, any>
  timeout?: number
  responseType?: ResponseType
  throwHttpError?: boolean
  body?: BodyInit | Record<string, any>
  onDownloadProgress?: ProgressCallback
}
```

> **Note**
> That its 'headers' behave differently than 'Options.headers', which overrides the global headers.

**ResInit**

ResInit extends from the [ResponseInit](https://developer.mozilla.org/en-US/docs/Web/API/Response/Response) type with some additional properties.

```typescript
interface ResInit extends ResponseInit {
  meta?: Record<string, any>
  body?: BodyInit | Record<string, any> | null
  timeout?: number
  responseType?: ResponseType
  throwHttpError?: boolean
  onDownloadProgress?: ProgressCallback
}
```

**Middleware**

The middleware must call next(req) to return a promise.

```typescript
type Middleware = (next: Next) => (req: Req) => Promise<Res>
```

### Standing on the shoulders of giants

Some of the inspiration for this project came from their.

- [Axios](https://github.com/axios/axios): Promise based HTTP client for the browser and node.js.
- [Ky](https://github.com/sindresorhus/ky): Tiny & elegant JavaScript HTTP client based on the browser Fetch API.
- [Redux](https://github.com/reduxjs/redux): Predictable state container for JavaScript apps.
- [Koa](https://github.com/koajs/koa): Next generation web framework for node.js.

### License

This project is licensed under the MIT License - see the [LICENSE](https://github.com/devtea2026/rerum-quaerat-porro-animi/blob/master/LICENSE) file for details.
