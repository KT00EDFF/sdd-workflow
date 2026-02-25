# Product Catalog Domain Reference

> This document defines the domain model, relationships, regulatory context, and architecture for the JPMC Product Catalog system. It serves as grounding context for the SDD Companion GPT.

---

## Domain Overview

The Product Catalog is the authoritative system for managing JPMC's product taxonomy, billing codes, and product configurations. It serves as the master reference for downstream systems including billing, pricing, reporting, and compliance.

**Architecture**: API-first (REST/OpenAPI 3.1). All product data is accessed and mutated through versioned APIs. No direct database access from consumers.

---

## Core Domain Entities

### Product

The top-level marketable unit offered to clients.

| Field | Type | Description |
|-------|------|-------------|
| `productId` | UUID | Immutable identifier |
| `name` | string | Display name |
| `description` | string | Client-facing description |
| `categoryId` | UUID | FK to ProductCategory |
| `lifecycleState` | LifecycleState | Current lifecycle phase |
| `configurationProfileId` | UUID | FK to ConfigurationProfile |
| `effectiveDate` | date | When the product becomes available |
| `expirationDate` | date | When the product sunsets (nullable) |
| `createdBy` | string | Audit: creator identity |
| `createdAt` | timestamp | Audit: creation time |
| `lastModifiedBy` | string | Audit: last modifier |
| `lastModifiedAt` | timestamp | Audit: last modification |

### ProductCategory

Hierarchical classification grouping related products.

| Field | Type | Description |
|-------|------|-------------|
| `categoryId` | UUID | Immutable identifier |
| `name` | string | Category name |
| `parentCategoryId` | UUID | FK to parent (nullable for root) |
| `level` | integer | Depth in hierarchy (0 = root) |
| `sortOrder` | integer | Display ordering |
| `isActive` | boolean | Soft-delete flag |

**Constraint**: Categories form a strict tree (no cycles). Maximum depth: 5 levels.

### TaxonomyNode

A node in the product classification taxonomy. Taxonomy provides the canonical vocabulary for how products are organized across the enterprise.

| Field | Type | Description |
|-------|------|-------------|
| `nodeId` | UUID | Immutable identifier |
| `code` | string | Machine-readable taxonomy code (e.g., `FX.SPOT.G10`) |
| `label` | string | Human-readable name |
| `parentNodeId` | UUID | FK to parent node (nullable for root) |
| `path` | string | Materialized path (e.g., `/FX/SPOT/G10`) |
| `isLeaf` | boolean | Whether products can be assigned here |
| `status` | enum | `ACTIVE`, `DEPRECATED`, `ARCHIVED` |

**Immutability Rule**: Taxonomy nodes are never deleted or renamed once published. Deprecated nodes are marked `DEPRECATED` with a `supersededBy` pointer. This ensures historical references remain valid.

### BillingCode

A code that maps product usage to billing line items.

| Field | Type | Description |
|-------|------|-------------|
| `billingCodeId` | UUID | Immutable identifier |
| `code` | string | The billing code (e.g., `BC-FX-001`) |
| `description` | string | What this code represents |
| `productId` | UUID | FK to Product (nullable for shared codes) |
| `taxonomyNodeId` | UUID | FK to TaxonomyNode |
| `billingRuleId` | UUID | FK to BillingRule |
| `activationDate` | date | When billing begins |
| `deactivationDate` | date | When billing ends (nullable) |
| `requiresApproval` | boolean | Whether activation requires workflow |
| `status` | enum | `DRAFT`, `PENDING_APPROVAL`, `ACTIVE`, `SUSPENDED`, `DEACTIVATED` |

**Activation Rules**:
1. New billing codes start in `DRAFT` status
2. Codes with `requiresApproval=true` must go through ApprovalWorkflow before becoming `ACTIVE`
3. Codes cannot be deleted — only deactivated
4. Reactivation of a deactivated code creates a new version with a new `billingCodeId`
5. SOX-regulated codes require dual approval (maker-checker)

### BillingRule

Business logic governing how a billing code calculates charges.

| Field | Type | Description |
|-------|------|-------------|
| `billingRuleId` | UUID | Immutable identifier |
| `name` | string | Rule name |
| `ruleType` | enum | `FLAT`, `TIERED`, `VOLUME`, `PERCENTAGE` |
| `parameters` | JSON | Rule-specific configuration |
| `effectiveDate` | date | When rule takes effect |
| `version` | integer | Rule version (monotonically increasing) |

**Constraint**: Rules are versioned, not mutated. A new version creates a new record. Historical billing always references the rule version in effect at billing time.

### ConfigurationProfile

A set of parameters governing product behavior.

| Field | Type | Description |
|-------|------|-------------|
| `profileId` | UUID | Immutable identifier |
| `productId` | UUID | FK to Product |
| `parameters` | JSON | Configuration key-value pairs |
| `environment` | enum | `DEV`, `UAT`, `PROD` |
| `version` | integer | Profile version |
| `approvedBy` | string | Who approved this configuration |
| `approvedAt` | timestamp | When approved |

