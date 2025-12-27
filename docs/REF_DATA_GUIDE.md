# **REF\_DATA\_GUIDE.md**

**Neptunia DataSystem (NDS) – Guide for utfylling av ref\_-tabeller**

---

**id: REF\_DATA\_GUIDE**

**title: Reference Data Guide**

**system: Neptunia DataSystem (NDS)**

**component: ref\_framework**

**version: 1.0**

**status: normative**

**scope: global**

**audience:**

  **\- architects**

  **\- developers**

  **\- domain\_experts**

  **\- partners**

**maintainer: Neptunia Media AS**

**owner: System Architecture**

**ssot: true**

**applies\_to:**

  **\- all\_ref\_tables**

**related\_docs:**

  **\- REF\_FRAMEWORK.md**

  **\- SSOT\_POLICY.md**

**created: 2025-12-24**

**last\_updated: 2025-12-24**

**change\_policy: restricted**


---

## **1\. Hva er ref\_-tabeller i NDS?**

Ref\_-tabeller er **systemets begrepsgrunnlag**.  
 De definerer språket NDS bruker for å forstå:

* arbeid

* relasjoner

* status

* roller

* verdier

* struktur

De er **ikke**:

* transaksjonsdata

* brukerdata

* logger

* prosessdetaljer

Ref\_-tabeller beskriver *hva noe er*, ikke *hva som skjedde*.

---

## **2\. Viktig prinsipp (les dette først)**

**Det er bedre å legge til en ny ref-verdi for sent enn for tidlig.**

Ref-data skal være:

* stabile

* forståelige

* langlivede

Ikke:

* komplette

* perfekte

* exhaustive

---

## **3\. Hvem er denne guiden for?**

Denne guiden er skrevet for:

* systemarkitekter

* utviklere

* domeneeksperter

* fremtidige partnere

* nye teammedlemmer (f.eks. Rasmus)

Den er ment å gjøre det **trygt å bidra**, uten å ødelegge strukturen.

---

## **4\. Hvordan tenke før du legger til en ref-verdi**

Før du foreslår eller legger til en ny verdi, still disse spørsmålene:

1. Er dette et **begrep**, ikke en hendelse?

2. Vil dette begrepet sannsynligvis eksistere om 5 år?

3. Kan det forklares med én enkel setning?

4. Vil flere deler av systemet ha nytte av det?

5. Er dette riktig abstraksjonsnivå?

Hvis svaret er “nei” på ett eller flere → vent.

---

## **5\. Hva er “minste meningsfulle sett”?**

Hver ref\_-tabell skal starte med et **lite, men riktig** sett verdier.

Typisk:

* 3–7 verdier

* tydelig forskjell mellom dem

* ingen overlapp

Eksempel (konseptuelt, ikke teknisk):

* Tjeneste

* Produkt

* Arrangement

Ikke:

* Tjeneste – Konsulent – Senior – Midlertidig

Det siste er **prosess/rolle**, ikke referanse.

---

## **6\. Hierarki: når og hvordan**

Noen ref\_-tabeller støtter hierarki (parent/child).

Bruk hierarki når:

* begrepene naturlig grupperer seg

* strukturen gir mer mening enn en flat liste

Ikke bruk hierarki for:

* statuser

* prioriteringer

* tekniske enum-er

Start flatt. Legg til hierarki først når det er åpenbart nyttig.

---

## **7\. `ref`\-koden – det viktigste feltet**

`ref` er:

* systemets stabile identifikator

* brukt i FK-er

* brukt i integrasjoner

* brukt i SSOT

Derfor skal `ref` være:

* kort

* stabil

* beskrivende

* skrevet i små bokstaver med `_`

Aldri:

* endre en eksisterende `ref`

* bruke midlertidige navn

* bruke menneskespråk direkte (`konsulenttjenester_norge_2025`)

---

## **8\. Aktiv / inaktiv – aldri slett**

Når et begrep ikke lenger brukes:

* det settes **inaktivt**

* det slettes ikke

* historikk beholdes

Dette er avgjørende for:

* rapportering

* revisjon

* tillit

* læring

Systemer som sletter begreper mister hukommelse.

---

## **9\. SSOT – når brukes det?**

Noen ref\_-tabeller er **normative**.  
 De bør styres fra dokumentasjon eller repo, ikke direkte i DB.

