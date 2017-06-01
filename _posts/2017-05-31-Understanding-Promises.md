---
layout: post
title:  "Understanding Promises"
date:   2017-05-31 23:00:00 -0800
author: Aaron Young
about-author: about
author-pic: profile-sm.jpg
cover-photo:
category: programming
tags: javascript
comments: true
---

In JavaScript, we can pass functions as arguments into another function, and execute or return that passed-in function at a later time. This passed-in function is called a callback function. While callbacks allow us to do asynchronous tasks like event handling and HTTP requests, **it doesn't allow us to handle the results of these tasks synchronously**. In other words, if you run multiple asynchronous tasks, each with their own callback functions, there isn't a way to process the callbacks in a specific order. This becomes a problem when we have dependencies between multiple asynchronous tasks. This is where Promises come in.

## What is a Promise?
A Promise is an object that represents an asynchronous task that has not yet finished, but will finish some time in the future. Once the task is finished, a callback is executed. A task can either be a success or a failure. Here's a simple example of a Promise:

```javascript
let promiseToDoLaundry = new Promise((resolve, reject) => {
  // Some Asynchronous Task
  let isSuccessful = true;
  if (isSuccessful) {
    resolve("Laundry is done!");
  } else {
    reject("Laundry is not done!");
  }
});
```

Notice that `resolve` is the callback for when the task was successful, and `reject` is the callback for when the task failed. We define what `resolve` and `reject` are by passing them to the `then(onFulfilled, onRejected)` function of our Promise. Promises also provide a `catch(onRejected)` method to define the reject callback, and is often used for better readability.

```javascript
promiseToDoLaundry
  .then((fromResolve) => {
    console.log(fromResolve);
  })
  .catch((fromReject) => {
    console.log(fromReject);
  })
```

```
> Laundry is done!
```

If the task was successful, `resolve` is called, and "Laundry is done!" is outputted to the console. If the task failed, `reject` is called, and "Laundry is not done!" is outputted on the console.
Obviously, there is no asynchronous task being done in this example, and I'm using the `isSuccessful` variable to indicate the status of the task to keep this example simple.

## Promises are composable
You probably noticed that the above example can be written simply using callbacks. This shouldn't be a surprise since we are only dealing with one asynchronous task, and we don't care what order the callbacks are being called. But what if you need to make several asynchronous tasks, and then handle the results synchronously? That's where Promises become more powerful than callbacks. Continuing the laundry theme of the first example, suppose you needed to wash clothes, dry clothes, then fold clothes in that order.

```javascript
let washClothes = () => {
  return new Promise((resolve, reject) => {
    resolve("Washed clothes,")
  })
}

let dryClothes = (message) => {
  return new Promise((resolve, reject) => {
    resolve(message + " Dry clothes,")
  })
}
let foldClothes = (message) => {
  return new Promise((resolve, reject) => {
    resolve(message + " Fold clothes!")
  })
}

washClothes()
  .then((result) => {
    return dryClothes(result);
  })
  .then((result) => {
    return foldClothes(result);
  })
  .then((result) => {
    console.log(result);
  })
```
```
> Washed clothes, Dry clothes, Fold clothes!
```

The thing to note here is that the functions being passed in to `then()` for `washClothes` and `dryClothes` both return Promises. In other words, Promises are composable, and `then()` calls can be chained and organized in a very readable, manageable, and synchronous way.

## Other Promise methods
There are several other methods that the Promise object offers. But the two most interesting ones are `Promise.race()` and `Promise.all()`.

Let's say you only want to process the results of the asynchronous tasks once *all* tasks have finished. Then, we can use `Promise.all()` by passing the Promises in an array. The results of all the Promises will be passed into the callback as an array, in the same order as defined in the original array.

```javascript
Promise.all([washClothes(), dryClothes(), foldClothes()])
  .then((results) => {
    // process all three results
    console.log("All Promises fulfilled")
  })
```

Let's say you only care about the fulfillment of *one* of the Promises. Then, we can use `Promise.race()` to process the first Promise that completes.

```javascript
Promise.race([washClothes(), dryClothes(), foldClothes()])
  .then((result) => {
    // process a single result
    console.log("One Promise fulfilled")
  })
```

Note that not all browsers have complete Promise implementations, so keep that in mind before using Promises in your applications.

I hope this helped you understand Promises a little bit more! Writing this post certainly helped me a lot!
