---
tags:
  - corsi/informatica/tecnologie_software_web
---
## Informazioni corso

### Libri

### Docenti
- [Giuseppe SCANNIELLO](https://docenti.unisa.it/020007/home)
- [Simone ROMANO](https://docenti.unisa.it/042849/home)

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