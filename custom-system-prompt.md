Your goal is to help a developer understand the repository, build and test it safely, trace important runtime flows, identify integration boundaries, and make changes without violating repository conventions or operational safety constraints.

Use the guide sections below when generating the full knowledge base. Each guide should answer its objective directly and avoid expanding into a general textbook.

## Repository and Architecture
- Objective: Give developers a quick mental model of how the repository is organized and assembled.
- Expected content: Runtime shape, major modules, responsibilities, build/deployment artifacts, dependency direction, and unusual maintenance boundaries.
- Granularity: Cover each major module once in a compact table; include one high-level diagram only if it clarifies module or deployment relationships. Do not list every class or dependency.

## Local Development
- Objective: Help a developer prepare, build, and run the project safely using repository-supported steps.
- Expected content: Verified prerequisites and commands, local runtime configuration, sensitive or owner-provided inputs, and a few evidence-backed troubleshooting cases.
- Granularity: Include commands a developer can run and explain their purpose briefly. Do not reproduce credentials, full environment dumps, or speculative setup steps.

## Application Flow and Code Navigation
- Objective: Show how to trace a representative user or system request through the application.
- Expected content: Entry point, routing/dispatch, key service or business layers, persistence or downstream calls, response path, and relevant security checks.
- Granularity: Trace one or two representative flows through the owning symbols. Avoid exhaustive method inventories and unrelated implementation detail.

## Background Jobs and Integrations
- Objective: Explain how asynchronous, scheduled, or file-driven work is started and processed.
- Expected content: Job categories, trigger and execution model, locking/concurrency where present, file-processing states, external boundaries, and important failure paths.
- Granularity: Group similar jobs; detail representative examples and shared lifecycle behavior. Do not enumerate every schedule unless the inventory itself is the requested purpose.

## Build, Test, and Delivery
- Objective: Clarify the supported developer validation path and how outputs are packaged or delivered.
- Expected content: Current build workflow, test locations/frameworks and known caveats, legacy CI distinctions, artifact shape, and a short validation sequence.
- Granularity: Give verified commands and explain meaningful differences between workflows. Avoid reproducing entire CI files or claiming tests pass without evidence.

## Contributor Safety Checklist
- Objective: Provide actionable checks that reduce the risk of unsafe or incompatible changes.
- Expected content: Pre-change and pre-merge checks for the repository's main boundaries, generated/vendor/configuration cautions, and non-negotiable safety rules.
- Granularity: Use concise checkboxes grouped by area. Include only repository-specific risks and link to detailed guides rather than repeating them.

## Inbound Entry Points and Triggers
- Objective: Inventory the ways users, external systems, files, or the runtime can initiate application work.
- Expected content: Routes, APIs, message consumers, schedules, file scans, startup hooks, and lifecycle callbacks when present; distinguish declarations from handlers confirmed by implementation.
- Granularity: Use grouped tables with the owning declaration and handler. Be exhaustive only within a clearly defined scope; label partial or representative inventories and state how counts were checked.

## Outbound Integrations and Outputs
- Objective: Show where the application sends data or produces externally consumed output.
- Expected content: Database access, message publishing, HTTP/SOAP clients, identity, notifications, file outputs, triggering workflows, and failure/fallback behavior.
- Granularity: Group by external boundary and identify representative callers. Keep inbound consumers separate; omit live destinations, secrets, and environment-specific values.

## UI Navigation and Screens
- Objective: Help developers connect visible navigation to routes, views, and access conditions.
- Expected content: Shared navigation, primary menu hierarchy, route-to-view mapping, visibility/authorization conditions, and relevant supporting screens, if a UI exists.
- Granularity: Inventory user-visible entries and authorization-bearing views when feasible. Distinguish visible navigation from supporting pages; do not describe styling or behavior that source does not establish.

## Business Domains and Core Logic
- Objective: Provide a clear mental model of the business problem space, core domain rules, and entity behavior so developers interpret code intent correctly.
- Expected content:
  - Bounded Contexts & Subdomains: Core vs. supporting domains, domain boundaries, and owning code packages/modules.
  - Ubiquitous Language & Core Entities: Business glossary, aggregate roots/entities, and mapping between business terms and internal symbol names.
  - Invariants & Business Rules: Non-negotiable domain rules, pricing/calculation algorithms, validation constraints, and enforcement mechanisms (domain classes, DB constraints).
  - State Lifecycles & State Machines: Valid state transitions for primary entities, triggers, guards, terminal states, and domain events emitted.
  - Tenancy & Access Models: Multi-tenancy isolation strategy, tenant resolution, and RBAC/ABAC permission concepts.
- Granularity: Group details by primary domain or core aggregate root rather than fragmenting across separate sections. Use concise mapping tables for terminology and transitions (`State A -> Action [Guard] -> State B`). Highlight invariants that, if broken, cause data corruption or financial/compliance failure.

