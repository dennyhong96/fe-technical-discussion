## Tech discussion

### How would you debug a web page? Or locate the faulty component

- Put debugger statements/break points around an area I think is causing the issue, and insect the value of variables
- Use React/Vue dev tools to locate the faulty component, and inspect the state/props
- Look for console errors that could uniquely identify the error. lookup on google/stack-overflow when I've never seen that type of error before

### What happens when you type URL into the browser end hit enter

- Browsers look-up DNS (Domain Name Server) to translate the URL into IP address
- First lookup DNS cache in the browser
- Then lookup operating system cache
- Then lookup the ISP - Internet Service Provider
- Establish a TCP 3-way handshake between client and server
- Syn packet-> Synchronize
- Syn-ack packet -> Synchronize acknowledged
- Act packet -> Acknowledged
- Both sides now know the other side exists and try to communicate with each other with HTTP requests
- Client sends a HTTP request to server (HTTP request is sent over the connection established by the TCP protocol)
- Server evaluates the path and query params of the request, and sends a HTTP response back to client with the the resource that was requested for
- Client renders the HTML, while also requesting for any additional resources it needs for the render - CSS, JavaScript, Images
- Each subsequent request completes a full request-response cycle
- Once the page is loaded, client sends any other async requests as needed

### Explain what the common HTTP methods are, how they differ, and what they are used for.

- GET, for retrieving a resources
- POST, for creating a resource
- PUT, for replacing a resource
- PATCH, for updating part of a resource
- DELETE, for deleteing a resource
- OPTIONS, ask server what are the options for a target resource. This is being sent implicitly by modern browsers.

### CSS box model

- Critical part of CSS rendering model
- 4 aspects of the box model
  - Content
  - Padding
  - Border
  - Margin
- Analogy of a person going on a winter walk, the content is the person themselves, inside a coat. The padding is the stuffing of the coat, the more stuffing, the more space the person takes up. The border is the material of the coat, it could have different thickness and color, styles. The margin is the person's personal space, its good to have 6 feet of personal space around us.

### This Keyword

- `this` is a reference to the context object when a function is called;
- default `this` context object is `window`;
- arrow function `->` doesn't have its own this context, it takes the this from outer scope
- Can use func.call, func.bind, func.apply to explicitly set the this context for a function call

### Inheritance

- To inherit obj1 from obj2 , you can link obj1 to obj2 object with Object.create(), you can also manually link them by making obj1's `__proto__` to point to obj2
- JS uses prototype inheritance. Each object has a `__proto__` link. If we access any property of an object, the JS engine first checks if the object itself has it, if not it will keep traverse up the `__proto__` chain and try to find the property until it hits Object's `__proto__` which is null. In the case it did not find it, it returns undefined.

### Javascript closures

- A closure is a function that remembers its outer variables and can access them.
- The inner Lexical Environment has a reference to the outer one.
- Those variables in the outer lexical scope are put into a [[Environment]] property on the function
- All functions are naturally closures in JavaScript

### Hoisting

- function, and var variable declaration (NOT assignments) are moved to the top of the current scope
- let and const are hoisted too. But they are in the temporary dead zone (ReferenceError - The block of code is aware of the variable, but it cannot be used until it has been declared)

### Asynchronous Javascript (Event Loop)

- When a JavaScript program is running, the engine maintians a call stack that contians a stack of execution contexts. It also maintains a `microtask queue` for queuing for promise and async/await tasks ---  `callback queue` is for queuing setTimeout/setInterval tasks
- The event loops is constantly ticking, for each tick of the event loop, if all of the function calls are finished and popped off the callstack and the call stack is empty. It prioritizes the microtask queue and dequeues a task from `microtask queue` then pushes it onto the callstack, if `microtask queue` is empty, it dequeues a task from `callback queue` and pushes it onto the callstack.
- Microtask Queue queues promise callbacks, and MutationObserver callbacks. (Priority)
- All other callbacks, timers, event listeners, etc.
- callback queue can go into starvation mode if microtasks keep creating new microtasks.

### How does pagination typically function for the frontend?

- Typically we manage two pieces of state, the pageNumber, and pageSize
- When we send a get request over to the API endpoint, we append the current pageNumber and pageSize as query string to the API url
- API returns us data, along with if there are more pages, we can then use those information to update our state to prepare for subsequent requests.

### MVC v.s. MVVM

- Model View ViewModel
  - The ViewModel is a facilitator that sits between the View and the Model. A feedback loop is created when the View triggers a UI event to the ViewModel. The ViewModel will base on the event then read or modify the Model and update the local store. When data in the store changes, it will trigger the two-way binding between ViewModel and View so the View reflects the updated data. (Vue.js)
- Model View Controller
  - A Controller facilitates View and Model. The View triggers a UI event that calls a Controller method to manipulate with the Model, and then, based on the result, the Model will call a certain method on the View to render the result.

