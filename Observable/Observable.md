# Observable

## Advanced asynchronous codes

Note:
We have callback, promise and async/await in Javascript now<br/>
Let's take a look on the evolution of the asynchronous codes in Javascript

---

## callback

```js []
const fs = require("fs");
fs.readFile("input", "utf8", (err, data) => {
  if (err) {
    logError(err);
    return;
  }
  fs.writeFile("output", processData(data), err => {
    if (err) {
      logError(err);
      return;
    }
    console.log("done");
  });
});
```

<span class="fragment">hard to read and reason</span><br/>
<span class="fragment">error handling boilderplate everywhere</span>

--

## callback hell

```js []
getUserData("username", (err, userdata) => {
  if (err) {
    logError(err);
    return;
  }
  const { id } = userdata;
  getUserCart(id, (err, cart) => {
    if (err) {
      logError(err);
      return;
    }
    const { items } = cart;
    checkout(items, (err, order) => {
      if (err) {
        logError(err);
        return;
      }
      const { id: orderId } = order;
      showOrder(orderId);
    });
  });
});
```

Note:
very deep nested functions make the code unreadable<br/>
`id` variable in the order cannot be reused

---

## Promise

```js []
const { promisify } = require("util");
const fs = require("fs");
const readFileAsync = promisify(fs.readFile);
const writeFileAsync = promisify(fs.writeFile);
readFileAsync("input", "utf8")
  .then(data => processData(data))
  .then(processedData =>
    writeFileAsync("output", processedData))
  .then(() => {
    console.log("done");
  })
  .catch(err => logError(err));
```

Note:
Promise with chainable `then` method<br/>
Node.js 8 has `util.promisify` to change the callback API to promise API

--

## Even better with async/await

```js []
const { promisify } = require("util");
const fs = require("fs");
const readFileAsync = promisify(fs.readFile);
const writeFileAsync = promisify(fs.writeFile);
(async () => {
  try {
    const data = await readFileAsync("input", "utf8");
    const processedData = processData(data);
    await writeFileAsync(processedData);
    console.log("done");
  } catch (err) {
    logError(err);
  }
})();
```

Note:
Needs async IIFE (immediately-invoked function expression)

---

### Promise is the answer to all asynchronous codes?<br/>

- It actually brings another issue<!-- .element: class="fragment" -->
- callback can be called multiple times<!-- .element: class="fragment" -->
- which means that callback<br/>can returns <!-- .element: class="fragment" -->**multiple values**
- but Promise can only returns <!-- .element: class="fragment" -->**one value**

--

### Promisify everything?

```js []
window.addEventListener("click", event => {
  console.log(event.clientX, event.clientY);
});
```

You can't promisify event listener (DOM Events) !!!<br/>
<span class="fragment">and ...<br/><br/></span>
<span class="fragment">Connection Stream</span><br/>
<span class="fragment">WebSockets</span><br/>
<span class="fragment">WebRTC</span><br/>
<span class="fragment">Web Workers</span><br/>
<span class="fragment">Animation</span><br/>

Note:
We can't just change the behaviour of existing implementation<br/>
async/await relies on this behaviour<br/>
To solve this, we need another interface to solve the issue

---

## Observable

<ul>
  <li>multiple values over time</li><!-- .element: class="fragment" -->
  <li>allows cancellation</li><!-- .element: class="fragment" -->
  <li>use standard combinators to filter<br/>and map the events in the stream</li><!-- .element: class="fragment" -->
  <li>lazy</li><!-- .element: class="fragment" -->
  <li><a href="https://github.com/tc39/proposal-observable">ES Stage 1 proposal</a></li><!-- .element: class="fragment" -->
</ul>

---

## Promise vs Observable

### Promise

```js []
const promise = new Promise((resolve, reject) => {
  ajaxCall((err, data) => {
    if (err) {
      reject(err);
      return;
    }
    resolve(data);
  });
});
```

--

## Promise vs Observable

### Observable

```js []
const observable = new Observable(observer => {
  const request = ajaxCall((err, data) => {
    if (err) {
      observer.error(err);
      return;
    }
    observer.next(data);
    observer.complete();
  });
  return () => {
    request.abort();
  };
});
```

