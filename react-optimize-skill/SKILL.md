---
name: react-optimize-skill
description: Use when measuring or improving a React frontend's page load, render-blocking resources, bundle size, React rendering, or JavaScript runtime performance in a browser.
---

# React Frontend Performance Optimization

Measure first, find the bottleneck affecting the real user experience, then make one targeted change and measure again. Do not optimize the Lighthouse score, bundle size, or render count in isolation from the reported problem.

## Workflow

1. Establish a reproducible baseline: affected route and interaction, production or development build, browser/device, viewport, network/CPU conditions, cache state, and user-visible symptom. Run Lighthouse with consistent settings and inspect field Core Web Vitals when available. Save the report and trace for comparison.
2. Classify the bottleneck before changing code:
   - Slow first render or LCP: inspect the critical rendering path, HTML response, render-blocking CSS, parser-blocking scripts, fonts, and the LCP asset's request priority/waterfall.
   - Large initial download or delayed route: inspect per-route and per-chunk JavaScript/CSS, dependencies, and unused code with the framework's bundle analyzer.
   - Slow interaction or janky scrolling: record a browser Performance trace and inspect main-thread long tasks, scripting, layout, paint, and React commits.
   - Slow images, fonts, or other resources: inspect transferred bytes, dimensions, format, cache headers, and request timing in the Network panel.
3. Improve critical rendering only where the trace shows blocking work:
   - The browser parses HTML into the DOM and CSS into the CSSOM, then combines them into a render tree before layout and paint. CSS needed for the first view can delay rendering; synchronous JavaScript can pause HTML parsing because it may inspect or change the DOM/CSSOM. The preload scanner can discover some resources while the main parser is blocked, but it does not remove the underlying dependency.
   - External scripts without `async` or `defer` can block HTML parsing. `async` downloads in parallel and executes as soon as ready, so execution order is not guaranteed and can interrupt parsing. `defer` downloads in parallel and executes after HTML parsing in document order. Module scripts are deferred by default. Choose based on dependencies and execution order; do not move framework-managed scripts or hydration code without understanding the framework.
   - Keep CSS needed for above-the-fold content available early. Remove unused CSS or defer non-critical styles only when the resulting page does not flash unstyled or shift layout. Preload/preconnect only resources proven critical by the request chain; speculative hints can compete with more important resources.
   - Do not rely on a fixed number of concurrent requests for HTTP/1.1 or HTTP/2. Browser scheduling, connection reuse, origin count, and protocol affect the waterfall; measure the actual page.
4. Reduce initial code and resource cost from largest impact to smallest:
   - Start with route-level splits and large client-only dependencies. Use the router/framework's route splitting where available. Lazy-load large UI needed only after an interaction or below the fold, with a useful loading state; keep code needed for the initial viewport on the critical path. Watch for code/data waterfalls and use route prefetching or parallel loading when supported.
   - For React, `lazy` with `Suspense` can defer component code; declare lazy components at module scope. For Next.js, follow the installed router's `next/dynamic` and server/client component guidance. Confirm the change reduces the initial route bundle rather than only moving cost to a moment that harms interaction.
   - Use a production bundle analyzer to find oversized packages and import chains. Tree shaking depends on the bundler, static ESM output, package side-effect metadata, and build mode. Named exports are not inherently smaller than default exports; do not rewrite imports or use deep imports unless the package supports them and the output confirms a benefit. Preserve side-effect imports such as CSS and polyfills.
   - Ensure production builds minify assets. Configure the server/CDN to negotiate text compression such as Brotli or gzip for JavaScript, CSS, HTML, and other compressible text. Verify the response's `Content-Encoding` and transferred size in DevTools; compression and minification solve different parts of transfer cost.
   - Optimize images with suitable dimensions and modern formats. Avoid lazy-loading the LCP image; lazy-load offscreen images where it reduces contention. Subset fonts and preload only fonts needed for the initial render.
5. For JavaScript runtime and React rendering, use a Performance trace and React DevTools Profiler to identify expensive functions, long tasks, avoidable commits, and layout work. Compare a production build where possible; development instrumentation can distort results. Fix the measured cause. Add `memo`, `useMemo`, or `useCallback` only when profiling shows repeated work and stable inputs make the optimization effective. Move substantial CPU-only work to a Web Worker only when it can run off the main thread without breaking required browser/DOM access.
6. Make one change at a time, then rerun the same measurement conditions. Compare the affected metric, trace, transferred bytes, route chunks, and visual behavior; keep the change only when it improves the target without creating a regression elsewhere.

## Guardrails

- Do not treat the critical rendering path as a checklist to blindly reorder every `<link>` and `<script>`. Preserve dependencies, accessibility, correct hydration, and visual stability.
- Do not lazy-load the current route's critical content or add code splitting without checking for loading waterfalls and interaction delay.
- Do not assume `export function`, named imports, or a library's individual function import guarantees tree shaking. Verify the library format and production bundle.
- Do not apply `memo`/`useMemo` broadly or chase a single noisy Lighthouse score. Use repeated lab runs and, when available, real-user data.
- Do not change CDN compression, caching, third-party scripts, or production headers without checking the deployment owner and existing configuration.

## Tools and references

- Lighthouse in [Chrome DevTools](https://developer.chrome.com/docs/devtools/lighthouse/) for a repeatable page audit.
- Chrome DevTools Network and Performance panels for request waterfalls, resource sizes, main-thread work, and long tasks.
- [React DevTools Profiler](https://react.dev/learn/react-developer-tools) and [React Profiler](https://react.dev/reference/react/Profiler) for component render measurements.
- [Critical rendering path](https://web.dev/learn/performance/understanding-the-critical-path) and [resource loading](https://web.dev/learn/performance/optimize-resource-loading) for parser, render blocking, `async`, `defer`, and preload behavior.
- [React `lazy`](https://react.dev/reference/react/lazy) and [Next.js package bundling](https://nextjs.org/docs/app/guides/package-bundling) for code splitting and bundle analysis.
- [Webpack tree shaking](https://webpack.js.org/guides/tree-shaking) for ESM and side-effect behavior.
- [Text compression](https://web.dev/articles/optimizing-content-efficiency-optimize-encoding-and-transfer) for Brotli/gzip and transfer size.
