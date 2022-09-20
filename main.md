---
title: Vysokoúrovňové jazyky pro \TeX
englishTitle: High-Level Languages for \TeX
author: Vít Novotný
podpis: Vít Novotný, witiko@mail.muni.cz
---

\TeX{} je assembler světa digitální sazby, který od spisovatelů a grafických
návrhářů vyžaduje netriviální programátorské dovednosti a který programátorům
poskytuje minimum vysokoúrovňových abstrakcí. V článku představím existující
značkovací, programovací a stylové jazyky pro \TeX, které umožňují dělbu práce
mezi spisovatelů, vývojáře a grafické návrháře a které usnadňují proces
přípravy elektronických dokumentů. Článek je volný přepis mé přednášky na
valném shromáždění \CSTUG u 14. května 2022. [@novotny2022vysokourovnove]

*vysokoúrovňové jazyky, programovací jazyky, značkovací jazyky, stylové jazyky,
\hologo{eTeX}, \hologo{pdfTeX}, \hologo{LuaTeX}, \LaTeXe, \LaTeX3, expl3,
**XML**, **XSL**, **CSS**, \hologo{ConTeXt}, **HTML**, markdown, **YAML**,
Ti$k$Z, Bib\LaTeX, Ly\hologo{LuaTeX}*

* * *

Kreativnímu jedinci je \TeX{} pozvánkou k tomu, aby se na chvíli stal
spisovatelem, grafikem, ilustrátorem, sazečem a vývojářem v jedné osobě.
Rozsáhlým dokumentům však prospívá, když obsah, stylopisy a programová výbava
vznikají do jisté míry nezávisle.[^1] Toto umožňuje dělbu práce mezi doménové
experty, aniž by neúnosně rostly náklady na vzájemnou koordinaci. Kreativní
jedinec, který dokáže zastoupit několik rolí, se může v případě potřeby omezit
jen na jednu z nich a věnovat jí svou plnou a ničím nerušenou pozornost.

 [^1]: Laskavý čtenář může namítnout, že rozdělení dokumentu na nezávislé části
       je fikce: Jsou obrázky v textu doménou spisovatele, nebo ilustrátora?
       Může spisovatel měnit písmo a barvu textu či způsob číslování seznamů,
       nebo jde o úkol grafika? Kde jsou hranice mezi rolemi grafika, sazeče
       a vývojáře?

       Tyto námitky jsou na místě především u akcidenčních a nízkonákladových
       dokumentů, jako jsou plakáty, básnické sbírky nebo jídelní a nápojové
       lístky. U takových dokumentů může být vhodnější zvolit celistvý přístup,
       kdy jsou role fluidní a mezi účastníky procesu přípravy dokumentu
       probíhá soustavná komunikace. Autor článku má zkušenosti převážně s
       přípravou odborných a technických dokumentů, u kterých jsou
       jednotvárnost a kompozicialita úmyslnými prvky stylu. Takové dokumenty
       lze tedy snadno rozdělit na dílčí části bez újmy na celku.

\TeX ový dokument můžeme rozdělit na obsah, stylopisy a programovou výbavu
jednoduše tak, že vytvoříme tři samostatné soubory

- text dokumentu označkovaný \TeX ovými makry,
- stylopis s nastavením délkových registrů, písem, výstupní rutiny, apod.,
- program s definicí značkovacích maker.

← Při následné dělbě práce ale můžeme narazit na to, že doménoví experti nejsou
schopni nebo ochotni používat při své práci \TeX, což má několik důvodů:

- Moderní značkovací jazyky jako markdown minimalizují poměr značek vůči textu
  pro snadný zápis a zvýšenou čitelnost, zatímco \TeX{} bývá při větším množství
  značek upovídaný a nepřehledný.[^2]
- Moderní stylovací jazyky jako **CSS** jsou deklarativní a nepožadují
  programátorské dovednosti, zatímco \TeX{} je imperativní programovací jazyk.
- Moderní programovací jazyky jako Python nabízí vysokoúrovňové abstrakce a
  bohaté knihovny vestavěných funkcí, zatímco \TeX{} nabízí pouze primitivní
  datové typy a operace nad nimi.

← Naštěstí existuje mnoho vysokoúrovňových jazyků pro \TeX, na které se výše
uvedená omezení nevztahují a které můžeme použít pro přípravu obsahu, stylopisů
a programové výbavy namísto nízkoúrovňového \TeX u.

 [^2]: Pro zlepšení čitelnosti textu označkovaného v \TeX u můžeme značky
       sestavit z interpunkčních znamének. Pokud se však pro usnadnění zápisu
       omezíme na znakovou sadu **ASCII**, skončíme s pouhými 32 znaky, z nichž
       10 už je využíváno \TeX em. Při větším množství značek by proto
       jednotlivá interpunkční znaménka odpovídala několika různým značkám.
       Vyřešení nejednoznačností by vyžadovalo buďto složitou lexikální a
       syntaktickou analýzu textu, pro kterou \TeX{} nemá vhodná primitiva ani
       datové typy, nebo bychom museli interpunkci ve značkách použít způsobem,
       který usnadňuje analýzu na úkor čitelnosti. Proto se při větším množství
       značek typicky místo interpunkce používají upovídaná \TeX ová makra.

 /un-lion-de-paris.pdf "Un Lion de Paris (Grandville, 1840--1842)"

V sekci~<#tex> shrnuji pojmy související s \TeX em, jako jsou stroje,
makrobalíky a formáty. V sekcích <#programovaci-jazyky> až
<#domenove-specificke-jazyky> nabízím přehled programovacích, značkovacích,
stylových a dalších doménově specifických vysokoúrovňových jazyků pro \TeX.
V sekci~<#zaver> shrnuji poznatky z článku a zamýšlím se nad dalším směřováním
\TeX u a vysokoúrovňových jazyků jako takových.

# \TeX{} jako strojový jazyk digitální sazby {#tex}
# Programovací jazyky pro vývojáře {#programovaci-jazyky}
# Značkovací jazyky pro spisovatele {#znackovaci-jazyky}
# Stylové jazyky pro grafické návrháře {#stylove-jazyky}
# Doménově specifické jazyky pro experty {#domenove-specificke-jazyky}
# Závěr {#zaver}

<!-- Plutarch: Education is not the filling of a pail, but the lighting of a fire.
     Tento dokument je na GitHubu. Pěkný zdrojový text znamená, že nepotřebuji
     WYSIWYG a ve skutečnosti mě rozptyluje. -->

* * *

\TeX{} is the assembly language of digital typesetting, which requires advanced
programming skills from authors and designers, and which provides few
high-level abstractions to programmers. In this article, I introduce existing
markup, programming, and style-sheet languages for \TeX, which enable the
division of labor between authors, programmers, and designers, and which
simplify the process of electronic document preparation. The article
transcribes my invited talk at the general assembly of \CSTUG{} on May 14,
2022. [@novotny2022vysokourovnove]

*high-level languages, programming languages, markup languages, style-sheet
languages, \hologo{eTeX}, \hologo{pdfTeX}, \hologo{LuaTeX}, \LaTeXe, \LaTeX3,
expl3, **XML**, **XSL**, **CSS**, \hologo{ConTeXt}, **HTML**, markdown,
**YAML**, Ti$k$Z, Bib\LaTeX, Ly\hologo{LuaTeX}*

* * *
