# Vývojová databáze na Supabase

## Založ vývojový projekt

Nejdřív zjisti, jestli je `supabase` CLI přihlášené (`supabase projects list`). Pak vyber cestu:

**Cesta přes CLI, když je přihlášené.** Uživatel nic nekopíruje. Vypiš mu existující projekty a zeptej se, jestli má některý vývojový, který smí rozbít, nebo jestli založit nový (`supabase projects create <nazev>-dev --region eu-central-1`). Pak `supabase link` na vybraný projekt a klíče vezmi z `supabase projects api-keys`. Uživateli řekni jen, že jsi projekt propojil a kde ho uvidí v konzoli. Nastavení Security z formuláře níž u projektu založeného z CLI nejde nastavit, proto každá tabulka dostane ochranu i zpřístupnění v migraci, viz skill "stack".

**Cesta přes prohlížeč, když CLI přihlášené není.** Projekt zakládá uživatel, ty ho navádíš. Řekni mu, ať otevře supabase.com, klikne na New project, a **počkej, až řekne, že formulář vidí.** Pak jdi shora dolů, **jedna sekce na jednu zprávu**, nech si popsat, co na obrazovce je. U každé volby řekni důsledek, ne název:

- **GitHub (optional)** - přeskočit. Připojení nechá Supabase samo nasazovat změny schématu z repozitáře. Migrace pouštíme z ruky a nechceš, aby ti do databáze něco sahalo dřív, než víš, co v ní je.
- **Project name** - s příponou `-dev`. Za měsíc bude vedle `-prod` a musí být na první pohled poznat, do kterého se dívá.
- **Database password** - hned Copy a uložit do správce hesel. Aplikace ho potřebovat nebude, ta jede na klíčích, ale bez něj se k databázi nedostane napřímo a znovu se nezobrazí.
- **Region** - nejbližší, z Česka Frankfurt (`eu-central-1`). Později se nedá změnit.
- **Postgres Type** v Advanced configuration - nechat `Postgres`. OrioleDB je alpha.

**Sekce Security je jediné místo formuláře, kde se rozhoduje o bezpečnosti, a dvě ze tří voleb jsou výchozí opačně, než je chceš.** Nehádej jejich stav, nech si přečíst, co je zaškrtnuté:

- ☑️ **Enable Data API** - nechat zaškrtnuté. Bez toho se aplikace k datům nedostane, `supabase-js` jede přes něj.
- ⬜️ **Automatically expose new tables** - **odškrtnout**, výchozí je zaškrtnuté. Supabase to sám v popisku doporučuje vypnout. Zaškrtnuté znamená, že nová tabulka je dosažitelná zvenku, jakmile vznikne, aniž by o tom kdokoli rozhodl. Odškrtnuté znamená, že se přístup musí ke každé tabulce napsat výslovně, a když se na to zapomene, aplikace spadne na `permission denied for table` i s návodem, co dopsat. To je ta chyba, kterou chceš. Opačně se nestane nic viditelného a data jsou venku.
- ☑️ **Enable automatic RLS** - **zaškrtnout**, výchozí je odškrtnuté. Založí pravidlo, které u každé nové tabulky zapne "dokud někdo výslovně neřekne, kdo sem smí, nesmí sem nikdo". Tohle je nejčastější díra ve vibecodovaných aplikacích: přibude funkce, přibude tabulka, pravidlo se k ní nedoplní. Tabulky naklikané v konzoli ochranu dostanou samy, ale my je zakládáme migracemi, a na ty to bez tohohle neplatí.

Uživateli to shrň jednou větou: jsou to dvě nezávislé pojistky a obě selhávají nahlas. Třetí jsou testy oprávnění v projektu.

Supabase výchozí hodnotu u expose postupně obrací, u nových projektů od dubna 2026 a u všech do konce října 2026. Když ji uživatel najde odškrtnutou rovnou, jen to potvrď a jdi dál.

## Vezmi si klíče

U cesty přes CLI si klíče vezmi sám a `.env.local` vyrob sám. U cesty přes prohlížeč počkej, až projekt naběhne, a proveď uživatele na **Project Settings → API keys**. Vyrob `.env.local` zkopírováním `.env.example` a dej do něj tři věci:

- Project URL → `NEXT_PUBLIC_SUPABASE_URL`
- publishable klíč (dřív `anon`) → `NEXT_PUBLIC_SUPABASE_ANON_KEY`
- secret klíč (dřív `service_role`, schovaný pod Reveal) → `SUPABASE_SERVICE_ROLE_KEY`

Nové projekty ukazují `sb_publishable_...` a `sb_secret_...`; starší klíče `anon` a `service_role` Supabase vyřazuje do konce roku 2026. Jsou to tytéž dvě role. Klíč `sb_secret_...` vzatý z CLI u některých projektů vrací `Invalid API key`; když se to stane, vezmi `service_role` a ověř testem, který klíč funguje.

U jiného frameworku než Next.js mají veřejné proměnné jeho předponu (třeba `VITE_`), viz skill "stack".

Test spojení je tady `npm run test:permissions`. Co musí vypsat, říká `SKILL.md`.

U cesty přes prohlížeč ještě projekt propoj: řekni uživateli, ať napíše `! supabase login` a dokončí přihlášení v prohlížeči, pak udělej `supabase link`. Bez propojení nejdou pouštět migrace a první řez s tabulkou se zasekne.

## Řekni rovnou, ať ho to nepřekvapí

- **Zdarma jsou dva aktivní projekty**, a to dohromady, ne na organizaci. Dvojice vývojový a ostrý se do toho vejde přesně, ale na další aplikaci už místo nezbude.
- **Projekt zdarma se po týdnu bez provozu uspí.** Když se po delší pauze vrátí a testy padají na spojení, není to chyba v kódu, jen se projekt musí probudit v konzoli. Uspaný projekt se do limitu nepočítá.
