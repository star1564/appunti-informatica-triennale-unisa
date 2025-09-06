---
tags:
  - corsi/informatica/reti_calcolatori
---

## Informazioni corso

### Libri
| Titolo              | Autore                                   | Edizione | Editore | Anno | ISBN          | Note |
| ------------------- | ---------------------------------------- | -------- | ------- | ---- | ------------- | ---- |
| Reti di calcolatori | Andrew S. Tanenbaum<br>David J.Wetherall | 5°       | Pearson | 2011 | 9788871926407 |      |

### Docenti
- [Christiancarmine ESPOSITO](https://docenti.unisa.it/030400/home)

### Link utili
- [Java Docs](https://docs.oracle.com/en/java/javase/15/docs/api/)
- [Guida html.it](https://www.html.it/guide/guida-java/)

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
