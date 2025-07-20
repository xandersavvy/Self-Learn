# Angular Roadmap

_A thorough, concept-first curriculum that turns a developer with basic JavaScript skills into an enterprise-ready Angular architect_

## Executive Summary

Angular 17 has matured into a platform that rewards **architectural discipline**, **reactive thinking**, and **lean performance techniques**. This roadmap delivers a 16-week, project-driven path—rich with ecosystem guidance, design patterns, and cross-cutting concerns (testing, accessibility, security, SSR, hydration, monorepos, and DevOps). By completing the phases and deep-dive modules, you will be able to

- design **domain-oriented, scalable workspaces** (Nx or standalone)
- master **RxJS + Signals** for fine-grained reactivity
- implement **NgRx state** the right way (facades, entity adapters, feature selectors)
- ship **SSR-hydrated, incrementally hydrated** apps with **Angular 17 control-flow** syntax
- lead teams with **testing pipelines, a11y audits, OWASP-aligned security, and CI/CD**

## Learning Architecture

| Phase                        | Duration    | Capstone Project                    | Core Themes                                                                    |
| ---------------------------- | ----------- | ----------------------------------- | ------------------------------------------------------------------------------ |
| 1. Fundamentals              | Weeks 1-4   | Personal Dashboard SPA              | TypeScript mastery · Stand-alone components · Routing guards · Reactive forms  |
| 2. Core Engineering          | Weeks 5-8   | Modular E-Commerce Catalog          | Dependency-Injection patterns · Signals vs RxJS · Lazy routes · Advanced forms |
| 3. Enterprise Systems        | Weeks 9-12  | SaaS Admin Portal (SSR + Hydration) | NgRx architecture · Feature slicing · Domain-driven libraries · Accessibility  |
| 4. Architecture & Leadership | Weeks 13-16 | Micro-frontends Workspace (Nx)      | Monorepos · Incremental hydration · Observability · DevOps & governance        |

## Phase 1 – Fundamentals (Weeks 1-4)

### 1. TypeScript for Angular

- **Advanced types** (mapped, conditional) and decorators [1]
- _Project drill_: refactor vanilla models into typed interfaces and utility types.

### 2. Stand-Alone Components & Routing

- Lifecycle hooks, change-detection zones, signals for local state [2]
- New **@if / @for** control-flow syntax and deferrable views [3][4].

### 3. Reactive & Template-Driven Forms

- Validators, async validation, control value accessors.
- **Design pattern**: “smart shell + dumb field components”.

### Capstone 1: Personal Dashboard SPA

- Features: user profile, task widget, chart widget (Angular CDK overlay for modals [5]).
- Deliverables: 80% unit-test coverage (Jest), Lighthouse a11y score ≥ 95.

## Phase 2 – Core Engineering Patterns (Weeks 5-8)

### 1. Dependency Injection at Scale

- Provider scope hierarchy, multi-providers, `InjectionToken` best practice [6].
- Pattern: **Module → Feature-Scoped Injector** to avoid singleton bloat.

### 2. Reactive Programming

| Tool        | When to Use                                      | Key Operators/Concepts                             | Sources |
| ----------- | ------------------------------------------------ | -------------------------------------------------- | ------- |
| **Signals** | local synchronous state, template reactivity     | `signal()`, `computed()`, `effect()`               | [2][7]  |
| **RxJS**    | async workflows, side-effects, WebSocket streams | `switchMap`, `takeUntilDestroyed`, `combineLatest` | [8][9]  |

- Bridging both: convert Observable → Signal for view models.

### 3. State Management with NgRx

- Folder convention: `feature/`, `data-access/`, `ui/`.
- **Entity adapters** for collections, **facades** for isolation [10][11][12].
- Effects = orchestration only—no display logic.

### 4. Advanced Forms & Validation Library

- Reactive form “Presenter” components.
- Library spotlight: **ngx-formly** dynamic forms.

