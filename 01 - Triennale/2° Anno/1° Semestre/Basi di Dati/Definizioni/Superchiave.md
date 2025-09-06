---
tags:
  - corsi/informatica/basi_di_dati
---
Si ha una **Superchiave** all'interno di una relazione o tabella di un [[010 Modello Relazionale|Modello Relazionale]], quando si ha un [[002 Sottoinsiemi|Sottoinsieme]] di [[ER3_Attributo|attributi]] con la proprietà di non avere la stessa combinazione di valori in più tuple.

Sia $sk$ un tale sottoinsieme di attributi di $R$:
$$t_1[sk]\neq t_2[sk]$$
L'insieme di attributi $sk$ è detto superchiave di $R$ se la condizione scritta qui sopra è vera per tutte le tuple di $R$.

> [!example]+ <font color="orange">Esempio</font>
> ![[Pasted image 20221201150703.png|600]]
> Dato che ogni elemento dell'insieme chiave sk è distinto, sk è una superchiave di `STUDENTE`.


%%[[000 Indice BD|↖ Ritorna all'indice ↖]]%%
