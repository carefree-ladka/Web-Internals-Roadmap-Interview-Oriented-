# 🌐 Web Internals Roadmap (Interview-Oriented)

*A comprehensive guide from fundamentals to advanced concepts for mastering web internals interviews*

---

## 1. Browser Architecture & Process Model

### Core Concepts

**Multi-process Architecture**
- Browser process (UI, bookmarks, navigation)
- Renderer process (per-tab isolation, parsing, layout, JavaScript execution)
- GPU process (hardware acceleration, compositing)
- Network process (requests, caching, service workers)
- Plugin process (isolated third-party plugins)

**Site Isolation & Security**
- Out-of-process iframes (OOPIF)
- Process-per-site vs process-per-site-instance
- Cross-origin read blocking (CORB)
- Spectre/Meltdown mitigations

**Inter-process Communication**
- Mojo IPC system
- Shared memory regions
- Message passing patterns

### Interview Questions

- Why does Chrome use separate processes for each tab?
- What triggers spawning a new renderer process?
- How does site isolation protect against cross-origin attacks?
- What are the memory vs security tradeoffs in multi-process architecture?

---

## 2. Navigation & Network Stack

### Request Lifecycle

**DNS Resolution**
- Recursive vs iterative queries
- DNS caching (browser, OS, router)
- DNS prefetching strategies
- DNS over HTTPS (DoH)

**Connection Establishment**
- TCP 3-way handshake (SYN, SYN-ACK, ACK)
- TLS 1.2 vs TLS 1.3 handshake
- Session resumption & 0-RTT
- ALPN (Application-Layer Protocol Negotiation)

**HTTP Evolution**
- HTTP/1.1: pipelining, persistent connections
- HTTP/2: multiplexing, header compression (HPACK), server push
- HTTP/3: QUIC, UDP-based, improved head-of-line blocking
- Performance characteristics of each version

### Caching Architecture

**Multi-layer Cache Strategy**
- Memory cache (fastest, volatile)
- Disk cache (persistent, larger capacity)
- Service Worker cache (programmable, offline-first)
- CDN edge cache (geographically distributed)

**Cache Control Mechanisms**
- Cache-Control directives (max-age, no-cache, no-store, immutable)
- ETag validation
- Conditional requests (If-None-Match, If-Modified-Since)
- Stale-while-revalidate patterns

**Resource Prioritization**
- Chrome's network priority model
- Resource hints (preconnect, prefetch, preload, dns-prefetch)
- Critical request chains
- Resource loading order (CSS → fonts → images → deferred scripts)

### Interview Questions

- Explain the complete flow from typing a URL to seeing content
- How does HTTP/2 multiplexing solve head-of-line blocking? What about HTTP/3?
- When would a browser serve from cache vs make a network request?
- What's the difference between preload, prefetch, and preconnect?
- How does connection keep-alive improve performance?

---

## 3. JavaScript Engine (V8 Deep Dive)

### Execution Pipeline

**Parsing & Compilation**
- Scanner → tokens
- Parser → Abstract Syntax Tree (AST)
- Ignition interpreter → bytecode
- TurboFan JIT compiler → optimized machine code
- Lazy parsing and compilation strategies
- Streaming compilation for large scripts

**Optimization Techniques**
- Hidden classes (fast property access)
- Inline caching (monomorphic, polymorphic, megamorphic)
- Speculation and deoptimization
- Escape analysis
- Function inlining

**Memory Management**
- Heap organization (young/old generation)
- Stack vs heap allocation
- Garbage collection algorithms:
  - Minor GC (Scavenger for young generation)
  - Major GC (Mark-sweep-compact for old generation)
  - Incremental marking
  - Concurrent and parallel GC
- Memory leaks: common patterns and detection

### Event Loop Architecture

**Task Queues**
- Call stack
- Macrotask queue (setTimeout, setInterval, I/O)
- Microtask queue (Promises, queueMicrotask, MutationObserver)
- Animation frame callbacks
- Idle callbacks

**Execution Model**
- How promises are scheduled
- Async/await implementation (state machines)
- Job queue processing order
- Event loop phases in Node.js vs browser

### Interview Questions

- Explain the event loop with microtasks and macrotasks
- What are hidden classes and why are they important?
- Why does changing object shape cause performance issues?
- How does garbage collection impact application performance?
- What happens when TurboFan deoptimizes code?
- Order of execution: setTimeout vs Promise.then vs requestAnimationFrame

---

## 4. Rendering Pipeline

### Critical Rendering Path

