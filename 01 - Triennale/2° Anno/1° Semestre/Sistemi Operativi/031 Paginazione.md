---
aliases: 
tags:
  - corsi/informatica/sistemi_operativi
paragrafo: Memoria
cssclasses:
  - 
---
La **Paginazione** è uno schema di gestione della [[030 Memoria Centrale|Memoria]] che consente allo spazio di [[Indirizzo Fisico|Indirizzamento Fisico]] di un processo di essere *non contiguo*. Questa evita la frammentazione esterna e la necessità di compattazione, due problemi che affliggono l'allocazione di memoria contigua.

Il metodo di base per implementare la paginazione consiste nel suddividere la memoria fisica e logica in blocchi di dimensione fissa. I blocchi della memoria fisica vengono detti **Frame**, mentre quelli della memoria logica vengono detti **Pagine**.
La dimensione dei blocchi *deve essere una potenza di $2$* (per esempio $2^9 = 512$).

È utile impostare una *Tabella delle Pagine* per tradurre gli [[Indirizzo Logico|Indirizzi Logici]] a quelli fisici. Contiene l'indirizzo di base di ciascun frame nella memoria fisica utilizzato da un processo.

![[Pasted image 20221201104137.png|550]]
<center>Modello di paginazione di memoria logica e memoria fisica</center>

Ogni indirizzo generato dalla CPU è diviso in due parti:
- Un **Numero di Pagina ($p$)**, serve come indice per la tabella delle pagine relativa al processo;
- Un **Offset di Pagina ($d$)**, è la posizione nel frame a cui viene fatto riferimento.

Se la dimensione dello spazio degli indirizzi logici è $\color{#61AFEF}2^m$ (quindi $|p|+|d|=m$ bit) e la dimensione di una pagina è di $\color{#61AFEF}2^n$ byte (quindi $|pagina|=n$ bit), allora gli $m-n$ bit [[Bit Più Significativo|più significativi]] indicano il numero di pagina, e gli $n$ bit [[Bit Meno Significativo|meno significativi]] indicano l'offset di pagina.

L'indirizzo logico ha quindi la forma seguente:
![[Pasted image 20221201110725.png|510]]

Quindi, l'indirizzo di base del frame viene combinato con l'offset della pagina per definire l'indirizzo di memoria fisica.

![[IMG_20221201_105950.jpg||650]]
<center>Hardware di paginazione</center>

> [!example]- <font color="orange">Esempio di Paginazione</font>
>![[Pasted image 20221201113722.png|500]]
>Abbiamo una memoria centrale di $32$ byte con pagine di $4$ byte (ovvero $2^2$).
>Quindi $n = 2$, mentre $m = 4$ dato che servono al massimo $4$ bit per rappresentare l'indirizzo nella memoria logica più grande (15).
>
>Se vogliamo trovare l'eqivalente dell'indirizzo 13 ($= \textcolor{#CC241D}{11}\textcolor{#61AFEF}{01}$) dobbiamo fare in questo modo:
>- Prendiamo gli $m-n$ bit più significativi: $\color{#CC241D}11$;
>	- Si convertono in decimale per trovare la pagina: $\textcolor{#CC241D}{11}_2 = \textcolor{#CC241D}{3}_{10}$ (pagina 3);
>	- Si cerca nella tabella delle pagine il frame corrispondente: pagina 3 → frame 2;
>	- Si converte il numero di frame in binario: $\textcolor{#CC241D}{2}_{10} = \textcolor{#CC241D}{10}_{2}$;
>- Prendiamo gli $n$ bit meno significativi (offset): $\color{#61AFEF}01$;
>- Dato che per rappresentare l'indirizzo nella memoria fisica più grande (31) ci vogliono 5 bit, l'indirizzo fisico sarà di 5 bit;
>- Si combinano i risultati: $0\textcolor{#CC241D}{10}\textcolor{#61AFEF}{01} = 9$.

___
%%
Tag Materia: #corsi/informatica/sistemi_operativi 
%%
[[000 Indice SO|↖ Ritorna all'indice ↖]] , [[030 Memoria Centrale|← Nota Precedente]] | [[032 Implementazione Tabella delle Pagine|Nota Successiva →]]

Altri collegamenti: 