### LifecycleState

Enum defining the product lifecycle phases:

```
DRAFT → UNDER_REVIEW → APPROVED → ACTIVE → SUSPENDED → SUNSET → ARCHIVED
```

**Transition Rules**:
- `DRAFT → UNDER_REVIEW`: Requires all mandatory fields populated
- `UNDER_REVIEW → APPROVED`: Requires approval workflow completion
- `APPROVED → ACTIVE`: Requires `effectiveDate <= today` and billing codes configured
- `ACTIVE → SUSPENDED`: Emergency action, requires incident reference
- `ACTIVE → SUNSET`: Requires sunset plan with client migration timeline
- `SUNSET → ARCHIVED`: All clients migrated, no active billing codes
- No backwards transitions except `SUSPENDED → ACTIVE` (with resolution reference)

### ApprovalWorkflow

Tracks approval processes for regulated operations.

| Field | Type | Description |
|-------|------|-------------|
| `workflowId` | UUID | Immutable identifier |
| `entityType` | enum | `PRODUCT`, `BILLING_CODE`, `CONFIGURATION` |
| `entityId` | UUID | FK to the entity under review |
| `workflowType` | enum | `STANDARD`, `DUAL_APPROVAL`, `EMERGENCY` |
| `status` | enum | `PENDING`, `APPROVED`, `REJECTED`, `ESCALATED` |
| `initiatedBy` | string | Who started the workflow |
| `approvers` | array | Ordered list of required approvers |
| `approvalEvents` | array | Timestamped approval/rejection events |
| `correlationId` | UUID | Links related workflows for tracing |

---

## Entity Relationships

```
ProductCategory (tree)
  └── Product
        ├── ConfigurationProfile (1:many, versioned)
        ├── BillingCode (1:many)
        │     ├── BillingRule (many:1, versioned)
        │     └── ApprovalWorkflow (1:many)
        └── ApprovalWorkflow (1:many)

TaxonomyNode (tree, immutable once published)
  └── BillingCode (1:many)
```

---

## Regulatory Context

### SOX Compliance
- All mutations to billing codes, products, and configurations must produce **immutable audit events**
- Dual approval (maker-checker) required for: billing code activation, configuration changes in PROD, product lifecycle transitions past APPROVED
- Audit trail must capture: who, what, when, why (correlation ID), and the before/after state

### Audit Event Structure
Every state change produces an event:

| Field | Description |
|-------|-------------|
| `eventId` | UUID |
| `timestamp` | ISO 8601 |
| `actor` | Identity of the person/system making the change |
| `action` | Verb (e.g., `BILLING_CODE_ACTIVATED`) |
| `entityType` | What was changed |
| `entityId` | UUID of the changed entity |
| `correlationId` | Links related events |
| `before` | JSON snapshot of prior state |
| `after` | JSON snapshot of new state |
| `reason` | Human-readable justification |

### Data Retention
- Audit events: 7-year minimum retention
- Deactivated billing codes: retained indefinitely (never hard-deleted)
- Archived products: retained with full history

---

## API Architecture

### Design Principles
- **REST over OpenAPI 3.1** with JSON request/response bodies
- **Versioned endpoints**: `/api/v1/products`, `/api/v2/products`
- **HATEOAS links** for discoverability
- **Pagination**: cursor-based for large collections
- **Idempotency**: all write operations accept idempotency keys
- **Correlation IDs**: required header on all requests (`X-Correlation-ID`)

### Key API Patterns
- **Optimistic concurrency**: ETags on all mutable resources
- **Bulk operations**: dedicated endpoints (e.g., `POST /api/v1/billing-codes/bulk`) with async processing and status polling
- **Webhooks**: notify downstream consumers of state changes
- **Rate limiting**: per-client, per-endpoint

### Consumer Landscape
- **Billing Engine**: reads billing codes and rules; generates invoices
- **Client Portal**: reads product catalog; displays available products
- **Reporting Platform**: reads all entities; generates compliance reports
- **Risk Systems**: reads product configurations; calculates exposure
- **Internal Admin UI**: full CRUD access for product managers

---

## Glossary

| Term | Definition |
|------|-----------|
| Maker-Checker | Dual-approval pattern: one person initiates, a different person approves |
| Materialized Path | Denormalized path string for efficient tree queries |
| Taxonomy | Canonical hierarchical classification of products |
| Billing Code | Maps product usage events to financial line items |
| Configuration Profile | Environment-specific parameters governing product behavior |
| Lifecycle State | Current phase of a product from draft to archived |
| Correlation ID | UUID linking related operations across systems for tracing |
| SOX | Sarbanes-Oxley Act — requires audit trails for financial controls |
