---
tags:
  - corsi/informatica/reti_calcolatori
paragrafo: Teoria delle Code
cssclasses:
  - 
---
La [[101 Teoria delle Code|teoria delle code]] mira a studiare il sistema di servizio quando ha raggiunto una situazione di regime e ciò avviene quando il sistema è stato in funzione *per un tempo sufficientemente grande*.

Quando lo stato del sistema sarà *fortemente influenzato* dallo stato iniziale e dal tempo che è trascorso dall'attivazione, si dice che è in *condizioni transitorie*.

> Trascorso un tempo sufficientemente grande, il sistema *diviene indipendente dallo stato iniziale* e si dice che il sistema ha raggiunto **condizioni stazionarie** o di equilibrio (steady–state).
> 
> La teoria delle code analizza principalmente sistemi in condizioni di stazionarietà.

Per effettuare l'analisi di un sistema in condizioni di stazionarietà si fa uso delle seguenti quantità fondamentali:
- $\color{#61AFEF}p_k$ indica la *probabilità che $k$ utenti siano presenti nel sistema*. Questa può dipendere dal tempo e quindi la si indica come $p_k(t)$. La maggior parte dei sistemi a coda di interesse raggiungono una situazione di equilibrio, indipendentemente dalla stato iniziale: $$\lim\limits_{t \to \infty}p_k(t)=p_k,\ \ \ k=0,1,\dots$$[^1]

- $\color{#61AFEF}N$ indica il *numero medio degli utenti nel sistema*: $$E(n(t)) = \sum_{k=0}^{\infty} (k-s)\cdot p_k(t)\ \ \Rightarrow\ \ \color{#61AFEF}N=\lim\limits_{t \to \infty}E(n(t))=\sum_{k=0}^{\infty} (k-s)\cdot p_k$$[^2]

- $\color{#61AFEF}N^q$ indica il *numero medio degli utenti nella coda*: $$E(n^q(t)) = \sum_{k=0}^{\infty} (k-s)\cdot p_k(t)\ \ \Rightarrow\ \ \color{#61AFEF}N^q=\lim\limits_{t \to \infty}E(n^q(t))=\sum_{k=0}^{\infty} (k-s)\cdot p_k$$[^3]

- $\color{#61AFEF}T$ indica il *tempo medio passato da un utente nel sistema*: $$T=\lim\limits_{t \to \infty}E(t_i^w)$$[^4]
- $\color{#61AFEF}T^q$ indica il *tempo medio passato da un utente nella coda*: $$T=\lim\limits_{t \to \infty}E(t_i^q)$$[^5]

> [!quote] Teorema di Little
> In un sistema a coda in condizioni stazionarie, vale che $$N=\lambda\cdot T$$[^6]
> Ovvero che il numero medio di clienti in un sistema p pari al tempo medio di permanenza nel sistema per il tasso medio di arrivo.
> Inoltre, vale che $$N^q=\lambda \cdot T^q$$ e che $$T=T\cdot q+\frac{1}{\mu}$$[^7]


___
[[000 Indice RC|↖ Ritorna all'indice ↖]] , [[102 Notazione di Kendall|← Nota Precedente]] 

[^1]: [[037 Limiti di Funzioni|Limiti (Analisi)]]
[^2]: [[102 Notazione di Kendall#^e49ae5|n(t), t]]
[^3]: [[102 Notazione di Kendall#^f68ae8|nq]]
[^4]: [[101 Teoria delle Code#^8c74b4|tw i]]
[^5]: [[101 Teoria delle Code#^1ceae4|tq i]]
[^6]: [[102 Notazione di Kendall#^2bdac8|lambda]]
[^7]: [[102 Notazione di Kendall#^e02df2|mu]]


Altri collegamenti: 