---
tags:
  - corsi/matematica/matematica_discreta
cssclasses:
  - 
---
## Informazioni corso

### Libri
| Titolo              | Autore                                                                      | Edizione | Editore     | Anno | ISBN          | Note                                |
| ------------------- | --------------------------------------------------------------------------- | -------- | ----------- | ---- | ------------- | ----------------------------------- |
| Matematica discreta | Costantino Delizia<br>Patrizia Longobardi<br>Mercede Maj<br>Chiara Nicotera |          | McGraw-Hill | 2009 | 9788838665127 | Alcune dimostrazioni sono sul libro |


### Docenti
- [Chiara Nicotera](https://docenti.unisa.it/005094/home)

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
