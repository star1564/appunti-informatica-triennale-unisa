---
aliases: 
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: Complessità di Tempo
cssclasses:
  - 
---
>Possiamo definire in generale che la [[051 Complessità di Tempo|complessità di tempo]] di una Macchina di Turing *dipende anche dal modello utilizzato* (singolo nastro, [[026 Macchina di Turing Multinastro|multinastro]], [[029 Macchina di Turing Non Deterministica|non deterministica]]).

> [!faq]- La complessità di tempo dipende dal modello di calcolo?
>*Si*
>$$L=\{0^k1^k\ |\ k\ge 0\}$$
>Esiste una Macchina di Turing con un nastro che decide $L$ in tempo $O(n\log n)$, ma non dovrebbe esistere una che lo decide in tempo $O(n)$.
>Esiste però una Macchina di Turing con due nastri che decide $L$ in tempo $O(n)$.
>
>> [!note] Descrizione di $M_3$
>> Su input $w$:
>> 1. Scorre il primo nastro verso destra e rifiuta se trova uno $0$ a destra di un $1$
>> 2. Scorre il primo nastro verso destra fino al primo $1$: per ogni $0$ scrive un $1$ sul secondo nastro.
>> 3. Scorre il primo nastro verso destra leggendo i simboli $1$ e scorre il secondo nastro verso sinistra. Per ogni $1$ letto sui due nastri, li cancella. Se i simboli letti non sono uguali, rifiuta.
>> 4. Se legge $\sqcup$ su entrambe i nastri, accetta.
>
>Ciascuna delle quattro fasi utilizza $O(n)$ passi, quindi il tempo di esecuzione è $TIME(n)$.

Questo evidenzia una importante differenza tra la teoria della computabilità e la teoria della complessità:
- Nella teoria della **computabilità** si ha la [[Tesi Church-Turing]], dove *tutti i modelli sono equivalenti*
- Nella teoria della **complessità**, la scelta del modello *influisce sulla complessità di tempo*

Tuttavia, queste varianti (*tranne quella non deterministica*) sono in grado di simulare l'una il comportamento dell'altra con un sovraccarico computazionale polinomiale, ovvero che questa simulazione può essere eseguita in un tempo che è una *funzione polinomiale del tempo originale*. 

> [!example]- <font color="orange">Esempio</font>
>Se una macchina di Turing multinastro risolve un problema in tempo $TIME(n)$, una macchina di Turing a nastro singolo può simularla in tempo $P(TIME(n))$, dove $P$ è un polinomio.


> [!question]- La complessità di tempo dipende dalla codifica utilizzata?
> *Si*
> Consideriamo ad esempio la [[044 Funzioni Calcolabili|funzione calcolabile]] $f$ definita da $$f(\langle m \rangle) = \langle m \rangle\#1^m$$
> Dove:
> - $m\in\mathbb{N}$
> - $\langle m \rangle$ è la rappresentazione in base $b\geq 1$ di $m$, ovvero la codifica che scegliamo per l'input
> - $1^m$ è la rappresentazione in base unaria (base uno) di $m$
>
>Il valore della funzione dipende dalla base che si sceglie per la rappresentazione, ovvero dalla codifica dell'input. A seconda della codifica che si sceglie, il valore della codifica cambia. In particolare cambia la lunghezza del valore della funzione rispetto alla lunghezza dell'input.
>>[!example]- <font color="orange">Esempio</font>
>>Supponiamo che $m=7$:
>>- Base 2: $f(111)=111\#1111111$, l'input ha lunghezza 3 e l'output ha lunghezza 11
>>- Base 10: $f(7)=7\#1111111$, l'input ha lunghezza 1 e l'output ha lunghezza 9
>>- Base 1: $f(1111111)=1111111\#1111111$, l'input ha lunghezza 7 e l'output ha lunghezza 15
>
>La macchina che calcola $f$ è quella che copia la codifica di $m$ in base $b\geq 1$ nella prima parte dell'output e la codifica unaria di $m$ nella seconda parte. Questa macchina avrà complessità di tempo $O(|\langle m^2 \rangle|)$, ovvero [[055 La Classe P|Polinomiale]].


