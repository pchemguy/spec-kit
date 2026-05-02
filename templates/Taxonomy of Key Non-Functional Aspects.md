---
url: https://chatgpt.com/c/69f5cb12-6d14-83eb-ab10-a57b41b1aa71
---

Below is a taxonomy you can use as a source model for later prompt instructions. I would call these **non-functional and form-factor aspects** or **system form aspects**. Some are user-visible, some are delivery/engineering constraints, but all can affect capability decomposition.

## Taxonomy of Key Non-Functional / Form-Factor Aspects

### 1. Runtime Platform

Where the software executes.

* **Browser-based web application**
    * pure client-side browser app
    * server-backed web app
    * progressive web app
    * single-page application
    * multi-page web app
* **Desktop application**
    * native desktop app
    * packaged web app shell
    * cross-platform desktop app
    * platform-specific desktop app
* **Mobile application**
    * native mobile app
    * cross-platform mobile app
    * mobile web app
    * responsive web app
* **Terminal runtime**
    * command-line app
    * interactive shell app
    * text user interface app
* **Server/runtime service**
    * API service
    * background daemon
    * worker service
    * scheduled job
* **Embedded or device runtime**
    * firmware
    * kiosk app
    * appliance app
    * IoT device app

Prompt relevance: runtime platform often affects access, installation, interaction model, persistence, permissions, and portability.

---

### 2. Access Model

How the user reaches or invokes the system.

* **Direct user launch**
    * open URL
    * launch desktop executable
    * run terminal command
    * open mobile app
* **Integrated access**
    * plugin
    * extension
    * embedded widget
    * IDE integration
    * browser extension
    * chat/bot integration
* **Programmatic access**
    * API
    * library function
    * SDK
    * CLI command used by scripts
* **Automated access**
    * scheduled execution
    * background listener
    * event-triggered workflow
    * webhook-triggered action

Prompt relevance: access model determines whether a “capability” is user-operated, embedded, automated, or developer-facing.

---

### 3. Product Form

What kind of deliverable the thing is.

* **Application**
    * user-facing app
    * admin app
    * internal tool
* **Library**
    * package
    * SDK
    * framework
    * reusable component
* **Service**
    * API service
    * microservice
    * backend service
    * data service
* **Tool**
    * CLI tool
    * developer tool
    * build tool
    * migration tool
    * diagnostic tool
* **Extension**
    * browser extension
    * IDE extension
    * plugin
    * integration connector
* **Workflow / automation**
    * script
    * pipeline
    * scheduled process
    * bot
* **Content / configuration artifact**
    * template
    * schema
    * prompt
    * policy
    * ruleset

Prompt relevance: product form affects who the “user” is and what counts as a user-facing capability.

---

### 4. User Interface Modality

How users interact with the system.

* **Graphical UI**
    * desktop GUI
    * web GUI
    * mobile GUI
    * kiosk GUI
* **Command-line interface**
    * command + flags
    * subcommands
    * pipeline-friendly CLI
    * REPL
* **Text user interface**
    * terminal UI
    * keyboard-driven panels
    * curses-style interface
* **Conversational interface**
    * chatbot
    * assistant command surface
    * natural-language interface
* **Voice interface**
    * voice commands
    * speech output
    * voice assistant integration
* **Programmatic interface**
    * API
    * SDK
    * function calls
    * query language
* **No direct UI**
    * background service
    * daemon
    * automated process
    * event-driven worker

Prompt relevance: UI modality often deserves a capability anchor only when it materially affects user access, control, discoverability, feedback, or workflow.

---

### 5. UI Interaction Style

How interaction is structured within the modality.

* **Form-based**
* **Command-driven**
* **Menu-driven**
* **Canvas/direct-manipulation**
* **Wizard/stepper**
* **Dashboard**
* **Document/editor model**
* **Spreadsheet/grid model**
* **Timeline/workflow model**
* **Search/query model**
* **Stack-based / queue-based / graph-based interaction**
* **Drag-and-drop**
* **Keyboard-first**
* **Touch-first**
* **Mouse-first**
* **Real-time interactive**
* **Batch submission**

Prompt relevance: interaction style may be a solution form, but it can be capability-relevant when it changes how users understand and control the system.

---

### 6. Packaging and Delivery

