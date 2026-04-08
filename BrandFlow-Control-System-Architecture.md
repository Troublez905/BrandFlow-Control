# BrandFlow Control System Architecture

## Correct Build Order

1. Project starter `.md`
2. MVP PRD
3. System architecture
4. Database schema
5. API routes
6. UI wireframes
7. Repo scaffold
8. Connector implementation

---

## 1. Purpose

This document defines the technical architecture for **BrandFlow Control**, a multi-user web application for controlled brand content operations across supported social, ecommerce, and creator platforms.

This architecture is designed for:
- multi-user internal company access
- multiple brands within one workspace
- official integrations first
- scheduled publishing and approval workflows
- auditability and role-based control
- connector-aware platform support
- AI-assisted content generation with human oversight

This document covers:
- tech stack
- service boundaries
- database schema
- queue architecture
- connector abstraction
- deployment model
- security model

---

## 2. Architecture Principles

### Primary principles
- **Official integrations first**; use supported APIs and approved auth flows.
- **Human-supervised automation**; AI assists creation and adaptation, but governance stays in user workflows.
- **Connector isolation**; each external platform is implemented independently behind a common interface.
- **Least privilege**; users and services receive only the permissions they need.
- **Auditability**; sensitive actions are logged.
- **Async reliability**; scheduled publishing and sync jobs run through a queue system.
- **Graceful degradation**; if a platform does not support a capability, the product falls back to manual-assist workflow rather than pretending support exists.
- **Multi-tenant correctness**; organizations, brands, accounts, and data boundaries must remain isolated.

---

## 3. System Overview

BrandFlow Control is best implemented as a web application with a browser-based frontend, a backend API, background workers, a relational database, object storage for media, and a connector layer for external platform communication.

### High-level component model
- frontend web app
- backend API service
- auth/session layer
- database
- Redis-backed queue
- worker service
- media storage
- AI service integration
- connector modules
- notification service
- analytics ingestion/snapshot jobs

### High-level request flow
1. User signs in to the web app.
2. Frontend calls backend APIs.
3. Backend validates identity, organization membership, and permissions.
4. Backend reads or writes data to PostgreSQL.
5. For scheduled or async work, backend enqueues jobs into Redis-backed queues.
6. Worker processes jobs such as publishing, token refresh, analytics sync, and notifications.
7. Connector modules communicate with external platforms using official APIs or safe manual-assist logic.
8. Results are logged back into the database and surfaced in the UI.

---

## 4. Recommended Tech Stack

## Frontend
**Recommended**
- Next.js
- TypeScript
- Tailwind CSS
- component library such as shadcn/ui or equivalent
- TanStack Query for server state
- React Hook Form for forms
- Zod for validation

**Why**
- strong developer velocity
- suitable for dashboard-style applications
- good routing and auth patterns
- strong TypeScript ecosystem
- deployable on modern app platforms without unnecessary complexity

## Backend
**Recommended**
- NestJS with TypeScript

**Alternative**
- FastAPI with Python if the team prefers Python-heavy backend development

**Why NestJS is preferred here**
- strong structure for modular services
- clean controller/service/provider architecture
- good fit for multi-module enterprise apps
- straightforward queue integration
- aligned language consistency with Next.js frontend

## Database
**Recommended**
- PostgreSQL

**Why**
- strong relational modeling
- reliable transactional support
- good fit for multi-tenant app data
- excellent compatibility with reporting, analytics snapshots, and audit trails

## ORM / Query Layer
**Recommended**
- Prisma for faster early-stage development

**Alternative**
- Drizzle ORM if tighter SQL control is desired

## Queue / Jobs
**Recommended**
- Redis
- BullMQ

**Why**
- supports delayed jobs, retries, and worker patterns
- suitable for publishing pipelines and analytics sync
- easy to reason about for MVP and growth stages

## Object Storage
**Recommended**
- S3-compatible storage

**Use cases**
- uploaded images
- video references and thumbnails
- generated export assets
- processed media metadata

## Authentication
**Recommended**
- Auth.js or Clerk for v1
- MFA support either in v1 or immediately after v1

## AI Services
**Recommended**
- model provider for text generation with structured outputs
- optional image captioning or metadata enrichment later

**Use cases**
- caption generation
- platform adaptation
- hashtag suggestion
- CTA generation
- metadata assistance
- content scoring

## Monitoring / Logging
**Recommended**
- structured application logs
- Sentry or equivalent for errors
- basic uptime and queue health monitoring
- database and worker observability

## Infrastructure / Deployment
**Recommended**
- Vercel for frontend
- Render, Fly.io, Railway, or AWS for backend and workers
- managed PostgreSQL
- managed Redis
- managed S3-compatible storage

---

## 5. Service Boundaries

The system should be modular even if initially deployed as a smaller number of services.

### A. Web Frontend
**Responsibilities**
- authenticated user interface
- role-aware navigation
- content editing and scheduling screens
- analytics dashboards
- account connection flows
- notification display

**Does not own**
- business logic
- token storage
- direct connector logic
- queue execution

### B. API Service
**Responsibilities**
- REST endpoints
- authentication and authorization
- organization and brand management
- content lifecycle management
- scheduling orchestration
- approval workflows
- publish log retrieval
- analytics query endpoints
- connector orchestration entry points

**Does not own**
- long-running publish execution
- long-running analytics sync
- media binary storage itself

### C. Worker Service
**Responsibilities**
- execute scheduled publish jobs
- retry failed publish jobs according to policy
- refresh tokens where needed
- pull analytics snapshots
- send notifications
- run safe automation recipes such as product-triggered draft creation

**Does not own**
- interactive UI logic
- user session management

### D. Connector Layer
**Responsibilities**
- platform-specific auth handling
- payload transformation
- capability declaration
- API request execution
- response normalization
- platform-specific error mapping

**Does not own**
- role logic
- global scheduling policy
- workspace-level permissions

### E. Media Service
This can initially be part of the API service.

**Responsibilities**
- upload authorization
- asset metadata persistence
- object storage references
- media validation rules
- linking assets to content items

### F. Notification Service
This can initially run inside workers.

**Responsibilities**
- in-app notification creation
- Gmail notifications where applicable
- event-driven notification dispatch
- de-duplication rules

### G. Analytics Aggregation Layer
This can initially be worker-driven.

**Responsibilities**
- pull supported metrics from connectors
- normalize analytics snapshots
- store trendable metrics
- produce dashboard-ready summaries

---

## 6. Proposed Module Breakdown

Inside the backend, organize code into clear modules.

### Core platform modules
- `auth`
- `users`
- `organizations`
- `memberships`
- `roles`
- `brands`
- `settings`
- `activity-logs`

### Content modules
- `content-items`
- `content-variants`
- `campaigns`
- `media-assets`
- `approvals`
- `scheduling`
- `publish-logs`

### Integration modules
- `connected-accounts`
- `platform-tokens`
- `connectors`
- `analytics`
- `notifications`

### Infrastructure modules
- `queue`
- `storage`
- `ai`
- `health`
- `metrics`

This keeps domain logic separated and makes connector growth manageable.

---

## 7. Database Schema

Below is the recommended relational schema direction for v1. Exact SQL can be defined in the next document, but the architecture needs the structure now.

## Core workspace tables

### `organizations`
Stores company workspaces.

**Fields**
- id
- name
- slug
- status
- created_at
- updated_at

### `users`
Stores user identities.

**Fields**
- id
- email
- password_hash or external_auth_id
- first_name
- last_name
- status
- mfa_enabled
- created_at
- updated_at

### `organization_memberships`
Maps users to organizations and roles.

**Fields**
- id
- organization_id
- user_id
- role_id
- membership_status
- invited_by_user_id
- created_at
- updated_at

### `roles`
Defines system roles.

**Fields**
- id
- key
- name
- description

## Brand and settings tables

### `brands`
Stores brand records under an organization.

