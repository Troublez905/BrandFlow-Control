# BrandFlow Control MVP PRD

## 1. Product Summary

**BrandFlow Control** is a multi-user web application for companies that need to manage content operations across social, ecommerce, and creator platforms from a single control center. It is designed for internal teams that create, review, schedule, publish, and analyze brand content across multiple channels while maintaining governance, visibility, and operational consistency.

The product solves the operational problem of fragmented brand publishing. Most teams currently switch between native platform dashboards, spreadsheets, chat threads, cloud drives, and ad hoc approvals to run content programs. BrandFlow Control centralizes those workflows into one application.

The intended users for v1 are:
- small to mid-sized companies with multi-person marketing or content teams
- founders or owners managing one or more brands
- agencies or internal teams coordinating publishing across several accounts
- content managers who need structured workflows, reusable content, and a scheduling layer
- reviewers and executives who need visibility into publishing status and failures

BrandFlow Control v1 is a **human-supervised social operations platform**, not a system for deceptive automation or platform abuse. It focuses on legitimate, role-based content operations using official integrations or safe workflow fallbacks.

---

## 2. Goals

### Primary goals for v1
- Provide a secure multi-user workspace for company content operations.
- Allow one organization to manage one or more brands from a single application.
- Let authorized users connect supported platform accounts using official authentication methods.
- Enable users to create one master content item and generate platform-specific variants.
- Support scheduled publishing for officially supported platforms.
- Introduce an approval workflow so content can be reviewed before publishing.
- Give owners and admins visibility into publishing health, failed posts, and upcoming schedule.
- Store and organize media assets for reuse across campaigns.
- Provide baseline analytics and activity reporting for content operations.
- Reduce operational friction compared with manually posting across many separate tools.

### Success criteria for v1
- A team can sign in, create a workspace, invite users, and assign roles.
- A content manager can create and schedule content for multiple connected accounts.
- A reviewer can approve or reject content before publication.
- An owner can see what is scheduled, what published successfully, and what failed.
- The system can support repeated internal use without workflow confusion or permission ambiguity.
- The product can operate as a controlled internal publishing system for a real company brand.

---

## 3. Non-Goals

The following are intentionally excluded from v1:

- automated account creation on third-party platforms
- bypassing anti-bot systems, CAPTCHA, or signup protections
- auto-follow, follow-unfollow, or follower harvesting systems
- deceptive engagement automation
- mass account farming
- stealth browser automation against unsupported platforms
- fake identity scaling or profile multiplication
- autonomous public reply engines without review
- paid ad buying and campaign budget automation
- customer support automation across all external channels
- full CRM replacement
- influencer outreach automation
- unsupported platform scraping as a primary system dependency
- marketplace reputation manipulation
- deep social listening or sentiment intelligence beyond basic analytics
- advanced cross-platform attribution modeling in v1

BrandFlow Control v1 is not a growth manipulation tool. It is a controlled brand operations product.

---

## 4. User Roles

## Owner
The highest-level workspace user. Owns the organization, manages high-level settings, visibility, and platform governance.

**Typical responsibilities**
- create and manage the workspace
- view all brands, accounts, users, analytics, and publishing history
- override approvals when necessary
- inspect failed posts and operational issues
- manage billing and core workspace configuration later

## Admin
Operational administrator with broad management permissions across brands, users, accounts, and workflows.

**Typical responsibilities**
- invite and manage users
- assign roles
- connect and disconnect accounts
- configure publishing rules
- manage approval chains
- monitor system health and operational queues

## Content Manager
Primary content operator who creates, edits, and schedules posts and campaigns.

**Typical responsibilities**
- create master content
- generate platform variants
- upload and manage media
- assign content to campaigns
- schedule content
- submit content for approval

## Reviewer
Approves or rejects content before publishing.

**Typical responsibilities**
- review platform variants
- check brand tone and compliance
- approve or reject scheduled items
- leave feedback on drafts

## Analyst
Read-focused role for analytics, performance review, and reporting.

**Typical responsibilities**
- inspect campaign and post-level analytics
- review publishing history
- compare platform performance
- export reports later
- identify content trends and timing insights

---

## 5. Core User Stories

### Workspace and access
- As an owner, I can create a workspace for my company so the team can operate from one system.
- As an admin, I can invite company members and assign them roles so access is controlled.
- As an owner, I can view activity across all brands and accounts so I understand operational status.

