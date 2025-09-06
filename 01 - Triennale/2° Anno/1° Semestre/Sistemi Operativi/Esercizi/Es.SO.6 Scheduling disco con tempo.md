---
tags:
  - corsi/informatica/sistemi_operativi
  - corsi
---
[[017 Scheduling di un Disco]]

> [!quote] Traccia
> Si consideri un disco dotato di una sola testina e 100 tracce, dove lo spostamento da una traccia alla adiacente richieda 1ms. Si supponga che al tempo 0ms mentre la testina si trova sulla traccia   18 e si sta muovendo verso la traccia 99, le richieste in sospeso siano (i tempi indicati sono in ms):
>
|     traccia     | 25  |  6  | 10  | 66  | 51  | 97  |
|:---------------:|:---:|:---:|:---:|:---:|:---:|:---:|
| tempo di arrivo |  0  |  4  | 12  | 26  | 41  | 67  |
>
> 1. Determinare come vengono servite le richieste seguendo le strategie [[017 Scheduling di un Disco|SSTF, FCFS, SCAN, C-LOOK]];
> 2. Valutare i tempi di attesa di ogni richiesta.

Il **Tempo di arrivo** è l'istante in cui compare la richiesta. Gli algoritmi ad un certo istante di tempo vedono solo quello che si è rilevato. 
Per esempio siamo all'istante 20, possiamo solo vedere le richieste <25, 6, 10>.

L'istante corrente si calcola così: $$\color{#CC241D}|traccia\ corrente - traccia\ target|\ +\ tempo\ corrente$$

![[Scan2022-10-12_153025.jpg]]

%%[[000 Indice SO|↖ Ritorna all'indice ↖]]%%