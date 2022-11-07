# Vysokoúrovňové jazyky pro TeX

Tento repozitář obsahuje zdrojový kód článku *Vysokoúrovňové jazyky pro TeX* ze
Zpravodaje CSTUGu 2022/1–4.

Článek vysázíte následujícími příkazy:

``` bash
vlna -r -l -v KkSsVvZzOoUuAaIi main.md
latexmk -lualatex -shell-escape -cd figures/lilypond-score.tex  # vyžaduje lilypond
latexmk -pdf -shell-escape main.tex
```

Článek je připravený ve vysokoúrovňových jazycích Markdown a BibLaTeX a sestává
z následujících souborů:

- `main.tex` – TeXový dokument, který při překladu vysází PDF dokument
  [`main.pdf`][1] s článkem
- `main.md` – Zdrojový kód článku ve vysokoúrovňovém značkovacím jazyce Markdown
  pro spisovatele
- `markdownthemewitiko_csbulletin.sty` – Téma `witiko/csbulletin` pro TeXový
  balíček Markdown, které udává vzhled jednotlivých značek jazyka Markdown
- `main.bib` – Seznam bibliografických zdrojů článku ve vysokoúrovňovém jazyce
  BibLaTeX pro knihovníky

 [1]: https://github.com/Witiko/high-level-languages-for-tex/releases/download/latest/main.pdf
