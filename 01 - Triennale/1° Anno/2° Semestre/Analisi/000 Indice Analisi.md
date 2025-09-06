---
tags:
  - corsi/matematica/analisi
---

## Informazioni corso

### Libri
| Titolo                             | Autore                                                                       | Edizione | Editore | Anno | ISBN          | Note |
| ---------------------------------- | ---------------------------------------------------------------------------- | -------- | ------- | ---- | ------------- | ---- |
| Analisi Matematica per Informatici | Patrizi Di Gironimo<br>Gerardo Iovane<br>Elmo Benedetto<br>Antonio Briscione |          | Aracne  |      | 9791259945839 |      |
| ~~Elementi di Analisi Matematica uno~~ | ~~Paolo Marcellini<br>Carlo Sbordone~~                                           |          | ~~Liguori~~ | ~~2016~~ | ~~9788820733834~~ |      |


### Docenti
- [Patrizia DI GIRONIMO](https://docenti.unisa.it/001594/home)

### Link utili
  - [YouMath](https://www.youmath.it/lezioni/analisi-matematica.html)
  - [Elia Bombardelli](https://www.youtube.com/channel/UC3_rz0ss9O7Yy0ypBG4M6lg)
  - https://www.desmos.com/calculator?lang=it
  - https://www.geogebra.org/graphing?lang=it

## Indice Lezioni

```dataviewjs
let tag = "#" + dv.current().tags[0]

dv.execute(
	"TABLE paragrafo AS Paragrafo" +
	" FROM " + tag +
	" WHERE (file != this.file) AND (paragrafo != NULL) AND (paragrafo != \"Matematica di Base\") AND (paragrafo != \"Proprietà di Funzioni\")" +
	" SORT file.name ASC"
)
```

## Indice Funzioni

```dataviewjs
let tag = "#" + dv.current().tags[0]

dv.execute(
	"TABLE paragrafo AS Paragrafo" +
	" FROM " + tag +
	" WHERE (file != this.file) AND (paragrafo = \"Proprietà di Funzioni\")" +
	" SORT file.name ASC"
)
```

## Indice Matematica di Base
```dataviewjs
let tag = "#" + dv.current().tags[0]

dv.execute(
	"TABLE paragrafo AS Paragrafo" +
	" FROM " + tag +
	" WHERE (file != this.file) AND (paragrafo = \"Matematica di Base\")" +
	" SORT file.name ASC"
)
```