**DOM Construction**
- HTML parsing (tokenization → tree construction)
- Incremental parsing
- Script blocking behavior
- Defer vs async vs module scripts

**CSSOM Construction**
- CSS parsing and cascade
- Specificity calculation
- Style computation (inherited, computed, used values)
- CSS blocking behavior

**Render Tree Construction**
- Combining DOM + CSSOM
- Excluding non-visual elements (display: none, <script>, <meta>)
- Creating render objects

**Layout (Reflow)**
- Box model calculations
- Flow vs flexbox vs grid layout algorithms
- Containing block concept
- Layout thrashing and forced synchronous layout
- Layout scope (global vs incremental)

**Paint**
- Paint order (background → borders → content → outlines)
- Paint invalidation
- Paint areas vs paint records
- Dirty rectangles

**Compositing**
- Layer promotion criteria (will-change, transforms, fixed positioning)
- Main thread vs compositor thread
- Hardware acceleration (GPU)
- Transform and opacity optimizations
- Layer squashing

### Performance Optimization

**Reflow vs Repaint**
- Properties that trigger reflow (width, height, top, left)
- Properties that trigger repaint only (color, background, visibility)
- Properties that trigger composite only (transform, opacity)

**Jank Prevention**
- 60fps target (16.67ms per frame)
- Long tasks (>50ms)
- Input latency
- Layout shift avoidance

### Interview Questions

- Explain the complete rendering pipeline from HTML to pixels
- What's the difference between layout, paint, and composite?
- Why is `transform` more performant than `top/left` for animations?
- What causes layout thrashing and how do you avoid it?
- When does the browser create a new layer?
- Why is reading `offsetHeight` after setting `width` expensive?

---

## 5. Critical Rendering Path Optimization

### Render Blocking Analysis

**CSS Blocking**
- Why CSS blocks rendering
- Media queries and conditional loading
- Critical CSS extraction
- CSS containment

**JavaScript Blocking**
- Parser blocking scripts
- `defer` attribute: execution after parsing, maintains order
- `async` attribute: execution when ready, no order guarantee
- `type="module"`: deferred by default, dependency resolution

### Resource Loading Strategies

**Resource Hints**
- `dns-prefetch`: resolve domain early
- `preconnect`: establish connection (DNS + TCP + TLS)
- `prefetch`: low-priority fetch for future navigation
- `preload`: high-priority fetch for current page (as="style|script|font")
- `modulepreload`: preload ES modules with dependencies

**Code Splitting**
- Route-based splitting
- Component-based lazy loading
- Dynamic imports
- Webpack chunks
- Tree-shaking dead code

**Image Optimization**
- Modern formats (WebP, AVIF, JPEG XL)
- Responsive images (srcset, sizes, picture element)
- Lazy loading (loading="lazy")
- Placeholder strategies (blur-up, LQIP)
- CDN transformations

### Core Web Vitals

**LCP (Largest Contentful Paint)**
- Target: <2.5s
- Optimize: server response, resource load time, client rendering

**FID/INP (First Input Delay / Interaction to Next Paint)**
- Target: FID <100ms, INP <200ms
- Optimize: reduce JS execution, break up long tasks

**CLS (Cumulative Layout Shift)**
- Target: <0.1
- Optimize: size attributes, font loading, dynamic content

### Interview Questions

- Why is CSS render-blocking and how can you optimize it?
- When should you use defer vs async vs module?
- Explain the difference between preload and prefetch
- How do you identify and fix high TTI (Time to Interactive)?
- What strategies reduce LCP in a React application?

---

## 6. Web Storage & State Management

### Storage Mechanisms

**Cookies**
- Size limit: 4KB per cookie
- Attributes: Domain, Path, Expires, Max-Age, Secure, HttpOnly, SameSite
- Sent with every request (performance impact)
- Use cases: session management, authentication

**LocalStorage**
- Size: ~5-10MB
- Synchronous API (blocking)
- Persists indefinitely
- Same-origin only
- Not accessible from workers

**SessionStorage**
- Size: ~5-10MB
- Tab-scoped
- Cleared on tab close
- Synchronous API

**IndexedDB**
- Size: unlimited (quota-based)
- Asynchronous (Promise-based)
- NoSQL key-value store
- Supports indexes, transactions
- Accessible from workers
- Best for large datasets

**Cache API**
- Part of Service Worker API
- Request/Response pairs
- Programmatic control
- Used for offline strategies

### Storage Quotas & Eviction

- Best effort vs persistent storage
- Quota calculation (% of disk space)
- LRU eviction policies
- Storage estimation API

