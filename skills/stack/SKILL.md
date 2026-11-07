---
name: stack
description: Jak se věci dělají v tomhle stacku a kde jsou pasti. Použij, když píšeš kód, zakládáš tabulku, řešíš přístup k datům, nasazuješ, nebo když něco funguje lokálně a jinde ne.
---

Tenhle dokument je **cache na to, co se z dokumentace nevyčte**: pasti a zvyklosti. Podrobnosti rozhraní si dohledej v aktuální dokumentaci, neopisuj je sem. Zastaraly by.

## Na čem projekt stojí

Zjisti to ze sekce Stack v `AGENTS.md`, a když tam není, ze souborů projektu. Pravidla níž platí v každém stacku. K nim:

- **Když projekt používá Supabase**, přečti si soubor `supabase.md` ve složce tohohle skillu.
- **Když aplikace stojí na Next.js nebo běží na Vercelu**, přečti si soubor `nextjs-vercel.md` ve složce tohohle skillu.
- **Když projekt má soubor `STACK.md`**, přečti si ho. Jsou v něm pasti, na které se přišlo v tomhle projektu.

Past, která tě stála čas a z dokumentace se nevyčte, zapiš do `STACK.md` v kořeni projektu (když není, založ ho), ať ji příští sezení nehledá znovu.

## Kde se rozhoduje, kdo smí na data

Nejdřív zjisti, kde v tomhle stacku leží pravidla přístupu:

- **V databázi**, když aplikace sahá na data přímo z prohlížeče nebo z telefonu (Supabase, Firebase a podobné). Pak jsou pravidla v databázi jediná ochrana, protože klíč v aplikaci vidí každý.
- **V kódu serveru**, když se k datům jde jen přes vlastní server (Django, Rails, Laravel, Express a podobné). Pak každý endpoint musí ověřit, kdo se ptá a jestli na to smí.

Testy oprávnění miř tam, kde pravidla leží.

## Tři způsoby přístupu k datům

Pletou se nejčastěji ze všeho:

- **Veřejný přístup z prohlížeče nebo aplikace.** Pravidla na něj platí. Jeho klíč je veřejný, počítej s tím, že ho každý vidí.
- **Serverový přístup za přihlášeného uživatele.** Pravidla platí taky, jen se běží na serveru.
- **Servisní přístup.** Pravidla **obchází úplně**. Patří výhradně do serverového kódu, který dělá něco za všechny, třeba zpracování platby.

Servisní klíč nesmí nikdy skončit v prohlížeči ani v aplikaci v telefonu a nikdy se s ním nesmí psát test oprávnění. Test se servisním přístupem projde i na úplně otevřených datech.

## Pravidla přístupu k datům

- **Zapni ochranu, když vzniká tabulka nebo nový endpoint, ne potom.** Tabulka bez ochrany je s veřejným klíčem čitelná komukoli z internetu. Nechráněná data jsou nejčastější díra v aplikacích stavěných s AI.
- **Pravidla piš pro čtení, zápis, úpravu i mazání zvlášť.** Výchozí stav je zákaz.
- **Soubory mají vlastní pravidla**, oddělená od tabulek. Zabezpečená tabulka neznamená zabezpečený soubor.
- Ke každé nové tabulce nebo endpointu s daty patří **test oprávnění ve stejném řezu**.

## Migrace

Struktura databáze vzniká migračními soubory v projektu. Jsou jediný zdroj pravdy a obě prostředí se z nich postaví stejně.

- **Změnu dělej novou migrací**, i když opravuješ včerejší. Hotová migrace už jinde proběhla, takže její přepsání se v druhém prostředí nikdy neprojeví.
- **Změny schématu dělej migrací, klikání v konzoli poskytovatele nech na prohlížení dat.** Co vzniklo klikáním, není v projektu a druhé prostředí o tom neví.
- **Když stack generuje typy ze schématu, po změně schématu je přegeneruj** a ulož je s migrací. Rozejité typy vypadají jako chyba v kódu a hledá se dlouho.
- **Do ostré databáze jde struktura a pravidla, nikdy data.** Vývojová data patří do souboru s výchozím naplněním, který se pouští jen na vývoji.

## Proměnné prostředí

Cokoli, co se zabalí do kódu pro prohlížeč nebo do aplikace v telefonu, je **veřejné**. Stacky to značí předponou (`NEXT_PUBLIC_`, `VITE_`, `EXPO_PUBLIC_`); zjisti, jakou má ten tvůj. Servisní klíč, klíč k platbám ani nic podobného tam nesmí.

Když se tajný klíč jednou dostal do kódu nebo do historie verzí, **nestačí ho smazat.** Musí se změnit u té služby.

## Prostředí

Vývojová a ostrá databáze jsou **dvě oddělená místa**, s jinou adresou a jinými klíči. Náhledová nasazení musí mířit na vývojovou, nikdy na ostrou. Tohle se snadno přehlédne a projeví se to tím, že si někdo na náhledu smaže skutečná data.

## Pasti, které stojí čas

- **Něco funguje lokálně a v provozu ne.** Skoro vždycky chybějící proměnná prostředí na hostingu, nebo kód, který počítá s tím, že běží v prohlížeči, a spustí se na serveru.
- **Data jsou stará.** Když uživatel po uložení vidí starý stav, hledej mezipaměť, ne chybu v ukládání.
- **Zpracování platby se dělá po návratu z platební brány.** Nikdy. Jistota o zaplacení pochází z oznámení od poskytovatele na server, uloženého tak, aby dvojí doručení nic nezkazilo. Návrat uživatele je jen obrazovka.
- **Placená služba se volá v cyklu.** Nejhorší faktury nevznikají z provozu, ale z jednoho dotazu, který se volá pořád dokola.

## Když si nejsi jistý rozhraním

Dohledej si to v aktuální dokumentaci. Ta se mění rychleji, než se stíhá přepisovat tenhle soubor.
