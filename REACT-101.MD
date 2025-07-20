# React

_There are lot of good resources freely available . So, I am not that much bothered with the doc you can just check them out_
_A structured, project-based curriculum with curated learning links and proven design patterns_

## 1 Orientation & Environment

| Goal                                   | Key Links                  |
| -------------------------------------- | -------------------------- |
| Install Node LTS, pnpm / npm           | Node download [1]          |
| Read the official quick-start          | react.dev/learn [2]        |
| Choose an editor (VS Code recommended) | VS Code docs [3]           |
| Create first app (Vite or CRA)         | Vite guide (react.dev) [2] |

## 2 Phase 1 – Core React Fundamentals (Weeks 1-4)

| Project                   | Concepts Mastered                                                            | Top Resources                                                   |
| ------------------------- | ---------------------------------------------------------------------------- | --------------------------------------------------------------- |
| **Interactive TODO List** | JSX, components, props, state, event handling, localStorage                  | Official tutorial “Thinking in React” [4] - MDN React intro [3] |
| **Weather Dashboard**     | Fetch API, async / await, conditional rendering, hooks intro                 | Fetch docs [3] - OpenWeather API guide                          |
| **Memory Card Game**      | Array map, key prop, lifting state up, timers, simple CSS transitions        | React fundamentals course freeCodeCamp video [5]                |
| **Expense Tracker**       | Forms, controlled vs uncontrolled inputs, basic context, charting (Chart.js) | GfG React forms article [6]                                     |

## 3 Phase 2 – Intermediate Development (Weeks 5-8)

| Project                        | Concepts Mastered                                                                      | Deep-Dive Links                                         |
| ------------------------------ | -------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| **Real-time Chat (Socket.IO)** | Node + Express server, WebSockets, useEffect cleanup, custom hooks                     | Socket.IO React guide - LogRocket WebSocket pattern [7] |
| **Movie Explorer**             | React Router, dynamic routes, query parameters, infinite scroll, Intersection Observer | React Router v6 tutorial [8]                            |
| **E-Commerce Cart**            | Complex state, reducers/useReducer, context vs Redux, optimistic UI, Stripe docs       | Redux official tutorial - Stripe React cookbook         |
| **Data Viz Dashboard**         | Charts, memoization, virtualization, responsive layouts                                | Patterns.dev data-heavy patterns [9]                    |

## 4 Phase 3 – Advanced Systems (Weeks 9-12)

| Project                         | Architectural Focus                                                                 | Learning Links                                |
| ------------------------------- | ----------------------------------------------------------------------------------- | --------------------------------------------- |
| **Blog CMS**                    | JWT auth, protected routes, role guards, rich-text editors, SEO, SSR/Next JS basics | Next.js docs - React SEO checklist            |
| **Distributed Micro-Frontends** | Module federation, code splitting, lazy + Suspense, context boundaries              | Micro-frontend guide (Martin Fowler) [10]     |
| **Collaborative Editor**        | Operational Transformation / CRDT concepts, concurrency, performance profiling      | OT pattern article - React Profiler docs [11] |
| **AI Content Assistant**        | OpenAI API usage, rate-limiting hooks, prompt design, ethical considerations        | OpenAI React quick-start                      |

## 5 Phase 4 – Architect & Production (Weeks 13-16)

### 5.1 Design Patterns to Internalize

| Category      | Pattern                       | Study Link                          |
| ------------- | ----------------------------- | ----------------------------------- |
| Composition   | Presentational ⁄ Container    | DEV guide [12]                      |
| Reuse Logic   | Higher-Order Components (HOC) | Patterns.dev HOC pattern [13]       |
| Reuse Logic   | Render Props                  | LogRocket render-props overview [7] |
| Reuse Logic   | **Custom Hooks**              | Hooks pattern [14][15]              |
| Flexible APIs | Compound Components           | Patterns.dev compound pattern [16]  |
| State Flow    | Provider / Context            | Syncfusion pattern guide [17]       |
| State Control | State Reducer                 | Aglowid 2025 list [18]              |
| UI Layout     | Layout Components             | GeeksforGeeks pattern list [19]     |

