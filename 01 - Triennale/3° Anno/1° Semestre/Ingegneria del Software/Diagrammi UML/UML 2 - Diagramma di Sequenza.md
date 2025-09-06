---
aliases:
  - Sequence Diagram
tags:
  - corsi/informatica/ingegneria_software
paragrafo: UML
cssclasses:
  - 
---
>Un **Diagramma di Sequenza** (*Sequence Diagram*) è un diagramma di comportamento che descrive il *comportamento dinamico* tra gli [[005 UML|Attori]] ed il [[002 Sistemi, Modelli e Viste|Sistema]], e tra gli oggetti ed il sistema.

Mostra come gli elementi interagiscono nel corso del tempo ed è organizzato in due direzioni:
- *Orizzontale*: gli elementi in ordine di apparenza nell'interazione (da sinistra a destra).
- *Verticale*: il tempo (dall'alto al basso).

![[Pasted image 20240116172151.png|600]]

In cima al diagramma ci sono gli oggetti che *esistono prima del messaggio* che ha inizializzato il diagramma.

Le *frecce orizzontali* tra le colonne rappresentano i **messaggi** o stimoli inviati da un oggetto ad un altro. L'elemento di origine *attende una risposta*.

Le *frecce tratteggiate* indicano i **messaggi di ritorno (o asincroni)**, ovvero un messaggio che viene inviato da un elemento di origine a quello di destinazione *come risposta* con un valore di ritorno. Quindi l'elemento di origine *non attende una risposta* prima di continuare. ^d6bd1d

La *ricezione di un messaggio* determina l'attivazione di un'**operazione**. Un'operazione è un servizio fornito ad altri oggetti e la sua attivazione è rappresentata da un *rettangolo* da cui altri messaggi possono originare. La lunghezza del rettangolo rappresenta il tempo durante il quale l'operazione è attiva. 

La sorgente di un messaggio indica l'**Attivazione** che ha inviato il messaggio, che è lunga quanto tutte le attivazioni annidate.

> [!example]- <font color="orange">Esempio</font>
> ![[Schermata del 2023-09-29 08-45-00.png|650]]
>In questo scenario, l'attore `WatchUser` preme due volte il pulsante 1 e una volta il pulsante 2 per impostare l'orologio un minuto avanti. Il caso d'uso `SetTime` si conclude quando `WatchUser` preme entrambi i pulsanti contemporaneamente.
>
>Da notare che i nomi sono sottolineati e preceduti da due punti, indicando che queste sono *istanze* di classi (quindi oggetti).

> [!tip]- Per il progetto o qualsiasi applicazione [[020 Stili di Architettura#Three-Tier|Three-Tier]] o [[020 Stili di Architettura#Model-View-Controller (MVC)|MVC]]
>Per creare diagrammi di sequenza con questi stili di architettura, devono essere presenti i tre componenti principali:
>- [[014 Modello ad Oggetti#^fc2909|Boundary]] - Rappresentano il [[020 Stili di Architettura#^37b01a|una parte del presentation layer (file HTML, ecc.) nel Three-Tier]] oppure la [[020 Stili di Architettura#^f67cb8|view nell'MVC]]
>- [[014 Modello ad Oggetti|Control]] - Rappresentano i controller nel presentation layer del Three-Tier oppure i [[020 Stili di Architettura#^efc839|Controller nell'MVC]]
>- Manager - Rappresentano l'[[020 Stili di Architettura#^0a5e48|application layer del Three-Tier]] oppure il [[020 Stili di Architettura#^c2d884|model dell'MVC]]
>
>**Esempio per il login:**
>
>![[Pasted image 20240317092123.png|800]]
>Da notare come vengono passati i valori all'interno dei messaggi.
>
>**Esempio per il login con errori (metodo 1):**
>
>![[Pasted image 20240317092202.png|800]]
>
>**Esempio per il login con errori (metodo 2):**
>
>![[Pasted image 20240317092228.png|800]]

### Notazioni grafiche per questo tipo di diagramma
| Concetto                         | Rappresentazione                             |
| -------------------------------- | -------------------------------------------- |
| Lifeline                         | *Linea tratteggiata verticale*               |
| Activation                       | *Rettangolo sulla Lifeline*                  |
| Messaggio                        | *Freccia con il messaggio*                   |
| Messaggio di Ritorno (asincrono) | *Freccia tratteggiata con il messaggio*      |
| Distruzione della lifeline       | *Croce sulla parte inferiore della lifeline* |


___
[[000 Indice IS|↖ Ritorna all'indice ↖]]

Altri collegamenti: 
- [freeCodeCamp - Video](https://youtu.be/WnMQ8HlmeXc?t=4639)
- [Visual Paradigm](https://www.visual-paradigm.com/guide/uml-unified-modeling-language/what-is-sequence-diagram/)
