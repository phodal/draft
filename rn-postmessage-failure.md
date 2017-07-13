[onMessage failing when there are JS warnings and errors on the page](https://github.com/facebook/react-native/issues/10865)

> Setting onMessage on a WebView overrides existing values of window.postMessage, 
but a previous value was defined.

```
(function() {
  var originalPostMessage = window.postMessage;

  var patchedPostMessage = function(message, targetOrigin, transfer) { 
    originalPostMessage(message, targetOrigin, transfer);
  };

  patchedPostMessage.toString = function() { 
    return String(Object.hasOwnProperty).replace('hasOwnProperty', 'postMessage'); 
  };

  window.postMessage = patchedPostMessage;
})();
```

ES6

```
const patchPostMessageFunction = () => {
  const originalPostMessage = window.postMessage;

  const patchedPostMessage = (message, targetOrigin, transfer) => {
    originalPostMessage(message, targetOrigin, transfer);
  };

  patchedPostMessage.toString = () => String(Object.hasOwnProperty).replace('hasOwnProperty', 'postMessage');

  window.postMessage = patchedPostMessage;
};

const patchPostMessageJsCode = `(${String(patchPostMessageFunction)})();`;
```

