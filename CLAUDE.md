# ZADÁNÍ PRO CLAUDE CODE: Webová prezentace závěrečného reportu z krizového řízení

## Co stavíme
Profesionální webový one-pager pro závěrečný report z krizového řízení firmy Automotive Interims. Report bude nasazen na GitHub Pages a chráněn přihlášením.

## Architektura a struktura repozitáře

```
automotive-interims-report/
├── index.html          # Hlavní HTML — layout, login, render engine, styly
├── content.md          # Obsah reportu v Markdown — JEDINÉ místo pro editaci obsahu
├── README.md           # Popis repozitáře
└── assets/
    └── ai-logo.png     # Logo Automotive Interims (stáhnout z webu automotive-interims.cz, malé decentní logo)
```

### Klíčový princip: oddělení obsahu od renderů
- `content.md` obsahuje VEŠKERÝ textový obsah reportu.
- `index.html` načítá `content.md` za běhu přes JavaScript fetch(), parsuje ho (použij knihovnu marked.js z CDN) a renderuje do HTML.
- Každá změna v `content.md` se automaticky projeví na webu po refreshi — bez nutnosti sahat do HTML.
- Obsah `content.md` je přiložen v souboru `Automotive_Interims_Zaverecny_Report.md` v tomto repozitáři. Překopíruj ho do `content.md`.

## Přihlášení (login screen)

### Požadavky
- Jednoduchý client-side login — report je určen jednomu člověku.
- **NESMÍ být hardcoded heslo v kódu.** Použij SHA-256 hash.
- V kódu bude uložen pouze SHA-256 hash uživatelského jména a hesla.
- Při přihlášení se zadané údaje zahashují v prohlížeči (Web Crypto API) a porovnají s uloženými hashi.
- Po úspěšném přihlášení se login schová a zobrazí se report.
- Session: uložit stav přihlášení do sessionStorage, aby při refreshi nemusel znovu zadávat heslo.

### Přihlašovací údaje (TYTO ÚDAJE ZAHASHUJ, NIKDY JE NEUKLÁDEJ V PLAINTEXTU):
- Jméno: `batek`
- Heslo: `automotive2026`

### Design login screenu
- Minimalistický, centrovaný formulář.
- Barevně v souladu s brand manuálem (viz níže).
- Logo Automotive Interims nad formulářem — decentní, malé.
- Toto je JEDINÉ místo, kde se logo zobrazí.

## Design a brand manual

### Barevná paleta (Paleta #3 — Teal + Mustard + Neutrals)
```
Primary Teal:     #008080
Accent Gold:      #FFDB58
Graphite:         #0E1116
Warm Bone:        #ECE8E0
Slate Grey:       #3E4A57
Fog Grey:         #E9EEF2
Deep Teal:        #0A6C6C  (hover stavy)
Muted Gold:       #C9A227  (alternativní akcent)
```

