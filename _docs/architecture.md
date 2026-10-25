# Weekly Project Feedback Tool — MVP Architecture

Status: agreed architecture; implementation has not started.

Product scope: [plan.md](plan.md).

## Stack

| Component | Choice | Purpose |
| --- | --- | --- |
| Web application | Django | Routing, business rules, authentication, permissions, admin, and migrations. |
| Interface | Django templates + HTMX + Alpine.js | Server-rendered pages, partial updates, and small client-side interactions. |
| Database | PostgreSQL | Projects, memberships, weekly cycles, feedback, clusters, votes, decisions, and actions. |
| Background jobs | Celery | Transcription, suggested clustering, and extraction of actions and decisions. |
| Task broker | Redis | Deliver background jobs to Celery workers. PostgreSQL remains the source of truth for application and processing status. |
| File storage | Private S3 storage through django-storages | Uploaded audio, video, and transcripts. |
| Board synchronization | Two-second polling during active meetings | Refresh meeting stage, visible feedback, clusters, and published voting results. |

## Application structure

Use a Django monolith with one application codebase. Django renders the pages and HTMX replaces relevant portions of the page after user actions or polling. Alpine.js handles small local interactions such as dialogs and toggles. There is no separate React frontend or standalone frontend API requirement for the MVP.

The web application and Celery worker share the codebase but run as separate processes. PostgreSQL, Redis, and S3 are supporting services. A single codebase does not mean a single runtime process.

The main screens are the project list, retrospective board, and outcomes view. Past cycles remain accessible within each project.

## Permissions and privacy

Django enforces project membership and facilitator permissions on every request, including HTMX requests and polling responses.

Before reveal, members can access only their own feedback. The facilitator cannot access other members' feedback before reveal through product screens or endpoints. Attributed submissions show the author's name after reveal; anonymous submissions exclude the author's identity from team and facilitator responses.

Author identity needed for editing and access checks must remain separate from shared representations. Product-level anonymity does not imply anonymity from database operators. General application admin access must not give ordinary facilitators a way to bypass these rules.

Files remain private and require authorization to access. Any temporary download links should be short-lived. Redis messages and background processing status must not expose private feedback or author identities to clients.

## Retrospective workflow

The facilitator controls progression through collection, reveal and clustering, voting, discussion, and outcomes review.

- During collection, members create and edit their own feedback.
- Reveal closes submissions and makes all feedback visible together.
- Automatic clustering suggests groups within each Start, Stop, or Continue category. The facilitator adjusts groups after team discussion.
- Voting starts after clustering is finalized. Each member has three votes in total and may allocate multiple votes to a topic.
- Members can see their own vote allocations. Other members' votes and running totals remain hidden until voting closes.
- Closing voting publishes a prioritized discussion list.
- After discussion, uploaded meeting material is processed into draft outcomes for facilitator review.

Django validates every state transition on the server. PostgreSQL transactions and appropriate locking enforce the three-vote limit under concurrent requests and prevent writes to a stage that has already closed. Clusters remain fixed while voting is open.

## Board synchronization

Use two-second polling while the retrospective meeting is active. Poll responses contain only data that the requesting member can access at the current stage. All participants gain access when the facilitator reveals feedback; individual screens update on their next poll.

Polling is an MVP simplicity choice. It provides eventual screen updates, not frame-exact simultaneous rendering. Avoid replacing active forms or disrupting local input during refreshes.

Server-sent events can be considered later if polling proves insufficient. A collaborative cursor system or complex simultaneous drag-and-drop experience is outside the current MVP.

## Background processing

1. An authorized user uploads audio, video, or a transcript to private storage.
2. Django records the upload and processing state in PostgreSQL and queues a Celery job after the database transaction commits.
3. A worker transcribes audio or video when necessary. Transcript uploads skip transcription.
4. A worker extracts proposed actions and decisions from the transcript.
5. Django displays processing progress or a recoverable error, then presents successful results as drafts.
6. The facilitator reviews, corrects, assigns action owners and optional due dates, and publishes the outcomes.

Suggested clustering also runs as a background job and must preserve category boundaries. Suggestions do not silently replace facilitator edits.

Jobs should support bounded retries and safe repeat execution without creating duplicate outcomes. A processing failure must not prevent the team from continuing the retrospective. AI and transcription providers, accepted file formats, upload limits, and media conversion requirements remain implementation decisions.

## Deployment shape

Deploy a Django web process and Celery worker using the same application build, with PostgreSQL, Redis, and private S3 storage available to both as needed. Credentials stay on the server and worker.

The hosting provider is not yet selected. A VM or container-based host is compatible with this design. Provider selection, backups, retention, monitoring, and production resource sizing remain implementation decisions.

## Rationale and tradeoffs

This stack keeps application logic in Python and uses Django's existing authentication, ORM, migrations, and admin capabilities. Server-rendered pages reduce the amount of frontend infrastructure needed for the first version. Celery keeps long-running media and AI work outside normal web requests.

React with a separate Django backend was considered. It offers more frontend flexibility but adds a separate application and interface contracts to maintain. The facilitator-led board does not currently justify that complexity.

Server-sent events were considered for synchronization. Two-second polling is the initial choice because it is simpler to implement and operate. Its tradeoff is a short delay before participants see meeting changes.

Celery with Redis is the selected background-processing approach; Django-Q was an alternative in the reference architecture. Keeping media processing in a dedicated worker makes its lifecycle independent of web requests.

## Outside this architecture's MVP scope

- Built-in recording or meeting-platform integrations.
- A separate React application.
- A microservices architecture.
- Collaborative cursors or unrestricted simultaneous board editing.
- External task-management integrations, chat, custom retrospective frameworks, and advanced analytics.