> Tip: start with one pattern per sprint, refactor an earlier project to use it, and document trade-offs.

### 5.2 Architecture Best-Practices

| Topic                                        | Key Reading                                    |
| -------------------------------------------- | ---------------------------------------------- |
| Folder and domain-driven structure           | GeeksforGeeks architecture best practices [20] |
| Atomic design and component libraries        | MaybeWorks architecture roadmap [21]           |
| Performance (memo, suspense, virtualization) | React.dev performance guide                    |
| Testing (RTL, Jest, Cypress)                 | Testing recipes in official docs [11]          |
| Accessibility by default                     | react.dev accessibility section                |
| Dev Ops: CI/CD, linting, type-safety         | ESLint + Prettier setup docs                   |

## 6 Specialized Tracks

- **Mobile & Desktop:** React Native docs [22]
- **Design Systems:** Storybook & Figma integration guides
- **3-D / Canvas:** React-Three-Fiber tutorials
- **State at Scale:** Redux Toolkit, Recoil, React Query

## 7 Recommended Continuous-Learning Sources

| Type                    | Link                                                 |
| ----------------------- | ---------------------------------------------------- |
| Official Docs           | react.dev (modern hooks-first) [2]                   |
| Patterns & Architecture | patterns.dev book [9]                                |
| Long-form Blogs         | Martin Fowler React articles [10]                    |
| Community Articles      | LogRocket, DEV, Aglowid, Syncfusion [18][23][12][17] |
| Video Series            | Programming with Mosh React 18 course [5]            |
| Interactive Labs        | Codecademy React 101 [24] - React Tutorial App [25]  |



# Comprehensive React & Angular Interview Question Bank

This extensive collection covers **practical UI and architecture patterns** in React and Angular—from fundamentals through advanced mastery, including state management, data flows, performance, micro-frontend integration, UI patterns (infinite scrolling, pagination, virtualization), forms, routing, component design, testing, and more. Obsolete patterns (e.g. mixins, deprecated lifecycle methods, AngularJS) are omitted.

## 1. Core Framework Concepts

1. Explain the virtual DOM in React and change detection in Angular.  
2. Describe React’s reconciliation algorithm and Angular’s zone.js change detection.  
3. How do function components differ from class components in React?  
4. Explain Angular’s NgModule, declarations, imports, providers, and bootstrap.  
5. How do you lazy-load modules in Angular?  
6. What are React Hooks and why were they introduced?  
7. Explain dependency injection in Angular and hierarchical injectors.  
8. How do you create and use custom React Hooks?  
9. Describe ViewEncapsulation modes in Angular.  
10. How do you handle template vs. reactive forms in Angular?

## 2. Component & UI Design Patterns

11. Presentational vs. Container components (React) / Smart vs. Dumb components (Angular).  
12. Higher-Order Components (HOCs) and Render Props in React.  
13. Compound Components pattern in React.  
14. Controlled vs. Uncontrolled components (React forms).  
15. Dynamic component loading with `React.lazy`/`Suspense` and Angular `loadChildren`.  
16. Provider pattern and Context API in React / Service providers in Angular.  
17. Feature modules and Shared modules in Angular.  
18. Portal pattern (`ReactDOM.createPortal`) and Angular CDK portals.  
19. Slot/Content projection with Angular `ng-content` vs. React `props.children`.  
20. Forwarding refs in React (`forwardRef`) vs. `@ViewChild` in Angular.

## 3. State Management & Data Flow

21. Local component state with `useState` (React) vs. component class properties (Angular).  
22. Lifting state up in React vs. shared services in Angular.  
23. Context API vs. Redux / NgRx store for global state.  
24. Immutable state updates with `Immer` or spread operators.  
25. Selector patterns (`reselect` in React, `createSelector` in NgRx).  
26. Effects/middleware (`redux-saga`, `redux-thunk` vs. NgRx Effects).  
27. Reactive forms state (`FormGroup`/`FormControl`) vs. uncontrolled React refs.  
28. Normalized state shape and entity adapters.  
29. Undo/redo patterns via action history.  
30. Optimistic UI updates and rollback strategies.