### Použití barev
- **Pozadí:** Střídání sekcí — Warm Bone (#ECE8E0) a bílá (#FFFFFF), případně Fog Grey (#E9EEF2) pro vizuální rytmus.
- **Texty:** Graphite (#0E1116) pro hlavní text, Slate Grey (#3E4A57) pro sekundární texty.
- **Nadpisy sekcí:** Primary Teal (#008080).
- **Akcentní prvky:** Accent Gold (#FFDB58) pro důležitá čísla, KPI boxy, highlight boxy. Používat střídmě.
- **CTA tlačítka:** Accent Gold s Graphite textem.
- **Hover efekty:** Deep Teal (#0A6C6C).
- **Traffic light systém v Sekci VI:** 🔴 použij červený tón, 🟡 použij žlutý/amber tón — NEPOUŽÍVEJ emoji, nahraď je vizuálními indikátory (tečky nebo štítky).
- **Risk Register (Sekce VII):** Obdobně vizuální indikátory místo emoji.

### Typografie
- Font: Inter nebo system-ui stack (sans-serif). Čistý, moderní, čitelný.
- Nadpisy: bold, teal.
- Body: 16-18px, dostatečný line-height (1.6-1.7).

### Layout a vizuální styl
- Maximální šířka obsahu: cca 800-900px, centrovaný.
- Dostatek whitespace — report musí "dýchat".
- Sticky navigace (TOC) na levé straně nebo nahoře — umožní rychlý skok na sekci.
- Sekce vizuálně oddělené — střídání pozadí, jemné bordery nebo spacing.

### Vypíchnutí důležitých čísel
V reportu jsou klíčová čísla, která chci vizuálně zvýraznit. Tam kde v markdown obsahu najdeš důležité metriky (počty projektů, BEP, konverzní poměry, počet nových accountů atd.), zobraz je jako:
- **KPI boxy / stat cards** — velké číslo, popisek pod ním, akcent barva (Gold nebo Teal).
- Typicky v Sekci I (exekutivní shrnutí) a Sekci V (výsledky).
- Příklady čísel k vypíchnutí: "4 → 10 projektů", "BEP = 13", "36 nových accountů", "0 % konverze new opps".

### Zdravotní karta (Sekce VI)
- Traffic light indikátory (🔴🟡) nahradit vizuálními badge/štítky s barvou.
- Každá oblast jako karta s indikátorem vlevo.
- Přehledné, scannovatelné — Batěk musí na první pohled vidět, kde je problém.

### Risk Register (Sekce VII)
- Tabulkový nebo kartový layout.
- Pravděpodobnost a dopad jako vizuální indikátory (barevné tečky).
- Kategorie jako štítky.

### Roadmapa (Sekce IX)
- Fáze vizuálně oddělené (timeline nebo cards).
- Priority jako barevné štítky (🔴 = červená, 🟡 = žlutá).
- Náročnost jako textový badge.

## Funkční tlačítka

### 1. Export do PDF
- Tlačítko "Uložit jako PDF".
- Spustí nativní print dialog prohlížeče (window.print()).
- Print-friendly CSS: skryje navigaci, login, tlačítka. Zachová formátování, barvy, tabulky.
- @media print stylesheet.

### 2. Kopírovat pro Word
- Tlačítko "Kopírovat do schránky pro Word".
- Zkopíruje obsah reportu jako **formátovaný rich text (HTML)** do schránky.
- Použij Clipboard API: navigator.clipboard.write() s Blob typu "text/html".
- Po vložení do Microsoft Word se zachová formátování — nadpisy, bold, tabulky.
- Po kliknutí zobrazit krátkou notifikaci "Zkopírováno do schránky".

### Umístění tlačítek
- Fixní floating panel vpravo dole nebo v horní liště vedle navigace.
- Decentní, nezasahující do čtení.

## Tonalita a styl obsahu (pro případné drobné úpravy v content.md)

Report je profesionální deliverable od seniorního krizového manažera pro majitele firmy. Platí:
- **Stručnost** — žádné zbytečné úvodní věty, které jen parafrázují nadpis. Rovnou k věci.
- **Neutrální tón** — místo "firma neměla / firma neumí" → popis stavu: "není definováno / chybí / slabá úroveň." Konstatování, ne obviňování.
- **Žádné přehnané výrazy** — žádné "katastrofický", "poprvé v historii", "bezprecedentní" apod.
- **Žádný self-promotion** — žádné "a s tím vám pomohu já", žádné zmínky NorthStar Lab v obsahu reportu.
- **Česky** — celý report je v češtině.

## Technické poznámky

### Hosting: GitHub Pages
- Repo bude pushnuté na GitHub.
- GitHub Pages zapnuté v nastavení repo (branch: main, root: /).
- URL: `https://<username>.github.io/automotive-interims-report/`
- Repo může být private — GitHub Pages funguje i s private repos.

### Markdown parsing
- Použij marked.js (z CDN: https://cdn.jsdelivr.net/npm/marked/marked.min.js).
- Po načtení a zparsování markdown aplikuj post-processing:
  - Nahraď 🔴 a 🟡 emoji vizuálními badge elementy.
  - Identifikuj klíčová čísla a zabal je do KPI boxů (můžeš použít speciální markdown syntax, např. `:::stat` bloky, nebo post-process na základě patternů).
  - Přidej ID atributy na sekce pro navigaci (TOC links).

### Responsivita
- Mobile-friendly. Na mobilu skrýt sidebar navigaci, zobrazit hamburger nebo top-nav.
- Tabulky responsivní (horizontální scroll na malých obrazovkách).

### Logo Automotive Interims
- Stáhni z webu https://www.automotive-interims.cz/ — použij jejich logo.
- Pokud není dostupné přímo jako soubor, stáhni screenshot a ořízni, nebo použij text "Automotive Interims" stylizovaný jako logo.
- Logo zobrazit POUZE na login screenu, decentně, malé.

## Shrnutí — co musí výsledek splňovat
1. ✅ Obsah oddělen od renderů (content.md vs index.html).
2. ✅ Login s SHA-256 hashovanými credentials.
3. ✅ Design dle brand manuálu (Teal + Gold + Neutrals).
4. ✅ Vypíchnutá důležitá čísla (KPI boxy).
5. ✅ Vizuální indikátory místo emoji (traffic lights, risk register).
6. ✅ Sticky navigace pro sekce.
7. ✅ Tlačítko "Uložit jako PDF" (print dialog).
8. ✅ Tlačítko "Kopírovat do schránky pro Word" (rich text HTML).
9. ✅ Responsivní design.
10. ✅ Print-friendly CSS.
11. ✅ Logo Automotive Interims pouze na login screenu.
12. ✅ Připraveno pro GitHub Pages deployment.
