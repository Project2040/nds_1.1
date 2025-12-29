# 🌊 Neptunia OS v2.0 (Diamond Matrix)

**Status:** Operativ Arkitektur v2.0 (Fullført Des 2025)  
**Arkitekter:** Rune & Mycelium AI  
**Sikkerhetsnivå:** Herdet (Hardened)

Neptunia OS er en regenerativ medie-motor bygget på en hybrid relasjons/graf-arkitektur. Systemet er designet for å koble mennesker, AI-agenter og innhold i et sømløst, sikkert nettverk.

---

## 🏛️ Arkitekturen: "The Holy Trinity"
Systemet er bygget på tre logiske lag som samhandler via en sentralisert "hjerne":

### 1. CORE (Substantivene)
Master-data og operative domeneobjekter som utgjør systemets ryggrad.
* **ncps_orders:** Sentral register for alle prosjekter og bestillinger.
* **assets_registry:** Universelt register for media, filer og dokumenter.
* **auth_users:** Register over menneskelige aktører og systembrukere.

### 2. REF (Adjektivene)
Standardisert referansedata som definerer systemets "språk". Over 110 tabeller generert via factory-mønster.
* **Dynamisk:** Statiske ENUM-verdier er eliminert til fordel for utvidbare referansetabeller.
* **Eksempler:** `ref_geo_country`, `ref_kno_knowledge_domain`, `ref_rel_relation_type`.

### 3. REL (Verbene) - "The Matrix"
En universell, polymorfisk lenkemotor i tabellen `rel_links` som kobler enhver entitet til enhver annen entitet.
* **Mekanisme:** Dynamiske koblinger (`from_table:id` -> `to_table:id`).
* **Semantikk:** Styres av relasjonstyper (f.eks. `derived_from`, `accountable`, `responsible`).
* **Ytelse:** Optimalisert med kompositt-indekser og virtuelle integritetsnøkler (`active_link_uid`).

---

## 🛡️ Applikasjonssikkerhet (The Guardian)
Etter den store arkitektur-vasken i desember 2025 følger kildekoden strenge isolasjonsprinsipper:

### 📂 Mappestruktur
* `/nds/`: **Inngangsdører (Public UI).** Inneholder kun grensesnitt-filer og CSS/JS.
* `/nds/app/`: **LÅST (Logic).** Inneholder database-klasser, logikk og sikkerhetsmoduler.
* `/nds/config/`: **LÅST (Credentials).** Inneholder sensitive miljøvariabler (`env.php`) og API-nøkler.
* `/nds/uploads/`: **KARANTENE.** PHP-kjøring er totalt blokkert via `.htaccess`.

### 🔑 Sikkerhetsregler for Utvikling/AI
1. **Bootstrap Mandatory:** Alle inngangsfiler i roten *må* starte med:  
   `$config = require_once __DIR__ . '/app/bootstrap.php';`
2. **Access Guard:** Alle filer i `/app/` og `/config/` skal starte med:  
   `if (!defined('NDS_BOOTSTRAP')) { http_response_code(403); die(); }`
3. **PDO Only:** Bruk alltid det globale `$pdo`-objektet fra bootstrap. Aldri opprett nye databasetilkoblinger manuelt.
4. **Session Hardening:** Systemet regenererer sesjons-ID ved hver innlogging for å hindre hijacking.

---

## 🚀 Drift og Verktøy
* **Blueprint:** Bruk `nds_visual_schema.php` for sanntids visualisering av systemets nervesystem (Mycelium Net).
* **Schema Architect:** Endringer i tabellstruktur krever **Level 5 Architect** rettigheter og utføres via `nds_schema_architect.php`.
* **Uplink:** Teknisk helsesjekk og API-tilkobling monitoreres via `uplink.php`.
* **Logs:** Systemfeil logges til serverens `error_log`. `display_errors` er deaktivert i produksjonsmodus.

---

**System Owner:** Neptunia Media AS (Rune Solberg)  
**Version:** 2.0.0-diamond-matrix  
*Unauthorized access to system internals is programmatically prohibited.*