Eksempler:

* innholdstyper

* lisensformer

* rettighetsnivåer

* policy-kategorier

I disse tilfellene:

* brukes SSOT (Markdown/YAML)

* endringer diskuteres før de deployes

* databasen er mottaker, ikke sannhet

Ikke alle ref\_-tabeller trenger SSOT.

---

## **10\. Hvem har ansvar?**

* **Systemiske ref-tabeller**  
   → arkitekturansvarlig

* **Domene-ref-tabeller**  
   → domeneekspert

* **Lokale / partnerspesifikke ref-tabeller**  
   → eier av domenet

NDS er bygget for **desentralisert ansvar**, ikke sentral kontroll.

---

## **11\. Arbeidsrytme (anbefalt)**

* Jobb med én ref\_-tabell av gangen

* Fyll noen få verdier

* Tenk langsiktig, ikke komplett

* Gå videre

Dette er regenerativ utvikling:

litt, riktig, over tid

---

## **12\. Viktig avslutning**

Ref\_-tabellene er **systemets språk**.  
 Hvis språket er klart, blir alt annet lettere:

* kode

* UI

* rapporter

* samarbeid

* skalering

Denne guiden er ment å beskytte det språket.

---

### **Status**

* Dette dokumentet er **normativt**

* Endres sjelden

* Gjelder alle ref\_-tabeller i NDS

# **Oversikt ref_ tabeller pr. 27.12.2025**

Her er listen over dine **ref-domener** (prefikser) og tilhørende tabeller, basert på databasen din.

Du har ganske riktig et omfattende sett (34 domener funnet), noe som bekrefter at du bygger et operativsystem og ikke bare en applikasjon. Dette gir deg "vokabular" for hele verdikjeden.

Her er oversikten til din `REF_DATA_GUIDE.md`:

### **1. AI & Automasjon (ref_ai_*)**

Definerer kunstig intelligens, agenter og verktøy.

* `ref_ai_agent_type`: Typer agenter (LLM, Coder, Vision etc.)
* `ref_ai_eval_metric`: Målekriterier for AI-kvalitet (Factuality, Tone etc.)
* `ref_ai_model_family`: Modellfamilier (Anthropic, OpenAI, Local)
* `ref_ai_model_version`: Spesifikke versjoner
* `ref_ai_prompt_type`: Typer prompts (System, User, Tool)
* `ref_ai_provider`: Leverandører av AI
* `ref_ai_run_status`: Status på kjøringer
* `ref_ai_tool_capability`: Hva en agent kan gjøre (Web search, Code exec)
* `ref_ai_tool_type`: Kategorisering av verktøy

### **2. API & Integrasjoner (ref_api_*)**

Håndterer tekniske koblinger mot omverdenen.

* `ref_api_auth_method`: Autentiseringsmetoder
* `ref_api_auth_type`: Typer auth (Bearer, OAuth)
* `ref_api_integration_type`: Spesifikke integrasjoner (Stripe, Bunny, GitHub)
* `ref_api_webhook_event`: Hendelser som trigges

### **3. Arkiv & Lagring (ref_arc_*)**

Livssyklus for data og filer.

* `ref_arc_archive_class`: Lagringsklasser (Hot, Cold, Permanent)
* `ref_arc_archive_status`: Status i arkivet
* `ref_arc_document_type`: Dokumenttyper
* `ref_arc_retention_policy`: Sletteregler
* `ref_arc_storage_class`: Fysisk lagringstype

### **4. Business Intelligence (ref_bi_*)**

Rapportering og måltall.

* `ref_bi_dashboard_status`: Status for dashboards
* `ref_bi_dimension_type`: Dimensjoner for analyse
* `ref_bi_metric_granularity`: Tidsoppløsning
* `ref_bi_metric_unit`: Enheter (NOK, Antall, %, CO2)
* `ref_bi_report_type`: Typer rapporter

### **5. Content Management (ref_cms_*)**

Innholdsproduksjon og publisering.

* `ref_cms_block_type`: Byggeklosser (Hero, Text, CTA)
* `ref_cms_content_type`: Innholdstyper (Article, Page, Course)
* `ref_cms_editor_type`: Editor-typer
* `ref_cms_license_badge`: Lisensmerking (CC BY, Copyright)
* `ref_cms_media_type`: Mediatyper (Image, Video, Audio)
* `ref_cms_post_status`: Publiseringsstatus (Draft, Published)
* `ref_cms_template_type`: Maler

