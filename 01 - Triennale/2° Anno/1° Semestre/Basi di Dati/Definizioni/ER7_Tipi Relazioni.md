---
tags:
  - corsi/informatica/basi_di_dati
---
Un **Tipo di Relazione** è un'associazione tra $n$ tipi di entità.

Il **Grado** di un tipo di relazione è il numero di entità che vi partecipano. Se il grado è 2, la relazione è detta *binaria*.

Per esempio, rappresentiamo il fatto che ogni impiegato $e_i$ lavora per un dipartimento $d_j$.

Definiamo il tipo di relazione LAVORA_PER tra i due tipi di entità IMMPIEGATO e DIPARTIMENTO: ogni relazione $r_i$ associa una entità IMPIEGATO $e_i$ con una entità DIPARTIMENTO $d_j$.

![[Pasted image 20221008123109.png]]

In questo caso il grado della relazione è binaria.

> [!info]- Relazione Terziaria
> ![[Immagine 2022-10-08 123235.png]]

## TIPI DI RELAZIONI
- **Relazione uno ad uno**: Un'entità dell'insieme di entità A può essere associata al massimo a un'entità dell'insieme di entità B e viceversa. ![[Pasted image 20221008125606.png]] ^220e06
- **Relazione uno a molti**: Un'entità dell'insieme di entità A può essere associata a più entità dell'insieme di entità B, ma un'entità dell'insieme di entità B può essere associata al massimo a un'entità. ![[Pasted image 20221008125652.png]] ^4ad5e9
- **Relazione molti a uno**: È il contrario della relazione uno a molti. Più entità dell'insieme di entità A possono essere associate al massimo a un'entità dell'insieme di entità B, tuttavia un'entità dell'insieme di entità B può essere associata a più di un'entità dell'insieme di entità A. ![[Pasted image 20221008125741.png]] ^27ba3d
- **Relazione molti a molti**: Un'entità di A può essere associata a più entità di B e viceversa. ![[Pasted image 20221008125855.png]] ^365d02

%%[[000 Indice BD|↖ Ritorna all'indice ↖]]%%