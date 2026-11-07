# Next.js a Vercel

Pasti, které platí, když aplikace stojí na Next.js nebo běží na Vercelu. Obecná pravidla ze `SKILL.md` platí dál.

- **Veřejné proměnné mají předponu `NEXT_PUBLIC_`.** Všechno s ní se zabalí do kódu pro prohlížeč.
- **Něco funguje lokálně a na Vercelu ne.** Skoro vždycky chybějící proměnná prostředí ve Vercelu, nebo kód, který počítá s tím, že běží v prohlížeči, a spustí se na serveru.
- **Data jsou stará.** Next.js si stránky a dotazy drží v mezipaměti agresivněji, než člověk čeká. Když uživatel po uložení vidí starý stav, hledej tohle, ne chybu v ukládání.
- **Náhledová nasazení na Vercelu** musí mít proměnné vývojové databáze, nikdy ostré.