### Capstone 2: Modular E-Commerce Catalog

- Architectural goals: feature libraries, lazy loaded routes, SSR-ready.
- Implement **NgRx entity** for products, **Signal-powered** cart side panel.

## Phase 3 – Enterprise Features & System Design (Weeks 9-12)

### 1. Server-Side Rendering, Hydration & Incremental Hydration

- Enable SSR with `@angular/ssr` and **Vite + esbuild** runtime [13].
- Hydration benefits (no flicker, better CWV) and `provideClientHydration()` [14][15].
- Incremental hydration with `withIncrementalHydration()` for above-the-fold @defer sections [16].

### 2. Accessibility & Internationalization

- CDK **FocusMonitor**, LiveAnnouncer, high-contrast media query [17][5][18][19].
- i18n with `$localize`, ICU messages.

### 3. Security & OWASP Alignment

- XSS mitigations: Strict Contextual Escaping, CSP modes [20][21].
- Token handling: HttpOnly cookies vs Authorization headers—trade-offs [21].

### 4. Testing Strategy

| Layer                 | Tooling                                                   | Focus                       |
| --------------------- | --------------------------------------------------------- | --------------------------- | -------- |
| Unit                  | Jest + TestBed                                            | pure functions, services    |
| Component/Integration | Jest DOM + Angular Testing Library (or Cypress Component) | templates, inputs/outputs   |
| E2E                   | Cypress                                                   | user journeys; stub backend | [22][23] |

### Capstone 3: SaaS Admin Portal

- Requirements: CSR & SSR with hydration; role-based NgRx guards.
- Deliverables: 100% Lighthouse a11y, CWE-score < 0.1, CI pipeline with Jest + Cypress.

## Phase 4 – Architecture, Scaling & Leadership (Weeks 13-16)

### 1. Workspace & Monorepo Engineering

- Nx generators, computation-caching, affected commands [24][25][26][27].
- **Domain-Driven Folders** (`product/feature`, `product/data-access`) vs horizontal slices.
- Automated dependency rules via `eslint-plugin-boundaries`.

### 2. Micro-Frontends & Module Federation

- Host/remote config, shared Angular singleton runtime.
- Routing hand-off and cross-app NgRx event bus.

### 3. Observability & Performance

- Core Web Vitals budgets, **Angular DevTools** profiler.
- RxJS tracing, Zone-less change detection (preview).

### 4. Governance & DevOps

- Conventional commits, semantic-release, Renovatebot for deps.
- Secure supply chain: `npm audit`, Snyk, GitHub Dependabot [28].

### Capstone 4: Micro-Frontend Workspace

- Compose three remotes (catalog, checkout, account).
- Incrementally hydrate critical fragments; observability via OpenTelemetry.

## Design Patterns Catalog

| Pattern                             | Purpose                                     | Reference           |
| ----------------------------------- | ------------------------------------------- | ------------------- |
| **Smart / Dumb Components**         | isolate presentation                        | Angular style guide |
| **Facade (NgRx)**                   | shield components from store complexity     | [10][11]            |
| **State Reducer (Component-Store)** | local reactive state without global store   | [12]                |
| **Control-Flow Blocks**             | concise `@if`, `@for`, deferrable templates | [3][4][29]          |
| **Overlay CDK**                     | UI portals & modals with focus management   | [5][30][31]         |
| **Incremental Hydration**           | bundle-size driven hydration                | [16]                |

## Key Ecosystem Libraries

| Area              | Library                                           | Why It Matters                      |
| ----------------- | ------------------------------------------------- | ----------------------------------- |
| Forms             | `@ngx-formly`                                     | dynamic, schema-driven forms        |
| State             | `@ngrx/*`, `@ngrx/entity`                         | predictable state at scale          |
| Component Toolkit | `@angular/cdk`                                    | a11y, overlay, drag-drop primitives |
| Testing           | `@testing-library/angular`, Cypress               | productive DX                       |
| Monorepo          | Nx                                                | generators, caching, boundaries     |
| Observability     | `@ngneat/elf`, `@rx-angular/state`                | fine-grained signals/RxJS state     |
| Security          | `@angular-devkit/build-angular:browser` CSP flags | build-time hardening                |

