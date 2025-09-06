---
tags:
  - corsi/informatica/progettazione_algoritmi
---

## Informazioni corso

### Libri

### Docenti
- [Ugo VACCARO](https://docenti.unisa.it/000806/home)

### Link utili


## Indice Lezioni
```dataviewjs
let tag = "#" + dv.current().tags[0]

dv.execute(
	"TABLE paragrafo AS Paragrafo" +
	" FROM " + tag +
	" WHERE (file != this.file) AND (paragrafo != NULL)" +
	" SORT file.name ASC"
)
```