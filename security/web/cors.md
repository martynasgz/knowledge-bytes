# CORS

**Cross-Origin Resource Sharing** - relevant when two different origins (different URLs) are trying to share their resources.

From [MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS): Cross-Origin Resource Sharing (CORS) is an HTTP-header based mechanism that allows a server to indicate any origins (domain, scheme, or port) other than its own from which a browser should permit loading resources.

## Example on What It Prevents Against

Imagine you log into your bank and after authorization your browser receives a token. The token is stored in local or session storage, unencrypted, within the browser.

All of the tabs in your browser session have access to local storage, including the token that let's you make additional API calls to the bank website.

Knowing this, I create a chrome extension or something with an embedded API call to steal your money. I programmed it so that it detects when you're on your bank website and waits for that token to appear in local storage and then uses the token to make my malicious API call.

But I wasted my time because your bank has CORS protection enabled and my url that's calling the API isn't on the list of allowed domains.

That's why we have CORS.