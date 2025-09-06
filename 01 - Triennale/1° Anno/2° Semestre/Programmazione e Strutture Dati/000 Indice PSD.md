---
tags:
  - corsi/informatica/programmazione_strutture_dati
---

## Informazioni corso

### Libri
| Titolo         | Autore           | Edizione | Editore | Anno | ISBN          | Note |
| -------------- | ---------------- | -------- | ------- | ---- | ------------- | ---- |
| Algoritmi in C | Robert Sedgewick | 4°       | Pearson | 2015 | 9788891900746 |      |

### Docenti
- [Vittorio FUCCELLA](https://docenti.unisa.it/004376/home)

### Link utili

## Indice Lezioni

```dataviewjs
let tag = "#" + dv.current().tags[0]

dv.execute(
	"TABLE paragrafo AS Paragrafo" +
	" FROM " + tag +
	" WHERE (file != this.file) AND (paragrafo != NULL) AND (paragrafo != \"Codice\")" +
	" SORT file.name ASC"
)
```

## Indice Laboratorio

```dataviewjs
let tag = "#" + dv.current().tags[0]

dv.execute(
	"TABLE paragrafo AS Paragrafo" +
	" FROM " + tag +
	" WHERE (file != this.file) AND (paragrafo = \"Codice\")" +
	" SORT file.name ASC"
)

```

