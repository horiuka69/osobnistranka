# Zpráva o přístupnosti (WCAG 2.2 AA)

Tato zpráva shrnuje testování přístupnosti osobní stránky [index.html](index.html) podle specifikace [WCAG 2.2](https://www.w3.org/TR/WCAG22/) na úrovni AA (všech 55 kritérií úrovně A a AA; kritérium 4.1.1 Parsing bylo ve WCAG 2.2 odstraněno).

## Jak bylo testováno

- **Lighthouse (Chrome DevTools, kategorie Accessibility):** po opravách skóre 100/100, 0 chyb a 0 varování.
- **W3C validátor (validator.w3.org):** 0 chyb a 0 varování.
- **Klávesnice:** průchod celou stránkou pomocí Tab, Shift+Tab a Enter, ověření skip linku i posouvání tabulky.
- **Přiblížení a rozestupy:** šířky 320 px (400 % z 1280 px), 640 px (200 %) a 1280 px bez vodorovného posuvníku; test rozestupů textu dle kritéria 1.4.12.
- **Media rules v Chrome DevTools (Rendering):** ověřena všechna povinná pravidla i jejich kombinace (tmavý režim + vyšší kontrast, tmavý režim + omezená průhlednost + omezený pohyb).

## Nálezy a opravy

### 1. Neshoda viditelného textu a přístupného jména loga (WCAG 2.5.3, A)
- **Zjištění:** Lighthouse hlásil chybu `label-content-name-mismatch` u odkazu `<a href="#uvod" class="logo" aria-label="Horia Marin – zpět na úvod">HM</a>`. Viditelný text `HM` nebyl součástí `aria-label`.
- **Oprava:** Viditelný text odkazu byl změněn na `Horia Marin` a zbytečný `aria-label` odstraněn.
- **Commit:** `59612ff` (`a11y: sjednocen viditelný text a název odkazu v hlavičce`)

### 2. Malá klikací plocha odkazů (WCAG 2.5.8, AA)
- **Zjištění:** Při měření rozměrů prvků v DevTools mělo logo na šířce 320 px výšku jen 23 px a odkazy v sekci Kontakt měly výšku 18 px, tedy méně než minimum 24×24 CSS px.
- **Oprava:** Přidán `padding`, `min-height` a mezery u loga, položek navigace i odkazů v kontaktech, takže všechny interaktivní prvky mají plochu alespoň 24×24 px (v praxi 28–48 px na výšku).
- **Commit:** `e486c96` (`a11y: zvětšena klikací plocha odkazů na minimálně 24x24 px`)

### 3. Lepivá hlavička zakrývala zaostřené prvky na úzkém displeji (WCAG 2.4.11, AA)
- **Zjištění:** Při průchodu klávesnicí zpět (Shift+Tab) na šířce 640 px zakryla lepivá hlavička (`position: sticky`) tlačítko „Přejít na kontaktní údaje“ a na 320 px zabírala hlavička 159 px při `scroll-padding-top: 4rem` (64 px).
- **Oprava:** Na menších displejích (`< 48em`) zůstává hlavička statická, lepivá je až od `48em` v jednom řádku; doplněno `scroll-padding-top: 6rem` na `html` a `scroll-margin-top: 6rem` na odkazech a sekcích. Zároveň byly odstraněny duplicitní `@media` bloky.
- **Commit:** `8fa6caa` (`a11y: lepivá hlavička nezakrývá zaostřené prvky na úzkém displeji`)

### 4. Nízký kontrast skip linku v tmavém režimu a okrajů prvků (WCAG 1.4.11, AA)
- **Zjištění:** Skip link měl natvrdo tmavé pozadí `#1a1a1a`, které v tmavém režimu splývalo s pozadím `#121212`. Okraje karet a tabulky měly kontrast pod 3 : 1 (světlý `#d4d4d8` měl 1,5 : 1, tmavý `#52525b` měl 2,8 : 1).
- **Oprava:** Skip link nyní používá `position: fixed`, barvy z proměnných `--accent` / `--accent-text` a výrazný okraj `--text`. Proměnná `--border` byla upravena na `#71717a` ve světlém (4,8 : 1) a `#9ca3af` v tmavém režimu (7,1 : 1).
- **Commit:** `39e71c9` (`a11y: zvýšen kontrast skip linku v tmavém režimu a okrajů prvků`)

## Novinky ve WCAG 2.2

| Kritérium | Úroveň | Stav | Jak je splněno / proč se nevztahuje |
| --- | --- | --- | --- |
| 2.4.11 Nezakryté zaměření (minimum) | AA | Splněno | Na úzkém displeji (`< 48em`) není hlavička lepivá; od `48em` má `position: sticky` a `scroll-padding-top: 6rem` + `scroll-margin-top: 6rem` brání zakrytí zaostřeného prvku. |
| 2.5.7 Pohyby tažením | AA | Nevztahuje se | Stránka nepoužívá žádné prvky ovládané tažením (drag and drop). |
| 2.5.8 Velikost cíle (minimum) | AA | Splněno | Všechny odkazy a tlačítka mají rozměry minimálně 24×24 CSS px (ověřeno v DevTools na 320 px i 1280 px). |
| 3.2.6 Konzistentní nápověda | A | Nevztahuje se | Jde o jednostránkový web bez opakujícího se prvku nápovědy napříč sadou stránek (kontaktní údaje jsou v sekci Kontakt). |
| 3.3.7 Opakované zadávání | A | Nevztahuje se | Na stránce není žádný formulář ani vícekrokové zadávání údajů. |
| 3.3.8 Přístupné ověřování (minimum) | AA | Nevztahuje se | Stránka nevyžaduje přihlášení ani ověřování uživatele. |

## Přehled kritérií WCAG 2.2 (úroveň A a AA)

### 1. Vnímatelnost

| Kritérium | Úroveň | Stav | Zdůvodnění |
| --- | --- | --- | --- |
| 1.1.1 Netextový obsah | A | Splněno | Všechny informační obrázky projektů mají popisný atribut `alt`, dekorativní profilová ikona má `alt=""`. |
| 1.2.1 Pouze zvuk a pouze video (předtočené) | A | Nevztahuje se | Stránka neobsahuje žádná zvuková ani video média. |
| 1.2.2 Titulky (předtočené) | A | Nevztahuje se | Stránka neobsahuje žádná videa. |
| 1.2.3 Zvukový popis nebo mediální alternativa (předtočené) | A | Nevztahuje se | Stránka neobsahuje žádná videa. |
| 1.2.4 Titulky (živé vysílání) | AA | Nevztahuje se | Stránka neobsahuje živé vysílání. |
| 1.2.5 Zvukový popis (předtočené) | AA | Nevztahuje se | Stránka neobsahuje žádná videa. |
| 1.3.1 Informace a vztahy | A | Splněno | Použity sémantické landmarky (`header`, `nav`, `main`, `section`, `article`, `footer`), správná hierarchie nadpisů `h1`–`h3`, seznamy `ul` a tabulka s `caption`, `thead`, `tbody` a `th` se `scope`. |
| 1.3.2 Smysluplné pořadí | A | Splněno | Pořadí prvků v DOM odpovídá vizuálnímu pořadí čtení na všech šířkách obrazovky. |
| 1.3.3 Smyslové charakteristiky | A | Splněno | Žádný text neodkazuje na prvky pouze tvarem, barvou nebo polohou na obrazovce. |
| 1.3.4 Orientace | AA | Splněno | Stránka neomezuje orientaci displeje, funguje na výšku i na šířku. |
| 1.3.5 Určení účelu vstupu | AA | Nevztahuje se | Na stránce nejsou žádné formulářové vstupy pro osobní údaje. |
| 1.4.1 Použití barvy | A | Splněno | Odkazy jsou odlišené podtržením (nejen barvou), aktivní prvek má viditelný `outline`. |
| 1.4.2 Ovládání zvuku | A | Nevztahuje se | Stránka nepřehrává žádný zvuk. |
| 1.4.3 Kontrast (minimum) | AA | Splněno | Ve světlém režimu text 17,4 : 1 a odkazy 6,7 : 1 (na přechodu ≥ 6 : 1), v tmavém text 15,3 : 1 a odkazy 10,4 : 1 (vše nad limitem 4,5 : 1). |
| 1.4.4 Změna velikosti textu | AA | Splněno | Velikosti písma a mezer jsou v `rem`/`em`; při zvětšení na 200 % se obsah zalamuje bez ztráty textu. |
| 1.4.5 Obrázky textu | AA | Splněno | Veškerý text na stránce je skutečný HTML text, ne obrázek textu. |
| 1.4.10 Přeformátování (Reflow) | AA | Splněno | Při šířce 320 px (ekvivalent 400 % na 1280 px) nemá stránka vodorovný posuvník; tabulka se posouvá pouze ve svém vlastním obalu. |
| 1.4.11 Kontrast netextových prvků | AA | Splněno | Rámeček fokusu (`#b45309` světlý 5,0 : 1, `#fbbf24` tmavý 11,5 : 1) i okraje karet a prvků (`#71717a` 4,8 : 1, `#9ca3af` 7,1 : 1) splňují ≥ 3 : 1. |
| 1.4.12 Rozestupy textu | AA | Splněno | Otestováno v DevTools s `line-height: 1.5`, `letter-spacing: 0.12em`, `word-spacing: 0.16em` a `margin-bottom: 2em` u odstavců – nic se neořezává ani nepřekrývá. |
| 1.4.13 Obsah při najetí myší nebo zaměření | AA | Nevztahuje se | Stránka nezobrazuje žádné tooltipy ani vyskakovací obsah při najetí myší nebo zaostření. |

### 2. Ovladatelnost

| Kritérium | Úroveň | Stav | Zdůvodnění |
| --- | --- | --- | --- |
| 2.1.1 Klávesnice | A | Splněno | Celou stránku lze projít a ovládat pouze klávesnicí (Tab, Shift+Tab, Enter, šipky v posuvném obalu tabulky). |
| 2.1.2 Žádná past na klávesnici | A | Splněno | Fokus lze kdykoli posunout dál i zpět po všech prvcích stránky, žádný prvek ho nezachytává. |
| 2.1.4 Zkratky s jedním znakem | A | Nevztahuje se | Stránka nedefinuje žádné vlastní klávesové zkratky. |
| 2.2.1 Nastavitelné časování | A | Nevztahuje se | Stránka nemá žádné časové limity. |
| 2.2.2 Pozastavení, zastavení, skrytí | A | Splněno | Jediná animace karet trvá 0,6 s (kratší než 5 s), neopakuje se a při `prefers-reduced-motion: reduce` se vypne. |
| 2.3.1 Maximálně tři záblesky nebo pod prahovou hodnotou | A | Splněno | Na stránce nic nebliká. |
| 2.4.1 Přeskočení bloků | A | Splněno | Na začátku `<body>` je funkční skip link „Přeskočit na hlavní obsah“ mířící na `<main id="obsah" tabindex="-1">`, plus sémantické landmarky. |
| 2.4.2 Titulek stránky | A | Splněno | `<title>` má popisný text „Horia Marin – student SPŠE Ječná \| osobní stránka“. |
| 2.4.3 Pořadí zaměření | A | Splněno | Pořadí fokusu kopíruje logickou strukturu dokumentu: skip link → navigace → úvod → tabulka → kontakt. |
| 2.4.4 Účel odkazu (v kontextu) | A | Splněno | Každý odkaz dává smysl i samostatně (např. „Přejít na kontaktní údaje“, „Můj profil horiuka69 na GitHubu“, „Web SPŠE Ječná“). |
| 2.4.5 Více způsobů | AA | Nevztahuje se | Web tvoří jediná stránka, ne sada více stránek; v rámci stránky lze přecházet přes navigaci i nadpisy sekcí. |
| 2.4.6 Nadpisy a popisky | AA | Splněno | Každá sekce má jasný nadpis (`h1`, `h2`, `h3`) a tabulka má `caption` i hlavičky řádků a sloupců. |
| 2.4.7 Viditelné zaměření | AA | Splněno | Pravidlo `:focus-visible` kreslí výrazný rámeček (`outline: 0.1875rem solid var(--focus)`) bez `outline: none`. |
| 2.4.11 Nezakryté zaměření (minimum) | AA | Splněno | Hlavička je na úzkém displeji statická a na širším displeji `scroll-padding-top` a `scroll-margin-top` (6 rem) zabraňují zakrytí prvku s fokusem. |
| 2.5.1 Gesta ukazatele | A | Nevztahuje se | Stránka nevyžaduje vícedotyková ani dráhová gesta. |
| 2.5.2 Zrušení akce ukazatele | A | Splněno | Odkazy se aktivují standardně až při puštění tlačítka myši (`click`), nikoli při stisknutí. |
| 2.5.3 Popisek v názvu | A | Splněno | Po opravě odkazu loga (`59612ff`) odpovídají přístupná jména všech odkazů jejich viditelnému textu. |
| 2.5.4 Ovládání pohybem | A | Nevztahuje se | Stránka nevyužívá naklánění ani pohyb zařízení. |
| 2.5.7 Pohyby tažením | AA | Nevztahuje se | Stránka neobsahuje žádné prvky ovládané tažením. |
| 2.5.8 Velikost cíle (minimum) | AA | Splněno | Po opravě (`e486c96`) mají všechny klikací prvky plochu alespoň 24×24 CSS px. |

### 3. Srozumitelnost

| Kritérium | Úroveň | Stav | Zdůvodnění |
| --- | --- | --- | --- |
| 3.1.1 Jazyk stránky | A | Splněno | Na kořenovém prvku je nastaveno `<html lang="cs">`. |
| 3.1.2 Jazyk částí | AA | Splněno | Text je celý v češtině; technické názvy a vlastní jména (HTML, CSS, JavaScript, Git, GitHub, Arduino) mají ve WCAG výjimku. |
| 3.2.1 Při zaměření | A | Splněno | Přesun fokusu na libovolný prvek nevyvolává žádnou neočekávanou změnu kontextu ani přesměrování. |
| 3.2.2 Při vstupu | A | Nevztahuje se | Stránka neobsahuje žádné formulářové prvky pro změnu nastavení nebo vstup. |
| 3.2.3 Konzistentní navigace | AA | Nevztahuje se | Web má jedinou stránku; hlavní navigace je stabilně v `<header>`. |
| 3.2.4 Konzistentní identifikace | AA | Splněno | Prvky se stejnou funkcí (navigační odkazy, karty projektů, externí odkazy) jsou označené i nastylované jednotně. |
| 3.2.6 Konzistentní nápověda | A | Nevztahuje se | Jednostránkový web bez opakujícího se mechanismu nápovědy napříč sadou stránek. |
| 3.3.1 Identifikace chyby | A | Nevztahuje se | Stránka neobsahuje formulář s validací vstupu. |
| 3.3.2 Popisky nebo instrukce | A | Nevztahuje se | Stránka neobsahuje žádné formulářové vstupy. |
| 3.3.3 Návrh opravy chyby | AA | Nevztahuje se | Stránka neobsahuje formulář s validací vstupu. |
| 3.3.4 Prevence chyb (právní, finanční, datové) | AA | Nevztahuje se | Stránka neumožňuje odesílání dat ani transakce. |
| 3.3.7 Opakované zadávání | A | Nevztahuje se | Stránka neobsahuje žádný proces zadávání údajů. |
| 3.3.8 Přístupné ověřování (minimum) | AA | Nevztahuje se | Stránka nemá přihlašování ani ověřování uživatele. |

### 4. Robustnost

| Kritérium | Úroveň | Stav | Zdůvodnění |
| --- | --- | --- | --- |
| 4.1.2 Název, role, hodnota | A | Splněno | Použity výhradně nativní HTML prvky (`a`, `nav`, `table`, `th`), posuvný obal tabulky má `role="region"` a `aria-labelledby`; W3C validátor i Lighthouse hlásí 0 chyb. |
| 4.1.3 Stavové zprávy | AA | Nevztahuje se | Stránka nemění svůj obsah dynamicky a nezobrazuje stavové hlášky. |