## Essential Resources

- **Official docs & RFCs** – angular.dev guides on DI, control flow, hydration [6][14][32]
- **NgRx Best Practices Repo** – hands-on patterns [33]
- **Angular Enterprise Architecture eBook** – automated validation rules [34]
- **RxJS Patterns Talks by Deborah Kurata** – practical marble thinking [8]
- **Angular CDK Overlay Tutorials** – advanced UI patterns [5][30]

## Milestone Checklist

| Competency             | Evidence                                               |
| ---------------------- | ------------------------------------------------------ |
| Advanced DI            | Custom multi-provider factory with scoped override     |
| Signals + RxJS Interop | `toSignal()` helper, signal-driven view model          |
| NgRx Entity & Facades  | Normalized product store with selectors                |
| SSR + Hydration        | CLI `ng build --ssr` with CWV improvements documented  |
| Incremental Hydration  | Above-the-fold `@defer` hydrated lazily                |
| Accessibility          | Axe-core zero critical issues                          |
| Testing                | 90% statements; E2E happy-path suite                   |
| Security               | CSP level-2 headers; JWT rotation strategy             |
| DevOps                 | Github Actions CI, Nx affected-build, semantic-release |

## Conclusion

Angular’s latest evolution removes friction and rewards **proactive architectural choices**. By mastering the concepts, projects, and patterns in this roadmap, you will:

1. **Architect** maintainable multi-team workspaces.
2. **Leverage** modern reactivity (Signals + RxJS) for predictable UIs.
3. **Ship** SSR-hydrated, a11y-first applications with confidence.
4. **Lead** engineering culture—testing, security, DevOps—in any Angular organization.


[ANGULAR PROJECTS](./ANGULAR_PROJECTS.MD)

## 1. Framework Fundamentals  
1. Explain the role of NgModule and its metadata: `declarations`, `imports`, `exports`, `providers`, `bootstrap`.  
2. How does Angular’s hierarchical Dependency Injection work?  
3. Describe the component lifecycle hooks: `OnInit`, `OnChanges`, `DoCheck`, `AfterViewInit`, etc.  
4. How does Ivy’s incremental DOM differ from ViewEngine?  
5. What are Angular decorators: `@Component`, `@Directive`, `@Injectable`, `@Pipe`?  
6. Explain Angular CLI project structure and workspace configuration.  
7. How do you configure multiple environments and replace files via `angular.json`?  
8. What is AOT vs. JIT compilation? Benefits and trade-offs.  
9. Explain Angular zones and change detection strategy (`Default` vs. `OnPush`).  
10. How do you disable zone.js for performance (Zone-less Angular)?  

## 2. Components & Templates  
11. How to create structural (`*ngIf`, `*ngFor`) vs. attribute directives.  
12. Explain content projection using `` and multi-slot projection.  
13. How to dynamically create components via `ComponentFactoryResolver` or Ivy’s `ViewContainerRef.createComponent`.  
14. Describe template reference variables (`#ref`) and `@ViewChild`/`@ContentChild`.  
15. How to pass data between components: Input/Output, `EventEmitter`, shared services.  
16. What are template vs. reactive forms? When to use each.  
17. How to implement form validation: built-in validators, custom validators, `AsyncValidator`.  
18. Explain use of `FormArray` for dynamic lists of controls.  
19. How to implement nested reactive forms and patch values.  
20. How to use Angular Material or NG-Bootstrap to build UI components.  

