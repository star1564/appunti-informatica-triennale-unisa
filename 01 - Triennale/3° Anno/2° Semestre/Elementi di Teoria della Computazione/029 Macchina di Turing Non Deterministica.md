---
aliases:
  - Funzione di transizione di una MdT Non Deterministica
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: Varianti delle Macchine di Turing
cssclasses:
  - 
---
>Una **Macchina di Turing Non Deterministica** è una [[025 Varianti delle Macchine di Turing|variante della Macchina di Turing deterministica]] in cui una [[021 Configurazione di una Macchina di Turing|configurazione]] *può evolvere in più di una configurazione successiva* in modo non deterministico.

## Definizione

> [!quote] Definizione Formale
> Una Macchina di Turing **Non Deterministica** è una settupla $(Q,\Sigma, \Gamma,\delta,q_0,q_{accept}, q_{reject})$ dove i suoi elementi sono definiti come in una Macchina di Turing deterministica, tranne per la sua funzione di transizione, che è definita nel seguente modo:
> $$\color{#61AFEF}\delta:(Q-\{q_{accept},q_{reject}\})\times\Gamma\to\mathcal{P}(Q\times\Gamma\times\{L,R\})$$
> 
> Quindi, per ogni $q\in Q-\{q_{accept},q_{reject}\}$ e per ogni $\gamma\in\Gamma$, risulta che $$\delta(q,\gamma)=\{(q_1,\gamma_1,d_1),\dots,(q_k,\gamma_k,d_k)\}$$
> con $k\ge 0$ e $(q_j,\gamma_j,d_j)\in Q\times\Gamma\times\{L,R\}$, per $j\in\{1,\dots,k\}$.

Ovvero che per ogni transizione che parte da uno stato non terminate con un qualsiasi carattere del nastro, la macchina può procedere effettuando $k$ scelte (la computazione è *completamente determinata* da una sequenza di scelte).


> [!example]- <font color="orange">Esempi</font>
>- $\Sigma=\{a,b\}$
>- $\Gamma=\{a,b,\sqcup\}$
>
>---
>
>![[Pasted image 20240506122446.png|400]]
>
>$$\delta(q_0,b)=\{(q_0,b,R), (q_1,b,R), (q_{reject},b,R)\}$$
>
>---
>
>![[Pasted image 20240506122632.png|200]]
>
>$$\delta(q_0,b)=\{(q_0,b,R), (q_1,b,R)\}$$
>$$\delta(q_1,a)=\{(q_1,b,R), (q_2,b,R)\}$$


## Computazione
Le nozioni di [[021 Configurazione di una Macchina di Turing|configurazione]], [[021 Configurazione di una Macchina di Turing#passo di computazione|passo di computazione]] e di [[021 Configurazione di una Macchina di Turing#Computazione su un input|computazione]] sono estese in maniera ovvia alle macchine Non Deterministiche.
Poiché la Macchina di Turing non è deterministica, ci possono essere *più configurazioni* $u'q'v'$ che sono prodotte da $uqv$ in un *singolo passo*.

>Come per le Macchina di Turing Deterministiche, si parte dalla *configurazione iniziale* $q_0\ w$, e la computazione *può terminare* (se si raggiunge una configurazione di arresto) oppure *non terminare*.

### Albero delle computazioni
In una macchina di Turing non deterministica, *l'insieme delle computazioni su una stringa input $w$* possono essere organizzate in un **albero delle computazioni** in cui:
- La *radice* è la configurazione [[021 Configurazione di una Macchina di Turing#^f0bd9d|iniziale]]
- I *nodi* sono configurazioni, i cui *figli* rappresentano le possibili configurazioni raggiungibili da quel nodo
- Le *foglie* sono le configurazioni di [[021 Configurazione di una Macchina di Turing#^d7a838|arresto]] contenenti $q_{accept}$ o $q_{reject}$


> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20240506124759.png|700]]


> [!example]- <font color="orange">Esempio di una computazione che non termina</font>
>![[Pasted image 20240506125216.png|450]]
>
>L'albero delle computazioni su `bab` non ha un numero finito di nodi.
>
>![[Pasted image 20240506125228.png|450]]

^4a5ce2



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