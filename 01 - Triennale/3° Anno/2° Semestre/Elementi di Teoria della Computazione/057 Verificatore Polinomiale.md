---
aliases: 
  - Certificato Verificatore Polinomiale (ETC)
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: Verificabilità in Tempo Polinomiale
cssclasses:
  - 
---
## Esempi
Per molti problemi possiamo **evitare la ricerca mediante forza bruta** ([[056 La Classe EXPTIME|EXPTIME-P]]) ed ottenere una soluzione polinomiale ([[055 La Classe P|P]]).
Tuttavia, i tentativi di evitare la forza bruta nel caso di altri problemi, tra cui molti utili ed interessanti, *non hanno avuto successo* e non sono noti algoritmi polinomiali per risolvere tali problemi. Questo per via di principi non ancora noti, oppure questi problemi semplicemente *non possono* essere risolti in tempo polinomiale. 

Una scoperta notevole riguardo a questa domanda dimostra che le complessità di molti problemi *sono legate tra di loro*. Infatti, un algoritmo polinomiale per un certo problema può essere utilizzato per risolvere un'intera classe di problemi. 

Per capire questo concetto, si considerano questi esempi.

### HAMPATH
Diciamo di avere un problema di decisione $HAMPATH$ che deve stabilire se un [[012 Grafi#Grafi Diretti (Orientati)|grafo orientato]] contiene un **[[012 Grafi#^cb9deb|Cammino Hamiltoniano]]** che collega due nodi specificati.

Il linguaggio associato a questo problema è $$HAMPATH = \{\langle G,s,t \rangle\ |\ G\text{ è un grafo orientato e ha un cammino Hamiltoniano da }s \text{ a }t\}$$
![[Pasted image 20240601140750.png]]

Se un grafo $G=(V,E)$ ammette un cammino Hamiltoniano da $s$ a $t$, allora esiste una sequenza $c$ di $|V|$ nodi distinti ($u_1,\dots,u_{|V|}$) tale che:
- $(u_{i-1}, u_i)\in E$, per ogni $i$ con $2\le i\le |V|$
- $u_1=s$
- $u_{|V|}=t$

$HAMPATH$ può essere deciso da un algoritmo di complessità esponenziale attraverso un metodo di forza bruta, che consiste nel costruire tutti i cammini in $G$ (di lunghezza uguale a $|V| - 1$). Però, *non è noto se esiste un algoritmo polinomiale* che decida l'appartenenza di una stringa $w$ al linguaggio.

Il linguaggio $HAMPATH$ ha una caratteristica chiamata **verificabilità polinomiale** che è importante per capire la sua complessità.
Anche se non conosciamo un algoritmo polinomiale per determinare se un grafo contiene un cammino Hamiltoniano, se un tale cammino è stato scoperto in qualche modo, *la verifica* che si tratta di un cammino Hamiltoniano può essere fatta *in tempo polinomiale*. 

Quindi, verificare l'esistenza di un cammino Hamiltoniano può essere molto più facile che determinare la sua esistenza.

Infatti, esiste un algoritmo $N$, *polinomiale* in $|\langle G,s,t \rangle|$, tale che, sull'input $\langle\langle G,s,t \rangle, c\rangle$, dove $c=(u_1,\dots,u_{|V|})$, decide il linguaggio $HAMPATH$. Basta verificare che i nodi della sequenza siano distinti.



### COMPOSITES
Un numero è **Composto** se è il prodotto di due interi maggiori di 1, ovvero che è *un intero non è primo*. Consideriamo il problema di stabilire se un numero non è primo. Questo si può formulare come un problema di decisione, a cui corrisponde un linguaggio associato, il linguaggio
$$COMPOSITES=\{\langle x \rangle\ |\ \text{esistono interi } p,q\text{, con }p,q>1\text{ tali che }x=pq\}$$

È possibile *verificare* che un numero è composto o primo in tempo polinomiale. Inoltre è stato dimostrato che $COMPOSITES\in P$.

## Definizione
> Un **Algoritmo di Verifica (verificatore) $V$** per un linguaggio $A$ è un algoritmo tale che 
> $$A=\{w\ |\ \exists c \text{ tale che }V\text{ accetta }\langle w,c \rangle\}$$
> Dove la stringa $c$ prende il nome di **certificato** (o prova). Quindi $A$ è il *linguaggio verificato* da $V$.

![[Pasted image 20240601144556.png|650]]

In pratica, $V$ è un algoritmo di verifica per un linguaggio $A$ se $V$ verifica le seguenti due proprietà:
1. $V$ è un algoritmo in $|w|$ con "due argomenti" $\langle w,c \rangle$
2. Per ogni stringa $w$, si ha che $w\in A$ se e solo se esiste $c$ tale che $V$ accetta $\langle w,c \rangle$. La stringa $c$ è usata per valutare che una stringa $w$ sia un elemento di $A$, è quindi una "soluzione" del problema.


> [!quote] Verificatore Polinomiale
> Un algoritmo $V$ è un verificatore per $A$ in **tempo polinomiale** se:
> 1. $A$ è il linguaggio verificato da $V$, ovvero $$A=\{w\ |\ \exists c \text{ tale che }V\text{ accetta }\langle w,c \rangle\}$$
> 2. $V$ ha la complessità di tempo polinomiale in $|w|$.
>
> Inoltre, il certificato $c$ deve avere *lunghezza polinomiale*, cioè che esiste $t$, tale che per ogni $w$, $\color{#61AFEF}|c|=O(|w|^t)$.

> [!example]- <font color="orange">Esempi</font>
> - Per $HAMPATH$ un certificato è la stringa $\langle G,s,t \rangle\in HAMPATH$, un cammino Hamiltoniano da $s$ a $t$.
> - Per $COMPOSITES$ un certificato è la stringa $\langle x \rangle\in COMPOSITES$, uno dei divisori di $x$.


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