## 3. Routing & Navigation  
21. Configure routes with `RouterModule.forRoot()` and child routes with `forChild()`.  
22. Explain route parameters, query parameters, and `RouteReuseStrategy`.  
23. How to guard routes using `CanActivate`, `CanDeactivate`, `Resolve`, `CanLoad`.  
24. How to implement lazy-loaded feature modules and preloading strategies.  
25. How to handle not found and wildcard routes.  
26. How to preserve scroll position and implement scroll restoration.  
27. How to animate route transitions with Angular animations.  
28. Explain router events: `NavigationStart`, `RoutesRecognized`, `NavigationEnd`.  
29. How to implement secondary/outlet routing (named outlets).  
30. How to perform programmatic navigation and relative vs. absolute paths.  

## 4. State Management & Data Flow  
31. Compare local component state vs. shared service state.  
32. Explain RxJS Observables vs. Promises in Angular.  
33. How to manage global state with NgRx Store: actions, reducers, selectors, effects.  
34. Describe component store (`@ngrx/component-store`) for localized state.  
35. How to handle side effects with NgRx Effects and ensure idempotency.  
36. Alternative state libraries: Akita, NGXS—compare their approaches.  
37. How to normalize API data with entity adapters.  
38. Implement optimistic updates and rollback on failure with NgRx.  
39. How to use RxJS subjects (`BehaviorSubject`, `ReplaySubject`) in services.  
40. Explain `async` pipe vs. manual subscription/unsubscription.  

## 5. UI & Design Patterns  
41. Infinite scrolling using `cdk-virtual-scroll-viewport` or custom scroll detection.  
42. List virtualization for large datasets with Angular CDK Virtual Scroll.  
43. Skeleton loaders and placeholder UIs during data fetch.  
44. Debounced search inputs with RxJS `debounceTime`, `distinctUntilChanged`.  
45. Pagination component design and client vs. server paging.  
46. Implement drag-and-drop with Angular CDK DragDrop.  
47. Modal/dialog service pattern using Angular Material Dialog or custom service.  
48. Toast/notification service with a single outlet component.  
49. Tabbed interfaces and dynamic component loading.  
50. Accordion/expansion panels via Angular Material or custom directives.  
51. Carousel/slider implementation with custom directives or third-party libraries.  
52. Implement “load more” button pattern vs. infinite scroll trade-offs.  
53. Form wizard (multi-step form) via child routes or dynamic component injection.  
54. Breadcrumb component using ActivatedRoute snapshots.  
55. Alert, confirmation dialogs with service-based APIs.  
56. Theming and dark mode toggling using CSS variables and Angular CDK Theming.  
57. Responsive layouts with Flex Layout or CSS Grid integration.  
58. Accessibility patterns: ARIA roles, `aria-live`, keyboard navigation.  
59. Image lazy loading with native `loading="lazy"` and intersection observers.  
60. Sticky headers and footers with CSS and scroll events.  

## 6. HTTP & Backend Integration  
61. Use `HttpClient` for REST calls, interceptors for headers, error handling.  
62. Global error interceptor vs. per-request error handler.  
63. Implement JWT authentication: interceptors for token refresh and retry.  
64. Caching HTTP responses with interceptors and RxJS `shareReplay`.  
65. Cancelable requests via `takeUntil` and `HttpRequest` cancellation.  
66. File upload with `HttpClient` using `FormData`.  
67. Downloading files/blobs and programmatic download trigger.  
68. Implement retry strategies with RxJS `retryWhen` and `exponentialBackoff`.  
69. Streaming large payloads with `reportProgress` and `observe: 'events'`.  
70. GraphQL integration with Apollo Angular: queries, mutations, subscriptions.  

## 7. Performance Optimization  
71. Change detection tuning with `OnPush` and manual `markForCheck`.  
72. Avoiding unnecessary subscriptions by using `async` pipe.  
73. Pure vs. impure pipes and performance impact.  
74. Preloading images and assets via `PreloadAllModules` strategy.  
75. Bundling and code splitting with differential loading (`es5/es2015`).  
76. Analyze bundle size with `source-map-explorer` or `webpack-bundle-analyzer`.  
77. Lazy load third-party libraries only when needed.  
78. Track performance metrics via `Perfume.js` or Angular Profiler.  
79. Optimize Angular Material theme inclusion to reduce styles size.  
80. Use trackBy function in `*ngFor` to minimize DOM updates.  

