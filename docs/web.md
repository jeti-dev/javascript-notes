---
title: Web
layout: default
---

# Web

## Performance

- FCP - first contenful paint: the first stuff the browser draws

- LCP - largest contentful paint: after my images and videos loaded
- - should be under 2,5s, over 4s is not good for SEO
- - compress images, use only a few fonts
- - use CDN
- - server side rendering is faster than doing some stuff first and only after getting the images
- - user rel="preload" to preload assets
- - fetchpriority="high/low"

- FID - first input delay: how much time it takes for the event to run when the user first interacts with the app
- - should be under 100ms, over 300ms is very bad
- - js: reduce execute time - lazy loading, web workers

- CLS - cummulative layout shift: elements on the page shouldn't jump around unexpectedly
- - use width and height for images or srcset (different sized images based on condition)

### Tips

| Technique                                | Description                                                                                | Benefit                                                             |
| ---------------------------------------- | ------------------------------------------------------------------------------------------ | ------------------------------------------------------------------- |
| **Minify CSS, JavaScript, and HTML**     | Remove unnecessary whitespace, comments, and characters from files.                        | Reduces file size, improving load times.                            |
| **Use Gzip or Brotli Compression**       | Compress assets (HTML, CSS, JS) before sending them to the browser.                        | Reduces the amount of data transferred over the network.            |
| **Optimize Images**                      | Use modern formats (e.g., WebP), resize images, and compress them.                         | Reduces image size without losing quality, improving load times.    |
| **Lazy Loading**                         | Load images and other resources only when they’re needed (as they come into the viewport). | Reduces initial load time and improves perceived performance.       |
| **Use a Content Delivery Network (CDN)** | Distribute static assets across multiple geographic locations.                             | Reduces latency and improves load times for global users.           |
| **Reduce HTTP Requests**                 | Combine CSS and JS files, use CSS sprites, and inline small assets.                        | Reduces the number of network requests, speeding up page rendering. |
| **Enable HTTP/2 or HTTP/3**              | Use multiplexing to send multiple requests over a single connection.                       | Reduces connection overhead and improves resource loading.          |
| **Use Efficient Caching**                | Set appropriate `Cache-Control` and `ETag` headers.                                        | Reduces the need to re-download unchanged assets.                   |
| **Preload and Prefetch**                 | Use `<link rel="preload">` and `<link rel="prefetch">` to load critical resources earlier. | Reduces waiting time for key resources.                             |
| **Reduce Render-Blocking Resources**     | Minimize or defer CSS and JS that block page rendering.                                    | Improves First Contentful Paint (FCP).                              |
| **Use Code Splitting**                   | Split JavaScript into smaller chunks and load them on demand.                              | Reduces initial bundle size and speeds up the main thread.          |
| **Eliminate Unused CSS and JavaScript**  | Remove unused styles and scripts from the project.                                         | Reduces file size and improves parsing time.                        |
| **Use Responsive Images**                | Serve different images based on screen size and resolution.                                | Improves performance on mobile and high-density screens.            |
| **Defer Third-Party Scripts**            | Load third-party scripts (like ads and analytics) asynchronously.                          | Prevents them from blocking the main thread.                        |
| **Reduce DOM Size**                      | Minimize the number of DOM elements and avoid deep nesting.                                | Improves browser rendering efficiency.                              |
| **Reduce Time to First Byte (TTFB)**     | Optimize server response time and database queries.                                        | Improves perceived performance and load speed.                      |
| **Use Service Workers**                  | Cache assets and handle network requests in the background.                                | Enables offline support and faster repeat visits.                   |
| **Reduce Redirects**                     | Minimize the number of redirects (e.g., `www → non-www`).                                  | Reduces round trips and improves TTFB.                              |
| **Use Web Workers**                      | Offload heavy computations to background threads.                                          | Prevents blocking the main thread, improving responsiveness.        |

### Repaint, reflow, critical rendering path

| Concept                           | Description                                                                                           | Causes                                                                                                 | Performance Impact                             | Example                                                                                     |
| --------------------------------- | ----------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ | ---------------------------------------------- | ------------------------------------------------------------------------------------------- |
| **Critical Rendering Path (CRP)** | The sequence of steps the browser takes to convert HTML, CSS, and JS into pixels on the screen.       | Parsing HTML, CSS, and JS; Building DOM and CSSOM; Creating Render Tree; Layout; Painting; Compositing | High                                           | - Parsing HTML and CSS<br>- Building render tree<br>- Painting elements                     |
| **Reflow (Layout)**               | Recalculates the size and position of elements in the document due to changes that affect the layout. | Changes to `width`, `height`, `margin`, `padding`, `position`, `font-size`, etc.                       | High (causes full re-render of affected areas) | `js<br>element.style.width = "300px";<br>element.style.margin = "20px";<br>`                |
| **Repaint**                       | Redrawing elements without changing layout (e.g., visual properties like color).                      | Changes to `background-color`, `visibility`, `box-shadow`, etc.                                        | Medium (less expensive than reflow)            | `js<br>element.style.backgroundColor = "red";<br>`                                          |
| **Compositing**                   | Combining separate layers (after painting) into the final visual frame.                               | `transform`, `opacity`, `z-index`                                                                      | Low (GPU-optimized)                            | `js<br>element.style.transform = "translateX(50px)";<br>element.style.opacity = "0.5";<br>` |

#### ✅ **Performance Tips**

- Minimize reflows by batching DOM changes.
- Prefer `transform` and `opacity` over direct layout changes.
- Use `requestAnimationFrame()` for animations.
- Use `classList.add()` instead of modifying CSS properties one by one.
- Avoid setting `innerHTML` repeatedly.
