# Testdocument — VOC Inventaris Herontwerp (1.04.02)

**Versie:** v4 · **Datum:** 26 april 2026 · **Scope:** `index.html` + `viewer.html`

Dit document beschrijft de volledige testcase voor een eindgebruiker die historisch onderzoek doet in het VOC-archief. Het combineert **functionele tests** (gebruikersscenario's) met **WCAG 2.1 niveau AA** toegankelijkheidschecks en **cross-browser/device** verificatie.

---

## 1. Persona's & doelgroep

| Persona | Achtergrond | Belangrijkste taak |
|---|---|---|
| **Mira** — promovendus geschiedenis | Hoog informatie-niveau, gebruikt sneltoetsen | Vindt 17e-eeuwse octrooien, vergelijkt versies |
| **Henk** — amateur-genealoog (68) | Lager tech-niveau, leest schermtekst hardop | Zoekt voorouder onder VOC-dienaren in Indië |
| **Asha** — slechtziende onderzoeker | Gebruikt screenreader (NVDA) + 200% zoom | Doorbladert resoluties, hoeft tekst niet zelf te lezen |
| **Diederik** — beleidsmedewerker OCW | Gebruikt enkel toetsenbord (motorische beperking) | Maakt overzicht voor compliance-rapport |

---

## 2. Testomgeving

- **Browsers**: Chrome 120+, Firefox 120+, Safari 17+, Edge 120+
- **Devices**: Desktop 1440×900, laptop 1280×800, tablet 768×1024, mobiel 375×667
- **Hulpmiddelen**: NVDA (Windows), VoiceOver (macOS), tools: axe DevTools, Lighthouse, WAVE
- **Dataset**: 96 inventarisitems uit Deel I/A (Octrooien 1–22, Resoluties 23–96)

---

## 3. Functionele testcases

### TC-1 · Eindgebruiker zoekt naar VOC-octrooien (Mira)
| Stap | Actie | Verwacht resultaat |
|---|---|---|
| 1 | Open `index.html` | Hero met titel "Verenigde Oost-Indische Compagnie" zichtbaar, tab "Inventaris" actief |
| 2 | Bekijk kerncijfers (Periode, Inv.nrs, Online, Taal) | 4 stats zichtbaar; "9.812 (57%) online" in NA-blauw geaccentueerd |
| 3 | Lees lijst — sectiekop "Octrooien (1–22)" sticky bovenaan | Bij scrollen blijft sectiekop staan tot volgende sectie |
| 4 | Tik in zoekveld: `intestaat` | Lijst krimpt naar 1 resultaat (inv.nr 20); active-pill `Zoekterm: "intestaat"` verschijnt |
| 5 | Klik rij inv. 20 | Detailpaneel rechts toont titel, datum 17 aug 1661, status "Fysiek", "Reserveer studiezaal"-knop |
| ✅ | **Pass-criterium** | Stappen 1–5 zonder fouten, max 3 klikken tot detail |

### TC-2 · Filter "alleen online" (Henk)
| Stap | Actie | Verwacht |
|---|---|---|
| 1 | Klik chip "Online" (groene dot) | Alle resultaten met groene status; aantal in `result-count` toont nieuwe count |
| 2 | Active-pill `Online beschikbaar` zichtbaar | Klikken op `×` herstelt naar "Alles" |
| 3 | Klik op online stuk (bv. inv. 2) | Detail toont preview-thumbnail van charter met "Open in viewer"-knop |
| 4 | Klik "Open in viewer" | Nieuw tabblad `viewer.html?n=2` opent met scan |
| ✅ | **Pass** | Filter→detail→viewer flow ≤ 3 klikken, viewer opent in nieuw tab (niet huidige) |

### TC-3 · Periode-filter (Mira)
| Stap | Actie | Verwacht |
|---|---|---|
| 1 | Klik chip "Periode: 1602–1795" | Popover met dual-range slider en numerieke inputs |
| 2 | Sleep linker thumb naar 1660 | Label updates live naar "Periode: 1660–1795"; lijst krimpt |
| 3 | Tik in "Tot"-veld: `1680` | Lijst toont alleen items met year ∈ [1660,1680] |
| 4 | Sluit popover (klik ergens anders) | Active-pill `1660 – 1680` zichtbaar boven lijst |
| 5 | Klik `×` op pill | Periode reset naar 1602–1795 |
| ✅ | **Pass** | Slider en input synchroon, lijst direct bijgewerkt |

### TC-4 · Genealoog op zoek naar voorouder (Henk)
| Stap | Actie | Verwacht |
|---|---|---|
| 1 | Tab "Beschrijving" | Tekst over VOC, hiaten, gebruiksaanwijzing |
| 2 | Klik "inventarislijst" link in stap 1 van "Hoe te gebruiken" | Springt terug naar Inventaris-tab, bovenaan pagina |
| 3 | Tik zoekterm: `boedel` | Resultaat inv. 21 (curatoren over boedels VOC-dienaren) |
| 4 | Lees detailpaneel | Datering 1665, status Online, ID `urn:nl:nationaalarchief:1.04.02:21` |
| ✅ | **Pass** | Onderzoek-flow voor non-expert in 4 stappen voltooid |

### TC-5 · Viewer — beleidsmedewerker (Diederik, alleen toetsenbord)
| Stap | Toetsen | Verwacht |
|---|---|---|
| 1 | `Tab` door header tot zoekveld → typ `octrooi` | Lijst filtert |
| 2 | `↓` om door rijen te bladeren | Selectie verschuift, detailpaneel update, geen muis nodig |
| 3 | `Enter` op online-rij | Viewer opent in nieuw tabblad |
| 4 | In viewer: `→` `→` | Bladert naar scan 3 |
| 5 | `+` `+` `+` | Zoom 144%, 173%, 207% — toolbar toont % |
| 6 | `R` | Rotatie 90° |
| 7 | `F` | Volledig scherm — chrome verdwijnt |
| 8 | `Esc` | Volledig scherm uit |
| ✅ | **Pass** | Volledige flow zonder muis; geen focus-trap; visible focus-ring overal |

### TC-6 · Viewer — annotaties en transcriptie
| Stap | Actie | Verwacht |
|---|---|---|
| 1 | Open `viewer.html?n=20` | Stage met perkament-scan, drop-cap "Wy", zegel rechts |
| 2 | Klik gele highlight op scan | Alert/popup met annotatie-tekst |
| 3 | Tab "Transcriptie" rechts | Volledige tekst octrooi 1661 met gele markering op `Successie ab intestato` en `Choromandel, Bengalen…` |
| 4 | Tab "Annotaties" | 3 annotaties met regel-verwijzingen |
| 5 | Beeldinstellingen: schuif "Helderheid" naar 130% | Scan zichtbaar lichter, label "130%" |
| 6 | Klik "Herstel standaard" | Sliders terug naar 100/100/100/0° |
| ✅ | **Pass** | Alle interacties responsief, geen jank, transcriptie scrollt onafhankelijk |

### TC-7 · Reset alle filters
| Stap | Actie | Verwacht |
|---|---|---|
| 1 | Pas 3 filters toe (status, periode, zoek) | 3 active-pills + "Wis alle filters"-knop verschijnen |
| 2 | Klik "Wis alle filters" | Alle pills weg, lijst toont 96 stukken, popovers gereset |
| ✅ | **Pass** | Eén klik herstelt initiële staat |

---

## 4. WCAG 2.1 AA toegankelijkheidstests

### A11Y-1 · Skip-links & landmarks
- [ ] `Tab` op load → eerste focus toont "Direct naar de hoofdinhoud" (zichtbaar) — **2.4.1 Bypass Blocks**
- [ ] Tweede `Tab` → "Direct naar filters"
- [ ] Page heeft genoeg landmarks: `<header role="banner">`, `<nav aria-label>`, `<main>`, `<aside>`, `<footer role="contentinfo">` — **1.3.1 Info & Relationships**
- [ ] Screenreader landmark-navigatie (NVDA: `D`) springt logisch

### A11Y-2 · Heading hierarchy
- [ ] Slechts 1× `<h1>` ("Verenigde Oost-Indische Compagnie") — **1.3.1**
- [ ] `<h2>` voor section titels, `<h3>` voor section-headers in lijst, geen overgeslagen niveaus
- [ ] Tool: WAVE → 0 heading-fouten

### A11Y-3 · Kleurcontrast (4.5:1 voor tekst < 18px, 3:1 voor groot)
| Element | Voorgrond | Achtergrond | Ratio | Pass? |
|---|---|---|---|---|
| Body tekst (`text-ink` #0e1b2c) | #0e1b2c | #ffffff | 16.4:1 | ✅ AAA |
| Secundair (`text-ink2` #39414e) | #39414e | #ffffff | 9.6:1 | ✅ AAA |
| Mute (`text-mute` #5d6470) | #5d6470 | #ffffff | 6.3:1 | ✅ AAA |
| NA-blauw link (`text-na-700` #0f3360) | #0f3360 | #ffffff | 11.3:1 | ✅ AAA |
| Status groen (`#0f5a2e`) | #0f5a2e | #ffffff | 7.0:1 | ✅ AAA |
| Status amber (`#7a5012`) | #7a5012 | #ffffff | 7.4:1 | ✅ AAA |
| Status rood (`#86251a`) | #86251a | #ffffff | 7.8:1 | ✅ AAA |
| Primary button text op #0f3360 | #ffffff | #0f3360 | 11.3:1 | ✅ AAA |
| Top strip op `bg-na-700` | #ffffff | #0f3360 | 11.3:1 | ✅ AAA |

**Tool**: axe DevTools "Color contrast" check → 0 issues verwacht. — **1.4.3 Contrast (Minimum)**

### A11Y-4 · Niet-tekst alternatieven
- [ ] Alle decoratieve SVG's hebben `aria-hidden="true"` — **1.1.1**
- [ ] Iconen-met-functie (zoeken, sluiten) hebben omliggend `<button aria-label="…">`
- [ ] Charter-preview heeft beschrijvende `aria-label="Open scans in viewer"`
- [ ] Logo "NA" gemarkeerd `aria-hidden="true"`, link heeft eigen `aria-label="Nationaal Archief — naar startpagina"`

### A11Y-5 · Focus-management
- [ ] `:focus-visible` rendert 3px NA-blauwe outline met 2px offset — **2.4.7 Focus Visible**
- [ ] Tab-volgorde logisch: top-strip → header-nav → zoek → menu → tabs → filters → lijst → detail → footer — **2.4.3 Focus Order**
- [ ] Focus springt nooit naar verborgen elementen (`hidden`-attribuut werkt)
- [ ] Detail-paneel ververst zonder focus te verliezen

### A11Y-6 · Toetsenbord-bediening
- [ ] **2.1.1 Keyboard**: alle muisacties hebben toetsenbord-equivalent
  - [ ] Filter-chips: `Space` of `Enter` toggled
  - [ ] Year slider: `←→` (1 jaar), `Page Up/Down` (10 jaar), `Home/End` (extremen) — **WAI-ARIA APG slider pattern**
  - [ ] Lijst-item: `Enter` (open viewer indien online), `Space` (selecteer)
  - [ ] Globaal: `/` focus zoek, `↑↓` rij wisselen, `Enter` open viewer
- [ ] **2.1.2 No keyboard trap**: kan altijd uit popovers via `Tab` of `Esc`

### A11Y-7 · Semantische statusinformatie (geen color-only)
- [ ] Status nooit alleen via dot-kleur — altijd ook tekstlabel ("Online" / "Fysiek" / "Beperkt") — **1.4.1 Use of Color**
- [ ] In detailpaneel: `aria-label="Beschikbaarheid: Online beschikbaar"` op status-element
- [ ] Filterchip "Online" heeft tekstlabel naast dot

### A11Y-8 · Live regions
- [ ] `<div role="status" aria-live="polite">` kondigt resultaat-aantal aan na elke filter-actie — **4.1.3 Status Messages**
- [ ] Test: NVDA aan, filter toepassen → "33 resultaten getoond" wordt voorgelezen
- [ ] Test: zoek typen → live count update, niet té vaak (alleen bij definitieve waarde)

### A11Y-9 · Formulier-labels
- [ ] Alle `<input>` hebben `<label>` (visueel of `sr-only`) — **3.3.2 Labels or Instructions**
- [ ] Zoekvelden: `<label class="sr-only">` voor screenreader
- [ ] Year-inputs: zichtbare labels "Vanaf" / "tot"
- [ ] Sort-dropdown: `<label for="sort">Sorteer:</label>`

### A11Y-10 · Reduced motion
- [ ] CSS `@media (prefers-reduced-motion: reduce)` zet alle transitions ≤0.01ms — **2.3.3 Animation from Interactions (AAA)**
- [ ] Test: macOS "Reduce motion" aan → smooth-scroll wordt instant

### A11Y-11 · Zoom & responsive
- [ ] Browser zoom 200% → geen horizontaal scrollen, alle content bereikbaar — **1.4.10 Reflow**
- [ ] Tekst-spacing test: `letter-spacing: 0.12em; word-spacing: 0.16em; line-height: 1.5` injecteren — geen knipverlies — **1.4.12 Text Spacing**
- [ ] Mobiel 375px: tabel reduceert naar 3 kolommen (status verbergt naar detail)

### A11Y-12 · Taal & document
- [ ] `<html lang="nl">` correct
- [ ] Engelse link gemarkeerd: `<a hreflang="en" lang="en">English</a>` — **3.1.2 Language of Parts**
- [ ] `<title>` beschrijvend per pagina (index ≠ viewer)

### A11Y-13 · Viewer-specifiek
- [ ] Stage-canvas alternatief: transcriptie-tab biedt volledige tekst → blinde gebruiker krijgt content — **1.1.1**
- [ ] Annotatie-overlay heeft `data-anno` tekst, klik triggert leesbare popup
- [ ] Toolbar-knoppen allemaal `aria-label` (geen alleen-icoon)
- [ ] Page-counter `<span>1 / 4</span>` is leesbaar; sliders hebben `role="slider"` met `aria-valuenow`

---

## 5. Cross-device responsive checks

| Viewport | Test | Verwacht |
|---|---|---|
| 1440×900 desktop | Standaard layout | Lijst 7/12 + detail 5/12 naast elkaar |
| 1024×768 tablet landscape | Layout breekt | Detail onder lijst (col-span-12) |
| 768×1024 tablet portrait | Filterchips wrappen | "Meer filters" popover behoudt 320px |
| 375×667 mobiel | Datum-kolom verbergt | Status alleen via dot+label in compact rij |
| 320×568 (kleinste) | Hero stacked | Cijfers 2 kolommen, h1 32px |

---

## 6. Performance

| Metric | Doel | Test |
|---|---|---|
| First Contentful Paint | <1.5s | Lighthouse |
| Largest Contentful Paint | <2.5s | Lighthouse |
| Cumulative Layout Shift | <0.1 | Lighthouse |
| Total Blocking Time | <200ms | Lighthouse |
| Bundle | <100kB (excl. fonts) | Network tab |

---

## 7. Acceptatiecriteria voor go-live

- [ ] **Alle TC-1 t/m TC-7** slagen op Chrome + Firefox + Safari
- [ ] **Alle A11Y-1 t/m A11Y-13** geverifieerd, axe DevTools toont 0 critical/serious issues
- [ ] **Lighthouse Accessibility score** ≥ 95
- [ ] **Lighthouse Performance score** ≥ 90 op desktop, ≥ 75 op mobiel
- [ ] **Manuele NVDA-test** — Mira-flow voltooid uitsluitend op gehoor in <5 min
- [ ] **Manuele toetsenbord-test** — Diederik-flow voltooid in <3 min zonder muis
- [ ] **Bron-controle** — alle 96 inv.nrs corresponderen met `nationaalarchief.nl/onderzoeken/archief/1.04.02`

---

## 8. Bugs / opmerkingen-template

```
ID:        BUG-001
TC:        TC-3 stap 2
Browser:   Firefox 121 / Windows
Severity:  high / medium / low
WCAG:      (indien van toepassing) 2.1.1
Beschrijving:
Verwacht:
Actueel:
Screenshot/recording:
```

---

## 9. Bekende beperkingen (niet-bug)

- **Echte scans**: viewer toont gestyleerde placeholder, geen IIIF-call naar NA-server (zou CORS-policy van NA vereisen)
- **17.297 items**: slechts 96 in dataset; uitbreiding vereist server-side pagination
- **Login/dossier**: knop aanwezig, geen backend
- **Vertaling EN**: link aanwezig, geen vertaling

---

*Einde testdocument — bij twijfel raadpleeg `index.html` regel-commentaar of `viewer.html` toolbar-tooltips.*
