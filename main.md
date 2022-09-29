---
title: Vysokoúrovňové jazyky pro \TeX
englishTitle: High-Level Languages for \TeX
author: Vít Novotný
podpis: Vít Novotný, witiko@mail.muni.cz
---

\TeX{} je strojový kód světa digitální sazby, který od spisovatelů a grafických
návrhářů vyžaduje netriviální programátorské dovednosti a který programátorům
poskytuje minimum vysokoúrovňových abstrakcí. V článku představuji vybrané
značkovací, programovací a stylové jazyky pro \TeX, které umožňují dělbu práce
mezi spisovatelů, vývojáře a grafické návrháře a které usnadňují proces
přípravy elektronických dokumentů. Článek je volný přepis mé přednášky na
valném shromáždění \CSTUG u 14. května 2022.~[@novotny2022vysokourovnove]

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
       syntaktickou analýzu textu, pro kterou \TeX{} nemá vhodné primitivní
       datové typy ani příkazy, nebo bychom museli interpunkci ve značkách
       použít způsobem, který usnadňuje analýzu na úkor čitelnosti. Proto se
       při větším množství značek typicky místo interpunkce používají upovídaná
       \TeX ová makra.

 /un-lion-de-paris.pdf "Un Lion de Paris (Grandville, 1840--1842)"

V sekci~<#tex> shrnuji pojmy související s \TeX em, jako jsou stroje,
makrobalíky a formáty. V sekcích <#programovaci-jazyky> až
<#domenove-specificke-jazyky> nabízím přehled programovacích, značkovacích,
stylových a dalších doménově specifických vysokoúrovňových jazyků pro \TeX.
V sekci~<#zaver> shrnuji poznatky z článku. V sekci~<#vyhled-do-budoucnosti> se
zamýšlím nad dalším směřováním \TeX u a vysokoúrovňových jazyků jako takových.

# \TeX{} jako strojový kód digitální sazby {#tex}

\TeX{} je nízkoúrovňový programovací jazyk pro digitální sazbu~[@knuth1984texbook].
Referenční implementací \TeX u je interpretr (tzv. *\TeX ový stroj*) \TeX 90 od
prof. Knutha~[@knuth1986texprogram]. Moderní \TeX ové stroje jako \hologo{eTeX}~%
[@breitenlohner1998etex], \hologo{pdfTeX}~[@thanh2022pdftex] a \hologo{LuaTeX}~%
[@luatex2022luatex] rozšiřují \TeX 90 o dodatečné primitivní příkazy, které
zvyšují vývojářský komfort.  *Makrobalíky* jako plain~[@knuth2021plain],
\LaTeX~[@lamport1994latex] a \hologo{ConTeXt}~[@hagen2001context] staví z
primitiv \TeX ových strojů vysokoúrovňové značkovací a programovací jazyky pro
spisovatele a vývojáře.

Na průsečíku strojů a makrobalíků vznikají *formáty* jako Lua\LaTeX{} a
\hologo{ConTeXt} MkIV. Formáty odpovídají kombinaci konkrétního formátu a
makrobalíku, např. $\text{\hologo{LuaTeX}} + \text{\LaTeX} = \text{Lua\LaTeX}$
a $\text{\hologo{LuaTeX}} + \text{\hologo{ConTeXt}} = \text{\hologo{ConTeXt}
MkIV}$. S formáty pracuje sazeč při překladu dokumentu z příkazové řádky pomocí
příkazů jako `lualatex` a `context`.

# Programovací jazyky pro vývojáře {#programovaci-jazyky}
# Značkovací jazyky pro spisovatele {#znackovaci-jazyky}
# Stylové jazyky pro grafické návrháře {#stylove-jazyky}
# Doménově specifické jazyky pro experty {#domenove-specificke-jazyky}
# Závěr {#zaver}

\TeX{} je strojový kód světa digitální sazby, který od spisovatelů a grafických
návrhářů vyžaduje netriviální programátorské dovednosti a který programátorům
poskytuje minimum vysokoúrovňových abstrakcí. V článku jsme si představili
značkovací, programovací a stylové jazyky pro \TeX, které umožňují dělbu práce
mezi spisovatele, vývojáře a grafické návrháře a které usnadňují proces
přípravy elektronických dokumentů. Představené jazyky netvoří úplný výčet.
Namísto toho jsem se v článku pokusil nastínit možnosti moderních \TeX ových
strojů a software mimo \TeX ové distribuce na příkladu několika jazyků.

# Výhled do budoucnosti {#vyhled-do-budoucnosti}

Veškeré jazyky, které jsme si představili v článku, jsou uměle navržené tak,
aby byly syntakticky jednoznačné a snadno strojově čitelné. Tím se zcela liší
od přirozených jazyků, které jsou víceznačné a jejich strojové zpracování bylo
svatým grálem informatiky od jejího vzniku. Umělé jazyky jsou překážkou pro
doménové experty, kteří ovládají svůj mateřský jazyk, ale nejsou vystudovaní
informatici. V posledních pěti letech došlo na poli umělé inteligence k
významným posunům~% [@devlin2018bert] [@brown2020language], které zvyšují
strojovou čitelnost přirozeného jazyka a umožňují automaticky generovat kód
programu na základě pokynů zadaných v přirozeném jazyce~[@papers2022code].
Budoucím vysokoúrovňovým značkovacím,[^5] programovacím i stylovým jazykem
proto může být přirozený jazyk.[^4]

 [^4]: Posuny v umělé inteligenci se týkají i dalších modalit, jako je obraz~%
       [@lu2019vilbert] To grafikovi umožní doplnit textový popis o nákresy;
       obrázek za tisíc slov. Tuto primární dokumentaci můžeme strojově
       přeložit na stylopis v umělém stylovém jazyce nižší úrovně, jako je
       **CSS**.

 [^5]: Dnešní jazykové modely dokážou v písemném projevu opravit překlepy~%
       [@zhou2019spelling] a doplnit chybějící interpunkci~[@zhu2021retrieving].
       O značkovacích jazycích můžeme uvažovat jako o rozšířené množině
       interpunkčních znamének, která nám umožňují text dále členit. Na první
       pohled by se tedy zdálo, že doplňování chybějících značek je typově
       stejný problém jako doplňování chybějící interpunkce a k jeho řešení je
       možné použít stejné techniky. Tuto hypotézu by bylo vhodné ověřit na
       kolekcích označkovaných textů.

* * *

\TeX{} is the assembly language of digital typesetting, which requires advanced
programming skills from authors and designers, and which provides few
high-level abstractions to programmers. In this article, I introduce selected
markup, programming, and style-sheet languages for \TeX, which enable the
division of labor between authors, programmers, and designers, and which
simplify the process of electronic document preparation. The article
transcribes my invited talk at the general assembly of \CSTUG{} on May 14,
2022.~[@novotny2022vysokourovnove]

*high-level languages, programming languages, markup languages, style-sheet
languages, \hologo{eTeX}, \hologo{pdfTeX}, \hologo{LuaTeX}, \LaTeXe, \LaTeX3,
expl3, **XML**, **XSL**, **CSS**, \hologo{ConTeXt}, **HTML**, markdown,
**YAML**, Ti$k$Z, Bib\LaTeX, Ly\hologo{LuaTeX}*

* * *
