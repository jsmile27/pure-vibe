# Jak psát obrazovky do okna

## Kousek, ne celá stránka

Do souboru piš jen obsah: nadpis, text a volby. Hlavičku, vzhled, stav připojení a zachycování kliknutí doplní okno samo.

Celou stránku (soubor začíná `<!DOCTYPE` nebo `<html>`) piš jen tehdy, když potřebuješ úplnou kontrolu nad vzhledem. Kliknutí se zachytí i tak.

Všechny texty, které uživatel uvidí, piš česky.

## Nejmenší příklad

```html
<h2>Které rozložení je lepší?</h2>
<p class="subtitle">Dívej se hlavně na to, co uvidíš jako první</p>

<div class="options">
  <div class="option" data-choice="a" onclick="toggleSelect(this)">
    <div class="letter">A</div>
    <div class="content">
      <h3>Jeden sloupec</h3>
      <p>Všechno pod sebou, nic neodvádí pozornost</p>
    </div>
  </div>
  <div class="option" data-choice="b" onclick="toggleSelect(this)">
    <div class="letter">B</div>
    <div class="content">
      <h3>Dva sloupce</h3>
      <p>Vlevo nabídka, vpravo obsah</p>
    </div>
  </div>
</div>
```

## Stavební kostky

**Volby A/B/C:** kontejner `options`, v něm `option` s `data-choice` a `onclick="toggleSelect(this)"`, uvnitř `letter` a `content`. Když smí uživatel vybrat víc možností najednou, přidej kontejneru `data-multiselect`.

**Karty s náhledem vzhledu:**

```html
<div class="cards">
  <div class="card" data-choice="tmavy" onclick="toggleSelect(this)">
    <div class="card-image"><!-- náhled --></div>
    <div class="card-body"><h3>Tmavý</h3><p>Popis</p></div>
  </div>
</div>
```

**Rámeček s maketou obrazovky:**

```html
<div class="mockup">
  <div class="mockup-header">Náhled: přehled objednávek</div>
  <div class="mockup-body"><!-- maketa --></div>
</div>
```

**Dvě makety vedle sebe:** obal dva `mockup` do `<div class="split">`.

**Pro a proti:**

```html
<div class="pros-cons">
  <div class="pros"><h4>Pro</h4><ul><li>…</li></ul></div>
  <div class="cons"><h4>Proti</h4><ul><li>…</li></ul></div>
</div>
```

**Kostky na maketu:** `mock-nav` (horní lišta), `mock-sidebar` (boční panel), `mock-content` (hlavní plocha), `<button class="mock-button">`, `<input class="mock-input">`, `placeholder` (prázdné místo).

**Text:** `h2` je nadpis obrazovky, `h3` nadpis části, `subtitle` vedlejší text pod nadpisem, `section` blok s mezerou pod sebou, `label` malý popisek velkými písmeny.

Když potřebuješ přesný vzhled tříd, přečti si soubor `scripts/frame-template.html`.

## Zásady

- **Jedna otázka na obrazovku**, napsaná nahoře celou větou: "Které rozložení působí přehledněji?", ne "Vyber".
- **Dvě až čtyři varianty**, výrazně odlišné. Uživatel vybírá směr, ne odstín.
- **Věrnost podle otázky.** Na rozložení stačí drátěný model, na vzhled skutečné barvy, písmo a obrázky.
- **Skutečný obsah, když na něm záleží.** U fotogalerie skutečné fotky, u ceníku skutečné ceny. Výplňový text schová, co je na návrhu špatně.
