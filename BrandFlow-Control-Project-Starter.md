# BrandFlow Control

## Project Starter

BrandFlow Control is a multi-user web application for managing brand content operations across social, ecommerce, and creator platforms through compliant integrations, AI-assisted content workflows, scheduling, approvals, publishing, and analytics.

---

## 1. Product goal

Build a secure online platform that allows company team members to:

- connect approved brand accounts
- create and manage content from one place
- adapt content per platform
- schedule and publish content
- manage assets and campaigns
- review approvals and activity history
- monitor analytics and performance

This product must be designed around official APIs, human-supervised automation, role-based permissions, and auditable actions.

---

## 2. What BrandFlow Control will do

### Core capabilities
- Multi-user workspace for company members
- Brand and account management
- Content creation studio
- AI-assisted captions, titles, hashtags, and descriptions
- Content calendar and recurring schedules
- Media library for images, videos, thumbnails, and documents
- Approval workflow before publishing
- Analytics dashboard by platform and campaign
- Shopify and Etsy product-to-content workflows
- Gmail notifications and approval routing

### Safe automation scope
- Create drafts automatically from campaign or store data
- Rewrite content for different platforms
- Queue scheduled content for approved channels
- Generate weekly or monthly recap content
- Notify team members when action is needed

### Explicitly excluded
- bulk account farming
- deceptive follower growth automation
- auto-follow or add-people growth loops
- bypassing platform signup protections
- stealth browser automation against platform terms

---

## 3. MVP definition

### Phase 1 MVP
- User authentication
- Organizations and team roles
- Brand profiles
- Connected account management
- Content studio
- AI content generator
- Media library
- Calendar and scheduling
- Approval workflow
- Publish logs
- Basic analytics dashboard
- Initial connectors:
  - Gmail
  - Shopify
  - Facebook Pages
  - Instagram Business
  - LinkedIn
  - X
  - YouTube metadata support

### Phase 2
- Etsy support
- TikTok support where officially permitted
- Twitch notification workflows
- Cross-channel campaign bundles
- Better analytics and attribution

### Phase 3
- Recommendation engine
- White-label capability
- Advanced workflow automations
- Revenue attribution expansion

---

## 4. First technical decisions

### Frontend
- Next.js
- TypeScript
- Tailwind CSS
- shadcn/ui

### Backend
- NestJS or FastAPI
- REST API
- background worker system
- webhook receivers

### Data and infra
- PostgreSQL
- Redis
- object storage for media
- queue processing with BullMQ or equivalent

### AI layer
- content generation
- platform adaptation
- content scoring
- best-time suggestions
- draft reply assistance

---

## 5. System modules

### A. Identity and access
Handles:
- login
- MFA
- team users
- roles and permissions
- audit trail

### B. Workspace and brands
Handles:
- organization settings
- multiple brands per company
- brand voice profiles
- timezone defaults
- campaign settings

### C. Connected accounts
Handles:
- OAuth connections
- token refresh
- permission scopes
- account sync status
- per-platform capability mapping

### D. Content studio
Handles:
- master content creation
- platform-specific variants
- AI-generated copy
- CTA generation
- hashtag and tag support
- asset attachment

### E. Scheduling engine
Handles:
- one-time schedules
- recurring schedules
- timezone-aware publishing
- staggered posting
- retry logic
- failure handling

### F. Approval workflow
Handles:
- draft review
- approval requests
- rejection comments
- role-based publishing control

### G. Analytics
Handles:
- post performance
- campaign summaries
- best posting windows
- engagement trends
- channel comparisons

---

## 6. User roles

### Owner
- full system control
- billing
- workspace settings
- account connections
- role assignment

### Admin
- manage users
- manage brands
- approve publishing
- view analytics
- manage campaigns

### Content Manager
- create and edit content
- schedule drafts
- manage content calendar

### Designer
- upload and manage assets
- prepare media packages

### Reviewer
- approve or reject drafts
- leave review notes

### Analyst
- view performance and reports

---

## 7. Initial database model

