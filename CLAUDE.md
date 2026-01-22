# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Context

**The Farm Documentation** - Architecture and development documentation for The Farm fantasy sports platform. Contains ADRs, system design documents, and developer guides.

## 🚨 Service Boundary Rules

**CRITICAL: Only work within this directory (`/docs`)**

1. **DO NOT** modify files in other service directories (clubhouse, press-box, dugout, etc.)
2. **DO NOT** make changes that span multiple services in a single session
3. **USE GitHub Issues** for cross-service coordination:
   - Create issues in the relevant repo for work needed in other services
   - Reference issues across repos: `fantasy-farming/docs#123`
   - Add cross-repo issues to [**The Farm** project](https://github.com/orgs/fantasy-farming/projects/1)
4. **USE Backstage** to understand service relationships:
   - View dependencies: http://localhost:3333/catalog/default/component/the-farm-docs
   - Check API contracts before making breaking changes
   - Verify downstream consumers in the service catalog

**If a task requires changes to multiple services:**
1. Break it into service-specific issues
2. Create issues in each affected service's repo
3. Link them in the master project for tracking
4. Work on each service independently

## The Farm Ecosystem Documentation

For comprehensive documentation on The Farm platform, use the **Backstage Developer Portal**:

| Resource | URL | Description |
|----------|-----|-------------|
| **Service Catalog** | http://localhost:3333/catalog | All services, APIs, and their relationships |
| **API Documentation** | http://localhost:3333/api-docs | OpenAPI, AsyncAPI, and GraphQL specs |
| **TechDocs** | http://localhost:3333/docs | Technical documentation for all services |
| **System Diagram** | http://localhost:3333/catalog/default/system/the-farm | Architecture and service dependencies |

**Start Backstage**: `cd sandlot/the-farm && docker compose -f docker-compose.orbstack.yml -f docker-compose.backstage.yml up -d backstage`

## Directory Structure

```
docs/
├── adr/                         # Architecture Decision Records
│   └── NNNN-title.md           # ADR format
├── architecture/               # System architecture docs
├── guides/                     # Developer guides
├── api/                        # API documentation
└── mkdocs.yml                  # MkDocs configuration
```

## Documentation Standards

### ADR Format
```markdown
# ADR NNNN: Title

## Status
Proposed | Accepted | Deprecated | Superseded

## Context
[Problem statement]

## Decision
[What was decided]

## Consequences
[Positive and negative outcomes]
```

### Adding Documentation

1. Create markdown files in appropriate directory
2. Update `mkdocs.yml` navigation if needed
3. TechDocs will auto-generate on Backstage refresh

## MkDocs Commands

```bash
# Preview docs locally
mkdocs serve

# Build static site
mkdocs build
```

## Backstage Service Catalog

This component is registered in the Backstage developer portal via `catalog-info.yaml`.

### Maintaining catalog-info.yaml

**Keep this file updated when:**
- Adding new documentation sections (update description)
- Changing ownership
- Adding new tags for discoverability

### Key Fields to Maintain

```yaml
metadata:
  name: the-farm-docs                # Component name
  description: ...                   # Keep current with doc scope
  tags: [documentation, architecture]
spec:
  type: documentation                # Documentation type
  owner: group:backend-team
```


## Backstage Developer Portal API

Programmatic access to the service catalog is available via the Backstage API.

**Base URL:** `https://fantasy-backstage.fly.dev`

**Authentication:** Include the API token in the Authorization header:
```bash
curl -H "Authorization: Bearer $BACKSTAGE_API_TOKEN" "https://fantasy-backstage.fly.dev/api/catalog/entities"
```

**Common Endpoints:**
| Endpoint | Description |
|----------|-------------|
| `/api/catalog/entities` | List all entities |
| `/api/catalog/entities?filter=kind=component` | Filter by kind |
| `/api/catalog/entities/by-name/:kind/:namespace/:name` | Get specific entity |
| `/api/search/query?term=X` | Full-text search |

**Examples:**
```bash
# Get this service's catalog entry
curl -H "Authorization: Bearer $BACKSTAGE_API_TOKEN" \
  "https://fantasy-backstage.fly.dev/api/catalog/entities/by-name/component/default/docs"

# List all APIs
curl -H "Authorization: Bearer $BACKSTAGE_API_TOKEN" \
  "https://fantasy-backstage.fly.dev/api/catalog/entities?filter=kind=api"

# Search for related services
curl -H "Authorization: Bearer $BACKSTAGE_API_TOKEN" \
  "https://fantasy-backstage.fly.dev/api/search/query?term=docs"
```

**Portal URL:** https://fantasy-backstage.fly.dev
