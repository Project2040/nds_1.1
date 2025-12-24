---
id: REL_FRAMEWORK
title: NDS Relationship Framework (The Matrix)
system: NDS
version: 1.0
component: rel_links
status: normative
last_updated: 2025-12-24
---

# NDS Relationship Framework (The Matrix)

**System Version:** NDS v1.2  
**Component:** `rel_links`

## Concept
Instead of maintaining specific link tables (e.g., `orders_has_assets`, `users_has_roles`), NDS uses a single **Universal Link Table** (`rel_links`). This allows for a flexible, "Graph-like" structure within a relational MySQL database.

## Structure (`rel_links`)

| Field | Description | Example |
|-------|-------------|---------|
| `from_table` | Source entity table name | `ncps_orders` |
| `from_id` | Source entity PK | `1` |
| `to_table` | Target entity table name | `ref_kno_knowledge_domain` |
| `to_id` | Target entity PK | `democracy` |
| `rel_type` | Semantic type (FK: `ref_rel_relation_type`) | `relates_to` |
| `strength` | Weight (FK: `ref_rel_strength_level`) | `strong` |
| `direction` | Logic (FK: `ref_rel_directionality`) | `symmetric` |

## 🛠️ Usage Examples

### 1. Creating a Link (SQL)
Linking an **Order** to a **Topic**:

```sql
INSERT INTO rel_links 
(from_table, from_id, to_table, to_id, rel_type, strength) 
VALUES 
('ncps_orders', '105', 'ref_tag_type', 'ai_safety', 'relates_to', 'strong');
**Version:** 1.0 (NDS v1.2)  
**Component:** `rel_links`
```
## Concept
Instead of maintaining specific link tables (`orders_has_assets`, `users_has_roles`), NDS uses a single **Universal Link Table** (`rel_links`). This allows for a "Graph-like" structure within MySQL.

## Structure (`rel_links`)

| Field | Description | Example |
|-------|-------------|---------|
| `from_table` | Source entity table name | `ncps_orders` |
| `from_id` | Source entity PK | `1` |
| `to_table` | Target entity table name | `ref_kno_knowledge_domain` |
| `to_id` | Target entity PK | `democracy` |
| `rel_type` | Semantic type (FK: `ref_rel_relation_type`) | `relates_to` |
| `strength` | Weight (FK: `ref_rel_strength_level`) | `strong` |
| `direction` | Logic (FK: `ref_rel_directionality`) | `symmetric` |

## 🛠️ Usage Examples

### 1. Creating a Link (SQL)
Linking an **Order** to a **Topic**:

```sql
INSERT INTO rel_links 
(from_table, from_id, to_table, to_id, rel_type, strength) 
VALUES 
('ncps_orders', '105', 'ref_tag_type', 'ai_safety', 'relates_to', 'strong');
---

### 2. Querying Relations (SQL)
Find all Orders related to the topic 'Democracy':

SQL

SELECT t1.* FROM ncps_orders t1
JOIN rel_links r ON r.from_table = 'ncps_orders' AND r.from_id = t1.id
WHERE r.to_table = 'ref_kno_knowledge_domain' 
  AND r.to_id = 'democracy';
```

## 🔒 Constraints

### Uniqueness: 
  The combination of from_table, from_id, to_table, to_id, and rel_type MUST be unique.

### Integrity: 
  rel_type must exist in ref_rel_relation_type.
