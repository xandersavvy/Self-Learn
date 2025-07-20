# Comprehensive Salesforce Developer Interview Question Bank

This extensive collection covers **Salesforce topics** including Apex, Aura Components, Lightning Web Components (LWC), OmniStudio/Vlocity, SOQL/SOSL, Flow/Process Automation, Integration (REST/SOAP/API), Sharing-Security, Einstein/AI, Metadata/Deployment, and DevOps. Questions focus on concrete technical knowledge and real-world scenarios.

## 1. Apex & Asynchronous Apex (1–30)

1. What is Apex and how does it differ from Java?  
2. Describe governor limits and how to handle them in bulk operations.  
3. Explain trigger contexts and order of execution.  
4. How do you bulkify Apex code? Provide examples.  
5. What is the difference between `Database.insert` and `insert`?  
6. How do you handle exceptions in Apex?  
7. Explain `@future` methods, limitations, and use cases.  
8. What are Queueable Apex and its advantages over future methods?  
9. Describe Batch Apex: scope, start, execute, finish.  
10. How do you chain batch jobs?  
11. What is Scheduled Apex and how do you schedule jobs?  
12. Compare `@AuraEnabled(cacheable=true)` vs. non-cacheable Apex.  
13. How do you write a test class and achieve >75% coverage?  
14. What are custom metadata types vs. custom settings?  
15. How do you perform DML partial success handling?  
16. Explain dynamic SOQL and dynamic SOSL in Apex.  
17. What is Apex REST and SOAP service creation?  
18. How do you handle callouts and enforce limits?  
19. Describe large data volume patterns (rollups, skinny tables).  
20. How do you prevent recursive triggers?  
21. Explain `@TestSetup` methods in test classes.  
22. What is a custom Apex interface and virtual classes?  
23. How do you serialize/deserialize JSON in Apex?  
24. Explain Context variables within triggers.  
25. What is the `Database.Stateful` interface?  
26. How do you debug Apex with System.debug and logs?  
27. Explain `Limits.get...()` methods usage.  
28. How do you implement custom authentication provider in Apex?  
29. Describe `ConnectApi.ChatterUsers` usage.  
30. How to integrate Apex with external OAuth2 services?

## 2. Lightning Aura Components (31–60)

31. What is the Aura Framework and its component bundle structure?  
32. Explain client vs. server controllers in Aura.  
33. How do you pass attributes (`@AuraEnabled`) between components?  
34. Describe event types: component, application, system events.  
35. How do you register and handle custom events?  
36. What are `aura:if`, `aura:iteration`, and `aura:set`?  
37. How do you perform navigation with `lightning:navigation`?  
38. Explain locker service and its security implications.  
39. How do you call Apex from Aura: imperative vs. wire-adapter.  
40. What is the purpose of helpers and renderers?  
41. How do you handle error messages in Aura?  
42. Explain dynamic component creation via `$A.createComponent`.  
43. How do you optimize performance: `access="GLOBAL"`, `aura:unescapedHtml`?  
44. How to implement infinite scroll and virtualization in Aura?  
45. Describe using `force:recordData` for CRUD without Apex.  
46. How do you implement drag-and-drop with HTML5 in Aura?  
47. Explain use of `workspaceAPI` in console apps.  
48. How do you test Aura components with Jasmine/Karma?  
49. Describe caching strategies with `cacheable=true`.  
50. How to migrate Aura components to LWC?  

## 3. Lightning Web Components (61–100)

