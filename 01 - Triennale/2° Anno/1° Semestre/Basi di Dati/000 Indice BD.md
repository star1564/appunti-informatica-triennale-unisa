---
tags:
  - corsi/informatica/basi_di_dati
---

## Informazioni corso

### Libri
| Titolo                                            | Autore                               | Edizione | Editore | Anno | ISBN          | Note |
| ------------------------------------------------- | ------------------------------------ | -------- | ------- | ---- | ------------- | ---- |
| Sistemi di basi di dati: Fondamenti e complementi | Ramez Elmasri<br>Shamkant B. Navathe | 7°       | Pearson | 2018 | 9788891902504 |      |

### Docenti
- [Genoveffa TORTORA](https://docenti.unisa.it/000751/home)
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