### **6. Kommunikasjon (ref_com_*)**

Språk og tone.

* `ref_com_channel_type`: Kanaltyper
* `ref_com_language`: Språkinnstillinger
* `ref_com_message_type`: Meldingstyper
* `ref_com_tone_style`: Tone-of-voice (Professional, Playful)

### **7. Kjerneaktører (ref_core_*)**

*Deprecated / Migrering*

* `ref_core_actor_type`: (Erstattet av `ref_crm_party_type`)

### **8. CRM & Kunder (ref_crm_*)**

Relasjonshåndtering.

* `ref_crm_contact_role`: Roller (Beslutningstaker, Bruker)
* `ref_crm_customer_tier`: Kundenivå (Gull, Sølv, Strategisk)
* `ref_crm_customer_type`: Kundesegment
* `ref_crm_industry`: Bransjer (IT, Helse, Offentlig)
* `ref_crm_lead_source`: Hvor kom kunden fra?
* `ref_crm_lead_status`: Salgsstatus
* `ref_crm_party_type`: Aktørtyper (Person, Org, AI, System)
* `ref_crm_pipeline_stage`: Salgsprosess

### **9. Utvikling & DevOps (ref_dev_*)**

Softwareutvikling.

* `ref_dev_env`: Miljøvariabler
* `ref_dev_environment`: Miljøer (Local, Staging, Prod)
* `ref_dev_release_status`: Release-status
* `ref_dev_repo_type`: Repository-typer
* `ref_dev_stack_component`: Teknologistack

### **10. Utdanning (ref_edu_*)**

Læringsplattform og kurs.

* `ref_edu_assessment_type`: Vurderingsformer (Quiz, Eksamen)
* `ref_edu_certification_level`: Sertifiseringsnivå
* `ref_edu_completion_status`: Fullføringsgrad
* `ref_edu_course_type`: Kurstyper
* `ref_edu_learning_format`: Format (Video, Tekst, Live)
* `ref_edu_level`: Vanskelighetsgrad

### **11. Events (ref_evt_*)**

Arrangementer.

* `ref_evt_attendance_status`: Oppmøtestatus
* `ref_evt_event_category`: Kategori (Webinar, Workshop)
* `ref_evt_event_status`: Status for eventet
* `ref_evt_event_type`: Type event
* `ref_evt_registration_status`: Påmeldingsstatus
* `ref_evt_ticket_type`: Billettyper

### **12. Økonomi (ref_fin_*)**

Regnskap og transaksjoner.

* `ref_fin_account_type`: Kontotyper (Drift, Finans)
* `ref_fin_cost_center`: Kostnadssteder
* `ref_fin_currency`: Valutaer
* `ref_fin_funding_source`: Finansieringskilder
* `ref_fin_invoice_status`: Fakturastatus
* `ref_fin_payment_method`: Betalingsmetoder
* `ref_fin_payment_terms`: Betalingsbetingelser
* `ref_fin_split_model`: Fordelingsnøkler (Robin Hood)
* `ref_fin_tax_category`: Skattekategorier
* `ref_fin_tax_code`: MVA-koder
* `ref_fin_transaction_type`: Transaksjonstyper

### **13. Fleet & Brand (ref_flt_*)**

Multisite og merkevarestyring.

* `ref_flt_brand`: Merkevarer (Neptunia, Merkur, SRAGI)
* `ref_flt_channel`: Distribusjonskanaler
* `ref_flt_channel_type`: Kanaltyper
* `ref_flt_platform`: Plattformer
* `ref_flt_product_line`: Produktlinjer

### **14. Gamification (ref_gam_*)**

Engasjement og belønning.

* `ref_gam_badge_type`: Utmerkelser
* `ref_gam_challenge_status`: Utfordringsstatus
* `ref_gam_point_type`: Poengtyper
* `ref_gam_progression_rule`: Regler for fremgang
* `ref_gam_reward_type`: Belønningstyper

### **15. Geografi (ref_geo_*)**

Steder og regioner.

