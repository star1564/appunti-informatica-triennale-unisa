---
aliases:
  - Accordo sulla Chiave
tags:
  - corsi/informatica/sicurezza
paragrafo: Algoritmi di Crittografia
cssclasses:
  - 
---
>Il **Protocollo Diffie-Hellman** è un [[Protocollo]] che permette a due entità di scambiare una [[004 Cifrari Simmetrici|chiave simmetrica]] tra di loro (**Accordo sulla Chiave**) comunicando *su un canale pubblico*, senza rivelare a nessun attaccante quella chiave.

Essenzialmente, si vuole creare una *nuova chiave simmetrica segreta* a partire da *due chiavi private* e da un *generatore*. 

Questo avviene attraverso l'operazione di **esponenziazione modulare**, ovvero $$x^y\text{ mod } n$$ ^82060c


### Algoritmo
Ci sono tre sezioni nella comunicazione:
- La sezione privata della prima entità
- La sezione privata della seconda entità
- La sezione pubblica e non sicura tra le due entità

L'algoritmo segue questi passaggi:
1. Nella sezione pubblica della comunicazione, le due entità che vogliono avere una chiave simmetrica unica, *stabiliscono in comune* due numeri: ^cbbaa5
	- $n$ è un numero *primo molto grande* (solitamente tra i 2000 e i 4000 bit).
	- $g$ è il **Generatore** di $Z_n^*$, un numero *primo* e solitamente *piccolo*. Un numero $g$ è generatore di $Z_n^*$ se  $$\{g^i\ |\ 1\le i\le n-1\} = Z_n^*$$![[Pasted image 20240614140558.png|500]]Viene scelto un numero che *produce più resti diversi possibili*. 

> [!question]- $g=1$ *non è mai* una buona idea

2. Le due entità creano le loro *[[005 Cifrari Asimmetrici|chiavi private]]* $a$ e $b$. Nessuno, a parte i loro proprietari, conoscono queste chiavi. 
	- $a$ e $b$ devono essere tra $1$ ed $n$.
3. Le due entità calcolano ed *espongono in pubblico*:
	- $\textcolor{#ef9a1a}{ag} = g^a\text{ mod }n$, per la prima entità
	- $\textcolor{#409e3d}{bg}= g^b\text{ mod }n$, per la seconda entità

![[SecretKeyExchangeComputerphile-Parte1.gif]]

4. Le due entità si scambiano i risultati $\textcolor{#ef9a1a}{ag}$ e $\textcolor{#409e3d}{bg}$.
5. Le due entità calcolano *in privato* la chiave finale, che *sarà uguale per tutte e due le entità* $$(\textcolor{#ef9a1a}{ag})^a\text{ mod }n = \textcolor{#c6552f}{abg} =(\textcolor{#409e3d}{bg})^b\text{ mod }n$$

![[SecretKeyExchangeComputerphile-Parte2.gif]]

### Sicurezza
La sicurezza di molte tecniche crittografiche si basa sulla *intrattabilità del logaritmo discreto*. 

Un esempio è proprio l'esponenziazione modulare. Questo anche grazie al fatto che i numeri scelti sono primi.
L'unico modo per un attaccante di conoscere l'esponente (la chiave privata) è quello di effettuare un attacco di [[003 Crittografia#^9a60da|Brute Force]], il che richiede un tempo altissimo.

> [!example]- <font color="orange">Esempio</font>
>Calcolare il logaritmo discreto di 4 in base 3 modulo 11.
>
>$$3^x\equiv 4 (\text{mod 11})$$
>
>Iteriamo su $x$:
>- $3^1\equiv 3 (\text{mod 11})$
>- $3^2\equiv 9 (\text{mod 11})$
>- $3^3\equiv 5 (\text{mod 11})$
>- $\color{#CC241D}3^4\equiv 4 (\text{mod 11})$
>
>Quindi, il logaritmo discreto di 4 in base 3 modulo 11 è 4​.

La lunghezza del modulo per lo scambio Diffie-Hellman ad oggi deve essere almeno *2048 bit*.

> [!error]+ Man-in-the-middle
>Questo protocollo non è sicuro conto attacchi di Man-in-the-middle. 
>Infatti, Diffie-Hellman *non può assicurare chi sia* l'altra entità con cui si sta comunicando.
>
>Una soluzione sarebbe quella di autenticare entrambe le entità nello scambio attraverso delle [[022 Firme Digitali|firme digitali]].

#### Forward Secrecy
È una proprietà di protocolli per accordo su chiavi, dove viene compromessa la chiave privata ad ogni comunicazione, ma le chiavi di sessione non vengono compromesse. Cioè, *non permette la decifrazione dei messaggi precedenti*.

> [!question] La sicurezza dei messaggi cifrati passati non dipende dalla compromissione futura della chiave privata

Questo viene realizzato attraverso il Diffie-Hellman autenticato in cui la chiave privata cambia sempre.

### Diffie-Hellman per chiavi asimmetriche
Questo protocollo può essere usato per generare due chiavi asimmetriche. Infatti, viene utilizzato spesso insieme all'[[017 RSA|RSA]].

Invece di utilizzare il valore comune ottenuto dal protocollo, lo si usa come *seed* per la generazione delle chiavi asimmetriche. 

___
[[000 Indice Sicurezza|↖ Ritorna all'indice ↖]]

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
- [Computerphile](https://www.youtube.com/watch?v=NmM9HA2MQGI) oppure [Spanning Tree](https://www.youtube.com/watch?v=85oMrKd8afY)
- [Computerphile - con la matematica, modulo](https://www.youtube.com/watch?v=Yjrfm_oRO0wI)