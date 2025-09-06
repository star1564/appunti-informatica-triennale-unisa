---
aliases: 
  - Class Diagram
  - Classe Associativa
tags:
  - corsi/informatica/ingegneria_software
paragrafo: UML
cssclasses:
  - 
---
>Il **Diagramma di Classi** (*Class Diagram*) è la tecnica di modellazione principale dell'[[005 UML|UML]]. È usato per la modellazione di una *struttura statica a [[Classi]]* di un sistema software object-oriented. Descrive le classi di un sistema, con i suoi attributi e metodi.

All'interno di questo diagramma, il **nome della classe** è *obbligatorio* e deve essere *unico*.

![[Pasted image 20240116165025.png]]

Il tipo degli attributi ed il tipo che viene ritornato dai metodi vengono chiamate *signature* e si scrivono dopo il nome e due punti.

![[Pasted image 20231020165700.png]]

### Modificatori di Accesso
Prima dei nomi degli attributi e metodi si inserisce il modificatore di accesso.

| Segno | Accesso |
| ---- | ---- |
| **`+`** | [[Tipi, Signatures e Visibilità (IS)#^9f7106\|public]] |
| **`-`** | [[Tipi, Signatures e Visibilità (IS)#^60f564\|private]] |
| **`#`** | [[Tipi, Signatures e Visibilità (IS)#^3e550d\|protected]] |
| **`-`** | [[Tipi, Signatures e Visibilità (IS)#^b51fae\|package]] |

### Notazioni grafiche per questo tipo di diagramma

| Concetto                    | Rappresentazione                                        |
| --------------------------- | ------------------------------------------------------- |
| Classe                      | *Rettangolo con un nome*                                |
| Attributi della classe      | *Estensione del rettangolo di classe*                   |
| Metodi della classe         | *Estensione del rettangolo di classe*                   |
| Associazione                | *Linea con un numero su ogni endpoint*                  |
| Relazione di Ereditarietà   | *Freccia con punta vuota*                               |
| Realizzazione (Interfaccia) | *Freccia tratteggiata con punta vuota*                  |
| Dipendenza                  | *Freccia tratteggiata*                                  |
| Relazione di Aggregazione   | *Linea con un rombo vuoto all'endpoint*                 |
| Relazione di Composizione   | *Linea con un rombo pieno all'endpoint*                 |
| Package                     | *Rettangolo con un nome in un altro rettangolo esterno* |




## Relazioni tra Classi
### Associazione
Le associazioni collegano le classi che **interagiscono tra di loro**. Si indica con una linea continua.

Si può specificare la *molteplicità* per indicare il numero di oggetti che possono prendere parte alla relazione (si inserisce un verbo in mezzo):
- *Uno a Uno*: si indica inserendo `1` da entrambi le parti; ^a78046
- *Uno a Molti*: si indica inserendo `1` vicino alla classe che partecipa una sola volta e si inserisce `*` vicino alla classe che partecipa molte volte; ^65d403
- *Molti a Molti*: si indica inserendo `*` da entrambi le parti. ^7f05ca
- *N a M*: si indica inserendo `N..M` vicino ad una classe, dove `N` ed `M` possono essere un qualsiasi numero o `*` (con `N` < `M`). Rappresenta la relazione "da `N` a `M`".

> [!example]+ <font color="orange">Esempio</font>
>![[Pasted image 20231020171034.png|500]]
>- Un tutor insegna a molti studenti.
>- Uno studente impara da molti tutor.
>
>![[Pasted image 20231020172648.png|500]]
>Un cliente può effettuare da 0 a molti ordini.

### Ereditarietà o Generalizzazione

^688b5f

È una schematizzazione della relazione di **[[015 Ereditarietà|Ereditarietà]]** che si ha tra la classe genitore e le classi discendenti. Si indica con una linea continua che *parte dalle classi discendenti* e si dirige con una *freccia vuota verso la classe genitore*.

![[Pasted image 20231020171414.png|500]]

### Realizzazione (Interfaccia)
È una relazione tra una **[[018 Interfacce|Interfaccia]]** e la classe che la implementa. Si indica con una linea tratteggiata e con una freccia piena indirizzata verso l'interfaccia.

![[Pasted image 20240318093712.png|400]]

### Dipendenza
La [[024 Dipendenze e Tipi di Iniezioni#Dipendenza|Dipendenza]] può essere tra due classi, dove i cambiamenti di una potrebbero influenzare l'altra (ma non al contrario).

In questa immagine, la `CustomerView` dipende da `Customer`.

![[Pasted image 20240318093645.png]]


### Aggregazione
È una relazione che mette insieme diverse classi in una sola grande classe. Si indica con una linea continua con un rombo vuoto sulla classe aggregatrice ("contenitore").

Ognuno dei componenti che formano la classe aggregatrice può funzionare *indipendentemente*.

![[Pasted image 20231020172103.png|500]]

### Composizione
Simile alla aggregazione, ma ognuno dei componenti che formano la classe aggregatrice *NON può funzionare* al di fuori della relazione. Si indica con una linea continua con un rombo pieno sulla classe aggregatrice ("contenitore").

![[Pasted image 20231020172333.png|500]]

### Package
Un **Package** è un meccanismo per *organizzare gli elementi in gruppi*.

![[Pasted image 20240116170541.png]]

### Classi Associative
Una **Classe Associativa** è usata per *contenere gli attributi e le operazioni di una associazione*.

![[Pasted image 20240125120820.png|700]]

___
[[000 Indice IS|↖ Ritorna all'indice ↖]]

Altri collegamenti: 
- [freeCodeCamp - Video](https://youtu.be/WnMQ8HlmeXc?t=579)
- [Visual Paradigm](https://www.visual-paradigm.com/guide/uml-unified-modeling-language/what-is-class-diagram/)