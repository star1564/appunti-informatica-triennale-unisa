---
aliases: 
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: NP-completezza
cssclasses:
  - 
---
![[063 NP-Completezza#^68fbfb]]

## Teorema $VERTEX\text{-}COVER$
 >[[065 VERTEX-COVER|Vertex Cover]] è NP-completo

Abbiamo [[065 VERTEX-COVER#^f64b0b|provato]] che $VERTEX\text{-}COVER$ appartiene alla classe [[058 La Classe NP|NP]]. Per concludere la prova, basta ora provare la [[062 Riducibilità in tempo polinomiale|riducibilità in tempo polinomiale]] $$3SAT\leq_p VERTEX\text{-}COVER$$

Sia $\phi$ una formula [[064 SAT e 3SAT#3CNF|3CNF]] con $i$ clausole ed $m$ variabili: 
$$(a_1 \lor b_1 \lor c_1) \land (a_2 \lor b_2 \lor c_2) \land \dots \land (a_i \lor b_i \lor c_i )$$

Definiamo un grafo non orientato $G$ e un intero $k$ tale che $\phi$ è soddisfacibile se e solo se $G$ ha un vertex cover di cardinalità $k$. Inoltre $\langle G,k \rangle$ può essere costruita in tempo polinomiale nella lunghezza di $\langle \phi \rangle$.

> [!note] Costruzione
> - $G$ contiene due vertici per ogni variabile $x$ etichettati con $x$ e $\overline{x}$ (*[[064 SAT e 3SAT#Gadgets|gadget]] per le variabili*). Chiamiamo $V_1$ questo insieme di vertici
> - $G$ contiene tre vertici per ogni [[025 Funzioni Booleane (AdE)#Clausola|clausola]], etichettati con i tre [[025 Funzioni Booleane (AdE)|letterali]] della clausola (*gadget per le clausole*). Chiamiamo $V_2$ questo insieme di vertici
> - Connettiamo i due vertici associati a una variabile con un arco
> - Connettiamo i tre vertici associati a una clausola tra loro in un triangolo
> - Connettiamo con un arco ogni vertice nel triangolo (associato a una clausola) al vertice in $V_1$ (gadget per le variabili) che ha la stessa etichetta
> - Se $\phi$ ha **$i$ clausole** ed **$m$ variabili**, allora $G$ ha $2m+3i$ vertici e $V=V_1\cup V_2$, quindi $\color{#CC241D}k=m+2i$
>   
>![[Pasted image 20240606152838.png|600]]

Proviamo che $\phi$ è soddisfacibile se e solo se $G=(V,E)$ ha un vertex cover di cardinalità $k$.

1. Sia $\phi$ soddisfacibile e sia $\tau$ un assegnamento che soddisfa $\phi$. Consideriamo il sottoinsieme $V'$ di $V$ che contiene:
	- Tutti i vertici in $V_1$ (gadget per le variabili) che hanno come etichette i letterali veri in $\tau$
	- Due vertici per ogni triangolo (gadget per una clausola), escludendone uno che ha etichetta uguale a un vertice selezionato al passo precedente (ne esiste almeno uno)

	- Se $\phi$ ha $i$ clausole ed $m$ variabili, allora questo sottoinsieme di $V$ ha taglia $k=m+2i$. 
	- Inoltre, $V'$ è un vertex cover:
		- Tutti gli archi in un triangolo sono coperti dai due vertici selezionati
		- Tutti gli archi tra due vertici di $V_1$ o tra un vertice di $V_1$ e un vertice di $V_2$ sono coperti dalla scelta dei vertici in $V_1$ o $V_2$

2. Supponiamo che $G=(V,E)$ abbia un vertex cover $V'$ di cardinalità $k=m+2i$ e proviamo che $\phi$ è soddisfacibile.
	- $V'$ deve contenere almeno due vertici di ogni triangolo e, per ogni arco tra due vertici di $V_1$ (gadget per le variabili), almeno uno dei due vertici
	- Siccome il numero dei triangoli è $i$ e il numero degli archi tra i vertici in $V_1$ è $m$, l'insieme $V'$ contiene *esattamente* due vertici di ogni triangolo e, per ogni arco tra due vertici di $V_1$, uno dei due vertici


Assegniamo valore vero ai letterali che sono etichette di vertici in $V'\cap V_1$. Proviamo che questo assegnamento $\tau$ soddisfa $\phi$, ovvero che questo assegnamento rende vera ogni clausola:
- Infatti, per ogni triangolo, esiste un vertice $u$ che non è in $V'$. 
- Ma $(u,v)$, con $v\in V_1$, deve essere coperto da $V'$. Quindi $v\in V'$.
- Ma $u$ e $v$ hanno la stessa etichetta (per la costruzione di $G$) che corrisponde ad un letterale a cui è assegnato valore `1` (per la costruzione di $\tau$)

Dunque, per ogni clausola, c'è un letterale a cui $\tau$ assegna valore `1` e quindi $\phi$ è soddisfacibile.



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