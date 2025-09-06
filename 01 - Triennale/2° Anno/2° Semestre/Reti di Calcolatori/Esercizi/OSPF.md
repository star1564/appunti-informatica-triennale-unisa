---
tags:
  - corsi/informatica/reti_calcolatori
  - esercizi
---
Si consideri la rete in figura dove sono indicati router, reti e costo associato alle interfacce dei router.
![[Pasted image 20230602110946.png|1000]]
Si supponga di utilizzare il protocollo di routing OSPF.
Si divida come mostrato in figura la rete in tre aree (area 0, area 1 e area 2) e si disegnino i grafi che rappresentano la rete *vista dal router R1, R7 ed R10*.

> [!tip]+ Procedimento
> 1. Vedere tutti i router e nodi all'interno dell'area di un router.
> 2. Calcolare tutti i percorsi indiretti verso i nodi di altre aree a partire dai router di frontiera.
> 3. Se in quell'area esiste un altro router di frontiera, fare la "ricorsione" anche su quella nuova area.
> 
> Vista dal router R1:
> 1. Riesce a vedere tutti i nodi e router all'interno della sua area (area 0).
> 2. Si deve calcolare il costo dei collegamenti indiretti dell'area 1 (per via del router R3) e anche dell'area 2 (per via del router R4 contenuto nell'area 0).
> ![[Pasted image 20230602112753.png|800]]
>
> Vista dal router R7:
> 1. Riesce a vedere tutti i nodi e router all'interno della sua area (area 1).
> 2. Si deve calcolare il costo dei collegamenti indiretti dell'area 0 (per via del router R3) e anche dell'area 2 (per via del router R4 contenuto nell'area 0).
> ![[Pasted image 20230602113007.png|800]]
>
> Vista dal router R10:
> 1. Riesce a vedere tutti i nodi e router all'interno della sua area (area 2).
> 2. Si deve calcolare il costo dei collegamenti indiretti dell'area 0 (per via del router R4) e anche dell'area 1 (per via del router R3 contenuto nell'area 0).
>![[Pasted image 20230602113136.png|800]]


%%
[[000 Indice RC|↖ Ritorna all'indice ↖]]
%%