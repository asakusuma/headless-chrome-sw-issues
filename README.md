# headless-chrome-sw-issues
When emulating an offline network condition in headless chrome, service worker fetch requests are somehow still succeeding even after clearing the cache. The request is not reaching the server. There seems to be some cache that is not getting cleared. This issue doesn't seem to happen when using a frame buffer. The issue is also intermittent. We discovered this issue while building some service worker tests, instrumented with the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/).

[Screencast video of reproduction](https://www.youtube.com/watch?v=CsPJVHv2vaw)

## Setup

```
python -m SimpleHTTPServer
```

In another terminal
```
chrome-canary --headless --remote-debugging-port=9200
```

## Steps to Reproduce

1. Open `http://localhost:9200` in chrome.
2. Click `about:blank`
3. Type in `http://localhost:8000` in url bar and hit enter
4. Inspect Application > Service Worker and ensure a worker is installed
5. Clear the cache and browser data
6. Go to the network devTools tab and clear all history
7. Focus the url bar and hit enter to navigate again
8. Notice that in the network tab we see a response from the service worker, even though we are offline and we have no cache

## Notes

* Issue is intermittent. There seems to be some sort of race condition.
* Issue happens in both Chrome and Chrome Canary. The only difference is that the `sw.js` request actually succeeds in stable Chrome.
* We also tried hitting the `Disable Cache` checkbox. That also sometimes reproduces the issue.