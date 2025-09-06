---
tags:
  - corsi/informatica/elementi_teoria_computazione
---
## Informazioni corso

### Libri
| Titolo                                                      | Autore                                                          | Edizione | Editore | Anno | ISBN          | Note                 |
| ----------------------------------------------------------- | --------------------------------------------------------------- | -------- | ------- | ---- | ------------- | -------------------- |
| Introduzione alla teoria della computazione                 | Michael Sipser                                                  | 8°       | Apogeo  | 2016 | 9788891616180 | Chiamato "Il Sipser" |
| Introduction to Automata Theory, Languages, and Computation | John E. Hopcroft<br><br>Jeffrey D. Ullman<br><br>Rajeev Motwani | 3°       | Pearson | 2006 | 9780321455369 | Chiamato "HUM"       |

### Docenti
- [Clelia DE FELICE](https://docenti.unisa.it/001119/home)
- [Rosalba Zizza](https://docenti.unisa.it/020880/home)

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