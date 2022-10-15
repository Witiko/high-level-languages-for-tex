---
title: Vysokoúrovňové jazyky pro \TeX
englishTitle: High-Level Languages for \TeX
author: Vít Novotný
podpis: Vít Novotný, witiko@mail.muni.cz
---

\TeX{} je strojový kód světa digitální sazby, který od spisovatelů a grafiků
vyžaduje netriviální programátorské dovednosti a programátorům
poskytuje minimum vysokoúrovňových abstrakcí. V článku představuji vybrané
značkovací, programovací a stylové jazyky pro \TeX, které umožňují dělbu práce
mezi spisovatele, vývojáře a grafiky a usnadňují proces
přípravy elektronických dokumentů. Článek je přepis mé přednášky na
valném shromáždění \CSTUG u 14. května 2022~[@novotny2022vysokourovnove].

*vysokoúrovňové jazyky, programovací jazyky, značkovací jazyky, stylové jazyky,
\hologo{eTeX}, \hologo{pdfTeX}, \hologo{LuaTeX}, LuaMeta\TeX, \LaTeXe, \LaTeX3,
Python\TeX, expl3, **XML**, DocBook, **TEI**, **XHTML**, **XSLT**, **CSS**, **CSL**,
\hologo{ConTeXt}, **HTML**, markdown, **YAML**, Pandoc, Ti$k$Z, Bib\LaTeX,
Bib\LaTeX ML, Ly\hologo{LuaTeX}*

* * *

Kreativnímu jedinci je \TeX{} pozvánkou k tomu, aby se na chvíli stal
spisovatelem, grafikem, ilustrátorem, sazečem a vývojářem v jedné osobě.
Rozsáhlým dokumentům však prospívá, když obsah, stylopisy a programová výbava
vznikají do jisté míry nezávisle.[^1] To umožňuje dělbu práce mezi doménové
experty, aniž by neúnosně rostly náklady na vzájemnou koordinaci. Kreativec,
který dokáže zastoupit několik rolí, se může v případě potřeby omezit
jen na jednu z nich a věnovat jí svou plnou a ničím nerušenou pozornost.

 [^1]: Čtenář může namítnout, že rozdělení dokumentu na nezávislé části
       je fikce: Jsou obrázky v textu doménou spisovatele nebo ilustrátora?
       Může spisovatel měnit písmo a barvu textu či způsob číslování seznamů
       nebo jde o úkol grafika? Kde jsou hranice mezi rolemi grafika,
       sazeče a vývojáře?

       Tyto námitky jsou na místě především u akcidenčních a nízkonákladových
       dokumentů, jako jsou plakáty, básnické sbírky nebo jídelní a nápojové
       lístky. U takových dokumentů může být vhodnější zvolit takový přístup,
       při kterém jsou role volnější a mezi účastníky procesu přípravy dokumentu
       probíhá soustavná komunikace. Autor článku má zkušenosti převážně s
       přípravou odborných a technických dokumentů, u kterých jsou
       jednotvárnost a kompozicialita úmyslnými prvky stylu. Takové dokumenty
       lze snadno rozdělit na dílčí části bez újmy na celku.

\TeX ový dokument můžeme rozdělit na obsah, stylopisy a programovou výbavu
jednoduše tak, že vytvoříme tři samostatné soubory:

- text dokumentu označkovaný \TeX ovými makry
- stylopis s nastavením délkových registrů, písem, výstupní rutiny, apod.
- program s definicí značkovacích maker

← Při dělbě práce ale můžeme narazit na to, že doménoví experti nejsou
schopni nebo ochotni používat při své práci \TeX, což má několik důvodů:

- Moderní značkovací jazyky jako markdown minimalizují poměr značek vůči textu
  pro snadný zápis a zvýšenou čitelnost, zatímco \TeX{} bývá při větším množství
  značek upovídaný a nepřehledný.[^2]
- Moderní stylovací jazyky jako **CSS** jsou deklarativní a nepožadují
  programátorské dovednosti, zatímco \TeX{} je imperativní programovací jazyk.
- Moderní programovací jazyky jako Python nabízí vysokoúrovňové abstrakce a
  bohaté základní knihovny vestavěných funkcí, zatímco \TeX{} nabízí pouze
  primitivní datové typy a operace nad nimi.