### Connected accounts
- As an admin, I can connect a company Instagram account so content can be scheduled from BrandFlow Control.
- As an admin, I can connect Facebook Pages, LinkedIn, X, YouTube, Shopify, Etsy, and Gmail where supported so the system has a single connection layer.
- As an admin, I can see token health and connection status so I know when an account needs reconnection.

### Content creation
- As a content manager, I can create one master post and adapt it to multiple platforms so I do not rewrite everything from scratch.
- As a content manager, I can use AI assistance to generate captions, titles, tags, and descriptions so my workflow is faster.
- As a content manager, I can upload images and attach them to content so posts are media-ready.

### Scheduling and approval
- As a content manager, I can schedule content for a future time so publishing is planned in advance.
- As a reviewer, I can approve or reject scheduled posts so only approved content goes live.
- As an admin, I can configure content to require review before publication so governance is enforced.

### Publishing and monitoring
- As an owner, I can see which posts failed to publish so I can resolve issues quickly.
- As an admin, I can review publish logs and retry failures where appropriate so the publishing pipeline is reliable.
- As a team member, I can view upcoming scheduled content in a calendar so the campaign plan is visible.

### Assets and campaigns
- As a team member, I can upload media and assign it to campaigns so assets stay organized.
- As a content manager, I can reuse assets across multiple content items so production is efficient.

### Analytics
- As an analyst, I can view baseline post and platform performance so I can identify what works.
- As an owner, I can see high-level trends across brands and channels so I can measure operational output.

---

## 6. Functional Requirements

### Authentication
- The system shall support secure user authentication for company members.
- The system shall support email/password login for v1.
- The system shall support password reset workflows.
- The system shall support optional MFA or reserve it for immediate post-v1 rollout.
- The system shall create user sessions securely.
- The system shall restrict access to workspace data based on organization membership.

### Organization / Workspaces
- The system shall allow creation of an organization workspace.
- The system shall allow an organization to contain multiple brands.
- The system shall allow users to be invited to a workspace.
- The system shall allow role assignment per user.
- The system shall maintain an audit trail of high-impact actions.

### Brand Management
- The system shall allow admins and owners to create and edit brand profiles.
- The system shall store brand-level metadata such as name, timezone, default tone, and content preferences.
- The system shall let content and connected accounts be associated with a specific brand.
- The system shall support one or more brands within the same organization.

### Connected Accounts
- The system shall provide a connected account center.
- The system shall support OAuth or official auth flows where available.
- The system shall store account connection status and token metadata securely.
- The system shall validate permissions and scopes for connected platforms where possible.
- The system shall display connection errors and required reconnect actions.
- The system shall track platform-specific capabilities by account.

### Content Creation
- The system shall allow creation of a master content item.
- The system shall allow generation of platform-specific variants from the master content.
- The system shall allow manual editing of every variant.
- The system shall support text, media attachments, titles, descriptions, hashtags, and CTA fields where relevant.
- The system shall support draft, in-review, approved, scheduled, published, failed, and archived states.
- The system shall provide AI-assisted copy generation and platform adaptation.

### Scheduling
- The system shall allow a user to schedule a post for a specific date and time.
- The system shall support timezone-aware scheduling at the brand level.
- The system shall prevent duplicate or invalid scheduling states.
- The system shall display all scheduled items in calendar and list views.
- The system shall enqueue publishing jobs for worker processing.

### Approvals
- The system shall support optional approval requirements before publication.
- The system shall allow reviewers to approve or reject content variants.
- The system shall store review decisions, timestamps, and notes.
- The system shall prevent unapproved content from being published when approval is required.
- The system shall notify relevant users when review status changes.

### Publishing
- The system shall publish content to supported platforms using official or safe integration methods.
- The system shall record each publish attempt in a publish log.
- The system shall store publish success, failure, and retry status.
- The system shall allow admins to inspect publish errors.
- The system shall support manual fallback flows where direct API publishing is not supported.

### Analytics
- The system shall display baseline analytics for supported platforms.
- The system shall show content status metrics such as drafts, scheduled, approved, published, and failed.
- The system shall show per-platform and per-brand publishing performance summaries.
- The system shall store analytics snapshots for trend comparisons.
- The system shall distinguish between publishing analytics and audience analytics where applicable.

### Notifications
- The system shall notify relevant users of review requests, approval decisions, publishing failures, and connection problems.
- The system shall support in-app notifications.
- The system shall support Gmail-based notification or approval workflows where applicable.
- The system shall avoid sending duplicate notifications for the same event.

---

## 7. Platform Support Matrix

