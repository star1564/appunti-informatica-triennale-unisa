---
aliases:
  - Precondizione
  - Postcondizione
  - Invariante
cssclasses:
tags:
  - corsi/informatica/ingegneria_software
---
>I **Contratti** sono *vincoli su una classe* che consentono agli sviluppatori di condividere le stesse ipotesi sulla classe. Un contratto specifica i vincoli che il [[018 CVS - Progettazione#^af5248|Class User]] deve soddisfare prima di utilizzarla, nonché i vincoli che vengono garantiti dal [[018 CVS - Progettazione#^8b99ec|Class Implementor]] e dal [[018 CVS - Progettazione#^9570c2|Class Extender]] quando viene utilizzata. 

I contratti includono tre tipi di vincoli.

- **Invarianti**: condizione che deve essere *sempre vera per tutto il caso d'uso*.
 ^fba292
- **Precondizione**: condizione che deve essere *vera prima dell'invocazione del caso d'uso* (come una funzione), affinché questo sia applicabile;
 ^e78e08
- **Postcondizione**: condizione che deve essere *vera dopo che il caso d'uso* (come una funzione) *sia finito*. Per le funzioni, definisce cosa sono i dati di output in funzione di quelli di input. ^438049


%%
[[000 Indice IS|↖ Ritorna all'indice ↖]]
%%