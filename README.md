# EZProxy
A Proxy for OpenAI API on Cloudflare Workers
Copy The code and implement the API_KEYs
```
addEventListener('fetch', event => {
  event.respondWith(handleRequest(event.request))
});

const OPENAI_API_URL = 'https://api.openai.com/v1/chat/completions';
const NEW_API_KEY = '';
const ORIGINAL_API_KEY = '';

async function handleRequest(request) {
  // Get the Authorization header from the request
  const authHeader = request.headers.get('Authorization');

  // Check if the original API key matches the expected value
  if (authHeader && authHeader === `Bearer ${ORIGINAL_API_KEY}`) {
    // Clone the request and modify its headers
    let modifiedRequest = new Request(request);
    modifiedRequest.headers.set('Authorization', `Bearer ${NEW_API_KEY}`);

    // Forward the request to the OpenAI API
    let response = await fetch(OPENAI_API_URL, modifiedRequest);

    // Return the response from the OpenAI API
    return response;
  } else {
    // If the original API key does not match, return an error message
    return new Response('Invalid API key', { status: 403 });
  }
}
```
