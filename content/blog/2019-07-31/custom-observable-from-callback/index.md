---
layout: post
title: Custom Observable from Callback Function
tags:
  - rxjs
  - callbacks
author: Dan Smith
date: '2019-07-31'
---

I had a problem come up the other day. We had a custom JavaScript
function for a vendor we were using that had a couple of callbacks in
it. We try to make our application as reactive as possible and normally
I would turn a callback-containing function like this into an observable
using one of the observable creator functions that RxJS provides.
However, with this particular function, the callbacks weren't in the
right place for the observable creators like `from` or `bindCallback`.
So what to do? Write my own observable creator of course!

The function's signature looked something like this:
```javascript
someFunction(params, moreParams, successCallback, errorCallback);
```

As you can see, this isn't the node callback pattern, and the params
that need to be passed in blocked from being able to use `from`. Also
couldn't use `fromEvent` since these aren't really events that you can
bind to like DOM events.

A couple of things to note here, the `successCallback` and `errorCallback`
signatures actually return different arguments. `success` only calls
the callback with one argument, while the `error` provides three.
Additionally, `success` will really only be called once, but `error`
could be called multiple times.

So I took a look at how `bindCallback` was written and basically copied
that, with a few minor adjustments.

Here's the code:
```typescript
import { Observable, Subject } from 'rxjs';
import { canReportError } from 'rxjs/internal/util/canReportError';

export function fromSomeFunction(
  callbackFunc: Function,
  params: any,
  moreParams: any
): (...args: any[]) => Observable<any> {

  return function(this: any): Observable<any> {
    const context = this;
    let subject: Subject<any>;
    return new Observable<any>((subscriber) => {
      if (!subject) {
        subject = new Subject();
        const handler = (...innerArgs: any[]) => {
          subject.next(innerArgs.length <= 1 ? innerArgs[0] : innerArgs);
        };

        try {
          callbackFunc.apply(context, [params, moreParams, handler, handler]);
        } catch (err) {
          if (canReportError(subject)) {
            subject.error(err);
          } else {
            console.warn(err);
          }
        }
      }
      return subject.subscribe(subscriber);
    });
  };
}
```
 
 You'll notice a couple of things about this. First, the function is
 returning another function. This allows us to specify what `this`
 should be in the observable creator. Important if the custom function
 is defined on an object and expects to have that object in scope.
 Second, we're creating a subject, setting up a `handler` function,
 then passing that handler function into the callback function (which
 at this point will be the `someFunction`). So when the 'callback' gets
 called, it'll have the proper context and will effectively call `.next`
 on our subject, passing along the arguments that the callback was called with.
 If there is only one argument provided, then we pass it through directly.
 If multiple arguments are provided, we pass them straight through as
 an array.
 
 So now given the object
 ```javascript
const myObject = {
  someFunction(params, moreParams, successCallback, errorCallback) {
    // do some stuff that relies on `myObject` being the context
  }
}
```
 
 Calling this function is going to look like this:
 ```javascript
const boundCallback = fromSomeFunction(myObject.someFunction, params, moreParams);
boundCallback(myObject).subscribe((args) => {
  // do some stuff with the returned args
})
```

Now every time one of the callbacks is called, we get a nice stream
that we can apply operators to and work with like we're used to with
RxJS.
