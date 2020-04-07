# Exclude google anaytics if loading on iframe

Iframe sessions would be counted too. Only sends data to the google analytics if it is not iframe.
Optionally use Tag Manager.

```js
function isIframe() {
  try {
    return window.top !== window.self;
  } catch(e) {
    return false;
  }
}
```
