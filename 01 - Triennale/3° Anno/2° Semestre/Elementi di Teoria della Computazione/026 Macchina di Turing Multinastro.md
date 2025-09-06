---
aliases:
    - Funzione di Transizione delle MdT Multinastro
tags:
    - corsi/informatica/elementi_teoria_computazione
paragrafo: Varianti delle Macchine di Turing
cssclasses:
    - 
---

> Una **Macchina di Turing Multinastro** (MdTM) è una [[025 Varianti delle Macchine di Turing|variante della Macchina di Turing deterministica]] in cui si hanno _più nastri contemporaneamente_ accessibili in lettura e scrittura, che vengono aggiornati tramite _più testine_ (una per ogni nastro).

![[Immagine 2024-05-03 104847.png]]


## Definizioni Formali

> [!quote] Definizione Macchina di Turing a _$k$ nastri_
> Dato un numero intero positivo $k$, una **Macchina di Turing con $k$ nastri** è una settupla $(Q,\Sigma, \Gamma,\delta,q_0,q_{accept}, q_{reject})$ dove i suoi elementi sono definiti come in una Macchina di Turing deterministica, tranne per la sua funzione di transizione, che è definita nel seguente modo:
> $$\color{#61AFEF}\delta:(Q-\{q_{accept},q_{reject}\})\times\Gamma^k\to Q\times\Gamma^k\times\{L,R,S\}^k$$
>
> Dove $\Gamma^k$ è il [[012 Prodotto Cartesiano tra Insiemi|prodotto cartesiano]] di $k$ copie di $\Gamma$ e $\{L,R,S\}^k$ è il prodotto cartesiano di $k$ copie di $\{L,R,S\}$.

^06fbdd

> [!quote] Definizione Formale Macchina di Turing _Multinastro_
> Una **Macchina di Turing Multinastro** è una macchina di Turing a $k$ nastri, dove $k$ è un numero intero positivo.

Le nozioni di [[021 Configurazione di una Macchina di Turing|configurazione]], [[021 Configurazione di una Macchina di Turing#passo di computazione|passo di computazione]], di [[024 Macchine Decisionali#Linguaggio Deciso|linguaggio deciso]] e di [[023 Linguaggio Turing-Riconoscibile|linguaggio riconosciuto]] da una Macchina di Turing sono estese in maniera ovvia alle macchine Multinastro.


## Computazione

> Una Macchina di Turing Multinastro $M=(Q,\Sigma, \Gamma,\delta,q_0,q_{accept}, q_{reject})$ inizia la computazione:
>
> -   Partendo dallo _stato iniziale_ $q_0$
> -   Con l'input $w\in\Sigma^*$ posizionato sulla parte più a sinistra _del primo nastro_. Le prime $n$ celle a sinistra, se $n=|w|$ è la lunghezza dell'input (il resto delle celle conterrà il carattere $\sqcup$)
> -   Gli _altri nastri_ conterranno solo il carattere $\sqcup$
> -   Tutte le testine saranno posizionate _sulle prime celle dei rispettivi nastri_
> -   In particolare, la _testina del primo nastro_ sarà posizionata sulla cella contenente _il primo simbolo di input_ (quello più a sinistra) se tale input è diverso dalla stringa vuota $\epsilon$

![[Pasted image 20240503114558.png|500]]

### Funzione di transizione

> Il [[015 Applicazioni#Dominio e Codominio di una Applicazione|Codominio]] della [[019 Funzione di Transizione delle Macchine di Turing|funzione di transizione]] di una Macchina di Turing a $k$ nastri è un insieme di [[026 Sequenza|sequenze]] $$(\textcolor{#e86162}{q_j},\textcolor{#00C575}{b_1,b_2,\dots,b_k},\textcolor{#FF6611}{d_1,d_2,d_k})$$di lunghezza dispari $(2k+1)$ dove:
>
> -   $\textcolor{#e86162}{q_j}\in Q$
> -   $\textcolor{#00C575}{b_t}\in\Gamma$, per $t\in\{1,\dots,k\}$
> -   $\textcolor{#FF6611}{d_t}\in\{L,R,S\}$, per $t\in\{1,\dots,k\}$

Se abbiamo la seguente situazione $$\delta(\textcolor{#53ccc5}{q_i},a_1,a_2,\dots,a_k)=(\textcolor{#e86162}{q_j},\textcolor{#00C575}{b_1,b_2,\dots,b_k},\textcolor{#FF6611}{d_1,d_2,d_k})$$allora, $M$ si trova nello stato $\textcolor{#53ccc5}{q_i}$ con le $k$ testine (una per nastro) posizionate su celle contenenti i caratteri $a_1,\dots,a_k$ rispettivamente.

Alla fine della transizione:

-   $M$ si troverà nello stato $\textcolor{#e86162}{q_j}$
-   $\textcolor{#00C575}{b_t}\in\Gamma$ sarà il simbolo scritto sulla cella del $t$-esimo nastro su cui la testina si trovava all'inizio della transizione (contenente $a_t$), per $t\in\{1,\dots,k\}$
-   La testina sul $t$-esimo nastro si troverà (per ogni $t\in\{1,\dots,k\}$):
    -   sulla stessa cella su cui si trovava all'inizio della computazione se $\textcolor{#FF6611}{d_t}=S$
    -   sulla cella di sinistra (se tale cella esiste e) se $\textcolor{#FF6611}{d_t}=L$
    -   sulla cella di destra se $\textcolor{#FF6611}{d_t}=R$

### Configurazioni

> Se $M=(Q,\Sigma,\Gamma,\delta,q_0,q_{accept}, q_{reject})$ è una Macchina di Turing con $k$ nastri, una sua [[021 Configurazione di una Macchina di Turing|configurazione]] ha la seguente forma $$(\textcolor{#e86162}{u_1}\textcolor{#00C575}q\textcolor{#fdaa39}{v_1}, \textcolor{#e86162}{u_2}\textcolor{#00C575}q\textcolor{#fdaa39}{v_2}, \dots, \textcolor{#e86162}{u_k}\textcolor{#00C575}q\textcolor{#fdaa39}{v_k})$$

Dove:

-   $\textcolor{#00C575}q\in Q$ è lo _stato corrente_ di $M$
-   $\textcolor{#e86162}{u_t}\textcolor{#00C575}q\textcolor{#fdaa39}{v_t}\in\textcolor{#e86162}{\Gamma^*}· \textcolor{#00C575}Q· \textcolor{#fdaa39}{\Gamma^S}$ per $t\in\{1,\dots, k\}$
    -   $\textcolor{#fdaa39}{\Gamma^S}=\Gamma^*\cdot(\Gamma-\{\sqcup\})\cup\{\sqcup,\epsilon\}$, ovvero che dopo l'ultimo simbolo di $\textcolor{#fdaa39}v$, il nastro contiene solo simboli blank
-   la _testina del $t$-esimo nastro_ è posizionata:
    -   sul primo simbolo di $\textcolor{#fdaa39}{v_t}$ se $v_t\neq\epsilon$
    -   su $\sqcup$ altrimenti
-   $\textcolor{#e86162}{u_t},\textcolor{#fdaa39}{v_t}\in\Gamma^*$ è il _contenuto del $t$-esimo nastro_, con la convenzione di aver eliminato tutti i simboli $\sqcup$ che seguono:
    -   l'ultimo carattere di $\textcolor{#fdaa39}{v_t}$ se $v_t\neq\epsilon$
    -   $\textcolor{#e86162}{u_t}$ altrimenti

#### Tipi di Configurazioni

Se $M=(Q,\Sigma,\Gamma,\delta,q_0,q_{accept}, q_{reject})$ è una Macchina di Turing con $k$ nastri, si avranno i due seguenti tipi di configurazioni:

-   $(q_0,w,\dots,q_0)$ è una [[021 Configurazione di una Macchina di Turing#^f0bd9d|Configurazione Iniziale]] con input $w\in\Sigma^*$
-   $(u_1,q_{accept},v_1,\dots,u_kq_{accept}v_k)$ è una [[021 Configurazione di una Macchina di Turing#^f0bd9d|Configurazione di Accettazione]]
-   $(u_1,q_{reject},v_1,\dots,u_kq_{reject}v_k)$ è una [[021 Configurazione di una Macchina di Turing#^f0bd9d|Configurazione di Rifiuto]]


## Linguaggio Riconosciuto

Il [[023 Linguaggio Turing-Riconoscibile|linguaggio riconosciuto]] da una Macchina di Turing Multinastro $M$ è $$\mathcal{L}(M)=\{w\in\Sigma^*\ |\ \exists u_t,v_t\in\Gamma^*,t\in\{1,\dots,k\}:(q_0w,\dots,q_0)\to^* (u_1q_{accept}v_1,\dots,u_kq_{accept},v_k)\}$$

---

##

> [!example]- <font color="orange">Esempio</font>
> Macchina di Turing a 2 Nastri per $\{0^n1^n\ |\ n>0\}$:
>
> 1.  Scorre il primo nastro verso destra fino al primo $1$:
>     -   Per ogni $0$, scrive $1$ sul secondo nastro
> 2.  Scorre il primo nastro verso destra e il secondo nastro verso sinistra:
>     -   Se i simboli letti non sono uguali, termina in $q_{reject}$
> 3.  Se legge $\sqcup$ su entrambi i nastri, termina in $q_{accept}$
>
> | Stato attuale | Simboli letti (nastro 1, nastro 2) | Valore funzione di transizione      |
> | ------------- | ---------------------------------- | ----------------------------------- |
> | $q_0$         | $(0,\sqcup)$                       | $q_0, (\sqcup,1),(R,R)$             |
> | $q_0$         | $(1,\sqcup)$                       | $q_1, (1,\sqcup),(S,L)$             |
> | $q_1$         | $(1,1)$                            | $q_0, (\sqcup,\sqcup),(R,L)$        |
> | $q_1$         | $(\sqcup,\sqcup)$                  | $q_{accept}, (\sqcup,\sqcup),(S,S)$ |
>
> Se $\delta(q,\gamma,\gamma')$ non è presente nella tabella, con $q\in Q-\{q_{accept},q_{reject}\}$, allora $\delta(q,\gamma,\gamma')=(q_{reject},\gamma,\gamma',S,S)$.
>
> ![[Pasted image 20240503124917.png|500]]
> Transizioni omesse vanno in $q_{reject}$ (senza cambiare i simboli letti o spostare la testina).

---

[[000 Indice ETC|↖ Ritorna all'indice ↖]]

```dataviewjs
/*
function extractUpperCaseLetters(inputString) {
	// Use a regular expression to match uppercase letters (A-Z)
	const uppercaseLetters = inputString.match(/[A-Z]/g);

	// Check if uppercaseLetters is not null, and join the matched letters into a string
	if (uppercaseLetters !== null) {
		return uppercaseLetters.join('');
	} else {
	    // If no uppercase letters were found, return an empty string
	    return '';
	}
}
*/

function extractNumberFromString(inputString) {
	const match = inputString.match(/^\d{3}/);

	if (match) {
		return match[0];
	} else {
		return null; // Return null if no match is found
	}
}

function startsWithNumber(inputString, targetNumber) {
  // Use a regular expression to check if the string starts with the target number
  const pattern = new RegExp(`^${targetNumber}`);
  return pattern.test(inputString);
}

function sort2Array(array){
	let res = []

	if (array[0] == undefined)
		res.push(array[1], array[1])
	else if (array[1] == undefined)
		res.push(array[0], array[0])
	else if (parseInt(extractNumberFromString(array[0])) > parseInt(extractNumberFromString(array[1])))
		res.push(array[1], array[0])
	else
		res.push(array[0], array[1])

	return res
}

let toDisplay = []
function searchPrevAndNext(tag, currentNumber) {
	const prevNumber = (parseInt(currentNumber) - 1).toString().padStart(3, "0");
	const nextNumber = (parseInt(currentNumber) + 1).toString().padStart(3, "0");

	let prevAndNext = [(dv.pages(tag).where(p => startsWithNumber(p.file.name, prevNumber) || startsWithNumber(p.file.name, nextNumber)).file.name)]

	// Prev = [0]; Next = [1]
	let sortedPrevAndNext = sort2Array(prevAndNext[0])

	if (currentNumber == "001"){
		// Se è la prima pagina aggiungi solo il prossimo
		let nextLink = "[[" + sortedPrevAndNext[1] + "|" + "Nota Successiva →" + "]]"
		toDisplay.push(nextLink)
	} else if (prevAndNext[0][1] == undefined){
		// Se è l'ultima pagina aggiungi solo il precedente
		let prevLink = "[[" + sortedPrevAndNext[0] + "|" + "← Nota Precedente" + "]]"
		toDisplay.push(prevLink)
	} else {
		let nextLink = "[[" + sortedPrevAndNext[1] + "|" + "Nota Successiva →" + "]]"
		toDisplay.push(nextLink)

		let prevLink = "[[" + sortedPrevAndNext[0] + "|" + "← Nota Precedente" + "]]"
		toDisplay.push(prevLink)
	}


}

if (dv.current().tags[0] == null || dv.current().tags[0] == undefined){
	dv.header(1, "Errore - Inserire il tag nelle proprietà del file")
} else {
	let tag = "#" + dv.current().tags[0]

	// Purtroppo obsidian non riesce a leggere i link e a creare connessioni nel grafo,
	// quindi ho disattivato il link all'indice automatico e la funzione extractUpperCaseLetters
	//let indexName = "000 Indice " + extractUpperCaseLetters(tag)
	//let index = dv.page(indexName).file
	//let indexLink = "[[" + index.name + "|" + "↖ Ritorna all'indice ↖" + "]]"
	//toDisplay.push(indexLink)

	let currentPage = dv.current().file
	let currentPageNumber = extractNumberFromString(currentPage.name)

	searchPrevAndNext(tag, currentPageNumber)

	dv.list(toDisplay)
}
```

Altri collegamenti:
