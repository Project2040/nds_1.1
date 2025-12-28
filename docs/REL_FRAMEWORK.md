
---
id: REL_FRAMEWORK

title: NDS Relationship Framework (The Matrix)

system: NDS

version: 1.1

component: rel_links

status: normative

last_updated: 2025-12-28
---

# NDS Relationship Framework (The Matrix)

**System Version:** NDS v1.3  
**Component:** `rel_links` + `nds_linker.php`

## Concept

Instead of maintaining specific link tables (`orders_has_assets`, `users_has_roles`), NDS uses a single **Universal Link Table** (`rel_links`). This allows for a flexible, "Graph-like" structure within a relational MySQL database - the **Mycelium Philosophy** in action.

## Core Tables

### `rel_links` - The Universal Link Table

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| `id` | INT | Auto-increment PK | `42` |
| `from_table` | VARCHAR(100) | Source entity table | `ncps_orders` |
| `from_id` | INT | Source entity PK | `1` |
| `to_table` | VARCHAR(100) | Target entity table | `ncps_clients` |
| `to_id` | INT | Target entity PK | `5` |
| `rel_type` | VARCHAR(50) | FK: `ref_rel_relation_type.ref` | `order_client` |
| `strength` | VARCHAR(20) | FK: `ref_rel_strength_level.ref` | `strong` |
| `direction` | VARCHAR(20) | FK: `ref_rel_directionality.ref` | `directed` |
| `metadata` | JSON | Optional extra data | `{"note": "VIP"}` |
| `is_active` | TINYINT | Soft delete flag | `1` |
| `created_by` | VARCHAR(100) | Who created | `user` |
| `created_at` | DATETIME | Timestamp | `2025-12-28 10:00:00` |

### `ref_rel_relation_type` - Relationship Types

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| `ref` | VARCHAR(50) | Primary key | `order_client` |
| `name` | VARCHAR(100) | Display name | `Ordre → Klient` |
| `description` | TEXT | Explanation | `Kunden som bestilte ordren` |
| `parent_ref` | VARCHAR(50) | Hierarchy | `NULL` |
| `sort_order` | INT | Display order | `100` |
| `is_active` | TINYINT | Active flag | `1` |
| `metadata` | JSON | **Icon & color** | `{"icon": "👤", "color": "#3b82f6"}` |
| `ssot_source` | VARCHAR(100) | Source of truth | `NDS` |
| `created_at` | DATETIME | Timestamp | `2025-12-28` |

## Relationship Type Categories

### Generic Types (Built-in)

| ref | name | Use Case |
|-----|------|----------|
| `relates_to` | Relates to | General connection |
| `derived_from` | Derived from | Content based on source |
| `part_of` | Part of | Component in whole |
| `version_of` | Version of | New version |
| `owns` | Owns | Ownership |
| `responsible` | Responsible | Accountability |

### Order-Specific Types (v1.1)

| ref | name | from_table | to_table | icon | color |
|-----|------|------------|----------|------|-------|
| `order_client` | Ordre → Klient | ncps_orders | ncps_clients | 👤 | #3b82f6 |
| `order_responsible` | Ordre → Ansvarlig | ncps_orders | auth_users | 👨‍💼 | #10b981 |
| `order_agent` | Ordre → AI Agent | ncps_orders | ncps_agents | 🤖 | #8b5cf6 |
| `order_supplier` | Ordre → Leverandør | ncps_orders | ncps_suppliers | 🏭 | #f59e0b |
| `order_related` | Ordre → Ordre | ncps_orders | ncps_orders | 🔗 | #06b6d4 |
| `order_contract` | Ordre → Kontrakt | ncps_orders | erp_contracts | 📜 | #ec4899 |
| `order_satellite` | Ordre → Satellitt | ncps_orders | ncps_satellites | 🛰️ | #14b8a6 |

## PHP Component: `nds_linker.php`

### Basic Usage

```php
require 'nds_linker.php';

// Handle form submissions (call at top of file)
$result = handleLinkerActions($pdo);
if ($result) {
    header("Location: ?msg=" . urlencode($result['msg']));
    exit;
}

// Render CSS once
renderLinkerStyles();

// Render universal linker (all relation types)
renderLinker($pdo, 'ncps_orders', $orderId);
```

### Filtered Usage (Dedicated Boxes)

