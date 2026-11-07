---
name: zaloz-projekt
description: Připrav projekt pro ostatní skilly. Spusť jednou na začátku, nebo na existující aplikaci, kterou chceš dát do pořádku.
disable-model-invocation: true
---

Spouští se jednou za projekt. Funguje na prázdné složce i na aplikaci, která už existuje.

Nejdřív zjisti fakta sám: co ve složce je, jestli je to git repozitář, na čem projekt stojí (`package.json`, `pyproject.toml`, `composer.json` a podobně) a v jakých verzích, jaké služby jsou napojené, jestli je nainstalované `gh` a jestli je uživatel přihlášený. Na nic z toho se neptej.

## 1. Vyzpovídej se na záměr

Zavolej skill "vyzpovidej". Ptáš se na **celý produkt, ne na první funkci**: jen na to, co mění tvar všeho, co se kdy postaví, a ze čeho vzniká `PRODUKT.md`, `KONTEXT.md` a `rozhodnuti/`. Jak přesně se která věc chová (co se stane při smazání, co vidí cizí člověk, jestli je to kopie nebo živý odkaz), se řeší až ve zpovídání na tu jednu věc před `/vibe:zadani`. Když se k takové otázce dostaneš, řekni "tohle probereme, až na to dojde", a jdi dál.

Ptej se jen na to, na co uživatel umí odpovědět z toho, jak má fungovat jeho produkt:

- Co ta aplikace má umět a komu slouží.
- **Co všechno to má umět**: seznam věcí, jednou větou každá, v pořadí, v jakém je chce mít. Bez detailu.
- Budou v ní účty více lidí a budou tam data, která nemá vidět nikdo další?
- Poteče přes ni platba?
- Jak se lidi budou přihlašovat?
- Kde chceš mít seznam úkolů: **(a)** přímo v projektu jako soubory, nic nenastavuješ, vidíš jen ty, nebo **(b)** na GitHubu jako issues, uvidí to i ostatní, ale potřebuješ účet a `gh`. Doporuč (a), pokud na tom nedělá s někým dalším.

Slovo **stavět** si drží skill "postav" a znamená řez funkčnosti. Tady nestavíš aplikaci, jen zakládáš základ, tak to tak i pojmenuj: po potvrzení shrnutí řekni, že teď **založíš základ projektu**, ne že to postavíš. Jinak uživatel čeká hotovou aplikaci a dostane prázdný projekt.

## 2. Rozhodni technické věci sám

Neptej se. Rozhodni a jednou větou zmiň důsledek.

Výchozí volba: **Next.js, Supabase, Vercel, Playwright na prohlížeč, Vitest na zbytek.** Když v projektu už něco běží, respektuj to, co tam je, a neměň to.

Jiný stack vyber, když ho uživatel sám chce, nebo když produkt potřebuje něco, co výchozí volba neumí: aplikaci v obchodech s aplikacemi, výpočty nebo data v Pythonu, napojení na systém, který už uživatel má. Vyber to, co se pro takovou věc běžně používá a má dobrou dokumentaci.

Stack zapiš do `rozhodnuti/` a do sekce Stack v `AGENTS.md`. Skill "stack" podle ní pozná, které pasti platí.

## 3. Založ paměť projektu

- `PRODUKT.md`, **krátký**, na jednu obrazovku: co to je a pro koho (dva odstavce), **Co bude umět** (číslovaný seznam věcí z rozhovoru, v pořadí, jednou větou každá), **Co do toho nepatří** (co uživatel vědomě odmítl), a věta, že detail každé věci vzniká až v jejím zadání a to, co se těžko vrací, je v `rozhodnuti/`. Není to zadání a nepíše se do něj, jak se věci chovají.
- `KONTEXT.md` se slovníkem z rozhovoru. Formát drží skill "slovnik".
- `rozhodnuti/` s prvním zápisem o zvoleném základu.
- `.ukoly/`, nebo nic, když si uživatel vybral GitHub. Do složky dej `README.md` s pravidly z odstavce níž a **jeden soubor na každou věc ze seznamu "Co bude umět"**: název `napad-<slug>.md`, uvnitř jedna dvě věty a řádek "Dospecifikuje `/vibe:vyzpovidej`". Uživatel tak od první minuty vidí, co je před ním, a nic z rozhovoru neleží jen v chatu.
- `prototypy/` a vyřaď ji z buildu, kontroly typů i testů. Je jen pro `/vibe:prototyp`.
- U webové aplikace zkopíruj `scripts/screenshot.mjs` ze souboru `screenshot.mjs` ve složce tohohle skillu a přidej příkaz `screenshot` (v projektu na Node `npm run screenshot`). Pouští ho `postav`, když se dívá na hotovou obrazovku. Potřebuje Node a Playwright, i když zbytek aplikace stojí na něčem jiném. Když aplikace neběží na `localhost:3000`, dej do příkazu `--base <adresa>`. Složku `screenshots/` dej do `.gitignore`. U aplikace bez webového rozhraní zapiš do `AGENTS.md`, jak se dá hotová obrazovka ukázat, nebo že nijak.
- `AGENTS.md`, **krátký**, na jednu obrazovku. Patří do něj výslovná instrukce přečíst si `KONTEXT.md` kvůli slovníku a `rozhodnuti/` kvůli tomu, co už je rozhodnuté, sekce **Stack** (na čem projekt stojí a přesné znění příkazů `check`, `verify` a `screenshot`), výpis ostatních příkazů projektu s jednou větou u každého, k čemu je, pravidlo, že se hotovo dokládá výstupem, ne tvrzením, a pravidlo, že popis commitu je jedna věta česky. Ze zvyklostí sem piš jen ty, které se z projektu nedají vyčíst: kam se zakládá nový kód a podle čeho se věci jmenují. Co ve složkách je, si agent přečte sám, výpis složek sem nepatří.
- `CLAUDE.md`, ve kterém je jen řádek `@AGENTS.md`. Claude Code tak `AGENTS.md` načte sám a ostatní skilly se na jeho sekci Stack můžou spolehnout. Dva soubory se stejným obsahem se za měsíc rozejdou a jeden z nich začne lhát.