| Platform | V1 Support | Notes |
|---|---|---|
| Gmail | Notifications / approvals | Full safe support |
| Facebook Pages | Scheduled posts | Official API path |
| Instagram Business | Scheduled posts | Via Meta |
| LinkedIn | Scheduled posts | Business/professional use |
| X | Scheduled posts | Depends on API tier |
| YouTube | Metadata / promo workflows | Not full social parity |
| Shopify | Product-triggered content workflows | Ecommerce integration |
| Etsy | Partial | Depends on supported endpoints |

### Matrix interpretation
- **Full publishing support** means BrandFlow Control can create, schedule, and track supported content through official paths.
- **Partial support** means the system may provide content generation, workflow support, metadata preparation, or selective integration rather than full native publishing.
- **Manual-assist support** may be used for channels where direct automation is limited or unstable.

---

## 8. Data Model

Core entities for v1:

- `organizations`
- `users`
- `organization_memberships`
- `roles`
- `brands`
- `connected_accounts`
- `platform_tokens`
- `content_items`
- `content_variants`
- `media_assets`
- `campaigns`
- `scheduled_jobs`
- `approval_requests`
- `approval_actions`
- `publish_logs`
- `analytics_snapshots`
- `notifications`
- `activity_logs`
- `platform_capabilities`

### Entity purpose summary

**organizations**  
Stores company workspace records.

**users**  
Stores application users.

**organization_memberships**  
Maps users to organizations and assigned roles.

**roles**  
Defines authorization profiles.

**brands**  
Stores brand-level configuration inside an organization.

**connected_accounts**  
Stores connected external platform accounts by brand.

**platform_tokens**  
Stores encrypted auth tokens and refresh metadata.

**content_items**  
Stores master content entries.

**content_variants**  
Stores platform-specific versions of content.

**media_assets**  
Stores uploaded images, video references, and metadata.

**campaigns**  
Groups content and assets into a campaign structure.

**scheduled_jobs**  
Stores publishing jobs and their execution status.

**approval_requests**  
Tracks content sent for approval.

**approval_actions**  
Tracks reviewer decisions and notes.

**publish_logs**  
Records platform publishing attempts and responses.

**analytics_snapshots**  
Stores periodic analytics data for reporting.

**notifications**  
Stores in-app and email notification events.

**activity_logs**  
Stores auditable user and system actions.

**platform_capabilities**  
Defines which actions are supported for each connected platform.

---

## 9. Permissions Model

### Owner
Can:
- view and manage the full workspace
- manage all brands
- invite and remove users
- assign or change roles
- connect or disconnect accounts
- view all analytics
- view all publish logs and failures
- override approval workflows
- manage system-wide settings

Cannot:
- bypass platform permission limits imposed by external services

### Admin
Can:
- manage brands
- manage connected accounts
- invite users where allowed by owner policy
- create and manage campaigns
- configure approvals
- inspect publishing jobs and failures
- view analytics
- edit settings within delegated scope

Cannot:
- take ownership of the workspace unless explicitly transferred

### Content Manager
Can:
- create master content
- edit platform variants
- upload assets
- assign content to campaigns
- schedule content
- submit content for approval
- view status of their content and assigned brands

Cannot:
- approve their own content if policy forbids it
- manage workspace-wide roles unless granted separately
- change high-level system settings

### Reviewer
Can:
- review submitted content
- approve or reject content
- leave review notes
- view scheduled items requiring review
- inspect final platform variants before approval

Cannot:
- manage connected accounts unless separately authorized
- alter workspace settings
- publish restricted content outside approval rules

### Analyst
Can:
- view analytics dashboards
- review publish history
- inspect performance summaries
- view campaign performance
- export reporting later in post-v1

Cannot:
- publish content
- approve content
- connect accounts
- edit workspace configuration

### Permission principles
- least privilege by default
- role-based access with future room for per-brand overrides
- auditable actions for sensitive operations
- restricted access to tokens, connection settings, and system logs

---

## 10. MVP Screens

## Login
Purpose:
- authenticate users into the system

Key elements:
- email field
- password field
- forgot password
- organization context if needed later
- MFA entry if enabled

## Dashboard
Purpose:
- provide an operational overview

Key elements:
- posts scheduled today
- pending approvals
- failed publish alerts
- connected account status
- recent activity
- analytics summary widgets

## Content Studio
Purpose:
- create and edit content

Key elements:
- master content editor
- platform variant tabs
- AI assist controls
- media attachments
- draft status
- campaign assignment
- review submission action

