---
aliases: 
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: NP-completezza
cssclasses:
  - 
---
![[063 NP-Completezza#^68fbfb]]

## Teorema $HAMPATH$
>[[058 La Classe NP#HAMPATH|HAMPATH]] è NP-completo.

Abbiamo già [[058 La Classe NP#^9730bd|provato]] che $HAMPATH\in NP$. Per concludere la prova, basta provare che dimostriamo la [[062 Riducibilità in tempo polinomiale|riducibilità in tempo polinomiale]] $$3SAT\leq_p HAMPATH$$

La riduzione converte [[032 Formule Booleane (ETC)|formule booleane]] di [[064 SAT e 3SAT#3CNF|3CNF]] in [[012 Grafi|grafi]], in cui i [[012 Grafi#^cb9deb|cammini Hamiltoniani]] corrispondono ad assegnamenti che soddisfano la formula.

I grafi contengono [[064 SAT e 3SAT#Gadgets|Gadget]] che simulano variabili e [[025 Funzioni Booleane (AdE)#Clausola|Clausole]]:
- Il gadget di una variabile è una struttura a rombo che *può essere attraversata in due modi*, corrispondenti ai due assegnamenti di verità.
- Il gadget di una clausola è un nodo.

Assicurare che il cammino attraversa ciascun gadget di clausola corrisponde ad assicurare che ciascuna clausola è soddisfatta nell'assegnamento che soddisfa la formula.

Sia $\phi$ una formula 3CNF con $k$ clausole $$(a_1 \lor b_1 \lor c_1) \land (a_2 \lor b_2 \lor c_2) \land \dots \land (a_k \lor b_k \lor c_k )$$
dove ciascun $a,b,c$ è un letterale $x_i$ o $\overline{x_i}$.

Siano $x_1,\dots,x_l$ le $l$ variabili di $\phi$.

Definiamo un grafo orientato $G$, con due vertici $s$ e $t$, tale che $\phi$ è soddisfacibile se e solo se $G$ ha un cammino Hamiltoniano da $s$ in $t$. Inoltre, $\langle G,s,t \rangle$ può essere costruita in tempo polinomiale nella lunghezza di $\langle \phi \rangle$.

Rappresentiamo: 
- Ciascuna variabile $x_i$ con una *struttura* di forma romboidale che contiene una riga orizzontale di nodi.
- Ciascuna clausola di $\phi$ con un *singolo nodo*

![[Pasted image 20240606173338.png]]

Il numero dei vertici sulla linea orizzontale è legato al numero di clausole nella formula.

La *struttura globale* di $G$ mostra quasi tutti gli elementi di $G$ e le loro relazioni. Mancano gli archi che rappresentano la relazione delle variabili con le clausole che lo contengono.

![[Pasted image 20240606173937.png]]

Ciascuna struttura contiene una riga orizzontale di nodi collegati tramite archi orientati in entrambe le direzioni. La riga orizzontale contiene $3k+1$ nodi in aggiunta ai due nodi alle estremità del rombo. Questi nodi sono raggruppati in coppie adiacenti, una per ciascuna clausola, con nodi separatori aggiuntivi tra le coppie.

![[Pasted image 20240606174244.png]]

Se la variabile $x_i$ è presente nella clausola $c_j$, aggiungiamo i due archi seguenti dalla coppia $j$-esima nell'$i$-esimo rombo al $j$-esimo nodo della clausola.

![[Pasted image 20240606174420.png]]

> [!example]- <font color="orange">Esempio</font>
>$$\phi=(\overline{x_1} \lor x_2 \lor \overline{x_3}) \land (x_1 \lor x_2 \lor x_3) \land (x_1 \lor \overline{x_2} \lor x_3) \land (\overline{x_1} \lor \overline{x_2}\lor \overline{x_3})$$
>
>Il grafo $G$ avrà tre rombi, corrispondenti alle variabili $x_1,x_2,x_3$ e quattro nodi $c_1,c_2,c_3,c_4$ associati alle quattro clausole.
>
>Si hanno tre rombi:
>- Quello corrispondente a $x_1$ sarà collegato ai nodi $c_2$ e $c_3$
>- Quello corrispondente a $x_2$ sarà collegato ai nodi $c_1$ e $c_2$
>- Quello corrispondente a $x_3$ sarà collegato ai nodi $c_2$ e $c_3$
>
>Nessun rombo sarà collegato al nodo $c_4$.

Se $\overline{x_i}$ è presente nella clausola $c_j$, aggiungiamo i due archi seguenti dalla coppia $j$-esima nell'$i$-esimo rombo al $j$-esimo nodo di clausola.

![[Pasted image 20240606175320.png]]

Dopo aver aggiunto tutti gli archi corrispondenti a ciascuna occorrenza di $x_i$ o di $\overline{x_i}$ in ciascuna clausola, la costruzione di $G$ è completa.

Per far vedere che questa costruzione funziona, dimostriamo che se $\phi$ è soddisfacibile, allora esiste un cammino Hamiltoniano da $s$ in $t$. Viceversa se esiste un tale cammino in $G$, allora $\phi$ è soddisfacibile.

1. Supponiamo che $\phi$ sia soddisfacibile.

Per mostrare che esiste un cammino Hamiltoniano da $s$ in $t$, in un primo momento ignoriamo i nodi delle clausole. 

Il cammino inizia da $s$, attraversa ciascun rombo in successione e termina in $t$.
Per raggiungere i nodi orizzontali in un rombo, il cammino può procedere a zig-zag da sinistra verso destra oppure il contrario.

L'assegnamento che soddisfa $\phi$ determina in quale senso la riga è attraversata:
- Se a $x_i$ viene assegnato `1`, il cammino procede a zig-zag (da sinistra a destra) attraverso il rombo corrispondente 
- Se a $x_i$ viene assegnato `0`, il cammino procede a zag-zig (da destra a sinistra)

![[Pasted image 20240606180114.png]]


Questo cammino copre tutti i nodi in $G$, eccetto i nodi delle clausole. Per includerli, aggiungiamo una deviazione per ognuno di tali nodi.

Per ciascuna clausola $c_j$, scegliamo uno dei letterali in $c_j$ a cui è assegnato `1`. Supponiamo che sia $x_i$ o $\overline{x_i}$.
- Se selezioniamo $x_i$ nella clausola $c_j$, attraversiamo la riga $i$ corrispondente nella direzione "corretta" per raggiungere il vertice $c_j$ da uno dei vertici della $j$-esima coppia nella riga e tornare nell'altro vertice. Ovvero, possiamo deviare alla $j$-esima coppia nell'$i$-esimo rombo.
	- Questo è possibile perché $x_i$ deve essere `1`, quindi il cammino procede a zig-zag (da sinistra verso destra) attraverso il rombo corrispondente.
	- Quindi, gli archi verso il nodo $c_j$ sono nell'ordine corretto per permettere una deviazione ed un ritorno.

- Se selezioniamo $\overline{x_i}$ nella clausola $c_j$, avremmo potuto deviare alla $j$-esima coppia nell'$i$-esimo rombo, perché $x_i$ deve essere `0`, quindi il cammino procede a zag-zig (da destra verso sinistra) attraverso il rombo corrispondente. 
	- Pertanto, gli archi verso il nodo $c_j$ sono nuovamente nell'ordine corretto per permettere una deviazione ed un ritorno.

Abbiamo così ottenuto il cammino Hamiltoniano.

2. Mostriamo che se un grafo $G$ ha un cammino Hamiltoniano da $s$ in $t$, allora esiste una assegnamento che soddisfa $\phi$.


Se il cammino Hamiltoniano è *normale*, cioè passa attraverso i rombi in ordine, da quello più in alto a quello più in basso, con deviazioni verso i nodi delle clausole come descritte prima, possiamo facilmente definire un assegnamento che soddisfa $\phi$. 

Alla variabile $i$-esima $x_i$:
- Assegniamo valore `1` se la riga orizzontale è attraversata da sinistra a destra, cioè se il cammino procede a zig-zag attraverso il rombo;
- Assegniamo valore `0` se la riga orizzontale è attraversata da destra a sinistra, cioè se il cammino procede a zag-zig attraverso il rombo.

L'assegnamento soddisfa la formula poiché ciascun nodo clausola è presente sul cammino, è connesso a una coppia di una riga e quindi contiene un letterale vero.

Osservando come la deviazione verso il nodo clausola avviene, possiamo stabilire quale dei letterali nella clausola corrispondente è vero.

Tutto ciò che resta da mostrare è che un cammino Hamiltoniano deve essere normale. La normalità può venir meno solo se il cammino entra in una clausola da un rombo e ritorna in un altro come nella figura seguente.

![[Pasted image 20240607133913.png]]

Il cammino va dal nodo $a_1$ verso $c$, ma invece di ritornare nello stesso rombo, ritorna in $b_2$ in un rombo differente. Se questo accade, $a_2$ oppure $a_3$ è un *nodo separatore*
- Se $a_2$ fosse un nodo separatore, gli unici archi entranti in $a_2$ sarebbero quelli da $a_1$ e $a_3$
- Se $a_3$ fosse un nodo separatore, $a_1$ ed $a_2$ sarebbero nella stessa coppia associata alla clausola $c$, quindi gli unici archi entranti in $a_2$ sarebbero quelli da $a_1$, $a_3$ e $c$

In ogni caso il cammino non potrebbe contenere il nodo $a_2$:
- Il cammino non può entrare in $a_2$ da $c$ o $a_1$ perché il cammino va altrove da questi nodi
- Il cammino non può entrare in $a_2$ da $a_3$ perché $a_3$ è l'unico nodo disponibile a cui $a_2$ punta, quindi il cammino deve uscire da $a_2$ passando per $a_3$.

Pertanto, il cammino Hamiltoniano deve essere normale.

Questa riduzione è ovviamente calcolabile in tempo polinomiale e la dimostrazione è completa.

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