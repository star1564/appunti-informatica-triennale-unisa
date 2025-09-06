---
tags:
  - corsi/informatica/grafica_interattivita
---
## Informazioni corso

### Libri
| Titolo                                     | Autore                                             | Edizione | Editore | Anno | ISBN          | Note |
| ------------------------------------------ | -------------------------------------------------- | -------- | ------- | ---- | ------------- | ---- |
| Computer Graphics and Virtual Environments | Mel Slater<br>Anthony Steed<br>Yiorgos Chrysanthou | 1°       |         | 2001 | 9780201624205 |      |


### Docenti
- [Andrea Francesco ABATE](https://docenti.unisa.it/004620/home)

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