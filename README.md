# headless-chrome-sw-issues
Reproduce issues with headless chrome and service workers offline

[Screencast video of reproduction](https://www.youtube.com/watch?v=CsPJVHv2vaw)

```
python -m SimpleHTTPServer
```

In another terminal
```
chrome-canary --headless --remote-debugging-port=9200
```

1. Open `http://localhost:9200` in chrome.
2. Click `about:blank`
3. Type in `http://localhost:8000` in url bar and hit enter
4. Inspect Application > Service Worker and ensure a worker is installed
5. Clear the cache and browser data
6. Go to the network devTools tab and clear all history
7. Focus the url bar and hit enter to navigate again
8. Notice that in the network tab we see a response from the service worker, even though we are offline and we have no cache

https://www.youtube.com/watch?v=CsPJVHv2vaw