Pravidla složky `.ukoly/`, která patří do jejího `README.md`: `napad-<slug>.md` je věc, která se ještě neprobírala. Jakmile se na ni dojde, `/vibe:zadani` z ní udělá složku `<slug>/` se souborem `zadani.md`, `/vibe:rozsekej` do ní přidá řezy `01-*.md`, `02-*.md`, a nápad smaže. `/vibe:postav` řezy uzavírá a hotovou složku přesune do `hotovo/`. Výpis `.ukoly/` je tak seznam toho, co je před námi, výpis `hotovo/` seznam toho, co už funguje.

Až jsou soubory na místě, commit s popisem česky. Paměť projektu se commituje hned, ne až s prvním kódem.

Dlouhý soubor s pravidly způsobí, že se přestanou dodržovat i ta důležitá. Když `AGENTS.md` přeteče obrazovku, škrtej.

Až to založíš, řekni uživateli, co mu v projektu přibylo, jednou větou u každé věci a jazykem důsledků. Uživatel ty soubory vidí, nezná je a bude se ptát, nebo si vedle nich začne zakládat vlastní:

- `PRODUKT.md`: na jednu obrazovku, co to je a co všechno bude umět. Když se ti změní představa, řekni to a přepíšeme ho.
- `KONTEXT.md`: slovník tvého projektu, aby agent říkal věcem stejně jako ty. Můžeš do něj sáhnout, když se ti nějaké slovo nelíbí.
- `rozhodnuti/`: co jsme si dohodli a co by se těžko vracelo. Ne každá dohoda, jen ty důležité.
- `.ukoly/`: co je před námi. Teď jsou tam nápady, jeden soubor na každou věc ze seznamu, a z každého bude složka se zadáním a kusy práce, až na něj dojde. Sem se budeš dívat nejčastěji.
- `AGENTS.md`: pravidla pro agenta, číst ho nemusíš.

A jednu větu navíc: **paměť projektu jsou tyhle soubory, ne chat.** Když se zavře okno nebo se pokračuje na jiném počítači, agent si to podstatné přečte odsud. Proto se nic důležitého nedomlouvá jen v rozhovoru.

## 4. Postav ověřování

Vytvoř **dva** příkazy, protože ověřování má dvě rychlosti. Pravidla drží skill "pravidla-testu".

- **`check`** na průběžné psaní: kontrola typů a rychlé testy, bez prohlížeče. Musí být otázkou sekund, jinak se přestane pouštět.
- **`verify`** na konec řezu: kontrola typů, testy oprávnění, integrační testy a testy v prohlížeči (u aplikace bez prohlížeče testy celé cesty tam, kde běží, viz skill "pravidla-testu").

V projektu na Node jsou to `npm run check` a `npm run verify`. Jinde je udělej tak, jak se ve stacku příkazy běžně spouštějí, a přesné znění zapiš do sekce Stack v `AGENTS.md`. Ostatní skilly je odtud čtou.

`verify` skládej z menších příkazů, jeden na vrstvu. Když je něco červené, dá se pak pustit jen ta vrstva, místo celé sady dokola.

Uživateli to vysvětli jednou větou: **kratší běží pořád, delší rozhoduje o tom, jestli je hotovo.**

U webové aplikace doinstaluj prohlížeče pro testování (u Playwrightu `npx playwright install`). Samotný balíček nestačí a chybová hláška, která z toho jinak vypadne, je pro uživatele nesrozumitelná.

**Testy oprávnění potřebují skutečnou databázi, a nikdy ne tu ostrou.**

Výchozí uspořádání jsou **dva oddělené projekty u poskytovatele databáze**: jeden vývojový, na kterém se testuje a smí se rozbít, a jeden ostrý, kam přijdou skuteční lidé. Jiná adresa, jiné klíče, takže se z vývoje na ostrá data nedá dosáhnout ani omylem.

Ostrý projekt zakládej až v `/vibe:pust-to-ven`. Teď stačí vývojový.

### Založ vývojový projekt