## 4. UI Patterns

31. Infinite scrolling vs. pagination; implement with Intersection Observer (React/Angular CDK).  
32. Virtualized lists (`react-window`/`react-virtualized` vs. Angular CDK `VirtualScrollViewport`).  
33. Skeleton loading placeholders for data fetching.  
34. Progressive image loading and lazy loading (`img loading="lazy"` vs. `NgOptimizedImage`).  
35. Form wizards (multi-step forms) with React Router or Angular Router child routes.  
36. Debounced search with RxJS `debounceTime` (Angular) vs. `useDebounce` hook (React).  
37. Infinite form fields (dynamic `FieldArray` in React Hook Form / `FormArray` in Angular).  
38. Tree view and nested menus using recursion.  
39. Drag-and-drop via `react-dnd` vs. Angular CDK DragDrop.  
40. Modal and dialog service patterns (`react-modal` vs. Angular Material Dialog service).  
41. Toast/notification service with singleton manager component.  
42. Tabs, accordions, and collapsible panels.  
43. Breadcrumbs component (React Router vs. Angular Router breadcrumbs).  
44. Side-drawer navigation and responsive layout.  
45. Responsive image sets and art direction (`srcset`/`sizes`).  
46. Pagination controls with page number ranges and ellipses.  
47. Infinite canvas and pinch-zoom gestures with React-PanZoom vs. Angular HammerJS.  
48. Accessible UI patterns: ARIA roles, keyboard navigation.  
49. Loading spinners vs. progress bars.  
50. Dark mode toggle and theming via CSS variables or Angular theming.

## 5. Routing & Navigation

51. React Router: nested routes, redirect, dynamic segments.  
52. Angular Router: child routes, guards (`CanActivate`, `CanDeactivate`).  
53. Route preloading strategies in Angular.  
54. Code splitting and route-based bundling in React.  
55. History management: hash vs. browser vs. memory routers.  
56. Parameterized routes and query parameters.  
57. Route resolvers (Angular) vs. data loaders (React Router v6.4+).  
58. Scroll restoration on navigation.  
59. Route transitions and animated page changes.  
60. Handling 404 and wildcard routes.

## 6. Data Fetching & Side Effects

61. Fetching via `fetch` or `axios` in React; `HttpClient` in Angular.  
62. Suspense data fetching with React Query or SWR vs. Angular HTTP interceptors.  
63. Polling and real-time updates via WebSocket or Server-Sent Events.  
64. Cancelable requests with `AbortController` vs. RxJS `takeUntil`.  
65. Data caching and stale-while-revalidate strategies.  
66. Prefetching and preloading data on hover or in background.  
67. Error boundaries (React) vs. ErrorHandler (Angular).  
68. Global loading indicator via interceptor or Redux middleware.  
69. Optimistic UI with mutation queues.  
70. Blob and file upload/download handling.

## 7. Styling & Theming

71. CSS Modules and styled-components in React vs. SCSS, CSS custom properties in Angular.  
72. CSS-in-JS (`styled-components`, `emotion`) vs. Angular component styles.  
73. Theming via context (React) vs. theming service (Angular Material).  
74. Utility-first CSS (Tailwind) integration.  
75. Responsive design with media queries and Flexbox/Grid.  
76. CSS variables for dynamic theming.  
77. BEM and naming conventions vs. Angular ViewEncapsulation.  
78. Dark mode strategy with `prefers-color-scheme` or JS toggles.  
79. Inline styles vs. external stylesheets trade-offs.  
80. SVG inlining and icon sprite patterns.

## 8. Performance Optimization