**Fields**
- id
- organization_id
- name
- slug
- timezone
- default_tone
- default_language
- status
- created_at
- updated_at

### `brand_settings`
Stores brand-level preferences.

**Fields**
- id
- brand_id
- approvals_required
- default_posting_rules_json
- notification_rules_json
- created_at
- updated_at

## Integration tables

### `connected_accounts`
Stores connected external platform accounts.

**Fields**
- id
- brand_id
- platform
- external_account_id
- account_name
- account_handle
- connection_status
- last_sync_at
- capability_profile_id
- created_at
- updated_at

### `platform_tokens`
Stores encrypted access and refresh token references.

**Fields**
- id
- connected_account_id
- token_type
- encrypted_access_token
- encrypted_refresh_token
- expires_at
- scopes_json
- created_at
- updated_at

### `platform_capability_profiles`
Defines supported capabilities for a connected account or platform version.

**Fields**
- id
- platform
- can_publish
- can_schedule
- can_fetch_analytics
- can_upload_media
- can_create_drafts
- can_manage_comments
- notes
- created_at
- updated_at

## Content tables

### `campaigns`
Groups related content.

**Fields**
- id
- brand_id
- name
- description
- status
- start_date
- end_date
- created_by_user_id
- created_at
- updated_at

### `content_items`
Stores master content records.

**Fields**
- id
- brand_id
- campaign_id
- title
- master_text
- content_type
- status
- created_by_user_id
- created_at
- updated_at

### `content_variants`
Stores per-platform variants.

**Fields**
- id
- content_item_id
- platform
- variant_title
- variant_text
- hashtags_json
- metadata_json
- status
- generated_by_ai
- last_edited_by_user_id
- created_at
- updated_at

### `media_assets`
Stores uploaded media metadata.

**Fields**
- id
- brand_id
- storage_key
- asset_type
- mime_type
- file_size
- width
- height
- duration_seconds
- original_filename
- uploaded_by_user_id
- created_at
- updated_at

### `content_variant_media`
Join table linking assets to variants.

**Fields**
- id
- content_variant_id
- media_asset_id
- sort_order
- created_at

## Approval and scheduling tables

### `approval_requests`
Tracks items sent for review.

**Fields**
- id
- content_variant_id
- requested_by_user_id
- assigned_reviewer_user_id
- status
- due_at
- created_at
- updated_at

### `approval_actions`
Stores reviewer decisions.

**Fields**
- id
- approval_request_id
- reviewer_user_id
- action
- notes
- created_at

### `scheduled_jobs`
Stores publishing jobs.

**Fields**
- id
- content_variant_id
- connected_account_id
- scheduled_for
- job_type
- state
- retry_count
- last_attempt_at
- locked_at
- created_at
- updated_at

### `publish_logs`
Stores outcomes of publish attempts.

**Fields**
- id
- scheduled_job_id
- connected_account_id
- platform
- result
- external_post_id
- error_code
- error_message
- raw_response_json
- created_at

## Analytics and notification tables

### `analytics_snapshots`
Stores periodic analytics data.

**Fields**
- id
- connected_account_id
- content_variant_id
- snapshot_date
- metrics_json
- created_at

### `notifications`
Stores in-app and email notification events.

**Fields**
- id
- organization_id
- user_id
- type
- title
- body
- channel
- status
- related_entity_type
- related_entity_id
- created_at
- read_at

### `activity_logs`
Stores auditable user and system actions.

**Fields**
- id
- organization_id
- actor_user_id
- actor_type
- action
- entity_type
- entity_id
- metadata_json
- created_at

---

## 8. Data Relationships

### Core relationships
- one organization has many brands
- one organization has many memberships
- one user can belong to many organizations through memberships
- one brand has many connected accounts
- one brand has many campaigns
- one brand has many content items
- one content item has many content variants
- one content variant can have many media assets through a join table
- one content variant can have zero or more approval requests
- one content variant can have zero or more scheduled jobs
- one scheduled job can have many publish log records
- one connected account can have many analytics snapshots