← Nicméně existuje mnoho vysokoúrovňových jazyků pro \TeX, na které se výše
uvedená omezení nevztahují a které můžeme použít pro přípravu obsahu, stylopisů
a programové výbavy namísto nízkoúrovňového \TeX u.

 [^2]: Čitelnost textu označkovaného v \TeX u se zlepší, pokud značky
       sestavíme z interpunkčních znamének. Pokud se však pro usnadnění zápisu
       omezíme na znakovou sadu **ASCII**, skončíme s pouhými 32 znaky, z nichž
       10 už je využíváno \TeX em. Při větším množství značek by proto
       jednotlivá interpunkční znaménka odpovídala několika různým značkám.
       Vyřešení nejednoznačností by vyžadovalo buďto složitou lexikální a
       syntaktickou analýzu textu, pro kterou \TeX{} nemá vhodné primitivní
       datové typy ani příkazy, nebo bychom museli interpunkci ve značkách
       použít způsobem, který usnadňuje analýzu na úkor čitelnosti. Proto se
       při větším množství značek typicky místo interpunkce používají upovídaná
       \TeX ová makra.

 ![un-lion-de-paris](un-lion-de-paris "Un Lion de Paris (Grandville, 1840--1842)")

V sekci~<#tex> shrnuji základní pojmy související s \TeX em, jako jsou stroje,
makrobalíky a formáty. V sekcích <#programovacijazyky> až
<#domenovespecifickejazyky> nabízím přehled programovacích, značkovacích,
stylových a dalších doménově specifických vysokoúrovňových jazyků pro \TeX.
V sekci~<#zaver> shrnuji poznatky z tohoto článku. V sekci~<#vyhleddobudoucnosti> se
zamýšlím nad dalším směřováním \TeX u a vysokoúrovňových jazyků jako takových.

↑

# Přehled základních pojmů {#tex}

↑

\TeX{} je nízkoúrovňový programovací jazyk pro digitální sazbu~[@knuth1984texbook].
Referenční implementací \TeX u je *stroj* \TeX 90 od
Donalda Knutha~[@knuth1986texprogram]. Moderní \TeX ové stroje jako \hologo{eTeX}~%
[@breitenlohner1998etex], \hologo{pdfTeX}~[@thanh2022pdftex] a \hologo{LuaTeX}~%
[@luatex2022luatex] rozšiřují \TeX 90 o dodatečné primitivní příkazy, které
zvyšují vývojářský komfort.  *Makrobalíky* jako plain~[@knuth2021plain],
\LaTeX~[@lamport1994latex] a \hologo{ConTeXt}~[@hagen2001context] staví z
primitiv \TeX ových strojů vysokoúrovňové značkovací a programovací jazyky pro
spisovatele a vývojáře.

*Formáty* odpovídají kombinaci stroje a makrobalíku, např.
$\text{\hologo{LuaTeX}} + \text{\LaTeX} = \text{Lua\LaTeX}$[^14] a
$\text{\hologo{LuaTeX}} + \text{\hologo{ConTeXt}} = \text{\hologo{ConTeXt}
MkIV}$. S formáty pracuje sazeč při přípravě dokumentu z příkazové řádky
pomocí příkazů jako `lualatex` a `context`.

 [^14]: Pokud neni stroj významný, hovoříme o formátech \*\LaTeX{} souhrnně
        jako o formátu \LaTeX.

↑

# Programovací jazyky pro vývojáře {#programovacijazyky}

↑

V této sekci popisuji, jaké možnosti nabízí moderní \TeX ové stroje a
makrobalíky vývojářům. Veškeré ukázky v této sekci vypíší text $1 + 2 = 3$.

Stroj \hologo{eTeX} a jeho následovníci jako \hologo{pdfTeX} a \hologo{LuaTeX}
nabízí primitivní příkaz `\numexpr`, který vyhodnocuje celočíselné aritmetické
výrazy:
``` tex
$ 1 + 2 = \numexpr 1 + 2 \relax $
```
← Příkaz `\numexpr` zvyšuje komfort oproti primitivním příkazům \TeX u 90:
``` tex
$ 1 + 2 = \newcount\x \x=1 \advance\x by 2 \the\x $
```
← Pro další primitivní typy má \hologo{eTeX} příkazy `\dimexpr`,
`\glueexpr` a `\muglueexpr`.

