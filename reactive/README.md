# Reactive Programming

### Analogy

#### [Spreadsheet](https://stackoverflow.com/a/1033066)

- As spreadsheet cells that use the data in other cells update automatically in the event of the changes in the said cells, 
changes in some data are propagated to other parts of the program.

- Points to consider
  - Just like a cell observing another cell cannot use the data without responding to its changes, maybe using `withLatestFrom` is an anti-pattern?
  - As cells do not dynamically observe new cells, the streams should be set in place from the beginning, and using `Single`s are an anti-pattern.

### Questions

#### Tight coupling

[This page](https://gist.github.com/staltz/868e7e9bc2a7b8c1f754#the-refresh-button) shows how button click events can be transformed to streams that can reactively trigger server API calls like so:

```js
var refreshButton = document.querySelector('.refresh');
var refreshClickStream = Rx.Observable.fromEvent(refreshButton, 'click');

var requestOnRefreshStream = refreshClickStream
  .map(function() {
    var randomOffset = Math.floor(Math.random()*500);
    return 'https://api.github.com/users?since=' + randomOffset;
  });
  
var startupRequestStream = Rx.Observable.just('https://api.github.com/users');

var requestOnRefreshStream = refreshClickStream
  .map(function() {
    var randomOffset = Math.floor(Math.random()*500);
    return 'https://api.github.com/users?since=' + randomOffset;
  });
  
var startupRequestStream = Rx.Observable.just('https://api.github.com/users');

var requestStream = Rx.Observable.merge(
  requestOnRefreshStream, startupRequestStream
);
```

From the viewpoint of dependencies, the `requestStream` is dependent on `requestOnRefreshStream`, which is a mapped `refreshClickStream`.

Abstracting away the request/response stream, it is a repository, which should be oblivious to the user interface layer. But here, we have a repository that is listening to the click events, which is then listened by the user interface.