* `ref_geo_address_type`: Adressetyper
* `ref_geo_country`: Land
* `ref_geo_county`: Fylker
* `ref_geo_language`: Språk (Geografisk)
* `ref_geo_municipality`: Kommuner
* `ref_geo_region`: Regioner
* `ref_geo_timezone`: Tidssoner

### **16. HR & Personell (ref_hrm_*)**

Menneskelige ressurser.

* `ref_hrm_compensation_type`: Lønnsmodeller
* `ref_hrm_contract_type`: Kontraktsformer
* `ref_hrm_employment_type`: Ansettelsesformer
* `ref_hrm_leave_type`: Fraværstyper

### **17. Hardware (ref_hw_*)**

Fysisk utstyr.

* `ref_hw_asset_status`: Utstyrsstatus
* `ref_hw_device_type`: Enhetstyper
* `ref_hw_maintenance_type`: Vedlikeholdstyper

### **18. Impact (ref_imp_*)**

Bærekraft og effektmåling.

* `ref_imp_metric_type`: Impact-metrikker
* `ref_imp_rating_scale`: Skalaer
* `ref_imp_reporting_period`: Rapporteringsperioder
* `ref_imp_sdg_goal`: FNs bærekraftsmål

### **19. Inventar (ref_inv_*)**

Lagerstyring.

* `ref_inv_item_type`: Varetyper
* `ref_inv_location_type`: Lokasjonstyper
* `ref_inv_movement_type`: Bevegelsestyper
* `ref_inv_stock_status`: Lagerstatus (På lager, Tomt)
* `ref_inv_unit_type`: Enhetstyper
* `ref_inv_uom`: Måleenheter

### **20. IPR & Rettigheter (ref_ipr_*)**

Opphavsrett og lisensiering.

* `ref_ipr_attribution_style`: Attribusjonskrav
* `ref_ipr_license_type`: Lisenstyper (MIT, CC, Proprietary)
* `ref_ipr_rights_scope`: Rettighetsomfang
* `ref_ipr_right_type`: Rettighetstyper
* `ref_ipr_usage_limit_type`: Begrensninger
* `ref_ipr_usage_scope`: Bruksområde

### **21. Kunnskap (ref_kno_*)**

Kunnskapsforvaltning.

* `ref_kno_article_status`: Artikkelstatus
* `ref_kno_confidence_level`: Tillitsnivå
* `ref_kno_knowledge_domain`: Fagområder
* `ref_kno_source_reliability`: Kildetroverdighet
* `ref_kno_source_type`: Kildetyper

### **22. Jus (ref_leg_*)**

Juridisk og compliance.

* `ref_leg_agreement_type`: Avtaletyper
* `ref_leg_compliance_framework`: Rammeverk (GDPR)
* `ref_leg_compliance_level`: Etterlevelsesnivå
* `ref_leg_document_status`: Dokumentstatus
* `ref_leg_jurisdiction`: Jurisdiksjon
* `ref_leg_policy_type`: Policy-typer

### **23. Markedsføring (ref_mkt_*)**

Kampanjer og sporing.

* `ref_mkt_campaign_type`: Kampanjetyper
* `ref_mkt_channel`: Kanaler
* `ref_mkt_content_format`: Innholdsformat
* `ref_mkt_utm_medium`: UTM Medium
* `ref_mkt_utm_source`: UTM Source

### **24. Noder (ref_nod_*)**

Franchise og nodestyring.

* `ref_nod_membership_role`: Medlemsroller
* `ref_nod_node_role`: Noderoller
* `ref_nod_node_status`: Nodestatus
* `ref_nod_node_type`: Nodetyper (HQ, Franchise, Partner)
* `ref_nod_sync_status`: Synkroniseringsstatus

### **25. Organisasjon (ref_org_*)**

Intern struktur.

* `ref_org_decision_type`: Beslutningstyper
* `ref_org_department_type`: Avdelinger
* `ref_org_entity_type`: Enhetstyper
* `ref_org_relationship_type`: Interne relasjoner
* `ref_org_role`: Roller
* `ref_org_role_type`: Rolletyper
* `ref_org_unit_type`: Enhetstyper

### **26. PR (ref_pr_*)**

Presse og samfunnskontakt.

* `ref_pr_audience`: Målgrupper
* `ref_pr_press_type`: Pressetyper
* `ref_pr_publication_status`: Status
* `ref_pr_statement_type`: Uttalelsestyper

