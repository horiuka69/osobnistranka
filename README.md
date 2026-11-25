# Osobní stránka – Horia Marin

Jednoduchá osobní stránka (HTML + CSS) s důrazem na přístupnost.

## Media rules

| Pravidlo | Větev | Co se mění a proč |
| --- | --- | --- |
| Šířka okna (`min-width`) | `feature/sirka` | Od 30em se navigace přeskupí ze sloupce do řádku a karty do dvou sloupců, od 48em je úvod vedle fotky a od 64em jsou tři karty vedle sebe, aby stránka fungovala od malého telefonu po široký monitor bez vodorovného posuvníku. |
| Tmavý režim (`prefers-color-scheme`) | `feature/tmavy-rezim` | Při tmavém nastavení systému se přepnou CSS proměnné v `:root` na tmavé barvy a `color-scheme` (v CSS i v `<meta>`) přizpůsobí i posuvníky a formulářové prvky, aby stránka neoslňovala a text měl dál dostatečný kontrast. |
| Vyšší kontrast (`prefers-contrast`) | `feature/vyssi-kontrast` | Při požadavku na vyšší kontrast má text černobílé barvy s kontrastem 21 : 1 (WCAG AAA), okraje jsou silnější, odkazy vždy podtržené a pozadí bez přechodů, aby byl obsah čitelný i pro lidi se slabším zrakem. |
| Omezený pohyb (`prefers-reduced-motion`) | `feature/omezeny-pohyb` | Při omezeném pohybu se vypne animace vyjetí karet, přechody odkazů, tlačítka a karet, jejich posun při najetí i plynulé rolování, protože pohyb může lidem s vestibulárními poruchami způsobit nevolnost (změna při najetí zůstává vidět barvou okraje a podtržením). |
| Průhlednost (`prefers-reduced-transparency`) | `feature/pruhlednost` | TODO |
| Vynucené barvy (`forced-colors`) | `feature/vynucene-barvy` | TODO |
| Kombinace (tmavý režim + vyšší kontrast) | `feature/vyssi-kontrast` | Když je zapnutý tmavý režim a zároveň vyšší kontrast, použije se bílý text na černém pozadí a světle modré odkazy (≥ 7 : 1), protože samotné pravidlo pro vyšší kontrast by tmavé schéma přepsalo na světlé. |

