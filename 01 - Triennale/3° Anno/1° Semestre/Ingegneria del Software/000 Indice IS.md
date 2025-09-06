---
tags:
  - corsi/informatica/ingegneria_software
---

## Informazioni corso

### Libri
| Titolo                                                             | Autore                           | Edizione | Editore | Anno | ISBN          | Note    |
| ------------------------------------------------------------------ | -------------------------------- | -------- | ------- | ---- | ------------- | ------- |
| Object-Oriented Software Engineering, Using UML, Patterns and Java | Bernd Bruegge<br>Allen H. Dutoit | 3°       | Pearson | 2018 | 9780136061250 | Inglese |

### Docenti
- [Andrea DE LUCIA](https://docenti.unisa.it/003241/home)

### Link utili


### Altre fonti
  - Appunti (G) di Ingegneria del Software a.a. 2021/2022 (autore sconosciuto)
  - Ingegneria del Software - Associazione Libera Mente

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