Stroje \hologo{LuaTeX} a LuaMeta\TeX~[@luatex2022luametatex] nabízí primitivní
příkaz `\directlua`, který umožňuje zadávat a spouštět programy v jazyce Lua:
``` tex
$ 1 + 2 = \directlua { tex.print(1 + 2) } $
```
← Kromě základní knihovny jazyka Lua mohou vývojáři interagovat s \TeX ovým
strojem a instalací \TeX u [@luatex2022luatex, kapitoly 5--10;
@luatex2022luametatex, kapitoly 4--10] a využívat rozšiřující softwarové
knihovny pro práci se soubory a zpracování textu
[@luatex2022luatex, sekce 4.3].

Stroje \hologo{pdfTeX} a \hologo{LuaTeX} rozšiřují primitivní příkaz `\input` o
variantu, která spouští libovolné programy pomocí příkazové řádky operačního
systému:
```tex
$ 1 + 2 = \input|" echo 1 + 2 | bc "\relax $
```
← Rozšířená varianta příkazu `\input` umožňuje integrovat \TeX ový kód
s širším ekosystémem programové výbavy mimo instalaci \TeX u.[^6]

 [^6]: Jednou z nevýhod rozšířené varianty příkazu `\input` je vazba na
       konkrétní příkazovou řádku a programovou výbavu, což snižuje
       přenositelnost dokumentů. V našem příkladu se jedná o příkazovou řádku
       Bourne shell z **UNIX**u~V7 (také `sh`), případně o zpětně kompatibilní
       nadstavby jako `bash`, `dash` a `ksh`, a o unixovou kalkulačku Bench
       calculator (`bc`).  Většina Linuxových distribucí ale používá příkazovou
       řádku kompatibilní s `sh` a zahrnuje `bc` v základní programové výbavě;
       náš příklad na nich tedy můžete bez úpravy vysázet.

       Další nevýhodou rozšířené varianty příkazu `\input` je skutečnost, že
       představuje bezpečnostní riziko a uživatel ji proto musí explicitně
       povolit parametrem `-shell-escape`. Například pro vysázení našeho
       příkladu musíme použít příkaz `pdftex -shell-escape` \meta{jméno
       dokumentu}.

Součástí experimentálního formátu \LaTeX3 je makrobalík `expl3-generic`, který
poskytuje vysokoúrovňový programovací jazyk expl3 [@latex2022style]
[@latex2022expl3] [@latex2022interfaces]:
``` tex
\input expl3-generic\relax
\ExplSyntaxOn
$ 1 + 2 = \int_eval:n { 1 + 2 } $
\ExplSyntaxOff
```
← Expl3 nabízí vysokoúrovňové datové typy pro obecné programování
(seznamy a hašové tabulky) a typografické programování (rakve[^7] a barvy) a
bohatou základní knihovnu funkcí pro řízení toku programu a \TeX ové expanze,
celočíselnou i reálnou aritmetiku a zpracování \TeX ových tokenů a unicodového
textu. Jazyk expl3 využívá primitivní příkazy strojů \TeX 90, \hologo{eTeX} a
pdf\TeX; funguje ale i s novějšími stroji, jako jsou \hologo{XeTeX},
\hologo{LuaTeX} a LuaMeta\TeX{} [@latex2022expl3, sekce 9].

 [^7]: Rakev (z anglického *coffin*) sestává z \TeX ového boxu, informací o
       jeho tvaru a rozměrech a množiny vertikálních a horizontálních přímek.
       Průsečíky přímek tvoří úchyty rakve, které slouží k vzájemnému
       pozicování rakví na stránce.