81. Memoization with `React.memo` and `useMemo` vs. `OnPush` change detection in Angular.  
82. Avoiding unnecessary re-renders with `shouldComponentUpdate` or `PureComponent`.  
83. Lazy loading images and code.  
84. Intersection Observer for infinite scroll and visibility detection.  
85. Chunking large computations with `requestIdleCallback` or Web Workers.  
86. Pre-compiling templates (Angular AOT) vs. React production builds.  
87. Bundle analysis (`webpack-bundle-analyzer`, `source-map-explorer`).  
88. Service Worker and PWA caching strategies.  
89. SSR hydration performance: Next.js vs. Angular Universal.  
90. Client-side caching (IndexedDB via Dexie) and offline support.

## 9. Testing & QA

91. Unit tests with Jest/React Testing Library vs. Jasmine/Karma/Angular Testing Library.  
92. Component snapshot testing vs. fixture-based tests.  
93. End-to-end tests with Cypress or Playwright.  
94. Mocking network requests with MSW (Mock Service Worker) vs. HttpTestingController.  
95. Accessibility testing with `axe-core/react` vs. `@angular/cdk/a11y`.  
96. Visual regression testing with Chromatic or Percy.  
97. Test coverage thresholds and enforcement.  
98. Integration tests for Redux/Ngrx stores.  
99. Testing effects/side-effects with `redux-saga-test-plan` vs. `NgRx Testing`.  
100. Performance testing with Lighthouse CI.

## 10. Micro-Frontends & Integration

101. Micro-frontend approaches: module federation (Webpack 5) vs. single-spa.  
102. Shared dependencies and version alignment.  
103. CSS isolation and scoping strategies.  
104. Communication between micro-frontends via custom events or state stores.  
105. Bootstrapping multiple SPA frameworks concurrently.  
106. Deploying fragments via CDN and dynamic loading.  
107. Single Sign-On integration across hosts.  
108. Cross-micro app routing and navigation.  
109. Shared authentication and user context.  
110. Error isolation and fallback UIs.

## 11. Accessibility & Internationalization

111. Keyboard navigation and focus management.  
112. ARIA roles, states, and properties for custom components.  
113. Announcing changes via `aria-live` regions.  
114. Color contrast auditing and theming.  
115. Internationalization with `react-intl` or `ngx-translate`.  
116. Right-to-left (RTL) support in layouts.  
117. Locale-aware date/number formatting.  
118. Dynamic language switching at runtime.  
119. Handling pluralization and ICU message syntax.  
120. Testing accessibility with `jest-axe` or `axe-puppeteer`.

## 12. Security & Best Practices

121. Prevent XSS via safe interpolation and sanitization (`DOMPurify`, Angular’s built-in sanitization).  
122. CSRF protection for REST APIs.  
123. Secure local storage usage and encryption.  
124. Handling JWT securely (httpOnly cookies vs. storage).  
125. OAuth2 flows in SPA (PKCE).  
126. Content Security Policy configuration.  
127. Dependency vulnerability scanning (`npm audit`, `ngx-audit`).  
128. Linting rules for code quality (ESLint, TSLint migrations).  
129. Secure coding guidelines for user input handling.  
130. CSP reporting and violation monitoring.

## 13. Advanced Topics

131. Server-side rendering with Next.js vs. Angular Universal.  
132. Static site generation vs. full SPA.  
133. Incremental static regeneration (ISR) in Next.js.  
134. Data fetching strategies: SSG, SSR, CSR, ISR.  
135. WebSockets and real-time updates with `socket.io` or RxJS WebSocket.  
136. GraphQL integration with Apollo Client vs. `apollo-angular`.  
137. Optimistic UI and cache updates in GraphQL.  
138. Revalidation and stale-while-revalidate in SWR/React Query.  
139. Microservice API gateway patterns in front-end (BFF).  
140. Edge computing: deploying UI assets at CDN edge workers.