### Design requirement
All tenant-sensitive queries must scope by `organization_id` either directly or through a trusted relationship path.

---

## 9. Queue Architecture

A queue system is required because publishing, analytics sync, notifications, and token maintenance should not run inline with user requests.

## Queue technology
- Redis
- BullMQ

## Core queues

### `publish-queue`
Handles scheduled and immediate publish jobs.

**Job types**
- publish_now
- publish_scheduled
- retry_publish

**Payload**
- scheduled_job_id
- organization_id
- brand_id
- connected_account_id
- content_variant_id

### `analytics-queue`
Handles analytics sync and snapshot jobs.

**Job types**
- sync_account_analytics
- sync_content_analytics
- snapshot_metrics

### `notification-queue`
Handles in-app or email notifications.

**Job types**
- send_review_request
- send_publish_failure
- send_connection_warning
- send_approval_result

### `token-maintenance-queue`
Handles connector auth maintenance.

**Job types**
- refresh_access_token
- mark_connection_expired
- revalidate_scopes

### `automation-queue`
Handles safe recipe-based actions.

**Job types**
- create_product_launch_draft
- generate_weekly_recap_draft
- enqueue_manual_publish_reminder

## Retry strategy
- use bounded retries
- classify retryable vs non-retryable errors
- exponential backoff for transient failures
- dead-letter or failed job inspection path for exhausted retries

## Idempotency requirements
Publishing jobs must be idempotent where possible. The system should prevent duplicate publish attempts caused by worker restarts or timing collisions.

## Queue observability
Track:
- queue depth
- failed jobs
- retry counts
- worker processing time
- oldest outstanding scheduled job
- publish success rate

---

## 10. Connector Abstraction

Every external platform must be isolated behind a common connector contract.

## Connector goals
- normalize the app-facing interface
- keep platform-specific code contained
- express capability differences honestly
- simplify testing and replacement
- avoid cross-platform logic leakage into the core domain

## Recommended interface

```ts
export interface PlatformConnector {
  platformKey: string;

  getCapabilities(): Promise<PlatformCapabilities>;

  connectAccount(input: ConnectAccountInput): Promise<ConnectAccountResult>;

  refreshAuth(input: RefreshAuthInput): Promise<RefreshAuthResult>;

  validateConnection(input: ValidateConnectionInput): Promise<ValidateConnectionResult>;

  createDraft(input: CreateDraftInput): Promise<CreateDraftResult>;

  publishNow(input: PublishNowInput): Promise<PublishResult>;

  schedulePost?(input: SchedulePostInput): Promise<ScheduleResult>;

  fetchAnalytics?(input: FetchAnalyticsInput): Promise<AnalyticsResult>;

  validateMedia(input: ValidateMediaInput): Promise<MediaValidationResult>;

  disconnectAccount(input: DisconnectAccountInput): Promise<DisconnectAccountResult>;
}
```

## Capability model
Each connector should declare whether it supports:
- auth
- publishing
- scheduling
- draft creation
- analytics
- media upload
- comment workflows
- profile metadata retrieval

## Connector output normalization
Normalize:
- success/failure status
- external identifiers
- error codes
- human-readable messages
- retryability
- rate-limit responses

## Connector modules to prepare for v1
- Gmail connector
- Meta connector for Facebook Pages and Instagram Business
- LinkedIn connector
- X connector
- YouTube connector
- Shopify connector
- Etsy connector

## Connector execution rule
No frontend code should communicate directly with external platforms. All connector actions must run through backend services.

## Unsupported capability behavior
If a connector cannot safely support an action:
- do not fake support
- surface a capability-disabled state in the UI
- offer manual-assist workflow if appropriate

---

## 11. Deployment Model

## Recommended MVP deployment layout

### Frontend
Deploy Next.js frontend separately.

**Suggested options**
- Vercel
- Netlify if needed, though Vercel is usually cleaner for Next.js

### Backend API
Deploy backend as a containerized service.

