---
aliases:
  - Use Case Diagram
tags:
  - corsi/informatica/ingegneria_software
paragrafo: UML
cssclasses:
  - 
---
>Un **Diagramma di [[011 Scenari e Casi d_Uso#Caso d'Uso|Casi d'Uso]]** (*Use Case Diagram*) è un diagramma di comportamento che descrive le funzionalità del sistema dal *punto di vista esterno di un [[005 UML#Attori|Attore]]*, come gli utenti o altri sistemi.

![[Pasted image 20240116163616.png|500]]

Identificando gli attori e i casi d'uso, si definiscono i confini del sistema, distinguendo i compiti svolti dal sistema e quelli gestiti dall'ambiente esterno. 
Gli attori esistono al di fuori dei confini del sistema, mentre i casi d'uso operano al suo interno.

> [!example]- <font color="orange">Esempio</font>
>![[Schermata del 2023-09-29 08-24-51.png|650]]
>Un Caso d'Uso UML che descrive la funzionalità di un semplice orologio. 
>L'attore `WatchUser` può visualizzare l'orario oppure impostarlo, metre l'attore `WatchRepairPerson` può cambiare la batteria.



### Notazioni grafiche per questo tipo di diagramma
| Concetto | Rappresentazione |
| ---- | ---- |
| [[002 Sistemi, Modelli e Viste#Sistema\|Sistema]] o [[002 Sistemi, Modelli e Viste#Modello\|Modello]] | *Rettangolo* |
| Casi d'uso | *Ovale* |
| Attori | *Stick-figure* |
| Associazione | *Linea* |
| Relazione "Include" | *Freccia tratteggiata con `<<include>>`* |
| Relazione "Extend" | *Freccia tratteggiata con `<<extend>>`* |
| Generalizzazione | *Freccia con punta vuota* |


## Relazioni tra Casi d'Uso
### Associazione
Rappresenta un'*interazione basilare* tra un attore ed un caso d'uso. Ogni attore deve interagire con almeno un caso d'uso.

![[Pasted image 20231007150011.png|500]]

### Include
Rappresenta una **dipendenza** tra un caso d'uso *base* ed uno *incluso*. 

Ogni volta che il caso d'uso base viene attivato, verrà attivato *anche* quello incluso. Ovvero che il caso d'uso base, per essere *completo*, ha bisogno di quello incluso.

La freccia è rivolta verso il caso d'uso che viene incluso.

![[Immagine 2023-10-07 150411 1.png|550]]

> [!example]- <font color="orange">Esempio</font>
>![[Immagine 2023-10-07 150840.png|600]]

Le *ridondanze* tra casi d'uso possono essere rimosse utilizzando una include, ovvero un caso d'uso sfrutta un altro caso d'uso.

Viene usato soprattutto quando una funzionalità è troppo complessa per poter essere risolta immediatamente. Una soluzione sarebbe quella di descrivere la funzionalità come una *aggregazione di funzionalità più semplici*. Pertanto il caso d'uso originale viene *decomposto* in più casi d'uso.

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20231007110203.png|600]]
>
>Il caso d'uso `CreateDocument` è composto dai casi d'uso `Scan`, `OCR` e `Check`.

### Extend
Offre **la possibilità** di *estendere il comportamento* di un caso d'uso base, solo se certi criteri sono *soddisfatti*. 
Infatti rappresenta casi invocati eccezionalmente o raramente *come gli errori*.

La freccia è rivolta verso il caso il caso base, inoltre, la condizione che attiva il caso d'uso che estende (`Extend Use Case`) *viene inserita nella entry condition* dello stesso.

![[Immagine 2023-10-07 151205.png|500]]

Il caso d'uso base può essere utilizzato anche da solo.

> [!example]- <font color="orange">Esempio</font>
>![[Immagine 2023-10-07 155131.png]]

### Generalizzazione
Questa relazione rappresenta l'[[015 Ereditarietà|Ereditarietà]] dei casi d'uso *specializzati*.
Ogni figlio condivide le stesse caratteristiche del genitore ma aggiungono qualcosa di particolare.

![[Immagine 2023-10-07 155523.png]]

___
[[000 Indice IS|↖ Ritorna all'indice ↖]]

Altri collegamenti: 
- [Lucid Software - Video](https://www.youtube.com/watch?v=4emxjxonNRI)
- [freeCodeCamp - Video](https://youtu.be/WnMQ8HlmeXc?t=3429)
- [Visual Paradigm](https://www.visual-paradigm.com/guide/uml-unified-modeling-language/what-is-use-case-diagram/)