### **27. Produksjon (ref_prod_*)**

Arbeidsflyt og produksjon.

* `ref_prod_activity_type`: Aktivitetstyper
* `ref_prod_cause_code`: Årsakskoder (feil)
* `ref_prod_corrective_action`: Korrigerende tiltak
* `ref_prod_defect_type`: Feiltyper
* `ref_prod_fulfillment_mode`: Leveransemodi
* `ref_prod_job_type`: Jobbtyper
* `ref_prod_process_type`: Prosesstyper
* `ref_prod_quality_gate`: Kvalitetsporter (Fact check, Legal check)
* `ref_prod_quality_gate_type`: Porttyper
* `ref_prod_step_type`: Stegtyper
* `ref_prod_workflow_stage`: Prosess-steg (Raw input -> Delivery)

### **28. Relasjoner (ref_rel_*)**

Koblinger i "The Matrix" (rel_links).

* `ref_rel_directionality`: Retning (Enveis, Symmetrisk)
* `ref_rel_entity_type`: Entitetstyper
* `ref_rel_provenance_type`: Opprinnelse (Human, AI, System)
* `ref_rel_relationship_type`: Relasjonstyper
* `ref_rel_relation_type`: Semantisk kobling (owns, uses, triggers)
* `ref_rel_strength_level`: Koblingsstyrke

### **29. Sikkerhet (ref_sec_*)**

Tilgangsstyring.

* `ref_sec_access_policy`: Tilgangspolicy
* `ref_sec_auth_method`: Autentisering
* `ref_sec_event_type`: Hendelsestyper
* `ref_sec_risk_level`: Risikonivå
* `ref_sec_security_level`: Sikkerhetsnivå (Public, Internal, Confidential)

### **30. Status (ref_sts_*)**

Generelle statuser på tvers av systemet.

* `ref_sts_confidence_level`: Tillitsnivå
* `ref_sts_generic_status`: Generisk status (Draft, Active, Archived)
* `ref_sts_job_status`: Jobbstatus
* `ref_sts_lifecycle_stage`: Livssyklus
* `ref_sts_order_status`: Ordrestatus
* `ref_sts_priority`: Prioritet (Low, Medium, High, Urgent)
* `ref_sts_risk_level`: Risiko
* `ref_sts_severity`: Alvorlighetsgrad

### **31. Support (ref_sup_*)**

Kundesupport.

* `ref_sup_category`: Kategori
* `ref_sup_priority`: Prioritet
* `ref_sup_sla_tier`: SLA-nivå
* `ref_sup_ticket_status`: Sakstatus
* `ref_sup_ticket_type`: Sakstype

### **32. System (ref_sys_*)**

Systemkonfigurasjon.

* `ref_sys_component_type`: Komponenter (DB, API, CMS)
* `ref_sys_currency`: Valuta (System)
* `ref_sys_data_type`: Datatyper
* `ref_sys_environment`: Miljø (Local, Prod)
* `ref_sys_error_code`: Feilkoder
* `ref_sys_event_type`: Hendelsestyper (System)
* `ref_sys_feature_flag`: Feature flags
* `ref_sys_job_type`: Jobbtyper
* `ref_sys_language`: Språk
* `ref_sys_tenant_type`: Tenant-typer
* `ref_sys_timezone`: Tidssoner
* `ref_sys_unit_type`: Enhetstyper

### **33. Tagging (ref_tag_*)**

Taksonomi og merking.

* `ref_tag_curation_state`: Kurateringsstatus
* `ref_tag_language_scope`: Språkomfang
* `ref_tag_namespace`: Navnerom (Global, Internal, SRAGI)
* `ref_tag_policy`: Tag-policy
* `ref_tag_tag_relation_type`: Tag-relasjoner
* `ref_tag_tag_type`: Tag-typer
* `ref_tag_taxonomy`: Taksonomier
* `ref_tag_type`: Typer (Topic, Org, Person)
* `ref_tag_visibility`: Synlighet

### **34. UX (ref_ux_*)**

Brukergrensesnitt.

* `ref_ux_component_type`: Komponenter (Card, Hero, Footer)
* `ref_ux_severity`: Alvorlighetsgrad (UX)
* `ref_ux_test_type`: Testtyper
* `ref_ux_theme_mode`: Tema (Dark/Light)