61. Explain LWC file structure: `.html`, `.js`, `.js-meta.xml`, `.css`.  
62. Describe reactive properties: `@api`, `@track`, `@wire`.  
63. Outline LWC lifecycle hooks and execution order.  
64. How do you call Apex imperatively vs. via `@wire`?  
65. Explain Shadow DOM encapsulation in LWC.  
66. How do you dispatch and handle events in LWC?  
67. Describe Lightning Data Service usage in LWC (`uiRecordApi`).  
68. How to implement infinite scrolling with IntersectionObserver?  
69. How do you lazy-load modules via dynamic `import()`?  
70. Explain navigation with `NavigationMixin`.  
71. How do you write Jest unit tests for LWC?  
72. How to mock Apex and wire adapters in Jest?  
73. Describe performance best practices in LWC (cacheable methods).  
74. How do you include static resources and SVG icons?  
75. Explain usage of `lightning-datatable` for editable grids.  
76. How to implement a wizard (multi-step form) in LWC?  
77. Describe integration of LWC in Visualforce and Aura.  
78. How to implement message passing via Lightning Message Service.  
79. Explain `lightning-record-form`, `-record-edit-form`, `-view-form`.  
80. How to secure LWC (Locker, CSP, `@salesforce/label`).  
81. How to use `@salesforce/user/Id` scoped modules.  
82. Describe use of `loadScript` and `loadStyle` from `platformResourceLoader`.  
83. How to handle errors globally with `errorCallback`.  
84. Explain responsive design using CSS custom properties.  
85. How to integrate LWC OSS in non-Salesforce environments?  
86. Describe `lightning-spinner` and `loading-state` management.  
87. How to programmatically open a modal via `lightning-primitive`.  
88. Explain two-way binding via getters/setters.  
89. How to implement list filtering and sorting in LWC?  
90. Describe subscription to empApi (PushTopic) in LWC.  
91. How to use `NavigationMixin.GenerateUrl` for deep links.  
92. Explain `getPicklistValues` and `getObjectInfo` wire-adapters.  
93. How to handle dynamic CSS classes via `classMap`.  
94. Describe integration of Send Email via `lightning/emailService`.  
95. How to perform offline caching in LWC PWA.  
96. How to integrate geolocation (`lightning-geolocation`).  
97. Explain slotting and composition in LWC.  
98. How to implement export-to-CSV functionality.  
99. Describe SEO implications of LWC in Experience Cloud.  
100. How to performance-profile LWC with Chrome DevTools.

## 4. OmniStudio / Vlocity / AgentExplorer (101–130)

101. What is OmniStudio/Vlocity and its core elements: FlexCards, OmniScripts, Integration Procedures, DataRaptors?  
102. Explain FlexCard use cases and data sources.  
103. How do you hide/show elements via view conditions?  
104. Describe Integration Procedures vs. OmniScripts.  
105. How to call Apex within OmniScript via Remote Action?  
106. Explain DataRaptor Extract, Transform, Load patterns.  
107. How do you implement error handling in Integration Procedures?  
108. Describe Velocity (Vlocity Template) syntax and use.  
109. How to deploy OmniStudio components via Metadata API?  
110. Explain Agent Console integration with OmniScripts.  
111. How to use Lightning Message Service within OmniStudio.  
112. Describe caching patterns for DataRaptors.  
113. How to version OmniScript and manage patches.  
114. Explain streaming capabilities in Vlocity with Platform Events.  
115. How do you unit-test OmniScripts?  
116. Describe iterative loops and branching in Integration Procedures.  
117. How to handle third-party API integration via HTTP Action.  
118. Explain Omniscript Modal & Navigation Actions.  
119. How to secure DataRaptor outputs and mask sensitive fields.  
120. Describe bulk DataRaptor write operations and error recovery.  
121. How to use Vlocity CPQ and Quote Calculation Procedures.  
122. Explain SBQQ integration vs. OmniStudio CPQ.  
123. How to deploy Vlocity metadata using `OmniStudioDeployment` tool.  
124. Describe attribute mapping between DataRaptors and FlexCards.  
125. How to manage large OmniScript transactions and timeouts.  
126. Explain Accelerator patterns in OmniStudio.  
127. How to optimize FlexCard rendering with `xr_conditionalRender`.  
128. Describe controlled error handling with Set Errors element.  
129. How to implement conditional container branching.  
130. Explain dynamic picklists using remote Lookup.

## 5. SOQL & SOSL (131–160)

