# TODO: NDS Docs Help System

**Opprettet:** 2025-12-26
**Status:** Planlagt
**Prioritet:** Medium

## Bakgrunn

NDS har rik dokumentasjon i `docs/` mappen, inkludert:
- `REF_DATA_GUIDE.md` - Guide for ref_-tabeller
- `REF_POLICY.yaml` - Policy for ref-data
- `ACTOR_MODEL.md` - Dokumentasjon for core_actors
- osv.

Denne dokumentasjonen bør være tilgjengelig direkte i UI-verktøyene.

## Mål

Legge til kontekstuell hjelp i NDS-verktøy som:
- `nds_schema_architect.php`
- `nds_enum_manager.php`
- `nds_dashboard.php`

## Foreslått implementasjon

### 1. Hjelpe-komponent (`nds_help.php`)

```php
<?php
/**
 * NDS Help System
 * Viser kontekstuell hjelp basert på docs/-filer
 */

function getHelpContent($topic) {
    $docs = [
        'ref_tables' => 'docs/REF_DATA_GUIDE.md',
        'ref_policy' => 'docs/REF_POLICY.yaml',
        'actors' => 'docs/ACTOR_MODEL.md',
        'enum' => 'docs/ENUM_GUIDE.md',
        // ...
    ];
    
    if (isset($docs[$topic]) && file_exists($docs[$topic])) {
        return parseMarkdown(file_get_contents($docs[$topic]));
    }
    return null;
}

function renderHelpButton($topic, $title = '?') {
    return '<button onclick="showHelp(\'' . $topic . '\')" 
            class="help-btn" title="Vis hjelp">
            ' . $title . '
            </button>';
}
```

### 2. Hjelpe-modal (JavaScript)

```javascript
function showHelp(topic) {
    fetch('nds_help.php?topic=' + topic)
        .then(response => response.json())
        .then(data => {
            document.getElementById('help-modal').innerHTML = data.content;
            document.getElementById('help-modal').classList.add('visible');
        });
}
```

### 3. Integrasjon i Schema Architect

```php
// Ved ref_-tabeller
if (strpos($current_table, 'ref_') === 0) {
    echo renderHelpButton('ref_tables', '📚 REF Guide');
}

// Ved core_actors
if ($current_table === 'core_actors') {
    echo renderHelpButton('actors', '📚 Actor Model');
}

// Ved ENUM-kolonner
if (strpos($col['Type'], 'enum') !== false) {
    echo renderHelpButton('enum', '❓');
}
```

### 4. Inline hjelpe-tips

Vis korte tips basert på kontekst:

```php
function getInlineTip($table, $column) {
    // ref-kolonner
    if ($column === 'ref') {
        return '💡 ref-koden må være kort, stabil, snake_case (REF_DATA_GUIDE §7)';
    }
    
    // is_active kolonner
    if ($column === 'is_active') {
        return '💡 Aldri slett - sett inaktiv i stedet (REF_DATA_GUIDE §8)';
    }
    
    // actor_type
    if ($column === 'actor_type') {
        return '💡 FK → ref_crm_party_type. Gyldige: organization, person, ai, system';
    }
    
    return null;
}
```

## Docs-struktur som må være på plass

```
docs/
├── REF_DATA_GUIDE.md      ✅ Finnes
├── REF_POLICY.yaml        ✅ Finnes
├── ACTOR_MODEL.md         ⚠️ Bør opprettes
├── ENUM_GUIDE.md          ⚠️ Bør opprettes
├── WORKFLOW_GUIDE.md      ⚠️ Bør opprettes
└── NDS_GLOSSARY.md        ⚠️ Bør opprettes
```

## Prioritert rekkefølge

1. **Fase 1:** Hjelpe-knapper som åpner docs i ny fane (enkelt)
2. **Fase 2:** Inline tips på kolonner
3. **Fase 3:** Modal med parsed Markdown
4. **Fase 4:** Kontekstsøk i docs

## Avhengigheter

- Markdown-parser (kan bruke Parsedown eller lignende)
- docs/-filer må være oppdatert
- CSS for modal/tooltip styling

## Notater

- Hjelp-systemet bør være usynlig for erfarne brukere
- Bør caches for ytelse
- Bør fungere uten JavaScript (fallback til lenke)
