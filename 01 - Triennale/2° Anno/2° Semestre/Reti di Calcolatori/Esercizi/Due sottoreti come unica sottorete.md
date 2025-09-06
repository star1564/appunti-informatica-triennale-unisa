---
tags:
  - corsi/informatica/reti_calcolatori
  - esercizi
---
>Date le due sottoreti 137.204.72.0 e 137.204.74.0 con netmask 255.255.255.0 della classe B 137.204.0.0, trovare la netmask che permette di utilizzare le due sottoreti come se fossero un'unica sottorete:
>1. la netmask deve essere scritta sia in binario sia in decimale.
>2. scrivere il nuovo indirizzo di broadcast della sottorete (sia in binario sia in decimale).
>
>Si consideri la possibilità di avere netmask costituite da 1 non contigui.

Consideriamo gli indirizzi delle sottoreti in binario:
$137.204.0100\ 10\textcolor{#CC241D}10.0$
$137.204.0100\ 10\textcolor{#CC241D}00.0$

Notiamo che i due indirizzi differiscono solo per il <font color="#CC241D">settimo bit del terzo byte</font> (a partire da sinistra).

Pertanto, bisognerà settare a $0$ il corrispondente bit nella netmask data:
$255.255.1111\ 11\textcolor{#CC241D}01=255.255.253.0$

Per calcolare il nuovo indirizzo di broadcast, bisogna prima calcolare il nuovo indirizzo di rete.
Per fare questo, basta mettere in AND bit a bit la netmask con un indirizzo qualsiasi della rete:
$137.204.0100\ 1010.0$
$255.255.1111\ 1101.0$
$\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_$
$137.204.0100\ 1000.0=137.204.72.0$

A questo punto si settano ad $1$ i bit del campo host (attenzione: anche il <font color="#CC241D">settimo bit del terzo byte</font>):
$137.204.0100\ 10\textcolor{#00C575}00.\textcolor{#00C575}{0000\ 0000}$
$255.255.1111\ 11\textcolor{#00C575}01.\textcolor{#00C575}{0000\ 0000}$
$\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_$
$137.204.0100\ 10\textcolor{#00C575}10.\textcolor{#00C575}{1111\ 1111}$

Pertanto l'indirizzo di broadcast sarà: $137.204.74.255$

%%
[[000 Indice RC|↖ Ritorna all'indice ↖]]
%%