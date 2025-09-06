---
aliases: 
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: NP-completezza
cssclasses:
  - 
---
![[063 NP-Completezza#^68fbfb]]

## Teorema $SUBSET\text{-}SUM$
>[[058 La Classe NP#SUBSET-SUM|SUBSET-SUM]] è NP-completo.

Abbiamo già [[058 La Classe NP#^481d51|provato]] che $SUBSET\text{-}SUM\in NP$. Per concludere la prova, basta ora provare la [[062 Riducibilità in tempo polinomiale|riducibilità in tempo polinomiale]] $$3SAT\leq_p SUBSET\text{-}SUM$$

Sia $\phi$ una formula $3CNF$ con $l$ variabili $x_1,\dots, x_l$ e $k$ clausole $c_1,\dots,c_k$.

Associamo a $\phi$ un insieme $S$ di numeri e un numero $t$ tali che $\phi$ è soddisfacibile se e solo se $\langle S,t \rangle\in SUBSET\text{-}SUM$. I numeri in $S$ ed il numero $t$ sono espressi nella notazione decimale. Inoltre $\langle S,t \rangle$ può essere costruita in tempo polinomiale nella lunghezza di $\langle \phi \rangle$.


> [!tip]+ Tabella
>Gli elementi di $S$ ed il numero $t$ rappresentano le righe di una **tabella**: 
>- Le righe al di sopra della linea doppia sono etichettate 
>$$y_1,z_1,y_2,z_2,\dots,y_l, z_l\quad\quad g_1,h_1,g_2,h_2,\dots,g_k,h_k$$
>- La riga sotto la linea doppia è $t$
>
>![[Pasted image 20240606155159.png|400]]
>
>Le cifre non presenti sono `0`.
>
>Pertanto, $S$ contiene una coppia di numeri $y_i,z_i$, per ciascuna variabile $x_i$ in $\phi$. Come indicato nella tabella, la rappresentazione decimale di questi numeri è in due parti:
>- La parte sinistra comprende un `1` seguito da $l-i$ cifre `0`, sia per $y_i$ che per $z_i$
>- La parte destra contiene una cifra per ciascuna clausola, dove: 
>	- La cifra di $y_i$ in colonna $c_j$ è `1` se la clausola $c_j$ contiene il letterale $\color{#61AFEF}x_i$
>	- La cifra di $z_i$ in colonna $c_j$ è `1` se la clausola $c_j$ contiene il letterale $\color{#61AFEF}\overline{x_i}$
>
>Inoltre, $S$ contiene una coppia di numeri $g_j,h_j$, per ciascuna clausola $c_j$. Anche qui la tabella si divide in due parti:
>- Nella parte sinistra si hanno solamente degli `0`
>- Nella parte destra, $g_j$ e $h_j$ sono due numeri *uguali* e consistono di un `1` seguito da $k-j$ cifre `0`
>
>$$S=\{y_i,z_i\ |\ 1\le i\le l\}\cup \{g_j,h_j\ |\ 1\le j\le k\}$$
>
>Infine, il **numero obbiettivo** $t$, la riga in basso nella tabella, consiste:
>- Nella parte sinistra di $l$ cifre uguali a `1` 
>- Nella parte destra di $k$ cifre uguali a `3`

> [!example]- <font color="orange">Esempi Tabelle Riduzione 3SAT a SUBSET-SUM</font>
>> [!tip] Basta pensare alla riga $y_i$ per i letterali normali, mentre la riga $z_i$ per i letterali negati
>
>Data la seguente formula booleana, definire $S$ e l'intero $t$ tali che $\langle S,t \rangle$ sia l'immagine di $\langle \phi \rangle$ nella riduzione polinomiale di $3SAT$ a $SUBSET\text{-}SUM$.
>$$\phi=(x_1 \lor x_2 \lor x_3) \land (\overline{x_1} \lor \overline{x_2} \lor x_3) \land (\overline{x_1} \lor x_2 \lor \overline{x_3})$$
>
>![[Pasted image 20240606161841.png]]
>
>$S=\{y_1,z_1,y_2,z_2,y_3,z_3,h_1,g_1,h_2,g_2,h_3,g_3\}$
>$t=111333$
>
>---
>Data la seguente formula booleana, definire $S$ e l'intero $t$ tali che $\langle S,t \rangle$ sia l'immagine di $\langle \phi \rangle$ nella riduzione polinomiale di $3SAT$ a $SUBSET\text{-}SUM$.
>$$\phi=(\overline{x_1} \lor x_2 \lor \overline{x_3}) \land (x_1 \lor x_2 \lor x_3) \land (x_1 \lor \overline{x_2} \lor x_3) \land (\overline{x_1} \lor \overline{x_2}\lor \overline{x_3})$$
>
>![[Pasted image 20240606162214.png]]
>
>$S=\{y_1,z_1,y_2,z_2,y_3,z_3,y_4,z_4,h_1,g_1,h_2,g_2,h_3,g_3,h_4,g_4\}$
>$t=1113333$

Sia $\phi$ soddisfacibile e sia $\tau$ un assegnamento che soddisfa $\phi$. 

Consideriamo il sottoinsieme $S'$ di $S$ che contiene $y_i$ se $\tau$ assegna a $x_i$ valore `1`, $z_i$ altrimenti. Se sommiamo ciò che abbiamo scelto fino ad ora, otteniamo un `1` in ciascuna delle prime $l$ cifre, perché abbiamo selezionato $y_i$ o $z_i$ per ciascun $i$.
Inoltre, ciascuna delle ultime $k$ cifre è un numero da `1` a `3`, perché ciascuna clausola è soddisfatta e quindi contiene da `1` a `3` letterali veri.

Quindi scegliamo un numero sufficiente di $g_j,h_j$ da aggiungere ad $S'$ per portare ciascuna delle ultime $k$ cifre fino a `3` e ottenere 
$$\displaystyle\sum_{s\in S'} s = t$$

Viceversa, supponiamo che esista un sottoinsieme $S'$ di $S$ tale che $\sum_{s\in S'} s = t$. Possiamo notare che:
- Tutte le cifre negli elementi di $S$ sono `0` o `1`
- Ciascuna colonna nella tabella che descrive $S$ contiene al massimo cinque cifre uguali a `1`
- Quindi, sommando elementi di un sottoinsieme di $S$ non si verifica mai un "riporto" nella colonna successiva

Per ogni $i$ con $1\le i\le l$, $S'$ deve contenere $y_i$ o $z_i$ ma non entrambi. Sia $\tau$ l'assegnamento definito come segue. Per ogni $i$ con $1\le i\le l$, assegniamo a $x_i$ 
- Valore `1` se $S'$ contiene $y_i$
- Valore `0` se $S'$ contiene $z_i$

Questo assegnamento $\tau$ soddisfa $\phi$.
Infatti, poiché le ultime $k$ cifre di $t$ sono uguali a 3, in ciascuna delle $k$ colonne finali, la somma è sempre 3.

Per ogni $j$ con $1\le j\le k$, almeno un `1` nella colonna $c_j$ deve venire da qualche $y_i$ o $z_i$ nel sottoinsieme $S'$, perché da $g_j$ ed $h_j$ può venire al massimo `2`.

Per ogni $j$ nella colonna $c_j$ vi deve essere una cifra uguale a `1` corrispondente a un $y_i$ o $z_i$ in $S'$:
- Se è $y_i$, allora $x_i$ è presente in $c_j$ e gli viene assegnato `1`, quindi $c_j$ è soddisfatta
- Se è $z_i$, allora $\overline{x_i}$ è presente in $c_j$ e a $x_i$ gli viene assegnato `0`, quindi $c_j$ è soddisfatta

Pertanto, $\phi$ è soddisfatta.

Infine, la riduzione può essere effettuata in tempo polinomiale.



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