---
tags:
  - corsi/matematica/metodi_matematici_informatica
---

## Informazioni corso

### Libri
| Titolo                                    | Autore           | Edizione | Editore     | Anno | ISBN          | Note    |
| ----------------------------------------- | ---------------- | -------- | ----------- | ---- | ------------- | ------- |
| Discrete Mathematics and Its Applications | Kenneth H. Rosen | 8°       | McGraw-Hill | 2019 | 9781260091991 | Inglese |

### Docenti
- [Clelia DE FELICE](https://docenti.unisa.it/001119/home)
- [Rosalba Zizza](https://docenti.unisa.it/020880/home)

### Link utili


## Indice

```dataviewjs
let tag = "#" + dv.current().tags[0]

dv.execute(
	"TABLE paragrafo AS Paragrafo" +
	" FROM " + tag +
	" WHERE (file != this.file) AND (paragrafo != NULL)" +
	" SORT file.name ASC"
)
```

