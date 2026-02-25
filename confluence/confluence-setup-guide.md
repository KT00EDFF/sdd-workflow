# Confluence Setup Guide

> Step-by-step instructions for setting up the Confluence space and templates. The GPM should do this during Week 1-2 of rollout.

---

## Prerequisites

- Confluence admin access (or permission to create a new space/section)
- All template files from the `confluence/templates/` directory
- Reference files from the `confluence/reference/` directory

---

## Step 1: Create the Space (or Section)

**Option A: New Space** (recommended for clean separation)
1. Create a new Confluence space: **"Product Catalog — Product Development Hub"**
2. Set permissions: all Product Catalog team members can view and edit

**Option B: Section in Existing Space**
1. In your existing Product Catalog space, create a top-level page: **"Product Development Hub"**
2. All subsequent pages will be children of this page

---

## Step 2: Create the Page Hierarchy

Build this page structure:

```
Product Development Hub (parent page)
├── 📋 Portfolio Dashboard
├── 🗺️ Product Capability Map
├── 📚 Templates
│   ├── Problem Brief Template
│   ├── Assumption Map Template
│   ├── Decision Record Template
│   └── Capability Record Template
├── 📖 Reference
│   ├── Lifecycle Guide
│   └── Triage Decision Tree
├── 🏗️ Active Work
│   ├── [Feature A] — Problem Brief
│   ├── [Feature B] — Problem Brief
│   └── ...
├── 📦 Capability Records
│   ├── [Capability 1] — Record
│   ├── [Capability 2] — Record
│   └── ...
└── 📝 Decision Records
    ├── DR-2026-001 — [Title]
    └── ...
```

---

## Step 3: Upload Templates

For each template in `confluence/templates/`, create a Confluence page and paste the markdown content. Then convert it to a Confluence template:

### Converting Markdown to Confluence Templates

1. **Create a page** with the template content
2. **Convert tables**: Confluence uses a table macro. Select the markdown table text and use Insert → Table, or use a markdown-to-Confluence converter
3. **Convert headers**: Markdown `##` headers map to Confluence Heading 2, etc.
4. **Save as template**:
   - Page menu → **...** → **Templates** → **Create template** (space-level)
   - Or: Space Settings → Content Templates → Create Template
5. **Add labels**: tag each template (e.g., `sdd-template`, `problem-brief`)

| Markdown File | Confluence Template Name | Labels |
|--------------|------------------------|--------|
| `problem-brief.md` | Problem Brief | `sdd-template`, `problem-brief` |
| `assumption-map.md` | Assumption Map | `sdd-template`, `assumption-map` |
| `decision-record.md` | Decision Record | `sdd-template`, `decision-record` |
| `capability-record.md` | Capability Record | `sdd-template`, `capability-record` |
| `product-capability-map.md` | Product Capability Map | `sdd-template`, `capability-map` |
| `portfolio-dashboard.md` | Portfolio Dashboard | `sdd-template`, `dashboard` |

---

## Step 4: Create the Portfolio Dashboard

1. Create a page from the Portfolio Dashboard template
2. Fill in initial values (mostly zeros/empty)
3. This page will be the GPM's primary operational view
4. Consider pinning it or adding it to the space sidebar for quick access

---

## Step 5: Create the Product Capability Map

1. Create a page from the Product Capability Map template
2. List all known domain areas (Taxonomy, Billing Codes, Product Lifecycle, Configuration, Reporting, Admin)
3. Begin listing known capabilities — even without full Capability Records
4. Mark each as "Documented" or "Not documented"
5. This becomes the starting point for the retrofit process

---

## Step 6: Create Reference Pages

Create these pages from the reference files (these are not templates — they're permanent reference docs):

| Markdown File | Confluence Page Name | Parent |
|--------------|---------------------|--------|
| `reference/lifecycle-guide.md` | Lifecycle Guide | Reference section |
| `reference/triage-decision-tree.md` | Triage Decision Tree | Reference section |

These should be marked as reference/read-only pages that PMs consult but don't edit (unless updating the process).

---

## Step 7: Set Up Page Labels and Macros

### Useful Labels
Apply these labels consistently to make content findable:

| Label | Applied To |
|-------|-----------|
| `problem-brief` | All Problem Brief pages |
| `assumption-map` | All Assumption Map pages |
| `decision-record` | All Decision Record pages |
| `capability-record` | All Capability Record pages |
| `tier-1` / `tier-2` / `tier-3` | Problem Briefs by tier |
| `in-discovery` / `ready-for-spec` / `in-development` / `shipped` | Problem Briefs by status |

### Useful Macros (on the Portfolio Dashboard)
- **Content by Label**: Display all pages with a specific label (e.g., all active Problem Briefs)
- **Page Properties Report**: If using Confluence page properties, aggregate status across Problem Briefs
- **Status Macro**: Visual status indicators (🟢🟡🔴) for features

---

## Step 8: Permissions

| Group | Access Level |
|-------|-------------|
| Product Catalog PMs | View + Edit (their own Problem Briefs, Assumption Maps, Decision Records) |
| Tech Leads | View + Edit (Capability Records technical sections, Decision Records) |
| Designers | View + Edit (Capability Records design sections) |
| GPM | View + Edit all; Admin for templates |
| Compliance Lead | View all; Edit compliance sections |

---

## Step 9: Verify Setup

Run through this checklist:

- [ ] Space/section created and accessible to the team
- [ ] All 6 templates uploaded and available as Confluence templates
- [ ] Portfolio Dashboard page created and pinned
- [ ] Product Capability Map page created with initial domain areas
- [ ] Lifecycle Guide reference page created
- [ ] Triage Decision Tree reference page created
- [ ] Active Work section ready for Problem Briefs
- [ ] Capability Records section ready for retrofit documentation
- [ ] Decision Records section ready
- [ ] Labels configured and consistent
- [ ] Permissions set correctly

---

## Ongoing Maintenance

### Weekly (GPM)
- Update Portfolio Dashboard
- Review new Problem Briefs for quality

### Monthly
- Archive shipped/closed Problem Briefs (move to an archive section or add `archived` label)
- Review and clean up labels

### Quarterly
- Review template effectiveness — any sections consistently left blank? Consider removing or simplifying.
- Update reference pages if the process has changed
- Review Capability Map progress

### When Templates Change
If templates are updated (e.g., after rollout retro in Week 6):
1. Update the Confluence template definition
2. Existing pages created from the old template are NOT auto-updated — this is fine
3. Notify the team of template changes
4. Update the corresponding markdown files in this repository