131. What is SOQL vs. SOSL? Use cases for each.  
132. How do you write polymorphic relationship queries (What.Type)?  
133. Explain parent-to-child and child-to-parent relationship queries.  
134. How to use aggregate functions: COUNT(), SUM(), AVG(), MAX(), MIN().  
135. Describe GROUP BY, HAVING, ROLLUP, CUBE usage.  
136. How do you paginate results efficiently (`OFFSET`, keyset pagination)?  
137. Explain `FOR UPDATE` in SOQL.  
138. How to query archived and deleted records (ALL ROWS).  
139. What are bind variables in dynamic SOQL?  
140. How to handle large result sets within governor limits (`batch size`, `queryMore`).  
141. Describe SOSL syntax and multiple object searches.  
142. How to search across fields and objects with SOSL: FIND ‘test*’ IN ALL FIELDS  
143. Explain using FIND [null] to find records with null fields.  
144. How to combine SOSL and SOQL for search+filter scenarios.  
145. How do you debug SOSL queries and interpret errors.  
146. What are selective filters and how they affect query performance.  
147. Explain Query Plan Tool for SOQL optimization.  
148. How to use Developer Console Query Editor vs. Workbench.  
149. Describe using Apex `Database.query` for dynamic SOQL.  
150. How to avoid SOQL injection.  
151. How to use Named Credentials with remote objects in SOQL via External Objects.  
152. Explain Cross-Org and Cross-Object queries via External Data Sources.  
153. How to use `WITH SECURITY_ENFORCED` in SOQL.  
154. Describe using `COUNT_DISTINCT()` in SOSL.  
155. How to store complex JSON-returned data via `JSON.deserializeUntyped`.  
156. Explain writing semi-join and anti-join queries.  
157. How to query large hierarchies via `GROUP BY HISTORY` (field history tracking).  
158. How to use `TODAY`, `LAST_N_DAYS`, `NEXT_N_DAYS` date literals.  
159. Describe Soft Delete behavior and `ALL ROWS`.  
160. How to test SOQL queries in Apex test classes.

## 6. Flow & Process Automation (161–190)

161. Describe differences: Workflow Rules, Process Builder, Flow Builder.  
162. When choose Record-Triggered vs. Scheduled Flow.  
163. How to bulkify record-triggered flows and avoid limits.  
164. Explain using “Get Records”, “Create Records”, “Update Records”, “Delete Records” elements.  
165. How to invoke Apex actions in Flow.  
166. How to create sub-flows and handle input/output variables.  
167. Describe use of Platform Events in Flow.  
168. How to implement rollback in autolaunched flows.  
169. How to schedule flows at specific times.  
170. Explain before-save lookup and update formulas vs. after-save flows.  
171. Describe screen flows with Lightning components.  
172. How to pass data between screens: screen component attributes.  
173. How to implement multi-screen wizard with decision elements.  
174. Explain error handling and fault paths in Flows.  
175. How to unit test flows: coverage and debug logs.  
176. Describe triggering flows from Platform Events or Apex.  
177. How to call Flows from LWC (`lightning-flow`).  
178. Explain use of invocable methods for Flows.  
179. How to migrate Process Builder to Flow.  
180. How to monitor Flow performance and view paused interviews.  
181. Describe transactional boundaries within flows vs. Apex.  
182. How to handle recursive flow calls.  
183. Explain use of “Collection Actions” in Flow loops.  
184. How to implement complex branching logic.  
185. Describe Salesforce Flow Extensions (Flow Orchestration).  
186. How to manage Flow metadata in CI/CD.  
187. Explain Flow interview limits and paused Flow storage.  
188. How to secure Flows with permission checks.  
189. How to version and rollback flows.  
190. Best practices for large enterprise flow design (modularity).

## 7. Integration & APIs (191–220)

191. Explain REST API vs. SOAP API vs. Bulk API vs. Streaming API.  
192. How to implement named credentials and external credentials.  
193. Describe `@RestResource` and `@Http*` Apex annotations.  
194. How to call external services via `HttpRequest` and `HttpResponse`.  
195. Explain use of `Continuation` for long-running callouts in LWC.  
196. How to handle OAuth2 JWT Bearer Token flow in Apex.  
197. Describe using Platform Events for event-driven integration.  
198. How to use Change Data Capture (CDC) for integration.  
199. Explain external objects with Salesforce Connect (OData).  
200. How to implement custom authentication provider.  
201. Describe use of `External Services` and OpenAPI in Salesforce.  
202. How to integrate with MuleSoft and API Gateway.  
203. Explain data transformation with DataWeave vs. Apex.  
204. How to use `Streaming API` and CometD clients.  
205. How to handle large payloads in Bulk API v2.  
206. Describe using `Batchable` Apex for integration scheduling.  
207. How to design idempotent integrations with external systems.  
208. Explain middleware orchestration vs. direct integration.  
209. How to test integration endpoints via Postman/Newman.  
210. How to implement governor-limit–aware integration patterns.  
211. Explain retry/backoff logic for flaky external calls.  
212. How to log integration errors to `PlatformEvent` or Custom Object.  
213. Describe secure file upload via `ContentVersion` REST API.  
214. How to stream large binary data with `Chatter Files`.  
215. How to use `ConnectApi` for Chatter integration.  
216. Explain `Apex Flex Queue` for large job orchestration.  
217. How to use `pubsubReplayId` for durable event replay.  
218. Describe data encryption in transit vs. at rest for integration.  
219. How to monitor integration health with Event Monitoring API.  
220. Best practices for versioning shared APIs in Salesforce.

