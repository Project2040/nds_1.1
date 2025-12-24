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

