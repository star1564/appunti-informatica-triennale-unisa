---
aliases:
  - Activity Diagram
tags:
  - corsi/informatica/ingegneria_software
paragrafo: UML
cssclasses:
  - 
---
>Un **Diagramma di Attività** (*Activity Diagram*) è un diagramma di comportamento di un sistema *in termini delle sue attività*.

![[Pasted image 20240316100843.png|650]]

I diagrammi di attività sono *simili ai diagrammi di flusso*, in quanto possono essere utilizzati per rappresentare il *flusso di controllo* (cioè l'ordine in cui avvengono le operazioni) e il *flusso di dati* (cioè gli oggetti che vengono scambiati tra le operazioni).

### Attività
Le **Attività** sono elementi di modellazione che rappresentano l'*esecuzione di un insieme di operazioni*. L'esecuzione di un'attività può essere innescata dal completamento di altre attività, dalla disponibilità di oggetti o da eventi esterni.

> [!warning] Le attività devono essere ad alto livello

Dopo che un'attività è conclusa, si possono avere altre attività che: 
- Possono essere eseguite in **parallelo** (*fork*): suddivide il comportamento in un insieme di *flussi di attività paralleli* o concomitanti.
- Possono essere **sdoppiate** (*decisione*): rappresenta una *condizione di test* per garantire che il flusso di controllo *percorra un solo percorso*.

Entrambi devono essere chiusi con un *nodo di merge*.

Una **swimlane** (*partizione*) è un modo per raggruppare le attività eseguite *dallo stesso attore o sistema* in un diagramma delle attività.

> [!example]- <font color="orange">Esempi</font>
>Una volta ricevuto l'ordine, le attività si dividono in due serie parallele di attività. Una parte compila e invia l'ordine, mentre l'altra si occupa della fatturazione.
>
>Sul lato riempimento dell'ordine, il metodo di consegna viene deciso in modo condizionato. A seconda della condizione, viene eseguita l'attività Consegna notturna o l'attività Consegna regolare.
>
>Infine, le attività parallele si combinano per chiudere l'ordine.
>
>![[Pasted image 20240316101259.png]]
>
>---
>
>![[Pasted image 20240316102014.png]]

### Notazioni grafiche per questo tipo di diagramma

| Concetto                | Rappresentazione                                        |
| ----------------------- | ------------------------------------------------------- |
| Attività                | *Rettangolo con angoli arrotondati*                     |
| Controllo del flusso    | *Freccia*                                               |
| Nodo iniziale           | *Cerchio pieno*                                         |
| Nodo finale             | *Cerchio pieno all'intenro di un altro cerchio esterno* |
| Nodo di decisione       | *Rombo con frecce uscenti*                              |
| Nodo merge di decisione | *Rombo con frecce entranti*                             |
| Nodo di fork            | *Rettangolo nero con frecce uscenti*                    |
| Nodo merge di fork      | *Rettangolo nero con frecce entranti*                   |
| Swimlane                | *Contenitore con il nome dell'attore*                   |


___
[[000 Indice IS|↖ Ritorna all'indice ↖]]

Altri collegamenti: 
- [Visual Paradigm](https://www.visual-paradigm.com/guide/uml-unified-modeling-language/what-is-activity-diagram/)