## 8. Sharing & Security (221–250)

221. Describe org-wide defaults, role hierarchy, sharing rules.  
222. Explain manual sharing vs. apex sharing reasons.  
223. How to implement row-level security with row-cause.  
224. Describe use of `with sharing` vs. `without sharing` in Apex.  
225. How to enforce field-level and object-level security in Apex and LWC.  
226. Explain use of `Security.stripInaccessible` in Apex.  
227. How to test security in Apex with `System.runAs()`.  
228. Describe shield platform encryption vs. classic encryption.  
229. How to audit data access via Field History Tracking vs. Event Monitoring.  
230. Explain two-factor authentication and session security settings.  
231. How to restrict CRUD and FLS in LWC and Aura.  
232. Describe cross-site scripting (XSS) defense in UI.  
233. How to prevent SOQL injection.  
234. Explain `WITH SECURITY_ENFORCED` in SOQL.  
235. How to secure custom REST endpoints with named credentials and authentication providers.  
236. Describe session timeout and IP restrictions.  
237. How to implement identity verification for API access.  
238. Explain delegated authentication and SSO integration.  
239. How to configure Permission Sets and Permission Set Groups.  
240. Best practices for secure metadata exposure in packages.  
241. How to use `Shield` for access monitoring and event logging.  
242. Describe `Lightning Locker` architectural isolation.  
243. How to handle file and document sharing permissions.  
244. Explain territory management sharing.  
245. How to implement row-level encryption with custom code.  
246. How to use `Profile` vs. `Permission Set` for least-privilege access.  
247. How to secure community/experience portal access.  
248. Describe critical updates and their impact on security.  
249. How to review security health check and mitigate findings.  
250. Best practices for automated security scanning in CI/CD.

## 9. Einstein & AI Services (251–270)

251. What is Einstein Prediction Builder and how to use it declaratively?  
252. How to call Einstein Vision/Language APIs from Apex?  
253. Explain `Einstein Analytics (CRM Analytics)` dataset and lens creation.  
254. How to embed Einstein Discovery in Lightning pages?  
255. Describe using Einstein Next Best Action with Strategies.  
256. How to train custom ML models with Data Prep recipes?  
257. Explain `Einstein Bot` creation and OmniStudio integration.  
258. How to expose Bot as Chat in Experience Cloud.  
259. Describe `Einstein Recommendation` setup for Commerce Cloud.  
260. How to secure and govern Einstein user data.  
261. Explain using `Apex PredictionService` client.  
262. How to integrate third-party AI services via Callouts.  
263. Describe `Intent and Sentiment` analysis in Chatbots.  
264. How to use `Einstein Search` for personalized results.  
265. Best practices for monitoring AI model drift and accuracy.  
266. How to use `Einstein Opportunity Scoring` in Sales Cloud.  
267. Explain retraining models with updated data.  
268. How to handle missing or skewed data in AI models.  
269. Describe A/B testing of Einstein strategies.  
270. How to instrument AI metrics in analytics dashboards.

## 10. Metadata, Deployment & DevOps (271–300)

271. Compare change sets vs. Metadata API vs. SFDX CLI.  
272. How to structure Salesforce DX project (`sfdx-project.json`, packages).  
273. Explain unlocked packages vs. managed packages.  
274. How to implement CI/CD with GitHub Actions or Jenkins.  
275. Describe use of `sfdx force:source:deploy` vs. `force:mdapi:deploy`.  
276. How to handle destructive changes.xml.  
277. Explain `Org Shape` and scratch org management.  
278. How to automate data seeding in scratch orgs.  
279. Describe `Salesforce CLI` plugin architecture.  
280. How to enforce code quality with PMD and ESLint for LWC.  
281. Explain automated test execution via `sfdx force:apex:test:run`.  
282. How to retrieve code coverage results programmatically.  
283. Describe deploying only changed metadata via CI diff.  
284. How to manage org‐specific config via scratch org definition.  
285. Explain `Data Import Wizard` vs. `Bulk API` in sandboxes.  
286. How to automate database seeding with `DataTree` plugin.  
287. Describe static code analysis with `ESLint`, `TSLint`, `PMD`.  
288. How to use `Apex PMD`, `LWC eslint` and fail builds on issues.  
289. Explain versioning and package dependencies in unlocked packages.  
290. How to rollback deployments on failures.  
291. Describe environment strategies: dev, QA, UAT, prod.  
292. How to mask sensitive data in sandboxes via Data Security Guardrails.  
293. Explain post deploy steps: permission set assignment, default record types.  
294. How to integrate feature flags (Custom Metadata) for progressive releases.  
295. Describe Canary releases via separate permission sets.  
296. How to monitor deployment performance and failures.  
297. Explain Org lifecycle management with Dev Stages and Sandboxes.  
298. How to use Git version branching strategies (Gitflow, trunk).  
299. Describe automated change tracking with `GitHub Dependabot`.  
300. Best practices for Salesforce DevOps Center or Gearset.