### Core tables
- organizations
- users
- roles
- user_roles
- brands
- connected_accounts
- platform_tokens
- content_items
- content_variants
- media_assets
- campaigns
- scheduled_jobs
- approval_requests
- publish_logs
- analytics_snapshots
- notifications
- audit_logs

### Minimal schema notes
- `organizations` owns brands and users
- `brands` own connected accounts and content
- `content_items` represent the master content object
- `content_variants` represent per-platform versions
- `scheduled_jobs` represent planned publish actions
- `publish_logs` capture success, failure, and raw connector responses

---

## 8. Initial connector strategy

Each platform must be implemented through a connector adapter so the rest of the app does not depend on platform-specific logic.

### Standard connector interface
```ts
interface PlatformConnector {
  connectAccount(): Promise<ConnectResult>;
  refreshAuth(): Promise<void>;
  createDraft(input: ContentPayload): Promise<DraftResult>;
  publishNow(input: PublishPayload): Promise<PublishResult>;
  schedulePost(input: SchedulePayload): Promise<ScheduleResult>;
  fetchAnalytics(input: AnalyticsRequest): Promise<AnalyticsResult>;
  listAccounts(): Promise<AccountSummary[]>;
  validateMedia(input: MediaValidationRequest): Promise<ValidationResult>;
}
```

### Support tiers
- Full Support; posting, scheduling, analytics
- Partial Support; some actions only
- Manual Assist; generate content and notify human to post
- Unsupported; no connector until compliant support exists

---

## 9. Initial UI structure

### Main navigation
- Dashboard
- Content Studio
- Calendar
- Campaigns
- Accounts
- Assets
- Analytics
- Team
- Audit Log
- Settings

### Dashboard widgets
- posts due today
- approval queue
- failed publish jobs
- account connection health
- top-performing content
- content generation queue
- campaign status summary

---

## 10. BrandFlow Control working principles

1. API-first where possible
2. Human approval for sensitive publishing
3. Editable AI output, never blind autopost by default
4. Full activity logging
5. Connector isolation per platform
6. Failure-safe retries and alerts
7. Team permissions at every critical action point

---

## 11. Build order

### Step 1
Create the product foundation:
- repository
- monorepo or app structure
- auth system
- database schema
- basic workspace setup

### Step 2
Build the core app shell:
- dashboard layout
- navigation
- role-aware access
- brand settings pages

### Step 3
Build content operations:
- content studio
- media library
- variant generation
- calendar UI

### Step 4
Build execution layer:
- scheduling engine
- worker queue
- publish logs
- notifications

### Step 5
Build first connectors:
- Gmail
- Shopify
- Meta
- LinkedIn
- X
- YouTube metadata flow

### Step 6
Build analytics and reporting:
- account health
- campaign reporting
- channel comparison

---

## 12. Immediate next implementation tasks

### Product tasks
- finalize MVP scope
- define platform support matrix
- define role permissions
- define approval rules
- define content lifecycle states

### Technical tasks
- choose backend framework
- choose auth provider
- choose monorepo structure
- design initial PostgreSQL schema
- define connector contract
- define job queue model
- define asset storage strategy

### UX tasks
- define information architecture
- wireframe dashboard
- wireframe content studio
- wireframe calendar
- wireframe account connection center

---

## 13. Suggested repository structure

```text
brandflow-control/
  apps/
    web/
    api/
    worker/
  packages/
    ui/
    types/
    config/
    connectors/
    ai/
    utils/
  infra/
    docker/
    migrations/
  docs/
    product/
    architecture/
    api/
```

---

## 14. Suggested first docs to create after this one

- `docs/product/mvp-scope.md`
- `docs/product/platform-support-matrix.md`
- `docs/architecture/system-overview.md`
- `docs/architecture/database-schema.md`
- `docs/architecture/connector-contract.md`
- `docs/ux/app-navigation.md`
- `docs/ux/role-permissions.md`
- `docs/operations/publishing-workflow.md`

---

## 15. Short positioning statement

BrandFlow Control is a multi-user AI-assisted brand operations platform that helps teams create, adapt, approve, schedule, publish, and analyze content across supported channels from one controlled workspace.
