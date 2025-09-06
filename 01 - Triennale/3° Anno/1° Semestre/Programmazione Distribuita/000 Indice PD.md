---
tags:
  - corsi/informatica/programmazione_distribuita
---

## Informazioni corso

### Libri
| Titolo                                           | Autore            | Edizione | Editore | Anno | ISBN          | Note                 |
| ------------------------------------------------ | ----------------- | -------- | ------- | ---- | ------------- | -------------------- |
| Programmazione con Oggetti Distribuiti: Java RMI | Vittorio Scarano  |          |         |      |               | Chiamato "Libro Blu" |
| Beginning Java EE 7                              | Antonio Goncalves | 1°       | Apress  | 2013 | 9781430246268 | Inglese              |

### Docenti
- [Maria Angela PELLEGRINO](https://docenti.unisa.it/033807/home)
- [Carmine SPAGNUOLO](https://docenti.unisa.it/028012/home)
- [Vittorio SCARANO](https://docenti.unisa.it/001717/home)

### Link utili

## Indice Lezioni

```dataviewjs
let tag = "#" + dv.current().tags[0]

dv.execute(
	"TABLE paragrafo AS Paragrafo" +
	" FROM " + tag +
	" WHERE (file != this.file) AND (paragrafo != NULL) AND (paragrafo != \"GlassFish Help\")" +
	" SORT file.name ASC"
)
```

## Indice GlassFish/Pratica

```dataviewjs
let tag = "#" + dv.current().tags[0]

dv.execute(
	"TABLE paragrafo AS Paragrafo" +
	" FROM " + tag +
	" WHERE (file != this.file) AND (paragrafo = \"GlassFish Help\")" +
	" SORT file.name ASC"
)
```