## 8. Testing & QA  
81. Unit testing components with TestBed, `ComponentFixture`.  
82. Mock services using `spyOn` and dependency injection overrides.  
83. Test HTTP services with `HttpClientTestingModule` and `HttpTestingController`.  
84. Test NgRx store with `provideMockStore` and selectors.  
85. E2E testing with Protractor or Cypress—compare setups.  
86. Accessibility testing with `axe-core/angular`.  
87. Snapshot testing for templates (Storybook integration).  
88. Test routing with `RouterTestingModule` and `Location`.  
89. Test asynchronous code with `fakeAsync` and `tick` vs. `async/await`.  
90. Measure code coverage with Karma coverage reporter.  

## 9. Security & Best Practices  
91. Prevent XSS by using Angular’s built-in sanitization; avoid `innerHTML`.  
92. Protect against CSRF via server-side tokens; Angular’s XSRF support.  
93. Secure JWT storage: in-memory vs. cookie vs. local storage trade-offs.  
94. HTTP header hardening: `Content-Security-Policy`, `X-Frame-Options`.  
95. Implement role-based UI elements via structural directives.  
96. Audit sensitive operations and route access with route guards.  
97. Avoid exposing secrets in environment files—use CI/CD injection.  
98. Dependency vulnerability scanning with `npm audit`, `retire.js`.  
99. Enforce strict TypeScript typing and linting (TSLint/ESLint).  
100. Secure forms against overposting by whitelisting fields.  

## 10. Build, Deployment & Tooling  
101. Angular CLI build targets: development vs. production flags.  
102. Differential loading for modern browsers.  
103. Ahead-of-time (AOT) vs. Just-in-time (JIT) builds.  
104. Custom webpack configuration via `ngx-build-plus`.  
105. Continuous integration with GitHub Actions, GitLab CI, or Jenkins pipelines.  
106. Dockerizing Angular apps and serving via NGINX.  
107. Integrate with CDNs for static asset hosting.  
108. Cache busting strategies: file hashing and service worker.  
109. Enabling service workers for PWA support with `@angular/pwa`.  
110. Build optimizations: removing polyfills or using `es2020` targets.  

## 11. Micro-Frontend Integration  
111. Module federation with Webpack 5 in Angular applications.  
112. Single-spa integration via `@single-spa/angular`.  
113. Shared dependency management to avoid duplicate bundles.  
114. Cross-framework composition (React in Angular host).  
115. Authentication and state sharing between micro-frontends.  
116. Styling isolation using CSS modules or Shadow DOM.  
117. Routing integration across multiple micro-frontends.  
118. Lazy loading remote modules and fallback UIs.  
119. Deployment strategies for independent micro-frontend lifecycle.  
120. Logging and error isolation per micro-frontend.  

---

