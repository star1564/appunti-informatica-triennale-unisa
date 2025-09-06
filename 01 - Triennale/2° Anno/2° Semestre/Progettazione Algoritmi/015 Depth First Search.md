---
aliases:
  - Visita in Profondità di Grafi
  - DFS
  - Depth First Search
tags:
  - corsi/informatica/progettazione_algoritmi
paragrafo: Alberi e Grafi
cssclasses:
  - 
---
>La **Visita in Profondità**, nota anche come *Depth First Search* (DFS), è un algoritmo utilizzato per esplorare o visitare tutti i nodi di un grafo ([[012 Grafi#Grafi Diretti e Non|diretto e non]]) in modo sistematico. 

A differenza della [[014 Breadth First Search|BFS]] che esplora il grafo livello per livello, la DFS esplora il grafo *seguendo un approccio più profondo* utilizzando la [[008 Algoritmi Ricorsivi|ricorsione]].

L'algoritmo produce come **output** un [[013 Alberi#Albero Radicato|Albero Radicato]] $T$ che contiene tutti i vertici del grafo $G$ che sono raggiungibili mediante un cammino da $r$.

> [!summary] Algoritmo ITERATIVO
> 1. Marca tutti i vertici come non visitati;
> 2. Si seleziona un nodo di partenza $s$ nel grafo;
> 3. Inserisci $s$ nello stack $S$;
> 4. Mentre $S$ non è vuoto:
> 	- Estrai un nodo $u$ da $S$;
> 	- Se $u$ non è stato visitato:
> 		- Marca $u$ come visitato;
> 		- Inserisci $u$ in $T$;
> 		- Per ogni arco $(u, v)$ uscente da $u$:
> 			- Se $v$ non è stato visitato:
> 				- Aggiungi $v$ allo stack $S$.

> [!summary] ‎Algoritmo RICORSIVO
>1. Si seleziona un nodo di partenza $v$ nel grafo e lo si inserisce in $T$;
>2. Marca $v$ come visitato;
>3. Per ogni arco $(v,u)$ uscente da $v$:
>    - Se $u$ non è stato visitato:
> 	   - Inserisci il nodo $u$ e l'arco $(v,u)$ nell'albero $T$;
> 	   - Chiama ricorsivamente la funzione sul nodo $u$;

> [!example]- <font color="orange">Esempio</font>
>Se si sceglie come nodo di partenza 1 nel grafo seguente abbiamo questi passaggi nell'albero T:
>
>![[Pasted image 20230607132455.png|700]]
>
>1. $1 \to 2, 3$
>2. $2 \to \not1, 3, 4$
>3. $3 \to \not1, \not2, 5, 7, 8$
>4. $5\to \not2, \not3, 4, 6$
>5. $4\to \not2, \not5$
>6. Si ritorna al punto (4) con le nuove informazioni: $5\to \not2, \not3, \not4, 6$
>7. $6\to \not5$
>8. Si ritorna al punto (4) con le nuove informazioni: $5\to \not2, \not3, \not4, \not6$
>9. Si ritorna al punto (3) con le nuove informazioni: $3 \to \not1, \not2, \not5, 7, 8$
>10. $7 \to \not3, 8$
>11. $8\to \not3, \not 7$
>12. Si ritorna al punto (3) con le nuove informazioni: $3 \to \not1, \not2, \not5, \not7, \not8$
>13. Fine

> [!quote]- Codice (Iterativo)
>```C
>DFS(G, s){
>	// Imposta tutti i nodi come non visitati
>	for(ogni nodo v in V){
>		scoperto[v] = false;
>		parent[v] = NULL;
>	}
>	
>	S <- s; // Inserisci il nodo di partenza s nello stack S
>	
>	while(S non è vuoto){
>		estrai il nodo v da S;
>		
>		if(scoperto[v] == false){
>			scoperto[v] = true;
>			T <- v, (parent[v], v); // Inserisci il nodo v nell'albero T
>			
>			for(ogni nodo x adiacente al nodo v){
>				if(scoperto[x] == false){
>					parent[x] = v;
>					S <- x; // Inserisci il nodo x nello stack S
>				}
>			}
>		}	
>	}
>}
>```

A differenza dell'albero BFS che tende ad essere "corto e largo", l'albero DFS tende ad essere "lungo e stretto".

![[Pasted image 20230607140018.png|550]]

> [!tip] Alcune proprietà del DFS
>1. Per ogni data chiamata ricorsiva di DFS($u$), tutti i nodi che vengono marcati come "visitati" dall'inizio della chiamata ricorsiva fino alla fine della ricorsione sono discendenti di $u$ nell'albero DFS $T$.
>2. Sia $T$ un albero DFS e siano $x$ e $y$ due nodi in $T$. Sia $(x,y)$ un arco del grafo $G$ che non è un arco di $T$. Allora o $x$ è un antenato di $y$ oppure $y$ è un antenato di $x$.
>3. Per ogni coppia di nodi $u$ e $v$ nel grafo, le loro [[012 Grafi#Componente Connessa|componenti connesse]] sono uguali o sono disgiunte (cioè non hanno alcun nodo in comune).

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
- [Michael Sambol (Youtube) - ITERATIVO CON STACK](https://www.youtube.com/watch?v=Urx87-NMm6c)