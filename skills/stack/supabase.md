# Supabase

Pasti a zvyklosti, které platí, když projekt stojí na Supabase, ať je kolem cokoli. Obecná pravidla ze `SKILL.md` platí dál.

## Klíče

- **Publishable klíč** (dřív `anon`) je veřejný klíč pro prohlížeč a aplikaci. Pravidla přístupu (RLS) na něj platí.
- **Secret klíč** (dřív `service_role`) je servisní přístup. RLS obchází úplně.

## Pravidla a granty

- **Ke každé nové tabulce patří ve stejné migraci `grant` pro role `anon` a `authenticated`.** Automatické zpřístupnění tabulek je vypnuté (nastavuje se při zakládání projektu), takže bez grantu aplikace spadne na `permission denied for table`. Grant otevírá dveře, kdo jimi projde, řídí jen RLS.
- **Zákaz RLS se neprojeví jako chyba, ale jako prázdný výsledek.** U `update` a `delete` volání projde a jen se nic nestane. Testy tomu musí odpovídat: ověřují, že se nic nevrátilo a nic nezměnilo, ne že přišla chyba.
- **Soubory ve Storage mají vlastní pravidla**, oddělená od tabulek.

## Migrace

- **Migrace se pouští `supabase db push` na propojený vývojový projekt.** Bez `supabase link` neprojde nic. Vývojová databáze je hostovaná a reset je zakázaný, takže se migrace ověřují tím, že projdou pushem, a čistý průchod od nuly se zkouší až při zakládání ostrého projektu.
- **Typy se generují `supabase gen types`.**

## Pasti

- **Projekt zdarma se po týdnu bez provozu uspí.** Když testy najednou padají na spojení a v kódu se nic nezměnilo, je to tohle. Probudí se v konzoli.
- **Odkaz z mailu funguje jen v prohlížeči, kde se o něj žádalo.** Výchozí šablona nese kód svázaný s prohlížečem, takže odkaz otevřený na telefonu po žádosti z počítače skončí na "odkaz neplatí". Šablona musí nést `token_hash` a aplikace ho potvrdí přes `verifyOtp` s typem `email`. Šablonu a povolené adresy návratu nastavuje uživatel v konzoli, ty mu napíšeš přesný postup. Testy se přihlašují tokenem vyrobeným servisním klíčem, mail neposílají; projekt zdarma pošle jen pár mailů za hodinu a ty patří uživateli na ruční ověření.
