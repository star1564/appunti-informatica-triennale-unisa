---
tags:
  - corsi/informatica/reti_calcolatori
---
> [!summary] Uso principale
>Serve per approssimare un [[006 Livello Fisico#^87f1d5|segnale digitale]] (la precisione aumenta all'aumentare di $k$). 
>In <font color="#CC241D">rosso, il segnale digitale</font> in <font color="#61AFEF">blu, il segnale creato con la serie di Fourier (con k=5)</font>.
>![[Immagine 2023-03-16 111216.png]]

> [!quote] Serie di Fourier
>Qualsiasi [[014 Funzione Periodica|Funzione Periodica]] sufficientemente regolare $y(t)$ può essere ottenuta *sommando un numero (anche infinito) di funzioni sinusoidali* ([[FT1 Funzione Seno|seno]] e [[FT2 Funzione Coseno|coseno]]).
>$$y(t)=a_0+\sum^{\infty}_{k=1}(a_k\ cos(k)\cdot t + b_k\ sin(k)\cdot t) $$
>Dove i parametri $a_0, a_k, b_k$ sono detti *coefficienti di Fourier*, che dipendono dai valori della funzione $y(t)$ nell'intervallo di tempo $\color{#61AFEF}T=2\pi$.

Le *Frequenze* $f_n$ sono multiple intere della frequenza fondamentale $f_0$ della funzione data: $$f_0=\frac{1}{T}\quad\quad f_n=n\cdot f_0$$ ^c10a2f

I coefficienti di Fourier sono determinati dalle seguenti formule (con [[066 Integrali|integrali]]): $$a_0=\frac{1}{T}\int_0^T y(t)\ dx\quad \quad a_k=\frac{2}{T}\int_0^T y(t)\cdot cos(k\cdot x)\ dx\quad\quad b_k=\frac{2}{T}\int_0^T y(t)\cdot sen(k\cdot x)\  dx$$ per ogni $k=1,\dots, n$.

Con un'opportuna configurazione dei parametri, la serie rappresenta le funzioni che hanno la caratteristica di essere oscillanti e periodiche.



%%
[[000 Indice RC|↖ Ritorna all'indice ↖]]
%%