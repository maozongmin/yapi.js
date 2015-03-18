# YAPI.js
yet another [Promises/A+](https://promisesaplus.com/) implementation

## Usage
You can use two ways to create a promsie.

### Option 1
Use `YAPI.createPromsie` method. 
```javascript
var promise = YAPI.createPromise(function (resolve, reject) {
  // if promise is fulfilled
  resolve();
  
  // or if promise is rejected
  reject();
});

promise.then(function () {
  // fulfilled callback
}, function () {
  // rejected callback
});
```
Here is an example of a simple XHR2 wrapper written using YAPI.js:
```javascript
var getJSON = function(url) {
  var promise = YAPI.createPromise(function(resolve, reject){
    var client = new XMLHttpRequest();
    client.open("GET", url);
    client.onreadystatechange = handler;
    client.responseType = "json";
    client.setRequestHeader("Accept", "application/json");
    client.send();

    function handler() {
      if (this.readyState === this.DONE) {
        if (this.status === 200) { resolve(this.response); }
        else { reject(this); }
      }
    };
  });

  return promise;
};
```
Check the [example.html](https://github.com/loveky/yapi.js/blob/master/example.html) for a full demo which uses the about XHR2 wrapper.

### Option 2
Manualy create a deferred object and resolve/reject it ondemand.
```javascript
var getUserInfo = function () {
  var deferred = YAPI.defer();
  
  // fetch user information throught AJAX
  deferred.resolve(); // if AJAX request succeed
  // or
  deferred.reject(); // if something wrong
  
  return deferred.promise;
};
```

## Todos
- [ ] refine readme
  - [ ] add usage information
  - [ ] add Promise/A+ logo after pass [Promises/A+ Compliance Test Suite](https://github.com/promises-aplus/promises-tests)
- [ ] provide minimized version
- [ ] provide `YAPI.all` method
- [ ] add unit tests
- [ ] test against [Promises/A+ Compliance Test Suite](https://github.com/promises-aplus/promises-tests)