### Interview Questions

- Why is LocalStorage not recommended for production apps?
- When should you use IndexedDB vs LocalStorage?
- What's the difference between Cache API and browser cache?
- Explain cookie security attributes (HttpOnly, Secure, SameSite)
- How do storage quotas work across different origins?

---

## 7. Web Security Fundamentals

### Same-Origin Policy (SOP)

**Origin Definition**
- Protocol + Domain + Port
- Why SOP exists (isolation of trust boundaries)
- What SOP restricts: DOM access, cookies, storage, network requests

### Cross-Origin Resource Sharing (CORS)

**Request Types**
- Simple requests (GET, POST, HEAD with simple headers)
- Preflight requests (OPTIONS) for complex requests
- Credentialed requests

**Headers**
- Request: Origin, Access-Control-Request-Method/Headers
- Response: Access-Control-Allow-Origin/Methods/Headers/Credentials

**Common Scenarios**
- API calls from different domains
- Font loading
- Canvas image manipulation

### Cross-Site Scripting (XSS)

**Types**
- Reflected XSS (URL-based)
- Stored XSS (database-persisted)
- DOM-based XSS (client-side)

**Prevention**
- Input validation and sanitization
- Output encoding
- Content Security Policy (CSP)
- Trusted Types API
- HTTPOnly cookies

### Content Security Policy (CSP)

**Directives**
- `default-src`, `script-src`, `style-src`, `img-src`
- `nonce` and `hash` for inline scripts
- `unsafe-inline`, `unsafe-eval` (avoid)
- Report-only mode for testing

### Cross-Site Request Forgery (CSRF)

**Attack Vector**
- Leveraging authenticated sessions
- State-changing requests from malicious sites

**Prevention**
- CSRF tokens (synchronizer tokens)
- SameSite cookies
- Origin/Referer header validation
- Double-submit cookies

### HTTPS & Transport Security

**TLS/SSL**
- Encryption in transit
- Certificate validation
- Perfect Forward Secrecy (PFS)

**HSTS (HTTP Strict Transport Security)**
- Force HTTPS for domain
- Preload lists
- Prevents downgrade attacks

### Interview Questions

- Explain Same-Origin Policy and why it exists
- When does a CORS preflight request occur?
- How does CSP mitigate XSS attacks?
- What's the difference between XSS and CSRF?
- Why should cookies be HttpOnly and SameSite?
- How does HSTS prevent man-in-the-middle attacks?

---

## 8. Modern Web APIs

### Web Workers

**Types**
- Dedicated Workers (one-to-one with page)
- Shared Workers (multiple pages)
- Service Workers (network proxy, offline)

**Communication**
- postMessage API
- Structured clone algorithm
- Transferable objects (ArrayBuffer, MessagePort)
- SharedArrayBuffer + Atomics

**Use Cases**
- Heavy computations off main thread
- Image processing
- Data parsing
- Background synchronization

### Service Workers & PWAs

**Lifecycle**
- Registration → Installation → Activation → Fetch/Message
- Update mechanism
- Waiting state and skipWaiting()

**Caching Strategies**
- Cache-first (offline-first)
- Network-first (online-first)
- Stale-while-revalidate
- Network-only / Cache-only

**PWA Features**
- Web App Manifest
- Add to home screen
- Background sync
- Push notifications
- Offline functionality

### WebAssembly (WASM)

**Overview**
- Binary instruction format
- Near-native performance
- Language-agnostic (C/C++/Rust compiled to WASM)

**Use Cases**
- Game engines
- Video/image codecs
- Cryptography
- ML inference

**Integration**
- Loading and instantiating modules
- JS ↔ WASM interop
- Linear memory model
- SIMD operations

### Worklets

**Types**
- Paint Worklet (custom rendering)
- Animation Worklet (off-thread animations)
- Audio Worklet (custom audio processing)
- Layout Worklet (experimental custom layouts)

### Interview Questions

- When should you use Web Workers?
- Explain the Service Worker lifecycle
- What's the difference between transferable and structured clone?
- How does offline-first caching work?
- Why do Service Workers require HTTPS?
- What problems does WebAssembly solve?

---

## 9. Browser Scheduling & Performance

### Task Scheduling APIs

**requestAnimationFrame**
- Synchronized with display refresh rate
- ~60fps (16.67ms intervals)
- Use for visual updates

**requestIdleCallback**
- Runs during browser idle time
- Timeout parameter for max wait
- Use for non-critical work

**Scheduler API (proposed)**
- postTask with priorities
- User-blocking, user-visible, background