How the software is packaged and provided to users.

* **Hosted delivery**
    * public web app
    * private hosted web app
    * SaaS
    * self-hosted server
* **Installable delivery**
    * installer
    * portable executable
    * package manager
    * app store
    * enterprise deployment package
* **Source-based delivery**
    * source repository
    * template repository
    * scaffold/generator
* **Containerized delivery**
    * Docker image
    * compose stack
    * Kubernetes chart
* **Package ecosystem delivery**
    * npm package
    * PyPI package
    * NuGet package
    * Maven package
    * Ruby gem
* **Artifact delivery**
    * static files
    * binary release
    * plugin bundle
    * extension package

Prompt relevance: delivery is not always a capability, but it becomes one when users care about installability, launch mode, offline use, portability, or deployment ownership.

---

### 7. Portability

Where the deliverable can move or run without rework.

* **Cross-platform portability**
    * Windows/macOS/Linux
    * iOS/Android
    * modern browsers
* **Portable execution**
    * no install required
    * portable executable
    * runs from USB/folder
* **Environment portability**
    * local machine
    * cloud
    * self-hosted
    * containerized
* **Data portability**
    * import/export
    * open file formats
    * backup/restore
    * migration support
* **Configuration portability**
    * portable settings
    * declarative config
    * environment-independent config

Prompt relevance: portability often belongs in access/delivery/environment anchors, unless it is central to the user need.

---

### 8. Deployment and Hosting Model

Who runs the system and where it is operated.

* **Local-only**
* **Hosted SaaS**
* **Self-hosted**
* **On-premises**
* **Cloud-hosted**
* **Hybrid local/cloud**
* **Edge-deployed**
* **Air-gapped/offline**
* **Multi-tenant**
* **Single-tenant**

Prompt relevance: deployment model can affect access, privacy, data ownership, operational responsibility, latency, and persistence expectations.

---

### 9. Connectivity and Offline Behavior

What network assumptions the system makes.

* **Always-online**
* **Offline-first**
* **Offline-capable**
* **Local-only**
* **Sync when online**
* **Graceful degraded mode**
* **Network-required for specific capabilities**
* **Peer-to-peer**
* **LAN-only**
* **Cloud-dependent**

Prompt relevance: connectivity often creates major user-facing differences in reliability, access, persistence, and synchronization.

---

### 10. Persistence and State Model

How state or data survives across use.

* **Ephemeral**
    * no saved state
    * session-only state
* **Local persistence**
    * browser storage
    * local files
    * embedded database
* **Remote persistence**
    * backend database
    * cloud storage
    * account-linked storage
* **Hybrid persistence**
    * local cache + sync
    * offline local store + server reconciliation
* **User-managed persistence**
    * save/load files
    * import/export
    * explicit backup
* **System-managed persistence**
    * autosave
    * version history
    * snapshots

Prompt relevance: persistence may be a capability when users intentionally save, resume, recover, sync, or transfer work.

---

### 11. Data Ownership and Privacy Boundary

Where user data lives and who controls it.

* **No data retained**
* **Local-only data**
* **Account-bound cloud data**
* **Organization-managed data**
* **Third-party processed data**
* **User-exportable data**
* **Encrypted-at-rest data**
* **End-to-end encrypted data**
* **Anonymous / pseudonymous use**
* **Authenticated user data**

Prompt relevance: privacy boundaries are often non-functional, but they shape trust, access, compliance, and deployment expectations.

---

### 12. Identity and Access Control

How users are identified and authorized.

* **No authentication**
* **Local profile**
* **Username/password**
* **Single sign-on**
* **OAuth/social login**
* **API keys**
* **Role-based access control**
* **Permission scopes**
* **Organization/team membership**
* **Admin vs standard user**
* **Guest access**

Prompt relevance: identity/access may be a separate capability if users manage accounts, permissions, sharing, or protected access.

---

### 13. Integration Surface

How the system connects with other systems.

* **File import/export**
* **Clipboard integration**
* **URL/deep link integration**
* **REST API**
* **GraphQL API**
* **Webhooks**
* **Plugin API**
* **SDK**
* **Database connection**
* **Third-party service integration**
* **OS integration**
* **Browser integration**
* **IDE/editor integration**

