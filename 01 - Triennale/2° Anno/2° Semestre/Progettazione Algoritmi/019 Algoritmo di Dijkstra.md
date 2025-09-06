---
aliases: 
tags:
  - corsi/informatica/progettazione_algoritmi
paragrafo: Alberi e Grafi
cssclasses:
  - 
---
>L'**Algoritmo di Dijkstra** è un algoritmo utilizzato per trovare il *percorso più breve da un nodo di partenza a tutti gli altri nodi* in un [[012 Grafi|grafo diretto o non diretto]] **pesato**, dove il costo (peso) per ogni arco deve essere $\color{#CC241D}\geq0$. Utilizza la [[011 Algoritmi Greedy|tecnica Greedy]].

L'algoritmo di Dijkstra è comunemente utilizzato in applicazioni di [[028 Strategie e Algoritmi di Routing#Basati sul Percorso più Breve (Distance Vector)|routing]], ma può essere applicato anche in altri contesti, come la determinazione del percorso più breve tra due città in una mappa stradale.

> [!summary] Algoritmo di Dijkstra
> Dato un grafo pesato (ovvero con il costo o lunghezza per ogni arco):
> 1. Scegliere un nodo di partenza $s$;
> 2. Impostare la distanza del nodo iniziale $d[s]=0$, mentre per il resto degli altri nodi si imposta $d[\dots]=\infty$;
> 3. Fino a quando tutti i vertici non sono stati visitati, eseguire queste operazioni:
> 	- Si seleziona il nodo $v$ con la distanza minore tra quelli non ancora visitati (inizialmente, il nodo di partenza sarà il nodo corrente);
> 	- Per ogni nodo adiacente non visitato $u$ del nodo corrente $v$, eseguire queste operazioni:
> 		- Calcola la distanza dal nodo di partenza $s$ al nodo $u$ attraverso questa formula $$d[u]=d[v]+peso\_ arco(v,u)$$
> 		- Se la distanza calcolata di $u$ è minore della distanza precedentemente assegnata allo stesso nodo: 
> 			- Si aggiorna il valore della distanza;
> 	- Dopo aver aggiornato tutte le distanze dei nodi adiacenti, si segna il nodo corrente $v$ come visitato.

> [!example]- <font color="orange">Esempio</font>
> ![[Pasted image 20230608103426.png]]
> ![[Pasted image 20230608103921.png]]
> ![[Pasted image 20230608103530.png]]

> [!example] ***[[demo-dijkstra.pdf|↪Altro esempio di Dijkstra (PDF)]]***

> [!quote]- Codice
>```C
>Dijkstra(G, s){
>	d[s] = 0;
>	parent[s] = NULL;
>	Q_P_Min <- s; // Inserisci nella coda a min priorità il nodo di partenza s
>	
>	for(ogni nodo x in V-{s}){
>		d[x] = infinito;
>		parent[x] = NULL;
>		Q_P_Min <- x;
>	}
>
>	Visitati = NULL;
>	while(Q_P_Min non è vuota){
>		curr = Extract_Min(Q_P_Min); // Restituisci e cancella dalla coda il nodo non visitato con distanza minore
>		Visitati <- curr; // Aggiungi curr ai nodi visitati
>		
>		// V-Visitati sono tutti i nodi non ancora visitati
>		for(ogni nodo u in V-Visitati adiacente a curr, con arco (curr, u)){
>			temp_dist_u = d[curr] + peso_arco(curr, u);
>			
>			if(temp_dist_u < d[u]){
>				d[u] = temp_dist_u;
>				Decrease_Key(Q_P_Min, u, d[u]);
>				parent[u] = curr;
>			}
>		}
>	}
>}
>```

---
> [!quote]- Dimostrazione del percorso più breve
> >Se un percorso $P$ di lunghezza minima dal nodo $s$ al nodo $v$ passa attraverso il nodo $u$, allora la parte di cammino $P'$ da $s$ a $u$ è esso stesso un cammino di lunghezza minima da $s$ a $u$.
> 
> ![[Pasted image 20230608094459.png|600]]
> 
> Questo perché se $P'$ non lo fosse, allora esisterebbe un cammino differente da $s$ a $v$ (linea rossa tratteggiata) di lunghezza inferiore. Ma ciò implicherebbe che si potrebbe sostituire il cammino $P'$ con la linea rossa tratteggiata ed andare da $s$ a $v$ con un cammino di lunghezza totale inferiore a quella di $P$, contro l'ipotesi dell'ottimalità di $P$.
>
> Quindi, un cammino minimo da $s$ ad un nodo generico $v$ *non ha una struttura arbitraria*, ma è una *estensione di cammini minimi* da $s$ a nodi $u$ più vicini ad $s$ di quanto lo sia $v$.

^d47736

Usando una [[017 Insiemi Dinamici#^a9ced4|(min)coda a priorità]], l'algoritmo di Dijkstra può essere implementato in un grafo con $n$ nodi ed $m$ archi in modo da richiedere tempo $O(m)$, più il tempo per eseguire $n$ `ExtractMin` ed $m$ `DecreaseKey`.

Se usiamo un [[018 Heap|min-heap]] per implementare la coda a priorità in questione, ogni operazione di `ExtractMin` e `DecreaseKey` richiede tempo $O(\log n)$, per un gran totale di $O(m\log n)$ operazioni per implementare l'algoritmo di Dijkstra.

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
- [Come funziona l'algoritmo di Dijkstra](https://www.youtube.com/watch?v=pVfj6mxhdMw)