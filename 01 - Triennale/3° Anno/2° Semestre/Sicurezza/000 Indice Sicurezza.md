---
tags:
  - corsi/informatica/sicurezza
---

> [!question] Domande esame
> Sono segnate alcune domande per l'esame, anche se potrebbero cambiare in futuro.

## Informazioni corso

### Libri
| Titolo                                                    | Autore            | Edizione | Editore | Anno | ISBN          | Note |
| --------------------------------------------------------- | ----------------- | -------- | ------- | ---- | ------------- | ---- |
| Cryptography and Network Security Principles and Practice | William Stallings | 8°       | Pearson | 2001 | 9780136707226 |      |


### Docenti
- [Alfredo DE SANTIS](https://docenti.unisa.it/000769/home)

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