## CumulusCI
301. What problem does CumulusCI solve for Salesforce projects, especially package development?
302. What are the concepts of tasks, flows, and org configurations in CumulusCI?
303. How do you author and automate repeatable deployment flows?
304. Describe integration with GitHub Actions and other CI/CD services.
305. How do you automate scratch org creation, data seeding, and test runs?
306. What are best practices for managing complex dependency chains or unmanaged package upgrades?

## Conga
307. What is Conga Composer and how does it automate document generation?
308. Describe use cases for Conga Contracts and Conga Sign.
309. How do you create and parameterize templates for reports and proposals?
310. Explain Conga’s integration with Flow, Process Builder, and Apex.
311. How do you troubleshoot failed merges or template rendering errors?
312. What are strategies for scaling Conga transaction volume and handling large document sets?
313. How do you ensure security/compliance with Conga Sign and e-signature processe

## Copado

314. What is Copado and how does it integrate with Salesforce DevOps lifecycle?
315. How do you manage user stories, environments, promotion paths, and releases in Copado?
316. Explain the Copado branching strategy and Git integration.
317. How do you configure automated validations (metadata deployment, Apex test runs)?
318. How does Copado handle data deployments and test data management?
319. What are rollback strategies and impact analysis in Copado?
320. How do you integrate static code analysis and quality gates?
321. Explain setting up Copado compliance and audit trails.
322. What are Copado CI Jobs and how can you integrate external CI/CD services?
323. How do you monitor deployment pipelines and detect merge conflicts?
324. Describe best practices for permissioning and operational governance in Copado.

## CumulusCI (Extended)
325. Describe autorun flows for integration, QA, and UAT orgs.
326. How do you extend CumulusCI with custom tasks and step libraries?
327. What’s the process to manage versioned, dependency-aware packages?

[1] https://s2-labs.com/blog/apex-interview-questions/
[2] https://www.linkedin.com/pulse/aura-framework-interview-questions-answers-onlineinterviewquestions-sybpf
[3] https://www.goliveclasses.co/salesforce-vlocity-interview-questions/
[4] https://sfdclesson.com/chapter-6-advanced-soql-techniques/
[5] https://www.crsinfosolutions.com/salesforce-apex-interview-questions/
[6] https://www.apexhours.com/salesforce-lightning-interview-questions-answers/
[7] https://hkrtrainings.com/salesforce-vlocity-interview-questions
[8] https://www.crsinfosolutions.com/salesforce-soql-and-sosl-interview-questions/
[9] https://www.apexhours.com/salesforce-interview-questions/
[10] https://www.finalroundai.com/blog/salesforce-lightning-interview-questions
[11] https://www.crsinfosolutions.com/salesforce-vlocity-interview-questions/
[12] https://www.sfdcamplified.com/soql-scenarios-in-apex/
[13] https://salesforcegeek.in/scenario-based-salesforce-developer-interview-questions-and-answers/
[14] https://www.sfdc-lightning.com/p/salesforce-interview-questions.html
[15] https://www.apexhours.com/salesforce-vlocity-interview-questions/
[16] https://www.linkedin.com/posts/sandeep-sharma-salesforcedeveloper_here-are-some-advanced-soql-queries-and-questions-activity-7240203538499297280-ZcLQ
[17] https://www.sfapps.info/100-salesforce-apex-interview-questions-and-answers/
[18] https://blog.salesforcecasts.com/aura-components-interview-questions/
[19] https://www.acte.in/salesforce-velocity-interview-questions-and-answers
[20] https://s2-labs.com/blog/soql-interview-questions/