---
name: ukaz
description: Ukaž uživateli během rozhovoru varianty obrazovky nebo diagram v okně prohlížeče a přečti si, na co klikl. Použij, když by otázka byla jasnější ukázaná než popsaná.
---

Okno v prohlížeči běží vedle rozhovoru. Posíláš do něj obrazovky, uživatel se dívá, kliká na varianty a odpovídá dál v rozhovoru. Jeho kliknutí si přečteš.

Na proklikání chování a stavů (prázdný seznam, chyba, zrušení uprostřed) a na náhled, který má zůstat jako soubor, doporuč `/vibe:prototyp`.

## Kdy ano a kdy ne

Rozhoduj u každé otázky zvlášť. Test: **pochopí to uživatel líp, když to uvidí, než když si to přečte?**

- **Do okna:** které ze dvou rozložení, který vzhled, jak jdou obrazovky po sobě, diagram toho, co s čím souvisí.
- **Do rozhovoru:** co má věc umět, co má na obrazovce být, co se stane při chybě, kdo co smí, volba mezi přístupy popsanými slovy.

## Nabídni to

Než okno nabídneš, ověř, že ho uživatel uvidí: agent běží na stejném počítači jako jeho prohlížeč (ne v cloudu ani na vzdáleném stroji) a je nainstalovaný Node. Když ne, pokračuj textem.

U první otázky, která do okna patří, se zeptej, jestli ho uživatel chce. Samostatně, před kolem, ve kterém by se okno použilo; když máš nástroj na otázky s výběrem, polož to jím. Řekni, že se otevře okno v prohlížeči s variantami na kliknutí a že se tím sezení rychleji vyčerpá. Když odmítne, ptej se dál jen textem.

## Spusť okno

Až uživatel souhlasí:

```bash
bash "<složka tohohle skillu>/scripts/start-server.sh" --project-dir "<kořen projektu>" --open
```

Skript vypíše JSON. Ulož si z něj `url`, `screen_dir` a `state_dir`. Uživateli dávej vždycky **celou** adresu z `url` i s částí `?key=…`, bez ní se okno neotevře. Když `.nahled/` není v `.gitignore`, přidej ho.

Na Windows skript zůstane běžet v popředí. Spusť ho na pozadí a JSON si přečti z jeho výstupu, nebo ze souboru `state/server-info` v nejnovější složce v `.nahled/`.

## Smyčka

1. **Ověř, že okno běží:** ve `state_dir` je soubor `server-info` a není tam `server-stopped`. Okno se zastaví samo po čtyřech hodinách nečinnosti a taky když skončí sezení agenta. Pak ho spusť znovu se stejným `--project-dir` a **ulož si nové `screen_dir` a `state_dir`** z výstupu. Adresa zůstane stejná a otevřená stránka se připojí sama.
2. **Přečti si soubor `obrazovky.md` ve složce tohohle skillu**, když jsi to v tomhle sezení ještě neudělal.
3. **Zapiš obrazovku jako nový soubor** do `screen_dir` nástrojem na zápis souborů, s výstižným názvem: `rozlozeni.html`, další verze `rozlozeni-2.html`. Okno ukazuje vždycky nejnovější soubor.
4. **Napiš uživateli** adresu, jednou větou, co v okně je, a že odpovídá v rozhovoru a v okně může i klikat. Pak ukonči tah.
5. **V dalším tahu si přečti `state_dir/events`**, když existuje. Je v něm jeho klikání, jedno kliknutí na řádek; poslední volba bývá ta konečná. Hlavní odpověď je ale to, co napsal v rozhovoru.
6. **Když odpověď mění obrazovku, pošli novou verzi.** K další otázce jdi, až když tahle sedí.
7. **Když další otázka do okna nepatří, pošli čekací obrazovku** `ceka.html` (další `ceka-2.html`) s textem `<p class="subtitle">Pokračujeme v rozhovoru…</p>`, ať uživatel nekouká na vyřešenou volbu.

Otázka je hotová, když je rozhodnutá a řekl jsi v rozhovoru, na čem jste se shodli.

## Na konci

Až skončí zpovídání nebo rozhovor, kvůli kterému okno běží, zastav ho: `bash "<složka tohohle skillu>/scripts/stop-server.sh" "<složka sezení>"`, kde složka sezení je ta nad `screen_dir`. Obrazovky zůstanou v `.nahled/`. Co z nich vyplynulo a bude se těžko měnit, zapiš přes skill "slovnik" do `rozhodnuti/`.
