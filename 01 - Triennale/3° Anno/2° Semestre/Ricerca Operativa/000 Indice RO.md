---
tags:
  - corsi/matematica/ricerca_operativa
---

> [!error] !!! Le note di Ricerca Operativa non sono complete e in parte sono errate !!!

## Informazioni corso

### Libri
| Titolo                               | Autore                                                     | Edizione | Editore | Anno | ISBN          | Note |
| ------------------------------------ | ---------------------------------------------------------- | -------- | ------- | ---- | ------------- | ---- |
| Linear Programming and Network Flows | Mokhtar S. Bazaraa<br>John Jeff Jarvis<br>Hanif D. Sherali |          |         | 2009 | 9780471485995 |      |


### Docenti
- [Raffaele CERULLI](https://docenti.unisa.it/001227/home)

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