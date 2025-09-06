---
tags:
  - corsi/matematica/probabilità_statistica
---

## Informazioni corso

### Libri


### Docenti
- [Antonio DI CRESCENZO](https://docenti.unisa.it/005305/home)

### Link utili
- [Playlist - Harvard University Statistics 110](https://www.youtube.com/playlist?list=PL2SOU6wwxB0uwwH80KTQ6ht66KWxbzTIo)
- [Playlist - Michel van Biezen](https://www.youtube.com/playlist?list=PLX2gX-ftPVXUWwTzAkOhBdhplvz0fByqV)
- [Introduction to Probability](https://www.probabilitycourse.com/)


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