---
aliases: 
tags:
  - corsi/informatica/progettazione_algoritmi
paragrafo: Alberi e Grafi
cssclasses:
  - 
---
>La **Tecnica del Backtracking** è un'algoritmo di ricerca che viene utilizzato per trovare soluzioni a problemi di [[007 Combinazioni|combinazione]], [[003 Permutazioni|permutazione]] o decisione in cui *è necessario esplorare tutte le possibili configurazioni o opzioni*. L'obiettivo del backtracking è quello di ridurre lo spazio di ricerca *escludendo* le possibili soluzioni che non possono portare a una soluzione valida.

L'idea di base del backtracking è di *costruire incrementalmente una soluzione candidata*, esaminando una serie di scelte possibili in ciascun passo. Se una scelta non porta a una soluzione valida, si fa **marcia indietro** (da cui il termine "backtracking") e si esplora una diversa scelta. Questo processo continua finché non viene trovata una *soluzione completa* o tutte le possibili scelte sono state esplorate.

Il backtracking è particolarmente utile quando il *numero di possibili soluzioni è grande* e *non è possibile esplorarle tutte in modo esaustivo*. È efficace anche in problemi di enumerazione. 

Il backtracking costruisce un *[[013 Alberi|albero]] delle soluzioni* dove ciascun vertice interno $x$ è una *soluzione parziale*, mentre i figli di $x$ sono stati creati estendendo la soluzione parziale del vertice $x$. Le foglie dell'albero sono le *soluzioni complete*.

> [!summary] Algoritmo GENERALE
> Rappresentiamo le soluzioni con un array $SOL=(a_1,a_2,\dots,a_n)$, dove ciascun elemento $a_i$ va selezionato da un insieme ordinato $S_i$ di *possibili candidati* per la posizione $i$.
> 
> Ad ogni passo della ricerca avremo costruito una soluzione parziale con elementi fissati per i primi $k$ elementi dell'array, dove $k\leq n$. Da questa soluzione parziale $(a_1, ..., a_k)$ costruiamo l'insieme $S_{k+1}$ dei *possibili candidati per la posizione $k+1$* e cerchiamo di estendere la soluzione parziale con un elemento $a_{k+1}\in S_{k+1}$.
>
> Fin quando l'estensione genera una soluzione parziale, continuiamo ad estenderla.
>
> $S_{k+1}$ potrebbe anche essere vuoto, ciò significa che non c'è modo di estendere la corrente soluzione parziale. Quando ciò accade, bisogna fare un passo indietro e rimpiazzare l'ultimo elemento $a_k$ nella soluzione con un altro candidato in $S_k$ dove $a'_k \neq a_k\in S_k$.
> 
>```C
>BackTracking(SOL, k){
>	if((a_1, ..., a_k) è una soluzione){
>		stampala;
>	} else {
>		calcola l'insieme soluzione S_k+1;
>		while (S_k+1 non è vuoto){
>			SOL[a_k+1] = un elemento di S_k+1; // Forma (a_1, ..., a_k, a_k+1)
>			rimuovi a_k+1 da S_k+1;
>			BackTracking(SOL, k+1);
>		}
>	}
>}
>```

> [!example]- <font color="orange">Esempio</font>
> Supponiamo di avere il seguente albero con soluzione parziale $(a_1, ..., a_k)$. Questa non è una soluzione, pertanto la vogliamo estendere. Generiamo il prossimo insieme di soluzioni $S_{k+1}$, da cui troviamo 3 possibili nodi da cui scegliere $d,e,f$.
> Supponiamo dal nodo $f$ *si possa trovare* una soluzione, mentre dagli altri no.
> ![[Pasted image 20230609101156.png|700]]
> Scegliamo i nodi da $S_{k+1}$ in questo ordine: $d,e,f$ ed assegniamo ad ogni iterazione del `while` $a_{k+1}$ ad uno di questi nodi.
> ![[Pasted image 20230609101337.png|800]]
> Ad un certo punto della ricorsione sui nodi $d, e$, il *loro* insieme $S_{k+1}$ sarà vuoto, pertanto non ci sarà la soluzione e si esegue il backtracking passo dopo passo fino a ritornare alla scelta tra $d,e,f$. 
> Quando si sceglie $f$, ad un certo punto verrà trovata una soluzione, che verrà stampata. L'algoritmo eseguirà vari backtracking per trovare altre soluzioni partendo dal nodo $f$ per poi finire l'esecuzione.

> [!example]- <font color="orange">Esempio 2</font>
> Generare tutti i $2^n$ vettori binari di lunghezza $n$.
> ![[Pasted image 20230609103219.png]]

___
[[000 Indice PA|↖ Ritorna all'indice ↖]]

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
- [Abdul Bari](https://www.youtube.com/watch?v=DKCbsiDBN6c)