```php
// Klient-boks - kun order_client relasjoner
renderLinker($pdo, 'ncps_orders', $id, [
    'filter_rel_type' => 'order_client',
    'filter_to_table' => 'ncps_clients',
    'title' => 'Klient'
]);

// Ansvarlig-boks
renderLinker($pdo, 'ncps_orders', $id, [
    'filter_rel_type' => 'order_responsible',
    'filter_to_table' => 'auth_users',
    'title' => 'Ansvarlig'
]);

// AI Agent-boks
renderLinker($pdo, 'ncps_orders', $id, [
    'filter_rel_type' => 'order_agent',
    'filter_to_table' => 'ncps_agents',
    'title' => 'AI Agent'
]);
```

### All Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `filter_rel_type` | string | null | Show only this relation type |
| `filter_to_table` | string | null | Show only links to this table |
| `title` | string | "Relasjoner" | Box header title |
| `compact` | bool | false | Minimal UI mode |
| `allowDelete` | bool | true | Allow removing links |
| `allowAdd` | bool | true | Allow adding links |
| `showCount` | bool | true | Show count in header |
| `emptyText` | string | "Ingen koblet" | Text when empty |

## SQL Examples

### Creating a Link

```sql
INSERT INTO rel_links 
(from_table, from_id, to_table, to_id, rel_type, strength, direction, is_active, created_by, created_at) 
VALUES 
('ncps_orders', 1, 'ncps_clients', 5, 'order_client', 'strong', 'directed', 1, 'user', NOW());
```

### Querying with Metadata

```sql
-- Get all order relations with icon/color from ref table
SELECT 
    l.*,
    rt.name as rel_type_name,
    JSON_UNQUOTE(JSON_EXTRACT(rt.metadata, '$.icon')) as icon,
    JSON_UNQUOTE(JSON_EXTRACT(rt.metadata, '$.color')) as color
FROM rel_links l
LEFT JOIN ref_rel_relation_type rt ON l.rel_type = rt.ref
WHERE l.from_table = 'ncps_orders' 
  AND l.from_id = 1
  AND l.is_active = 1;
```

### Finding Related Orders

```sql
-- All clients linked to order #1
SELECT c.id, ca.name
FROM ncps_clients c
JOIN core_actors ca ON c.actor_id = ca.id
JOIN rel_links r ON r.to_table = 'ncps_clients' AND r.to_id = c.id
WHERE r.from_table = 'ncps_orders' 
  AND r.from_id = 1
  AND r.rel_type = 'order_client'
  AND r.is_active = 1;
```

### Adding New Relation Type

```sql
INSERT INTO ref_rel_relation_type 
(ref, name, description, sort_order, is_active, metadata, created_at) 
VALUES 
('order_partner', 'Ordre → Partner', 'Samarbeidspartner på ordren', 180, 1, 
 '{"icon": "🤝", "color": "#a855f7"}', NOW());
```

## Constraints

### Uniqueness
The combination of `from_table`, `from_id`, `to_table`, `to_id`, and `rel_type` MUST be unique when `is_active = 1`.

```sql
-- Enforced in nds_linker.php before INSERT
SELECT id FROM rel_links 
WHERE from_table = ? AND from_id = ? 
  AND to_table = ? AND to_id = ? 
  AND rel_type = ? AND is_active = 1;
```

### Integrity
- `rel_type` must exist in `ref_rel_relation_type`
- `strength` should exist in `ref_rel_strength_level`
- `direction` should exist in `ref_rel_directionality`

### Soft Delete
Links are never physically deleted. Set `is_active = 0` instead:

```sql
UPDATE rel_links SET is_active = 0 WHERE id = ?;
```

## Visual: The Mycelium Model

```
┌─────────────────────────────────────────────────────────────────┐
│                     ORDRE (ncps_orders)                         │
│                           │                                     │
│                      rel_links                                  │
│         ┌─────┬─────┬─────┼─────┬─────┬─────┐                   │
│         ▼     ▼     ▼     ▼     ▼     ▼     ▼                   │
│        👤    👨‍💼   🤖    🏭   🔗    📜    🛰️                  │
│      Klient Ansv. Agent  Lev. Ordre Kontr. Sat.                 │
│                                                                 │
│  ref_rel_relation_type provides icon/color via metadata JSON    │
└─────────────────────────────────────────────────────────────────┘
```

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2025-12-24 | Initial framework |
| 1.1 | 2025-12-28 | Added order-specific types, metadata JSON for icon/color, nds_linker.php v2.0 with filter support |
