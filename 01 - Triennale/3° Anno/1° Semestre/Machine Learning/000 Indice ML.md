---
tags:
  - corsi/informatica/machine_learning
---
## Informazioni corso

### Libri
| Titolo                             | Autore     | Edizione | Editore        | Anno | ISBN          | Note    |
| ---------------------------------- | ---------- | -------- | -------------- | ---- | ------------- | ------- |
| Designing Machine Learning Systems | Huyen Chip | 1°       | O'Reilly Media | 2022 | 9781098107963 | Inglese |

### Docenti
- [Giuseppe POLESE](https://docenti.unisa.it/004281/home)
- [Loredana CARUCCIO](https://docenti.unisa.it/027648/home)

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