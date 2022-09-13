# Vysokoúrovňové jazyky pro TeX

Tento repozitář obsahuje zdrojový kód článku *Vysokoúrovňové jazyky pro TeX* ze
Zpravodaje CSTUGu 2022/1–4. Článek je připravený ve vysokoúrovňovém značkovacím
jazyce Markdown a sestává z následujících souborů:

- `main.md` – Zdrojový kód článku ve vysokoúrovňovém značkovacím jazyce Markdown
- `markdownthemewitiko_csbulletin.sty` – Téma pro LaTeXový balíček Markdown,
  které udává vzhled jednotlivých značek jazyka Markdown
- `main.tex` – TeXový dokument, který při překladu vysází článek

Článek vysázíte následujícím příkazem:

``` tex
latexmk -pdf -shell-escape main.tex
```