Vývojáře zvyklé na kategorie znaků v konvenčních formátech jako plain \TeX,
\LaTeX{} a \hologo{ConTeXt} mohou u explu překvapit podtržítka a dvojtečky s
kategorií písmene a bílé znaky (␣, ↵) s kategorií ignorovaného znaku.
Studie však ukazují, že oddělování slov v identifikátorech pomocí\\\_podtržítek
je čitelnější než oddělování slov pomocíKapitálek [@sharif2010eye], mezery jsou
častým zdrojem chyb při programování v \TeX u a dvojtečky v explu slouží pro
oddělení názvu příkazu (`\int_eval`) od jeho typové signatury (`:n`).[^8]
Kategorie znaků v explu jsou tedy účelné.

 [^8]: Typové signatury umožňují definovat příkazy s jiným typem argumentu,
       než s jakým příkazy voláme. Můžeme např. zadefinovat příkaz
       `\pozdrav:n`, který bude přijímat jeden argument s \TeX ovými tokeny:
       ``` tex
       \cs_new:Nn \pozdrav:n { Ahoj,~#1! }
       ```
       ← Při volání příkazu pro nás ale může být snazší použít jako argument
       např. jméno proměnné:
       ``` tex
       %~-
       \cs_generate_variant:Nn \pozdrav:n { v }
       %~+
       \tl_new:N   \g_jmeno_tl
       \tl_set:Nn  \g_jmeno_tl { světe }
       %~-
       \pozdrav:v { g_jmeno_tl }  \% Expanduje na \pozdrav:n{světe} a poté na Ahoj, světe!
       %~+
       ```
       ← Díky typovým signaturám může expl3 automaticky přetypovat argumenty,
       což zvyšuje komfort.

↑

# Značkovací jazyky pro spisovatele {#znackovacijazyky}

↑

V této sekci se podíváme na některé existující značkovací jazyky a možnosti
jejich využití v \TeX u.

> Je překvapivě obtížné přesvědčit o tom uživatele, ale nedostatky \LaTeX u pro
> koncentraci, psaní a myšlení si v ničem nezadají s nedostatky Wordu. Je to
> z toho prostého důvodu, že \LaTeX{} dává spisovateli do rukou příliš velkou moc:
> Vždy existuje další makrobalík, který můžeme zavést v preambuli, stejně jako
> vždy existuje další rozbalovací nabídka ve Wordu.
> ---~@thompson2010pandoc [, v překladu autora]

Formát \LaTeXe{} poskytuje jednoduchý značkovací jazyk pro přípravu dokumentů,
který je možné rozšiřovat pomocí makrobalíků:[^14]

 [^14]: Většina \TeX ových formátů poskytuje vlastní značkovací jazyky. Formát
        \LaTeXe{} je však zdaleka nejznámější a také nejdůslednější v odstínění
        spisovatele od programování a stylování.

``` latex
\documentclass{book}
\usepackage[czech]{babel}
\title{Ukázkový dokument v~\LaTeX u}
\author{Vít Novotný}
\date{14. května 2022}
\begin{document}
\maketitle
\chapter{Kapitola}
Ahoj, \LaTeX u!
\end{document}
```

← Pokud nejsme spokojeni s automatickým výstupem vysokoúrovňových značek jako
`\chapter`, můžeme místo nich použít stylovací příkazy \LaTeX u jako `\textbf`
a primitivní příkazy \TeX ového stroje jako `\vskip`. Toto může být vhodné pro
řešení konkrétních typografických nedostatků, které nelze řešit systémově.
Nadměrné užívání nízkoúrovňových příkazů ale narušuje dělbu práce a vede k
nejednotnému vzhledu koncového dokumentu.

Mimo svět \TeX u jsou oblíbené značkovací jazyky založené na metajazyku
**XML**. Můžeme si navrhnout buďto svůj vlastní **XML** jazyk~%
[@wagner2017kombinace] nebo využít existující jazyk jako DocBook, **TEI**,
nebo **XHTML** (vizte níže):

``` xml
<?xml version="1.0" encoding="utf-8"?>
<html xml:lang="cs" xmlns="http://www.w3.org/1999/xhtml">
    <head>
        %~-
        <title>Ukázkový dokument v XHTML</title>
        %~+
        <meta name="author" content="Vít Novotný" />
    </head>
    <body>
        <h1>Kapitola</h1>
        <p>Ahoj, XHTML!</p>
    </body>
</html>
```

← Pro zpracování tohoto dokumentu \TeX em požádáme o pomoc vývojáře.  Ten buďto
využije vestavěnou podporu **XML** v pokročilých \TeX ových formátech jako
\hologo{ConTeXt}~[@contextgarden2022xml; @maier2019typesetting] nebo připraví
program v jazyce **XSLT**:

``` xml
<?xml version="1.0" encoding="utf-8"?>
<stylesheet xmlns="http://www.w3.org/1999/XSL/Transform"
            xmlns:xhtml="http://www.w3.org/1999/xhtml">
            version="1.0">
    <output method="text" />

    <!-- Následující část zpracovává prvek <head> -->
    <template match="xhtml:head">
        \documentclass{book}
        \usepackage[czech]{babel}
        <!-- Zpracuj zanořené prvky <title> a <meta> -->
        <apply-templates select="*"/>
    </template>

    <template match="xhtml:title">
        \title{<value-of select="text()" />}
    </template>

    <template match="xhtml:meta">
        <choose>
            <when test="@name = 'author'">
                \author{<value-of select="@content" />}
            </when>
        </choose>
    </template>

    <!-- Následující část zpracovává prvek <body> -->
    <template match="/xhtml:html/xhtml:body">
        \begin{document}
        \maketitle
        <!-- Zpracuj zanořené prvky <h1> a <p> -->
        <apply-templates select="*"/>
        \end{document}
    </template>

    <template match="xhtml:h1">
        \chapter{<value-of select="text()" />}
    </template>

    <template match="xhtml:p">
        <value-of select="text()" /> \par
    </template>
</stylesheet>
```

← Tento program převede náš **XML** dokument na následující \LaTeX ový
dokument: %, který můžeme přímo vysázet:

``` tex
\documentclass{book}
\usepackage[czech]{babel}
\title{Ukázkový dokument v XHTML}
\author{Vít Novotný}
\begin{document}
\maketitle
\chapter{Kapitola}
Ahoj, XHTML! \par
\end{document}
```

**XML** jazyky mají nadstandardní podporu v softwarových knihovnách, což
usnadňuje další zpracování **XML** dokumentů. Na rozdíl od \LaTeX u nemůže spisovatel v
**XML** jazycích snadno řešit konkrétní typografické nedostatky, což ztěžuje
přípravu akcidenčních a nízkonákladových dokumentů.  Vzhledem k vysokému poměru
značek vůči textu je pro spisovatele výhodné použít specializovaný textový
editor pro snadný zápis.

> V markdownu je spisovatel vždy konfrontován pouze s jednou otázkou, a je to
> ta správná otázka: Jak by měla znít následující věta?
> ---~@thompson2010pandoc [, v překladu autora]

Odlehčené značkovací jazyky jako **YAML**oň[^9]~[@benkiki2021yaml]
a markdown~[@gruber2004markdown] minimalizují poměr značek vůči textu pro
snadný zápis a zvýšenou čitelnost:

 [^9]: **YAML**oň podle vzoru jabloň

``` md
---
%~-
title:  Ukázkový dokument v YAMLoni a markdownu
%~+
author: Vít Novotný
date:   2022-05-14
lang:   cs
---

# Kapitola
%~-
Ahoj, YAMLoni a *markdowne*!
%~+
```

← Pro zpracování tohoto dokumentu \TeX em můžeme použít např. konverzní nástroj
Pandoc~[@macfarlane2022pandoc], makrobalík
`markdown`~[@novotny2022markdowna] nebo oba zároveň~[@drehak2021priama;
@novotny2022markdownb, sekce 2.3].

Markdown je jednoduchý jazyk, kterému chybí značky pro složitější a méně časté
prvky jako tabulky, poznámky a citace. Pandoc i `markdown` proto nabízí
rozšiřující značky[^12] a umožňují uživatelům vytvářet značky
vlastní~[@macfarlane2022pandoc, sekce Filters] [@novotny2022markdowna, sekce
2.1.2] nebo připojovat k prvkům doplňující *atributy*:[^13]

``` md
%~-
Ahoj, rozšiřující značky[^1] a [atributy]{.vysazej-mne-tucne}.

 [^1]: Ahoj, já jsem rozšiřující značka pro poznámky.
%~+
```

← Jazyk markdown původně vznikl jako preprocesor jazyka **HTML** a spisovatel
v něm proto může využívat také **HTML** značky:[^10]

 [^10]: V `markdown`u je třeba zápis **HTML** značek povolit volbou `html`.

 [^12]: V Pandocu a `markdown`u je třeba zápis tabulek, poznámek a citací
        povolit volbami `pipe_tables`, `footnotes` a `citations`.

 [^13]: V Pandocu a `markdown`u je třeba zápis atributů povolit volbami
        `header_attributes`, `fenced_code_attributes`, `inline_code_attributes`
        a `link_attributes`.

