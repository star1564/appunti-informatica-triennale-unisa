---
tags:
  - corsi/informatica/architettura_degli_elaboratori
cssclasses:
  - 
---
## Informazioni corso

### Libri
| Titolo                               | Autore                                 | Edizione | Editore    | Anno | ISBN          | Note               |
| ------------------------------------ | -------------------------------------- | -------- | ---------- | ---- | ------------- | ------------------ |
| Struttura e progetto dei calcolatori | David A. Patterson<br>John L. Hennessy | 4°       | Zanichelli | 2015 | 9788808352026 | Ha delle appendici |


### Docenti
- [D'ANGELO Gianni](https://docenti.unisa.it/004492/home)

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