### Real-time Methods

- WebSockets
  - Bidirectional between client and server
  - No headers, less overhead
  - Does not support HTTP2 protocol
  - Introduces complexity across the stack
- SSE
  - Server push events to the client, client subsribes to event
  - No headers, less overhead
  - Supports HTTP2 protocol
  - Not as complex as WebSocket
- Long-Polling
  - Send a GET request to server for every interval
  - Large overhead, each request has full header, tokens, etc.
  - Longer latency compared to alternatives
  - Easy to implement

### API Protocols

- REST
  - Not as agile as GraphQL
  - Client can't select which fields to return, unless define extra query params or extra endpoints
  - Scalable, can take advantage of HTTP caching
- GraphQL
  - Agile, client can select the fields it need
  - Good for multi-level nested data
  - Can't take advantage of HTTP caching, both query and mutation is done with POST method

### How was your CI/CD pipeline configured in your most recent position?

- Pipeline triggered when merging a PR to master
- Initialize Pipeline
- Pre-Deployment tasks
- Run unit tests
- Source code analysis - Shiftleft
- Create staging build
- Staging deployment
- Staging post deployment
- Purge Caches
- Create production build
- production deployment
- production post deployment
- Purge Caches

### Gitflow

- We branch off of dev branch to create individual feature branches
- We open a pull request back to dev branch when we finish the feature
- When we accumulate a set of new features, tech lead promote merge dev branch to qa branch, qa engineer works off qa branch
- When new features go through qa, tech lead promote qa branch master, which automatically deploys the code to staging environment first
- Product owner does UAT on staging environment
- On release date, tech lead deploys the code to production manually from mater branch
- When there are hot fixes need to be done, I branch off from master to create a hot fix branch, and open a pull request back to master when I finish the fix
- After that, tech lead will merge the hot fix down to lower environments

### Caching

- [Cache-Control](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control):header can be used to control caching behaviors of the browser for a given resource.
- [Etag](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/ETag): a header that acts like a version id, client can set etag to with request, and server can compare determine if the resource has newer version, if so returns new version, otherwise returns a 304 Not Modified with no body.

### Front-end performance concepts