``` html
Ahoj také <abbr title="hatmatilko">HTML</abbr>.
```

← Pro sazbu matematiky, další rozšíření repertoáru značek a řešení konkrétních
typografických nedostatků může spisovatel v markdownu použít i primitivní
příkazy \TeX ového stroje a příkazy \TeX ových formátů:[^11]

 [^11]: V Pandocu je třeba zápis matematiky a \TeX ových značek povolit volbami
        `tex_math_dollars` a `raw_attribute`. V `markdown`u je třeba povolit
        volbu `hybrid`.

``` tex
%~-
Ahoj i vám, `\LaTeX u`{=latex} a $\text{matematiko}$.
%~+
```

← Nadužívání rozšiřujících značek a atributů snižuje čitelnost vstupních
dokumentů, ztěžuje jejich další zpracování, narušuje dělbu práce a vede k
nejednotnému vzhledu koncových dokumentů. Užívejte je proto s mírou.

Různé značkovací jazyky mají různé výhody a může být vhodné je kombinovat.
Markdown a **YAML**oň mohou sloužit jako vstupní jazyky pro spisovatele. Pomocí
Pandocu můžeme vstupní dokumenty převést do **XML** mezijazyka pro archivaci a
další zpracování. Z **XML** mezijazyka získáme dokument v \LaTeX u nebo jiném
\TeX ovém formátu, který může sloužit jako koncový jazyk pro sazeče.

# Stylové jazyky pro grafiky {#stylovejazyky}

V \TeX u nelze využívat vysokoúrovňové deklarativní stylové jazyky jako **CSS**
a grafici jsou proto odkázáni na programování v \TeX u. V této sekci stručně
referuji o tom, jaké možnosti usnadnění nabízí grafikům formáty \LaTeXe{} a
\LaTeX3.

Formát \LaTeXe{} poskytuje vysokoúrovňové příkazy pro vytváření pojmenovaných
stylů stránek (`\newpagestyle`), změny obsahu záhlaví a zápatí (`\markboth`) a
nastavení rodiny, řezu a velikost písma nezávisle na sobě a nezávisle na
konkrétním písmu (`\textbf`) [@latex2021fntguide]. \LaTeXe{} dále poskytuje
délkové registry pro nastavování rozměrů sazebního zrcadla (`\textheight`) a
vzhledu některých \LaTeX ových prvků jako seznamy (`\itemsep`).  Rozšiřující
makrobalíky \LaTeX u jako `enumitem`, `geometry` a `fancyhdr` umožňují
grafikovi měnit další aspekty vzhledu koncového dokumentu bez programování.

Součástí formátu \LaTeX3 je makrobalík `xtemplate` [@latex2022xtemplate]
[@niederberger2012xtemplate], který sazeči, grafikovi a vývojáři pomáhá
společně připravovat stylopisy. Nejprve sazeč definuje *typy* prvků dokumentu,
např. sekce:

``` latex
\usepackage{xtemplate}
\ExplSyntaxOn
\DeclareObjectType { sekce } { 1 }  \% Sekce mají 1 argument: název
```

Grafik pro každý typ vytvoří *šablonu*. Šablona zadává vysokoúrovňové
grafické parametry, kterými se od sebe liší různé úrovně sekcí:

``` latex
\DeclareTemplateInterface { sekce } { moje-šablona } { 1 }
  {
    mezera-nad : skip,
    mezera-pod : skip,
    písmo : tokenlist,
    zarovnání : choice { doleva, doprostřed, doprava } = doleva,
  }
```

Vývojář implementuje parametry šablony pomocí vysokoúrovňového jazyka expl3,
stylovacích příkazů \LaTeX u a primitivních příkazů \TeX ového stroje:

``` latex
\skip_new:N \l_mezera_nad_skip
\skip_new:N \l_mezera_pod_skip
\tl_new:N \l_pismo_tl
\tl_new:N \l_zarovnani_tl
\DeclareTemplateCode { sekce } { moje-šablona } { 1 }
  { \% Hodnoty parametrů ukládáme do lokálních proměnných
    mezera-nad = \l_mezera_nad_skip ,
    mezera-pod = \l_mezera_pod_skip ,
    písmo = \l_pismo_tl ,
    zarovnání = {
      doleva = \cs_set_eq:NN \l_zarovnani_tl \raggedright ,
      doprava = \cs_set_eq:NN \l_zarovnani_tl \raggedleft ,
      doprostřed = \cs_set_eq:NN \l_zarovnani_tl \centering ,
    },
  }
  { \% Při spuštění šablony vysázíme nadpis sekce
    \AssignTemplateKeys
    \par \skip_vertical:N \l_mezera_nad_skip
    \group_begin:
      \l_zarovnani_tl
      \l_pismo_tl
      #1
      \par \skip_vertical:N \l_mezera_pod_skip
    \group_end:
  }
```

Grafik navrhne *instance* šablony s konkrétními hodnotami parametrů pro různé
úrovně sekcí jako kapitoly:

``` latex
\DeclareInstance { sekce } { kapitola } { moje-šablona }
  {
    mezera-nad = 10pt,
    mezera-pod = 12pt,
    písmo = { \Large \bfseries } ,
  }
\ExplSyntaxOff
```

← Když pak spisovatel v dokumentu zadá nadpis sekce, spustí se
příslušná instance:

``` latex
\UseInstance{sekce}{kapitola}{Název mojí kapitoly}
```

Při použití balíku `xtemplate` může být grafik součástí procesu přípravy
dokumentu a průběžně upravovat instance, aniž by musel programovat.

# Doménově specifické jazyky pro experty {#domenovespecifickejazyky}

Kromě programovacích, značkovacích a stylových jazyků existuje mnoho dalších
vysokoúrovňových jazyků pro doménové experty, jako jsou knihovníci, ilustrátoři a
hudebníci. V této sekci uvádím přehledový výčet několika takových jazyků.

Makrobalík Ti$k$Z~[@tantau2021pgf] poskytuje jazyk pro přípravu ilustrací:

``` tex
\usemodule[tikzducks]
\usecolors[xwi]
\starttext
\starttikzpicture
\duck[tophat, bowtie]
\duck[scale=0.3, xshift=60pt,
                 yshift=40pt]
\stoptikzpicture
\stoptext
```

 /figures/tikz-ducks.tex

← Jazyk Ti$k$Zu je nezávislý na použitém \TeX ovém formátu (zde ConTeXt MkIV) a
lze ho rozšiřovat pomocí makrobalíků (zde `tikzducks`~[@carter2020tikz]).

Makrobalík Bib\LaTeX~[@lehnman2022biblatex] poskytuje jazyk pro přípravu
bibliografie:

``` bib
@online{lehnman2022biblatex,
    title = {The Bib\LaTeX{} Package},
    subtitle = {Programmable Bibliographies and Citations},
    author = {Philip Kime and Moritz Wemheuer and Philipp Lehman},
    date = {2022-07-12},
    url = {https://ctan.org/pkg/biblatex},
    urldate = {2022-10-06},
}
```

← Jazyk Bib\LaTeX u je přesně definovaný~[@lehnman2022biblatex, sekce 2] a
bibliografické záznamy lze validovat a převést do **XML** jazyka Bib\LaTeX ML
pro další zpracování [@novotny2018priprava, sekce 3.4]. Bib\LaTeX{} lze
rozšiřovat pomocí makrobalíků s citačními styly [@luptak2016sadzba]. Chybí
podpora stylopisů ve standardním deklarativním stylovém jazyce **CSL**.

Makrobalík LyLua\TeX~[@peron2019lyluatex] poskytuje jazyk pro notový zápis:

``` tex
\score {
    \relative c' {
        \time 4/4
        \clef treble
        %~-
        c4 d8 e f8 g a b | c4 b8 a g8 f e d |
        c8 g' e g c,8 g' e g | c,4 e c r \bar "|."
    %~+
    }
}
```

![lilypond](lilypond)

← LyLua\TeX{} potřebuje \TeX ový formát Lua\LaTeX{} a instalovaný program LilyPond.

# Závěr {#zaver}