--

## To execute

### Promise

```js []
promise
  .then(data => {
    console.log(data);
  })
  .catch(err => {
    console.err(data);
  });
```

--

## To execute

### Observable

```js []
const subscription = observable.subscribe({
  next(data) {
    console.log(data);
  },
  error(err) {
    console.err(err);
  },
  complete() {
    console.log("complete");
  }
});
subscription.unsubscribe();
```

---

## Observable implementations

- [RxJS 6](https://github.com/ReactiveX/RxJS)
- [zen-observable](https://github.com/zenparsing/zen-observable)
- [fate-observable](https://github.com/shanewholloway/node-fate-observable)

---

## RxJS 6

- an implementation of <!-- .element: class="fragment" -->[ReactiveX](http://reactivex.io/intro.html)
- Lodash for events<!-- .element: class="fragment" -->
- Reactive programming<!-- .element: class="fragment" -->
- Very similar to Functional<br/>Reactive Programming<!-- .element: class="fragment" -->

Note:
ReactiveX is a library for composing asynchronous and event-based programs by using observable sequences<br/>

--

## Stream programming

![Stream](./Observable/stream.jpg)

--

### Used by

- Angular 2/4/5
- Circle.js
- Microsoft
- Github
- Netflix
- Airbnb
- Trello
- Slack

--

### ReactiveX

<iframe src="http://reactivex.io/languages.html"></iframe><!-- .element: style="width: 100vw; height: 50vh;" -->

[Image](./Observable/ReactiveX.png)

Note:
Implemented in many programming languages: Java, .Net, Scala, Clojure, Swift and many others

--

## npm

[most depended-upon packages in npm](https://www.npmjs.com/browse/depended)

- top 36 most depended-upon packages in npm
- higher ranked than redux, just after webpack
- (as of November 2017)

---

## Transform data

### Promise

```js []
promise
  .then(items => {
    console.log(items);
    return items;
  })
  .then(items => items.filter(item => item.price >= 500))
  .then(filteredItems => {
    console.log(filteredItems);
    return filteredItems;
  })
  .then(filterItems => filterItems.map(item => item.id))
  .then(ids => {
    console.log(ids);
    return ids;
  });
```

--

## Transform data

### RxJS 6

```js
observable
  .do(console.log)
  .filter(item => item.price > 500)
  .do(console.log)
  .map(item => item.id)
  .do(console.log);
```

Note:
Much more cleaner

---

### Autocomplete with lodash, callback, superagent

<div>

```js []
let apiRequest;
const inputCallback = _.debounce(autoComplete, 200);
inputElement.addEventListener('keyup', inputCallback);
function autoComplete(event) => {
  const value = event.target.value.trim();
  if (apiRequest) {
    apiRequest.abort();
  }
  apiRequest = callApi({
    url: `${API_URL}/autocomplete-job?query=${value}`,
  });
  apiRequest
    .catch((err) => {
      return {
        response: []
      };
    })
    .then(({response}) => {
      apiRequest = null;
      console.log(response)
    });
};
```

</div><!-- .element: style="margin: 0 auto; height: 70vh; overflow-y: auto;" -->

--

### Autocomplete with RxJS

```js []
const inputStream$ =
  Observable.fromEvent(inputElement, "keyup");
const autoComplete$ = inputStream$
  .debounceTime(200)
  .map(event => event.target.value.trim())
  .switchMap(value =>
    Observable.ajax({
      url: `${API_URL}/autocomplete-job?query=${value}`
    }).catch(err => {
      return Observable.empty();
    })
  );
autoComplete$.subscribe({
  next({ response }) {
    console.log(response);
  }
});
```

---

## Links

- [Official manual](http://reactivex.io/rxjs/manual/overview.html#introduction)
- [Learn RxJS](https://www.learnrxjs.io/)
- [RxMarbles](http://rxmarbles.com/)

--

## Video

<iframe width="640" height="360" src="https://www.youtube.com/embed/AslncyG8whg" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