## Calendar
Purpose:
- visualize and manage publishing schedule

Key elements:
- day, week, month views
- scheduled posts
- approval state indicators
- drag-and-drop rescheduling later if feasible
- filter by brand, platform, campaign, and status

## Accounts
Purpose:
- manage connected platforms

Key elements:
- account connection cards
- connection status
- token health
- permission scope indicators
- reconnect actions
- supported capability labels

## Assets
Purpose:
- manage media library

Key elements:
- upload area
- folders or campaign grouping
- metadata
- image and video previews
- assignment to content items
- asset reuse tracking later

## Analytics
Purpose:
- view content and operational performance

Key elements:
- per-brand filters
- per-platform metrics
- publishing success vs failure
- top-performing content summaries
- trend cards
- baseline charts

## Settings
Purpose:
- configure workspace and brand behavior

Key elements:
- organization settings
- brand settings
- user and role management
- approval policies
- notification preferences
- future billing placeholder

---

## 11. Technical Architecture

### Frontend
Recommended:
- Next.js
- TypeScript
- Tailwind CSS
- component library such as shadcn/ui or equivalent

Responsibilities:
- authenticated app shell
- role-aware navigation
- content editing experience
- calendar views
- analytics dashboards
- notification center

### Backend
Recommended:
- NestJS or FastAPI
- REST API with clean service boundaries
- background worker services for scheduling and publishing

Responsibilities:
- auth and authorization
- workspace and role logic
- content lifecycle management
- connector orchestration
- scheduling and publish job dispatch
- analytics ingestion and reporting

### Database
Recommended:
- PostgreSQL as primary relational database

Responsibilities:
- users and organizations
- brands and connected accounts
- content and scheduling records
- approval and publish logs
- analytics snapshots
- audit trails

### Queue / Jobs
Recommended:
- Redis-backed queue such as BullMQ
- worker processes for scheduled publishing and retries

Responsibilities:
- scheduled publish execution
- retry handling
- token refresh tasks
- analytics sync jobs
- notification dispatch

### Storage
Recommended:
- S3-compatible object storage

Responsibilities:
- uploaded images
- videos or media references
- thumbnails
- generated export assets later

### Auth
Recommended:
- Auth.js, Clerk, or custom auth layer depending on stack direction

Responsibilities:
- secure login
- session management
- password reset
- optional MFA
- organization membership enforcement

### AI Services
Recommended:
- AI provider for copy generation and platform adaptation
- prompt layer with guardrails and structured outputs

Responsibilities:
- generate post drafts
- rewrite content by platform
- suggest hashtags, titles, descriptions, and CTAs
- maintain editable outputs rather than autonomous publishing decisions

### Connector Pattern
Each external platform should be implemented behind a connector abstraction so every integration can declare:
- auth method
- supported actions
- publishing capability
- analytics capability
- media requirements
- token lifecycle rules
- error handling rules

---

## 12. Launch Plan

### Stage 1: Internal Alpha
Audience:
- founder and a very small internal test group

Objectives:
- validate login, roles, brands, content creation, and scheduling workflow
- test one or two platform connectors deeply
- verify publish logging and failure visibility
- identify workflow confusion in UI and permissions

Exit criteria:
- users can reliably create, review, and schedule content
- account connections are stable enough for repeated testing
- no major permission or audit gaps remain

### Stage 2: Team Beta
Audience:
- broader internal company team or selected partner team

Objectives:
- run BrandFlow Control in day-to-day content operations
- validate approval flows across multiple users
- test calendar usage under real content load
- compare native-platform workflow time versus BrandFlow Control workflow time
- refine analytics summaries and notification usefulness

Exit criteria:
- content teams can use the product without workarounds for core flows
- failed publishes are diagnosable
- platform support limitations are clearly communicated in-product
- team role boundaries behave correctly

### Stage 3: First Production Use
Audience:
- the company's real publishing operation for approved brands

Objectives:
- publish live content through supported channels
- measure operational reliability
- verify auditability and content governance
- stabilize the support model for connected accounts and scheduled publishing

Production readiness requirements:
- strong logging
- stable queue processing
- connection health monitoring
- clear fallback behavior for unsupported or failed publishing actions
- documented internal operating procedures for team use

---

## Closing Definition

BrandFlow Control v1 is a controlled, multi-user content operations platform for legitimate brand publishing across supported channels. Its value comes from centralization, workflow governance, scheduling, AI assistance, and operational visibility. The MVP should remain tightly scoped, connector-aware, and safe by design.