[1] https://www.geeksforgeeks.org/angular-js/angular-dependency-injection/
[2] https://www.syncfusion.com/blogs/post/angular-signals-reactive-programming
[3] https://www.syncfusion.com/blogs/post/whats-new-angular-17-syncfusion
[4] https://www.infragistics.com/blogs/control-flow-in-angular-17/
[5] https://www.angulartraining.com/daily-newsletter/displaying-an-overlay-with-angular/
[6] https://angular.dev/guide/di
[7] https://www.divotion.com/blog/how-do-angular-signals-work-under-the-hood
[8] https://www.youtube.com/watch?v=vtCDRiG__D4
[9] https://dzone.com/articles/angular-rxjs-reactive-operators
[10] https://javascript.plainenglish.io/best-practices-for-organizing-ngrx-state-in-large-applications-3abd31b4456d
[11] https://dev.to/this-is-angular/ngrx-tips-i-needed-in-the-beginning-4hno
[12] https://christianlydemann.com/ngrx-industry-best-practices/
[13] https://daily.dev/blog/angular-17-everything-you-need-to-know-in-one-place
[14] https://angular.dev/guide/hydration
[15] https://blogs.halodoc.io/angular-ssr-hydration/
[16] https://angular.dev/guide/incremental-hydration
[17] https://www.youtube.com/watch?v=iO81nlnNQBQ
[18] https://30dayscoding.com/blog/implementing-accessibility-a11y-in-angular-applications
[19] https://www.tutorialspoint.com/angular/angular-accessibility.htm
[20] https://owasp.org/www-chapter-london/assets/slides/OWASPLondon20170727_AngularJS.pdf
[21] https://pragmaticwebsecurity.com/files/cheatsheets/angularowasptop10.pdf
[22] https://www.geeksforgeeks.org/software-testing/cypress-vs-jest/
[23] https://stackoverflow.com/questions/66217682/should-i-use-both-cypress-and-jest-together
[24] https://dev.to/this-is-angular/angular-with-nx-in-2023-seriously-you-should-480j
[25] https://nx.dev/getting-started/tutorials/angular-monorepo-tutorial
[26] https://nx.dev/blog/architecting-angular-applications
[27] https://www.youtube.com/watch?v=m1NTKbHwNJk
[28] https://www.simform.com/blog/angular-best-practices/
[29] https://angularexperts.io/blog/angular-control-flow/
[30] https://briantree.se/angular-cdk-overlay-tutorial-learn-the-basics/
[31] https://blog.angulartraining.com/how-to-display-an-overlay-with-angular-38345414fdad
[32] https://angular.dev/reference/migrations/control-flow
[33] https://github.com/rainerhahnekamp/ngrx-best-practices
[34] https://angularexperts.io/products/ebook-angular-enterprise-architecture/
[35] https://www.simplilearn.com/tutorials/angular-tutorial/angular-dependency-injection
[36] https://medium.angularaddicts.com/angular-addicts-issue-6-ng-conf-2022-ngrx-best-practices-application-architecture-tips-more-feabd4406a29
[37] https://angular-material.dev/articles/exploring-cdk-context-menu-text-popover
[38] https://positiwise.com/blog/whats-new-in-angular-17
[39] https://www.freecodecamp.org/news/angular-dependency-injection/
[40] https://v6.material.angular.io/cdk
[41] https://www.geeksforgeeks.org/angular-js/angular-17-whats-new/
[42] https://material.angular.dev/cdk/categories
[43] https://www.youtube.com/watch?v=bjU2OBq_ASc
[44] https://angular.io/guide/dependency-injection
[45] https://material.angular.io/cdk/portal/overview
[46] https://30dayscoding.com/blog/building-enterprise-grade-angular-applications
[47] https://www.rockyourcode.com/notes-on-rxjs-patterns-in-angular/
[48] https://www.innoraft.ai/blog/proven-tools-and-techniques-reliable-angular-testing
[49] https://www.bacancytechnology.com/blog/angular-hydration
[50] https://www.c-sharpcorner.com/article/advanced-rxjs-patterns-in-angular/
[51] https://dev.to/bndf1/modern-testing-practices-in-angular-from-unit-to-integration-testing-2c01
[52] https://www.pluralsight.com/courses/angular-monorepos-nx
[53] https://www.peerbits.com/blog/complete-guide-to-testing-angular-apps.html
[54] https://www.angularminds.com/blog/understanding-angular-hydration-how-it-works-and-its-benefits
[55] https://www.packtpub.com/en-at/product/reactive-patterns-with-rxjs-for-angular-9781801811514?type=print
[56] https://bugasura.io/blog/angular-website-testing-challenges/
[57] https://infinum.com/blog/introducing-angular-signals/
