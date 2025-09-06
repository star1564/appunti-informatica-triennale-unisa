---
aliases: Component Diagram
tags:
  - corsi/informatica/ingegneria_software
paragrafo: UML
cssclasses: 
---
>I **Diagrammi dei Componenti** (*Component Diagram*) mostrano le *dipendenze tra le componenti **fisiche** di un sistema o sottosistema* (codice sorgente, librerie, eseguibili).

I **Componenti** sono entità autonome che *forniscono servizi ad altri componenti* o utenti.  ^2c581e

![[Pasted image 20240119142356.png|450]]

Le dipendenze sono mostrare con frecce tratteggiate dal componente client al componente fornitore.

![[Pasted image 20240119142410.png|500]]

Un diagramma a componenti suddivide il sistema effettivo in fase di sviluppo in vari livelli di funzionalità. Ogni componente è responsabile di un obiettivo chiaro all'interno dell'intero sistema e interagisce con gli altri elementi essenziali solo in base alla necessità.

### Notazioni grafiche per questo tipo di diagramma

| Concetto | Rappresentazione |
| ---- | ---- |
| Componente | *Rettangolo con due rettangoli al lato sinistro* |
| Interfaccia | *Cerchio* |
| Dipendenza | *Freccia tratteggiata verso l'interfaccia* |




___
[[000 Indice IS|↖ Ritorna all'indice ↖]]

Altro:
- [Visual Paradigm](https://www.visual-paradigm.com/guide/uml-unified-modeling-language/what-is-component-diagram/)