- Network performance
  - Optimize assets to load them faster
    - Request Header - accept-encoding: gzip, deflate, br tells server what type of encoding it accepts
    - GZIP
      - Fast and good at compressing
      - 50% smaller
    - [Brotli](https://wp-rocket.me/blog/brotli-vs-gzip-compression/)
      - for modern browsers that support it
      - even faster than GZIP
      - 70% smaller
      - designed to compress streams data, no need to compress whole static files ahead of time
  - Images
    - Webp format for browsers that support it, fallback to png for high detail images, JPEG for photos
    - Use SVG for geometry shapes
    - Use video for animations instead of gifs
    - Use font for logos
    - Image optimizaion service
      - Send viewport size along with the image url as query string
      - https://company-img-123.png?viewport=300x300
      - The service returns optimized image of appropriate size
    - Cache and serve the images from CDN (viewport size doesn't change that often) - Content lives closer to user
    - Lazy load images "below the fold" with intersection observer
    - Show image placeholders while loading (help reduce customer's perceived loading time)
  - HTTP2 Protocol
    - Multiple HTTP connection calls (HTTP/1 supports only 6);
    - A server can push an event to a client;
    - Compress headers;
    - More secure
    - Cons
      - Server push can be abused;
      - Can be slower if LB (Load Balancer) supports HTTP/1 and server HTTP/2
    - Enables Multiplexing
      - We can featch hundreds of resources in parallel under one connection
      - Enables use to be more granular in bundle(code) splitting
        - route level bundle
        - runtime bundle
        - vendors bundle
        - analytics bundle
        - ES6 Bundle
          - For modern browsers, so we don't need to ship unnecessary polyfills
- Rendering performance

  - Critical Rendering Path — steps browser makes to paint the page. The steps are:
    - DOM — browser compiles the Document Object Model;
    - CSSOM — browser compiles the CSS Object Model;
    - Render Tree — browser combines DOM and CSSOM to render the tree;
    - Layout — browser computes the size and position of each of the objects;
    - Paint — browser converts the tree into the pixels in the screen;
    - Optimize CRP:
      - Optimize the order of sources — load critical resources as soon as possible;
      - Minimize the number of sources — reduce the number, load async;
  - Reflow — Browser recalculates the position and geometries of the elements after rerender.
    - Optimize Reflow:
      - reduce DOM depths;
      - avoid long CSS selectors, minimize rules;
  - CSS is (render blocking)
    - Keep CSS lean
    - Use BEM to avoid deeply nested selectors
    - Inline critical styles to eliminate flash of unstules content
    - Use CSS transform based animation instead of JavaScript, prevent triggering browser reflow
    - Re-Paint: (draw pixels: color, shadows; layout changes trigger repaint), use will-change to optimize layout repaint
  - Scripts
    - Inline critical scripts to eliminate fetching additional resource
    - Load scripts with async/defer attributes to prevent render blocking
  - DOM performance (Avoid having too many DOM nodes)
    - Pagination - for data tables
    - List virtualization with Infinite scrol for multi-media feed
      - Use constant number of nodes for list elements
      - Replace data on the list elements when we need to display need data
  - First contentful paint (FCP)
    - Use SSR technology to render intial data on the server, and send complete HTML to client
    - Then the client can rehydrate the state and take over.
  - Cumulative layout shift (CLS)
    - Pre-define image height and width to prevent layout shifts when image is loaded
  - link rel relationship resource hints
    - link rel="preload" — load high priority resources faster
    - link rel="preconnect" — Accerlerate establishing the connection to a 3rd party origin
    - link rel="prefetch" — loads low prior resources and cache
    - link rel="dns-prefetch" — accerlerate dns lookup of a 3rd party domain
    - link rel="prerender" — similar to prefetch + caches whole the page

- JavaScript performance
  - prevent operations from blocking UI interactions(single threaded)
    - Use promise/async to run operations that takes time
    - Cache results of heavy computation
      - Web Workers
      - WASM
    - If absolutely need to use JS for animation, use requestAnimatinFrame instead of setTimeout

### SSR v.s. CSR

- SSR --- Use SSR if a site is stable, static, SEO focused
  - pros
    - Faster page load (viewable, but not interactable);
    - Better for search engines (faster indexing);
    - Better with sites that have a lot of static content (blogs);
  - cons
    - More server requests;
    - Slower render to interact;
    - Full page reloads;
- CSR --- Use CSR if site under development, dynamic;
  - pros
    - Faster render after initial load;
    - Best for web app;
  - cons
    - The initial load can require more time

### Security

- [_Access-Control-Allow-Origin_](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Allow-Origin)(Cross-Origin Resource Sharing --- CORS) Defines a list of domains that can access the API of the origin domain;
- [_X-Frame-Options_](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options) --- Prevent clickjacking from iframe; The Content-Security-Policy HTTP header has a "frame-ancestors" directive which obsoletes this header for supporting browsers.
- [_Cross-Site Request Forgery_](https://owasp.org/www-community/attacks/csrf) (CSRF). *Attack*: user has a session (logged in), attacker creates link, user clicks on the link and performs the request, attacker steals user session. *Prevent: *captcha, log out from visited site
- [_Content-Security-Policy_](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP) --- prevent execution of code
- Don't allow the user to input any HTML `innerHtml`
- Use UI frameworks, keep node_modules updated, and limit of usage 3rd party services

### Accessibility

- Semantic HTML structure
  - header, footer, nav, aside, section, h1, h2, h3...
- Contrast ratio - 4.5:1
- Dynamic font-sizes with rem values and CSS clamp
- All images have valid alt text
- Provide proper aria-roles and attributes for custom components
  - aria-haspopup to announce the element can load another layer
  - All inputs elements should have aria-live attribute. So assistive technology can inform user of content change
- Support multiple color schemes
  - For people with different types of color blindness
- Maintain tab order, focus management
- Skip/Escape Link to support teleporting to specific sections of the app
- Hotkeys
  - Users can be really productive using our pp

### Comparing and contrasting JS frameworks

- React.js

  - Focus a lot on immutability, we can't just mutate the state in react, we call a setState function, that's how it knows to trigger a re-render
  - Heavily utilized JavaScript's closure
  - Everything is just pure JavaScript, (JSX compiles to React.createElement), no magic going on
  - You have to choose your tools state management, router.

- Vue.js
  - Focus on reactivity
  - Reactivity is based on getters and setters on Object.defineProperty, and JavaScript proxy, when you mutate the state, the setter is triggered, and it will trigger the subscribers
  - Don't quite like the template syntax is less intuitive vs. JSX, too much magic
  - Comes with a suite of default tools to handle state management and routing, has a plugin system

### Performance metrics (Lighthouse)

- [First Contentful Paint](https://web.dev/first-contentful-paint/) - Marks the time the first text or image is painted

- [Speed Index](https://web.dev/speed-index/) - How quickly the contents of a page are visibly populated

- [Largest Contentful Paint](https://web.dev/lcp/) - Marks the time that the main content has likely loaded

- [Time to Interactive](https://web.dev/interactive/) - Amount of time it takes for the page to be fully ineractive

- [Total Blocking Time](https://web.dev/lighthouse-total-blocking-time/) - Time between FCP and TTI

- [Cumulative Layout Shift](https://web.dev/cls/) - Measures movement of visible elements (glitches)

### When don't know

- To be honest, I'm not so sure about that, but I'd be happy to do more research into it.