**Suggested options**
- Render
- Fly.io
- Railway
- AWS ECS / App Runner if moving toward stronger control

### Worker Service
Deploy workers as a separate containerized process using the same codebase with a different process entrypoint.

### Database
Use managed PostgreSQL.

### Redis
Use managed Redis.

### Storage
Use S3-compatible object storage.

## Environment separation
Maintain separate:
- local development
- staging
- production

Each environment should have:
- separate databases
- separate Redis instances or namespaces
- separate storage buckets/prefixes
- separate connector app credentials when required

## Deployment flow
- merge into main branch
- run CI checks
- deploy frontend
- deploy API
- deploy worker
- run database migrations
- run smoke tests

## CI/CD requirements
- lint
- typecheck
- unit tests
- migration safety checks
- environment validation
- optional integration tests for connectors with mocks

---

## 12. Security Model

Security is central because the system stores organization data, content, platform tokens, scheduling workflows, and user roles.

## Identity and access
- secure password hashing
- MFA support
- short-lived sessions where possible
- role-based authorization
- per-organization access scoping
- future support for SSO if needed

## Token security
- encrypt access and refresh tokens at rest
- restrict token visibility to service layers only
- never expose raw tokens to frontend clients
- rotate credentials when required
- store minimum scopes required

## Application security
- input validation on every endpoint
- output encoding where necessary
- CSRF protection if relevant to chosen auth approach
- rate limiting on auth and sensitive endpoints
- audit logging for sensitive actions
- secret management through environment vault or platform secret store

## Data isolation
- every tenant-sensitive query must scope through organization membership
- forbid cross-organization record access even if IDs are guessed
- enforce authorization at service layer, not only UI layer

## Media security
- signed upload URLs
- file type validation
- file size limits
- malware scanning layer later if needed
- restricted direct access policies

## Queue security
- workers validate organization and record existence before executing jobs
- job payloads should reference internal IDs, not include sensitive tokens directly
- connector secrets fetched only server-side during execution

## Audit coverage
Audit these actions at minimum:
- user invitations
- role changes
- connected account connects/disconnects
- approval decisions
- content publication attempts
- settings changes
- manual retries of failed jobs

## Security boundaries
The system must not implement:
- mass account creation
- anti-bot bypass logic
- follower farming systems
- unauthorized automation against unsupported platforms
- deceptive identity orchestration

---

## 13. API Boundary Direction

The detailed route list belongs in the next build document, but the architecture should define domain boundaries now.

### Expected route groups
- `/auth`
- `/organizations`
- `/memberships`
- `/brands`
- `/accounts`
- `/campaigns`
- `/content`
- `/approvals`
- `/schedule`
- `/assets`
- `/analytics`
- `/notifications`
- `/settings`
- `/health`

### API style
- REST is sufficient for v1
- typed request and response DTOs
- strict validation
- pagination on list endpoints
- filterable list endpoints for dashboards and calendar views

---

## 14. Scalability Notes

The MVP does not need premature microservices, but the architecture should avoid coupling that blocks growth.

### Keep simple now
- one frontend app
- one backend API service
- one worker service
- one database
- one Redis
- one object store

### Scale later by splitting
- connector-heavy workloads into dedicated workers
- analytics sync into its own worker pool
- media processing into separate services
- read replicas for analytics-heavy dashboards
- event-driven architecture if usage grows substantially

The key is modular code first, service split later.

---

## 15. Build Sequence After This File

After `BrandFlow-Control-System-Architecture.md`, the practical build sequence remains:

1. database schema document
2. API routes document
3. UI wireframes
4. repo scaffold
5. connector implementation
6. queue workers
7. auth and role enforcement
8. publishing and analytics validation

---

## 16. Final Architecture Position

BrandFlow Control should be built as a connector-aware, multi-tenant web platform with a React-based frontend, a structured TypeScript backend, a relational database, async worker processing, and strong access control. The architecture should optimize for operational clarity, safe automation, and reliable publishing workflows rather than brittle over-automation.