\TeX{} je strojový kód světa digitální sazby, který od spisovatelů a grafických
návrhářů vyžaduje netriviální programátorské dovednosti a programátorům
poskytuje minimum vysokoúrovňových abstrakcí. V článku jsme si představili
značkovací, programovací a stylové jazyky pro \TeX, které umožňují dělbu práce
mezi vývojáře, spisovatele a grafické návrháře a které usnadňují proces
přípravy elektronických dokumentů.

# Výhled do budoucnosti {#vyhleddobudoucnosti}

Chybějící podpora vysokoúrovňových deklarativních stylových jazyků jako **CSS**
a **CSL** představuje překážku pro grafiky, kteří tak musí při práci s \TeX em
programovat. Makrobalíky jako `xtemplate` jsou první vlaštovky důsledného
oddělení typografického programování od grafického designu. V budoucnu má být
součástí formátu \LaTeX3 systém **LDB**~[@mittelbach2011latex3]
[@mittelbach2013using], který grafikům umožní postihnout hraniční případy jako
seznam bezprostředně po nadpisu (v notaci **LDB** `!head<list`), zanořené
seznamy (`<list*<list`) a druhý popisek obrázku (`<float*<caption>*<caption`).

Veškeré jazyky, které jsme si představili v článku, jsou uměle navržené tak,
aby byly syntakticky jednoznačné a snadno strojově čitelné. Tím se zcela liší
od přirozeného jazyka, který je víceznačný a jeho strojové zpracování bylo
svatým grálem informatiky od jejího vzniku. Umělé jazyky jsou překážkou pro
doménové experty, kteří ovládají svůj mateřský jazyk, ale nejsou vystudovaní
informatici. V posledních pěti letech došlo na poli umělé inteligence k
významným posunům~[@devlin2018bert] [@brown2020language], které zvýšily
strojovou čitelnost přirozeného jazyka a umožňují automaticky generovat kód
programu na základě pokynů zadaných v přirozeném jazyce~[@papers2022code].
Budoucím vysokoúrovňovým značkovacím,[^5] programovacím i stylovým jazykem
proto může být přirozený jazyk.[^4]

 [^4]: Posuny v umělé inteligenci se týkají i dalších modalit, jako je obraz~%
       [@lu2019vilbert]. To grafikovi umožní doplnit textový popis o nákresy,
       neboť obrázek vydá za tisíc slov. Tuto primární dokumentaci můžeme
       kdykoliv strojově přeložit na stylopis v umělém jazyce nižší úrovně,
       jako je **CSS**.

 [^5]: Dnešní jazykové modely dokážou v písemném projevu opravit překlepy~%
       [@zhou2019spelling] a doplnit chybějící interpunkci~[@zhu2021retrieving].
       O značkovacích jazycích můžeme uvažovat jako o rozšířené množině
       interpunkčních znamének, která nám umožňují text dále členit. Na první
       pohled by se tedy zdálo, že doplňování chybějících značek je typově
       stejný problém jako doplňování chybějící interpunkce a k jeho řešení je
       možné použít stejné techniky. Tuto hypotézu by bylo vhodné ověřit na
       kolekcích označkovaných textů, jako jsou webové stránky označkované v
       jazyce **HTML**.

* * *

\TeX{} is the assembly language of digital typesetting, which requires advanced
programming skills from authors and designers, and which provides few
high-level abstractions to programmers. In this article, I introduce selected
markup, programming, and style-sheet languages for \TeX, which enable the
division of labor between authors, programmers, and designers, and which
simplify the process of electronic document preparation. The article
transcribes my invited talk at the general assembly of \CSTUG{} on May 14,
2022~[@novotny2022vysokourovnove].

*high-level languages, programming languages, markup languages, style-sheet
languages, \hologo{eTeX}, \hologo{pdfTeX}, \hologo{LuaTeX}, LuaMeta\TeX,
\LaTeXe, \LaTeX3, Python\TeX, expl3, **XML**, DocBook, **TEI**, **XHTML**,
**XSLT**, **CSS**, **CSL**, \hologo{ConTeXt}, **HTML**, markdown, **YAML**, Pandoc,
Ti$k$Z, Bib\LaTeX, Bib\LaTeX ML, Ly\hologo{LuaTeX}*

* * *