Tenhle krok se **nedá přeskočit ani odložit na uživatele**. Skill pokračuje, až když se projekt k databázi připojí. Zbytek zakládání (základ, ověřování, pojistky) může být hotový dřív, ale skill nekončí bez databáze.

**Když databáze běží na Supabase, přečti si soubor `supabase.md` ve složce tohohle skillu a postupuj podle něj.**

U jiné databáze postupuj podle dokumentace jejího poskytovatele a dodrž totéž co u Supabase:

- Vývojová databáze je oddělená od budoucí ostré: jiná adresa, jiné klíče.
- Výchozí stav je zákaz přístupu, kde to poskytovatel umí nastavit. Kde ne, chrání data kód serveru a hlídají to testy oprávnění.
- Projekt je s databází propojený tak, aby šly pouštět migrace.

Když projekt zakládá uživatel v prohlížeči, naváděj ho po jedné sekci formuláře na jednu zprávu a u každé volby řekni důsledek, ne název.

### Společné pro každou databázi

Soubor s klíči (u Next.js `.env.local`) patří do `.gitignore`. **Servisní klíč obchází všechna pravidla přístupu.** Je jen na přípravu a úklid testovacích dat, nikdy nesmí skončit v kódu, který běží v prohlížeči, ani na localhostu, a nikam ho neposílej.

Na konci spusť test spojení a vypiš výstup. Musí v něm být `passed`. Test, který se bez klíčů **přeskočí**, je v pořádku jako ochrana pro později, ale `skipped` není důkaz spojení. Dokud test nevypíše `passed`, databáze není napojená a skill neskončil.

Uživateli řekni v důsledcích: kde se testuje, proč to není tam, kde budou skutečná data, a co ho to stojí.

Na prázdném projektu bude sada prázdná. To je v pořádku, naplní se s prvním řezem. Ale příkaz musí existovat a projít od první minuty.

## 5. Zapni pojistky

Nejdražší chyba není špatný kód, ale ztracená práce. Agent umí jedním příkazem zahodit den.

**Verzování od první minuty.** Když projekt není v gitu, založ ho a udělej první commit. Bez toho nemá smysl nic dalšího. Uživateli řekni jednou větou, co to pro něj znamená: **po každém hotovém kusu se dá kdykoli vrátit zpátky.**

**Zakázané příkazy.** Do nastavení oprávnění projektu přidej mezi zakázané ty, které nevratně mažou práci: tvrdý návrat na starší stav, mazání nesledovaných souborů, násilné přepsání vzdálené větve, mazání větví a reset propojené databáze (u Supabase `supabase db reset --linked`, u jiné databáze její obdoba). Nejde o hook ani o skript, jen o seznam v nastavení. Když agent takový příkaz potřebuje, řekne si o to a rozhodne člověk. Když zápis do nastavení neprojde, zkus to znovu později v tomhle sezení; uživateli řekni, co se zapisuje a proč, a nech to na sobě, ne na něm.

**Checkpointy jako návyk.** Po každém hotovém a ověřeném řezu commit. Ne po každé změně, ale po každém kusu, který funguje. Vysvětli uživateli rozdíl: **vracení v aplikaci není záloha.** Vrací jen změny souborů, které dělal agent přes editaci, ne to, co se stalo příkazem nebo v databázi.

## 6. Když aplikace už existuje

Navíc: zmapuj, co v ní je, a **hned spusť skill "kdo-vidi-co"**. U aplikace, která vznikla vibecodingem, je pravděpodobné, že data nejsou chráněná. Zjisti to dřív, než na tom začneš stavět dál.

## Hotovo je, když

- `verify` proběhne a skončí zeleně a test spojení s databází v něm vypsal `passed`, ne `skipped`.
- Projekt je propojený s vývojovou databází tak, aby šly pouštět migrace (u Supabase `supabase link`).
- Sekce Stack v `AGENTS.md` říká, na čem projekt stojí, a obsahuje přesné znění `check` a `verify`, u webové aplikace i `screenshot`.
- `KONTEXT.md` obsahuje pojmy z rozhovoru, ne obecné fráze. `PRODUKT.md` má seznam věcí z rozhovoru a v `.ukoly/` je na každou z nich nápad.
- Uživatel ví, kde má seznam úkolů a jak se spustí aplikace.
- Uživatel ví, k čemu jsou `PRODUKT.md`, `KONTEXT.md`, `rozhodnuti/` a `AGENTS.md`, a slyšel, že paměť projektu jsou soubory, ne chat.
- Paměť projektu je commitnutá.
- Projekt je v gitu, nebezpečné příkazy jsou zakázané a uživatel vlastními slovy řekl, jak se vrátí o krok zpět.
- Řekl jsi jednou větou, na čem to stojí, bez seznamu technologií.
- Uživatel ví, že aplikace zatím neexistuje, a ví, že další krok je **`/vibe:vyzpovidej` v novém sezení** na tu jednu věc, kterou chce postavit první. Ne `/vibe:zadani`: to jen sepisuje, co už zaznělo, a po tomhle skillu zaznělo jen to projektové.