Prompt relevance: integrations may be primary capabilities when users explicitly rely on external workflows.

---

### 14. Execution Mode

How work is performed.

* **Interactive**
* **Batch**
* **Streaming**
* **Real-time**
* **Asynchronous job**
* **Scheduled**
* **Event-driven**
* **Background processing**
* **Manual trigger**
* **Automatic trigger**
* **Single-user session**
* **Collaborative multi-user session**

Prompt relevance: execution mode often affects observability, feedback, cancellation, progress, state, and error handling.

---

### 15. Performance and Responsiveness

How quickly or efficiently the system must behave.

* **Immediate response**
* **Low-latency interaction**
* **Real-time update**
* **Large-data handling**
* **High-throughput processing**
* **Resource-constrained operation**
* **Startup-time constraint**
* **Memory/CPU footprint constraint**
* **Battery-sensitive operation**

Prompt relevance: performance should usually not become a capability anchor unless users explicitly care about speed, scale, responsiveness, or resource constraints as part of the experience.

---

### 16. Reliability and Recovery

How the system handles failure, interruption, or correction.

* **Undo/redo**
* **Autosave**
* **Crash recovery**
* **Retry**
* **Checkpointing**
* **Rollback**
* **Conflict recovery**
* **Error visibility**
* **Safe failure/no mutation**
* **Backup/restore**
* **Version history**

Prompt relevance: recovery can be a user-facing capability when the user intentionally corrects, restores, resumes, or protects work.

---

### 17. Observability and Feedback

How users understand what the system is doing.

* **Status messages**
* **Progress indicators**
* **Logs**
* **Audit trails**
* **Notifications**
* **Warnings**
* **Error messages**
* **Preview before commit**
* **History view**
* **State visibility**
* **Telemetry/metrics dashboard**

Prompt relevance: feedback becomes a capability when users rely on it to control, verify, trust, or recover from actions.

---

### 18. Accessibility and Internationalization

How broadly and inclusively users can operate the system.

* **Keyboard accessibility**
* **Screen-reader compatibility**
* **Color contrast**
* **Reduced-motion support**
* **Touch accessibility**
* **Localization**
* **Multiple languages**
* **Locale-aware formatting**
* **Time zone handling**
* **Right-to-left layout**
* **Input method support**

Prompt relevance: these are often constraints, but can be capability anchors when the target scope explicitly centers inclusive access or multilingual use.

---

### 19. Configuration and Customization

How users adapt the system to their preferences or environment.

* **User preferences**
* **Theme/settings**
* **Keyboard shortcuts**
* **Config files**
* **Environment variables**
* **Profiles**
* **Presets**
* **Plugin configuration**
* **Admin configuration**
* **Policy-controlled configuration**

Prompt relevance: configuration is a capability when users intentionally tailor behavior, not when it is merely an implementation parameter.

---

### 20. Distribution Audience and Operational Ownership

Who the system is for and who operates it.

* **End-user consumer**
* **Power user**
* **Developer**
* **Administrator**
* **Operator**
* **Organization/team**
* **Internal-only audience**
* **Public audience**
* **Enterprise-managed audience**
* **Self-service user**
* **Managed deployment user**

Prompt relevance: this helps distinguish app vs tool vs library vs admin surface, and prevents the wrong user perspective.

---

## Condensed Prompt-Oriented Taxonomy

For prompt use, I would compress the above into these major categories:

```markdown
### Non-Functional and Form-Factor Aspect Taxonomy

When analyzing the target scope, the LLM MUST inspect whether any of the following aspects are explicitly stated or strongly implied:

1. **Product Form** — application, library, service, tool, extension, workflow, configuration artifact.
2. **Runtime Platform** — browser, desktop, mobile, terminal, server, embedded, cloud, local.
3. **Access Model** — direct launch, integrated access, programmatic access, automated access.
4. **User Interface Modality** — GUI, CLI, TUI, conversational UI, voice UI, API, no direct UI.
5. **Interaction Style** — form-based, command-driven, menu-driven, editor-like, dashboard, direct manipulation, batch, real-time.
6. **Packaging and Delivery** — hosted, installable, portable, package-manager, source-based, containerized, plugin/extension bundle.
7. **Portability** — cross-platform runtime, portable execution, data portability, configuration portability.
8. **Deployment and Hosting** — local-only, SaaS, self-hosted, on-premises, cloud, hybrid, air-gapped, multi-tenant.
9. **Connectivity and Offline Behavior** — online-only, offline-first, offline-capable, sync, degraded mode, local-only.
10. **Persistence and State** — ephemeral, local persistence, remote persistence, hybrid sync, user-managed files, autosave/versioned state.
11. **Identity and Access Control** — no auth, local identity, account login, SSO, roles, permissions, API keys.
12. **Integration Surface** — files, clipboard, URLs, APIs, webhooks, SDKs, plugins, OS/browser/IDE integration.
13. **Execution Mode** — interactive, batch, streaming, scheduled, event-driven, background, asynchronous.
14. **Reliability and Recovery** — undo, redo, retry, rollback, autosave, crash recovery, backup/restore, safe failure.
15. **Observability and Feedback** — status, progress, warnings, errors, logs, audit trails, notifications, previews.
16. **Performance and Resource Constraints** — latency, throughput, startup time, large-data handling, memory/CPU/battery limits.
17. **Accessibility and Internationalization** — keyboard access, screen reader support, contrast, localization, locale/timezone behavior.
18. **Configuration and Customization** — preferences, profiles, themes, shortcuts, config files, policies, presets.
19. **Privacy and Data Ownership** — local-only data, cloud data, exportability, encryption, retention, third-party processing.
20. **Target Audience and Operational Ownership** — consumer, power user, developer, admin, operator, organization, public/internal/enterprise.
```

## How this should affect capability decomposition

Not every item in the taxonomy should become a capability anchor. The rule should be:

```markdown
A non-functional or form-factor aspect SHOULD become a capability anchor only when it materially affects user-visible value, access, control, trust, recovery, portability, environment, or experience.

A non-functional or form-factor aspect SHOULD NOT become a capability anchor merely because it is an implementation choice, architectural preference, or delivery detail.
```

For your RPN calculator case:

* **Mathematical calculation** = core user capability.
* **RPN interaction** = interaction model; may be an anchor if it materially defines user control/experience.
* **Undo** = recovery capability.
* **Browser web app** = runtime/access/delivery aspect.
* **Packaged desktop version later** = packaging/platform/delivery aspect.

That taxonomy should push the LLM away from “RPN Calculation” as a single umbrella and toward clearer user-centric capability boundaries.

---

#### Non-Functional and Form-Factor Classification

The LLM MUST classify usability, access, launch, delivery, environment, runtime, platform, portability, packaging, interface, persistence, connectivity, identity, integration, reliability, recovery, observability, performance, accessibility, configuration, privacy, and operational-ownership aspects before deciding whether they belong in the capability anchor set.

Each such aspect MUST be treated as one of:

1. **Capability-relevant aspect** — the aspect materially changes user-visible value, access, control, recovery, trust, portability, environment, or experience.
2. **Cross-cutting constraint** — the aspect constrains multiple capabilities but is not itself a distinct user-recognizable capability area.
3. **Implementation workstream** — the aspect implies separable build, packaging, integration, deployment, or delivery work but does not itself define a core user capability.

A non-functional or form-factor aspect MUST become a capability anchor only when it is a capability-relevant aspect.

A cross-cutting constraint MUST NOT become a capability anchor merely because it applies broadly.

An implementation workstream MUST NOT become a capability anchor merely because it implies separable engineering work.

An implementation workstream MAY correspond to a capability anchor only when the resulting deliverable creates a distinct user-facing access mode, interaction surface, operating context, recovery behavior, or trust boundary.

The LLM MUST NOT confuse implementation separability with user-facing capability separability.


| Aspect                | Classification                                                | User-facing impact                | Implementation implication         |
| --------------------- | ------------------------------------------------------------- | --------------------------------- | ---------------------------------- |
| Cross-platform parity | Cross-cutting constraint                                      | Same behavior across environments | Test matrix / compatibility checks |
| Browser web app       | Capability-relevant access aspect                             | User can access through browser   | Web runtime build                  |
| Desktop packaging     | Capability-relevant access aspect + implementation workstream | User can launch as desktop app    | Electron wrapper/package           |
| Undo                  | Capability-relevant recovery aspect                           | User can correct mistakes         | History/state model                |
