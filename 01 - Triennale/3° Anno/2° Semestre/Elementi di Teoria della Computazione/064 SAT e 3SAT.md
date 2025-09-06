---
aliases:
    - 3CNF
    - SAT_CNF
    - Gadget
tags:
    - corsi/informatica/elementi_teoria_computazione
paragrafo: NP-completezza
cssclasses:
    - 
---

## SAT

> Il linguaggio $\color{#CC241D}SAT$ rappresenta il linguaggio del [[001 Logica Proposizionale|Problema di Decisione]] della [[032 Formule Booleane (ETC)#Formule Soddisfacibili|soddisfacibilità di una formula booleana]]. Ovvero che, data una formula booleana $\phi$, questa è soddisfacibile?
> $$SAT=\{\langle \phi \rangle\ |\ \phi \text{ è una formula booleana soddisfacibile}\}$$

> [!quote]+ Teorema SAT in NP
>
> > $SAT\in NP$
>
> Un [[057 Verificatore Polinomiale#Definizione|certificato]] per $\langle \phi \rangle$ sarà un assegnamento $c$ di valori alle variabili di $\phi$.
> Un algoritmo $V$ che verifica $SAT$ in tempo polinomiale nella lunghezza di $\langle \phi \rangle$ sarà il seguente.
>
> > [!note] Descrizione di $M$
> > Sull'input $y$:
> >
> > 1. Verifica se $y=\langle \langle \phi \rangle, c \rangle$, dove $\phi$ è una formula booleana e $c$ è un assegnamento di valori alle variabili di $\phi$. Se non lo è, rifiuta.
> > 2. Sostituisce ogni variabile della formula con il suo corrispondente valore, e quindi valuta l'espressione.
> > 3. Accetta se $\phi$ assume valore `1`, altrimenti rifiuta.
> >
> > $$\exists c: \langle \langle \phi \rangle, c \rangle\in \mathcal{L}(V)\iff \langle \phi \rangle\in SAT$$

^cadc87


## 3SAT

### 3CNF

> Una [[032 Formule Booleane (ETC)|formula booleana]] è in **forma normale 3-congiuntiva $3CNF$** se è un AND di [[025 Funzioni Booleane (AdE)#Clausola|Clausole]] e tutte le clausole hanno tre [[025 Funzioni Booleane (AdE)#Letterale|Letterali]].

> [!example]+ <font color="orange">Esempio</font>
>
> -   $(\overline{x_1} \lor x_2 \lor x_3) \land (x_3 \lor \overline{x_6} \lor x_6)$
> -   $(\overline{x_1} \lor x_2 \lor x_3) \land (x_3 \lor \overline{x_6} \lor x_6) \land (x_3 \lor \overline{x_5} \lor x_5)$

> [!summary] Abbreviazioni
>
> -   $CNF$ - formula booleana in forma normale congiuntiva, ovvero un AND di clausole dove ogni clausola ha un solo letterale, quindi un AND di letterali
>     -   $SAT_{CNF}=\{\langle \phi \rangle\ |\ \phi\text{ è una formula booleana soddisfacibile in }CNF\}$
> -   $3CNF$ - formula booleana in forma normale 3-congiuntiva, ovvero un AND di clausole dove ogni clausola ha 3 letterali
> -   $kCNF$ - formula booleana in forma normale k-congiuntiva, ovvero un AND di clausole dove ogni clausola ha k letterali

### Definizione

> Il $\color{#CC241D}3SAT$ rappresenta il linguaggio associato al problema di decisione della soddisfacibilità di una $3CNF$.
> $$3SAT=\{\langle \phi \rangle\ |\ \phi \text{ è una formula 3CNF soddisfacibile}\}$$

> [!quote]+ Teorema 3SAT riducibile in tempo polinomiale a CLIQUE
>
> > $3SAT$ è [[062 Riducibilità in tempo polinomiale|riducibilie in tempo polinomiale]] a [[058 La Classe NP#CLIQUE|CLIQUE]].
>
> Sia $\phi$ una formula $3CNF$ con _$k$ clausole_ $$(a_1 \lor b_1 \lor c_1) \land (a_2 \lor b_2 \lor c_2) \land \dots \land (a_k \lor b_k \lor c_k )$$
>
> Consideriamo la funzione $f$ che associa $\langle \phi \rangle$ la stringa $\langle G,k \rangle$, dove $G=(V,E)$ è il [[012 Grafi#Grafi Non Diretti (Non Orientati)|grafo non orientato]] definito come segue:
>
> -   $V$ ha $3\cdot k$ vertici, divisi in $k$ gruppi di tre nodi (_triple_) $t_1,\dots,t_k$. La tripla $t_j$ corrisponde alla clausola $(a_j \lor b_j \lor c_j)$ e ogni vertice in $t_j$ corrisponde a un letterale in $(a_j \lor b_j \lor c_j)$, quindi $V=\{a_1,b_1,c_1,\dots,a_k,b_k,c_k\}$
> -   Non ci sono archi tra i vertici in una tripla $t_j$
> -   Non ci sono archi tra un vertice associato a un letterale $x$ e i vertici associati al letterale $\overline{x}$
> -   Ogni altra coppia di vertici è connessa da un arco
>
> La funzione $f$ è [[044 Funzioni Calcolabili|calcolabile]] e può essere [[061 Funzioni calcolabili in tempo polinomiale|calcolata in tempo polinomiale]].
>
> Per provare che $f$ è una riduzione di tempo polinomiale di $3SAT$ a $CLIQUE$ resta da dimostrare che $$\langle \phi \rangle\in3SAT\iff \langle G,k \rangle\in CLIQUE$$
> Ovvero che $\phi$ è soddisfacibile _se e solo se $G$ ha una k-clique_.
>
> Supponiamo che $\phi$ abbia un assegnamento di soddisfacibilità. Questo assegnamento di valori alle variabili rende vera ogni clausola $(a_j \lor b_j \lor c_j)$ e quindi esiste almeno un letterale vero in ogni clausola $(a_j \lor b_j \lor c_j)$.
>
> Scegliamo un letterale vero in ogni clausola $(a_j \lor b_j \lor c_j)$ e consideriamo il sottografo $G'$ di $G$ indotto dai nodi corrispondenti ai letterali scelti.
>
> $G'$ è una k-clique. Infatti $G'$ ha $k$ vertici, poiché abbiamo scelto un letterale in ognuna delle 4 clausole e poi i $k$ vertici di $G$ corrispondenti a tali letterali.
> Due qualsiasi vertici in $G'$ non si trovano nella stessa tripla (corrispondono a letterali in clausole diverse) e non corrispondono a una coppia $x$, $x$ perché corrispondono a letterali veri nell'assegnamento di soddisfacibilità. Quindi due qualsiasi vertici in $G'$ sono connessi da un arco in $G$.
>
> Viceversa, supponiamo che $G$ abbia una k-clique $G'$. Poiché due nodi in una tripla non sono connessi da un arco, ognuna delle $k$ triple contiene esattamente uno dei nodi della k-clique.
> Consideriamo l'assegnamento di valori alle variabili di $\phi$ che renda veri i letterali corrispondenti ai nodi di $G'$. Ciò è possibile perché in $G'$ non ci sono vertici corrispondenti a una coppia $x, \overline{x}$.
>
> Ogni tripla contiene un nodo di $G'$ e quindi ogni clausola contiene un letterale vero. Questo è un assegnamento di soddisfacibilità per $\phi$, quindi $\langle \phi \rangle\in 3SAT$.

^b1992a

### Gadgets

È possibile usare $3SAT$ per _ridurre in tempo polinomiale_ un problema, molto spesso usata per provare la [[063 NP-Completezza|NP-Completezza]] di un problema.

Quando costruiamo una riduzione di tempo polinomiale _da $3SAT$ ad un linguaggio_, cerchiamo _strutture_ in quel linguaggio che possono simulare le variabili e le clausole nelle formule booleane. Tali strutture sono a volte chiamate **gadget** (o _componente_).

Infatti, bisogna:

1. Definire per ogni _variabile_ un gadget che modella l'assegnazione di verità alla variabile
2. Definire per ogni _clausola_ un gadget che modella la soddisfacibilità della clausola
3. Collegare i due insiemi di gadget per garantire che l'assegnazione alle variabili soddisfi tutte le clausole

> [!example]+ <font color="orange">Riduzione polinomiale 3SAT a CLIQUE</font>
> Nella riduzione polinomiale $3SAT\leq_p CLIQUE$:
>
> 1.  Ogni variabile è simulata da un nodo
> 2.  Ogni clausola è simulata da una tripla di nodi
> 3.  Questi nodi sono collegati da archi, tali che un assegnamento alle variabili che soddisfa tutte le clausole è quello che assegna valore `Vero` ai letterali corrispondenti ai nodi nella clique.

^42a5bf

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