Bisogna quindi considerare delle codifiche "ragionevoli" e **non "prolisse"**, cioè tali che non vi siano istanze la cui rappresentazione sia artificiosamente lunga e che *non richiedono una generazione esponenziale dei dati*. In particolare, occorre scartare la rappresentazione unaria degli interi positivi.

Codifiche "ragionevoli" dei dati sono quelle **polinomialmente correlate**, cioè quelle che *consentono di passare da una di esse a una qualunque altra codifica "ragionevole"* delle istanze dello stesso problema in un [[054 Tempo Polinomiale ed Esponenziale|Tempo Polinomiale]] rispetto alla rappresentazione originale.


## Multinastro

> [!quote] Teorema 7.8
>Sia $t(n)$ una funzione tale che $t(n)\ge n$. Per ogni Macchina di Turing deterministica *multinastro* $M$ con complessità di tempo $t(n)$ esiste una Macchina di Turing a *singolo nastro* $M'$ con complessità di tempo $O(t^2(n))$, *equivalente* ad $M$.
>
>Dove $t^2:\mathbb{N}\to \mathbb{N}$ è la funzione definita da $t^2(n)=(t(n))^2$. 

^4ed120

Il teorema afferma che $M'$ utilizza $O([t(n)]^2)$ passi per simulare $t(n)$ passi di $M$.

Quindi, se $L$ è deciso in [[054 Tempo Polinomiale ed Esponenziale|Tempo Polinomiale]] su una Macchina di Turing multinastro, allora $L$ è deciso in tempo polinomiale su una Macchina di Turing a nastro singolo.

Questo mostra una differenza *al più quadratica o polinomiale* tra le complessità di tempo dei problemi misurati su macchine di Turing deterministiche a *singolo nastro e multinastro*.  ^3386b3

## Non Deterministica
La [[029 Macchina di Turing Non Deterministica|Macchina di Turing non deterministica]] non corrisponde a nessun meccanismo di computazione reale, ma la definizione di tempo di esecuzione di una macchina di Turing non deterministica è utile per caratterizzare un'importante classe di linguaggi.

> [!quote] Tempo di esecuzione di una Macchina di Turing non deterministica
> Sia $N=(Q,\Sigma, \Gamma,\delta,q_0,q_{accept}, q_{reject})$  una Macchina di Turing non deterministica che sia un [[024 Macchine Decisionali|Decisore]], ovvero che tutte le computazioni per ogni input $w$ terminano in una [[021 Configurazione di una Macchina di Turing#^fa0644|Configurazione di Arresto]].
> 
> Il **tempo di esecuzione di $N$** è la funzione $f:\mathbb{N}\to\mathbb{N}$ dove $f(n)$ è il massimo numero di passi eseguiti da $N$ *in ognuna delle computazioni* su ogni input di lunghezza $n\in\mathbb{N}$.
> 
> ![[Pasted image 20240529115354.png|600]]

È quindi il tempo usato dalla computazione corrispondente alla *ramificazione più lunga* dell'[[029 Macchina di Turing Non Deterministica#Albero delle computazioni|albero delle computazioni]] su $w$. Quindi $f(n)$ è la massima altezza degli alberi delle possibili computazioni su $w$.

> [!quote] Teorema 7.11
> Sia $t(n)$ una funzione tale che $t(n)\geq n$. Per ogni Macchina di Turing a nastro singolo *non deterministica* $N$, avente tempo di esecuzione $t(n)$, esiste una Macchina di Turing a nastro singolo *deterministica* e di complessità di tempo $2^{O(t(n))}$, *equivalente* ad $N$.

Questo mostra una differenza *al più esponenziale* tra le complessità di tempo dei problemi misurati su macchine di Turing *deterministiche e non deterministiche*. Questa differenza è considerata "grande". ^76c5ec

___
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