### Performance Monitoring

**Performance API**
- Navigation Timing
- Resource Timing
- User Timing (mark/measure)
- Paint Timing
- Long Tasks API

**Observer APIs**
- PerformanceObserver
- IntersectionObserver
- MutationObserver
- ResizeObserver

### Long Task Management

**Identification**
- Tasks >50ms
- Total Blocking Time (TBT)
- Input latency impact

**Mitigation**
- Task splitting (yield to main thread)
- Web Workers for heavy computation
- Code splitting and lazy loading
- Debouncing and throttling

### Interview Questions

- When should you use requestAnimationFrame vs setTimeout?
- How does requestIdleCallback help performance?
- What is Total Blocking Time and why does it matter?
- How do you break up long JavaScript tasks?
- Explain the difference between debouncing and throttling

---

## 10. Advanced Performance Optimization

### Measuring Performance

**Network Waterfall Analysis**
- Identify blocking resources
- Analyze request chains
- Find long-running requests
- Detect redundant requests

**Runtime Performance**
- Chrome DevTools Performance panel
- Flame charts and call trees
- Frame timing analysis
- Memory profiling

**Real User Monitoring (RUM)**
- Field data vs lab data
- Core Web Vitals tracking
- Error tracking
- Performance budgets

### Optimization Techniques

**Network**
- HTTP/2 server push
- Early hints (103 status code)
- Resource bundling vs unbundling
- Compression (gzip, brotli)

**Rendering**
- Eliminate layout thrashing
- Virtualization (windowing large lists)
- Content-visibility CSS property
- CSS containment

**JavaScript**
- Code splitting by route
- Tree shaking unused code
- Lazy hydration
- Partial hydration
- Islands architecture

**Images**
- Responsive images
- Art direction with picture element
- Lazy loading with IntersectionObserver
- Progressive image formats
- Image CDN optimizations

### Memory Management

**Memory Leaks**
- Detached DOM nodes
- Event listener accumulation
- Global references
- Closure traps
- Timer/interval cleanup

**Detection**
- Heap snapshots
- Allocation timeline
- Detached node investigation

### Interview Questions

- How do you optimize a slow page given a waterfall chart?
- What causes memory leaks in JavaScript applications?
- Explain layout thrashing and how to prevent it
- How do you measure and improve Core Web Vitals?
- What's the difference between debouncing and throttling?

---

## 11. Modern Web Architecture

### Rendering Patterns

**Client-Side Rendering (CSR)**
- Pros: rich interactivity, SPA experience
- Cons: slower FCP, SEO challenges

**Server-Side Rendering (SSR)**
- Pros: fast FCP, SEO friendly
- Cons: slower TTI, server load

**Static Site Generation (SSG)**
- Pre-rendered at build time
- CDN-friendly
- Limited dynamic content

**Incremental Static Regeneration (ISR)**
- Revalidate static pages periodically
- Balance static + dynamic

**React Server Components (RSC)**
- Server-only components
- Zero bundle size for server components
- Automatic code splitting

### Edge Computing

**Edge Functions**
- Cloudflare Workers
- Vercel Edge Functions
- CDN-based compute

**Benefits**
- Reduced latency (geographically close)
- Dynamic content at edge
- Personalization without origin round-trip

### Micro-frontends

**Approaches**
- Build-time integration
- Server-side integration
- Client-side integration (runtime)
- Module Federation (Webpack 5)

**Challenges**
- Shared dependencies
- Routing coordination
- State management
- Performance overhead

### Interview Questions

- Compare CSR, SSR, and SSG - when to use each?
- What are React Server Components and what problems do they solve?
- How does edge computing improve performance?
- What are the tradeoffs of micro-frontends?

---

## 12. System Design for Frontend

### Real-time Systems

**WebSockets**
- Full-duplex bidirectional communication
- Persistent connection
- Use cases: chat, live updates, gaming

**Server-Sent Events (SSE)**
- Unidirectional (server → client)
- Auto-reconnection
- Event stream protocol
- Use cases: notifications, live feeds

**HTTP/2 Server Push**
- Proactive resource delivery
- Cache awareness issues
- Less common than expected

**Polling Strategies**
- Short polling (repeated requests)
- Long polling (held connections)
- Tradeoffs vs WebSockets

### Large-Scale Features

**Infinite Scroll / Feed**
- Virtualization (react-window, react-virtuoso)
- Cursor-based pagination
- Optimistic updates
- Skeleton screens

**Image Upload & Processing**
- Client-side preview
- Chunked uploads for large files
- Presigned URLs (S3)
- Image optimization pipeline
- Progress tracking

**Notification System**
- Push API and Service Workers
- Permission management
- Notification queue
- Read/unread state sync

**Search & Autocomplete**
- Debouncing user input
- Client-side caching
- Server-side indexing
- Fuzzy matching
- Result ranking

### Scalability Patterns

**CDN Strategy**
- Static asset distribution
- Cache invalidation techniques
- Versioned URLs
- Cache-Control headers

**State Management**
- Client state vs server state
- Optimistic updates
- Cache invalidation
- Real-time synchronization

**Performance at Scale**
- Code splitting strategies
- Lazy loading patterns
- Bundle size budgets
- Third-party script management

### Interview Questions

- Design a real-time chat application
- How would you implement infinite scroll efficiently?
- Design an image upload system for large files
- How would you implement a notification system?
- WebSockets vs Server-Sent Events - when to use each?

---

## 13. Must-Know Interview Questions

### Conceptual Deep Dives

1. **What happens when you type a URL in the browser?**
   - Complete flow: DNS → TCP → TLS → HTTP → Parse → Render → Paint → Composite

2. **Why do browsers use multi-process architecture?**
   - Stability, security, parallelism, site isolation

3. **Explain the event loop and microtask queue**
   - Call stack, macrotasks, microtasks, execution order

4. **What makes CSS render-blocking?**
   - Must construct CSSOM before rendering, blocks paint

5. **How do browsers create and composite layers?**
   - Layer promotion criteria, main thread vs compositor thread

6. **How does HTTP/2 solve head-of-line blocking?**
   - Multiplexing, but TCP-level HOL still exists (fixed in HTTP/3/QUIC)

7. **Why do long JavaScript tasks freeze the UI?**
   - Single-threaded main thread, blocks rendering pipeline

8. **How does the browser decide to repaint a region?**
   - Dirty rectangle tracking, paint invalidation, optimization heuristics

### Debugging & Optimization

9. **Optimize a slow page given a waterfall chart**
   - Identify render-blocking, long requests, request chains, prioritization issues

10. **How does TLS handshake work?**
    - ClientHello → ServerHello → Certificate → Key exchange → Finished

11. **Why are cookies HttpOnly?**
    - Prevent XSS access to session tokens

12. **Explain CORS preflight**
    - OPTIONS request for non-simple requests, checks permissions before actual request

13. **How to secure a web app from XSS?**
    - Input sanitization, output encoding, CSP, Trusted Types, HttpOnly cookies

14. **Prefetch vs preload vs dns-prefetch vs preconnect?**
    - Different priority levels and purposes in resource loading

### Advanced Topics

15. **Difference between reflow, repaint, and composite?**
    - Reflow (layout), repaint (pixels), composite (layers to screen)

16. **Why is object shape stability important in V8?**
    - Hidden classes, inline caching, deoptimization costs

17. **How does garbage collection work and impact performance?**
    - Generational GC, pause times, incremental marking

18. **When should you use Web Workers?**
    - Heavy computation, parsing, crypto - anything that blocks main thread

19. **Explain Service Worker caching strategies**
    - Cache-first, network-first, stale-while-revalidate patterns

20. **How does HTTP/3 improve on HTTP/2?**
    - QUIC protocol, UDP-based, eliminates TCP head-of-line blocking

---

## Study Strategy

### Priority Levels

**Must Know (Interview Essentials)**
- Browser rendering pipeline
- Event loop & JavaScript execution
- Network stack (DNS, TCP, HTTP versions)
- CORS & security basics
- Performance optimization fundamentals

**Should Know (Senior Level)**
- Multi-process architecture
- V8 internals and optimization
- Service Workers & PWAs
- Advanced caching strategies
- System design patterns

**Nice to Know (Staff/Principal)**
- WebAssembly integration
- Edge computing architecture
- Micro-frontend patterns
- Browser scheduling internals
- Advanced security (CSP, Trusted Types)

### Practice Approach

1. **Build mental models** - visualize the complete flow
2. **Trace real scenarios** - walk through actual page loads
3. **Measure everything** - use Chrome DevTools extensively
4. **Explain simply** - can you explain to a non-technical person?
5. **Connect concepts** - how do pieces fit together?

### Resources

- Chrome DevTools documentation
- web.dev (Google's web fundamentals)
- MDN Web Docs
- V8 blog
- Performance testing (WebPageTest, Lighthouse)

---

*Good luck with your interviews! Master the fundamentals, understand the tradeoffs, and always be ready to explain the "why" behind technical decisions.*