[1] https://dev.to/anuradha9712/react-design-patterns-2acc
[2] https://roshancloudarchitect.me/mastering-design-patterns-in-angular-building-scalable-and-maintainable-web-applications-fc3d5c682942
[3] https://js.devexpress.com/Angular/Demos/WidgetsGallery/Demo/DataGrid/InfiniteScrolling/
[4] https://radixweb.com/blog/react-design-patterns
[5] https://www.geeksforgeeks.org/system-design/design-patterns-in-angular/
[6] https://dev.to/anirbmuk/angular-infinite-scrolling-using-rxjs-and-ngrx-4p8i
[7] https://reactpatterns.com
[8] https://angular-enterprise.com/en/ngpost/courses/design-patterns/
[9] https://www.pluralsight.com/resources/blog/guides/how-to-implement-infinite-scrolling-with-reactjs
[10] https://refine.dev/blog/react-design-patterns/
[11] https://dev.to/galwhocod3z/design-patterns-angular-34dk
[12] https://dev.to/dipakahirav/day-8-angular-signals-and-infinite-scrolling-5e4f
[13] https://www.patterns.dev/react/
[14] https://blogs.halodoc.io/commonly-used-design-patterns-in-angular/
[15] https://stackoverflow.com/questions/72816124/how-to-implement-infinite-scrolling-with-infinite-data-in-react
[16] https://magicui.design/blog/react-design-patterns
[17] https://material.angular.dev
[18] https://blog.ag-grid.com/implementing-infinite-loading-in-an-angular-store-application/
[19] https://ui-patterns.com/patterns
[20] https://www.youtube.com/watch?v=TwPcU7Zu3nI
[1] https://legacy.reactjs.org
[2] https://react.dev/learn
[3] https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Frameworks_libraries/React_getting_started
[4] https://react.dev
[5] https://www.youtube.com/watch?v=SqcY0GlETPk
[6] https://www.geeksforgeeks.org/reactjs/react/
[7] https://blog.logrocket.com/react-design-patterns/
[8] https://reactrouter.com
[9] https://www.patterns.dev/vanilla/
[10] https://martinfowler.com/articles/modularizing-react-apps.html
[11] https://legacy.reactjs.org/docs/complementary-tools.html
[12] https://dev.to/anuradha9712/react-design-patterns-2acc
[13] https://www.patterns.dev/react/hoc-pattern/
[14] https://javascriptpatterns.vercel.app/patterns/react-patterns/hooks-pattern
[15] https://www.patterns.dev/react/hooks-pattern/
[16] https://www.patterns.dev/react/compound-pattern/
[17] https://www.syncfusion.com/blogs/post/react-design-patterns
[18] https://aglowiditsolutions.com/blog/react-design-patterns/
[19] https://www.geeksforgeeks.org/reactjs/react-design-patterns/
[20] https://www.geeksforgeeks.org/reactjs/react-architecture-pattern-and-best-practices/
[21] https://maybe.works/blogs/react-architecture
[22] https://reactnative.dev/docs/getting-started
[23] https://dev.to/fpaghar/react-component-design-patterns-part-1-5f0g
[24] https://www.codecademy.com/learn/react-101
[25] https://react-tutorial.app
[26] https://www.w3schools.com/REACT/DEFAULT.ASP
[27] https://forum.freecodecamp.org/t/react-new-beta-documentation/560313
[28] https://legacy.reactjs.org/docs/react-component.html
[29] https://legacy.reactjs.org/blog/2015/07/03/react-v0.14-beta-1.html
[30] https://react-bootstrap.netlify.app
[31] https://www.reddit.com/r/reactjs/comments/10pflby/beta_reactjs_documentation_is_actually_quite_good/
[32] https://www.youtube.com/playlist?list=PL0Zuz27SZ-6PrE9srvEn8nbhOOyxnWXfp
[33] https://codesandbox.io/s/react-beta-docs-d8ddg
[34] https://www.angularminds.com/blog/react-architecture-patterns-in-reactjs-apps
[35] https://namastedev.com/blog/react-design-patterns-for-real-projects-6/
[36] https://www.fullstack.com/labs/resources/blog/production-level-patterns-for-react-hooks
[37] https://www.designrush.com/best-designs/websites/trends/react-design-patterns
[38] https://dev.to/kurmivivek295/hooks-pattern-in-react-1bfn
[39] https://www.uxpin.com/studio/blog/react-design-patterns/
