---
tags:
  - corsi/informatica/sistemi_operativi
---

## Informazioni corso

### Libri
| Titolo                                           | Autore                                    | Edizione | Editore           | Anno     | ISBN              | Note |
| ------------------------------------------------ | ----------------------------------------- | -------- | ----------------- | -------- | ----------------- | ---- |
| Sistemi operativi: Concetti ed esempi            | Abraham Silberschatz<br>Peter Baer Galvin | 10°      | Pearson           | 2019     | 9788891904553     |      |
| ~~Advanced Programming in the UNIX Environment~~ | ~~W. Richard Stevens<br>Stephen A. Rago~~ | ~~3°~~   | ~~Adison Wesley~~ | ~~2013~~ | ~~9780321637734~~ |      |

### Docenti
- [Adele Anna RESCIGNO](https://docenti.unisa.it/001727/home)
- [Andrea Francesco ABATE](https://docenti.unisa.it/004620/home)

### Link utili


## Indice Lezioni
```dataviewjs
let tag = "#" + dv.current().tags[0]

dv.execute(
	"TABLE paragrafo AS Paragrafo" +
	" FROM " + tag +
	" WHERE (file != this.file) AND (paragrafo != NULL) AND (paragrafo != \"Laboratorio SO\")" +
	" SORT file.name ASC"
)
```

## Indice Laboratorio

```dataviewjs
let tag = "#" + dv.current().tags[0]

dv.execute(
	"TABLE paragrafo AS Paragrafo" +
	" FROM " + tag +
	" WHERE (file != this.file) AND (paragrafo = \"Laboratorio SO\")" +
	" SORT file.name ASC"
)
```