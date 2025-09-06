---
tags:
  - corsi/informatica/programmazione_object_oriented
---

## Informazioni corso

### Libri
| Titolo                                       | Autore        | Edizione | Editore | Anno | ISBN          | Note |
| -------------------------------------------- | ------------- | -------- | ------- | ---- | ------------- | ---- |
| Concetti di informatica e fondamenti di Java | Cay Horstmann | 7°       | Apogeo  | 2019 | 9788891639431 |      |

### Docenti
- [Christiancarmine ESPOSITO](https://docenti.unisa.it/030400/home)
- [Salvatore LA TORRE](https://docenti.unisa.it/004821/home)

### Link utili
- [Java Docs](https://docs.oracle.com/en/java/javase/15/docs/api/)
- [Guida html.it](https://www.html.it/guide/guida-java/)

## Indice Lezioni

```dataviewjs
let tag = "#" + dv.current().tags[0]

dv.execute(
	"TABLE paragrafo AS Paragrafo" +
	" FROM " + tag +
	" WHERE (file != this.file) AND (paragrafo = \"Eclipse Help\")" +
	" SORT file.name ASC"
)

```


```dataviewjs
let tag = "#" + dv.current().tags[0]

dv.execute(
	"TABLE paragrafo AS Paragrafo" +
	" FROM " + tag +
	" WHERE (file != this.file) AND (paragrafo != NULL) AND (paragrafo != \"Eclipse Help\")" +
	" SORT file.name ASC"
)
```