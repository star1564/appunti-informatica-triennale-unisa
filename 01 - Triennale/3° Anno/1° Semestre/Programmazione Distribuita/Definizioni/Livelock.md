---
tags:
  - corsi/informatica/programmazione_distribuita
---
Un thread spesso *agisce in risposta all'azione di un altro thread*. Se anche l'azione dell'altro thread è una risposta all'azione di un altro thread, potrebbe verificarsi un livelock.

>Come nel caso del [[Deadlock]], i thread **Livelock** non sono in grado di compiere ulteriori progressi. Tuttavia, i thread non sono bloccati, sono semplicemente *troppo occupati a rispondersi a vicenda per riprendere il lavoro*. 

> [!example] <font color="orange">Esempio</font>
>Ciò è paragonabile a due persone che tentano di sorpassarsi in un corridoio: Alphonse si sposta alla sua sinistra per lasciar passare Gaston, mentre Gaston si sposta alla sua destra per lasciar passare Alphonse. Vedendo che si stanno ancora bloccando, Alphone si sposta alla sua destra, mentre Gaston si sposta alla sua sinistra.

%%
[[000 Indice PD|↖ Ritorna all'indice ↖]]
%%