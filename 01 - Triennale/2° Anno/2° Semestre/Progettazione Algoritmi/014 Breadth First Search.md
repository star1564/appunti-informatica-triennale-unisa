---
aliases:
  - Visita in Ampiezza di Grafi
  - BFS
  - Breadth First Search
tags:
  - corsi/informatica/progettazione_algoritmi
paragrafo: Alberi e Grafi
cssclasses:
  - 
---
>La **Visita in Ampiezza**, nota anche come *Breadth First Search* (BFS), è un algoritmo utilizzato per esplorare o visitare tutti i nodi di un grafo ([[012 Grafi#Grafi Diretti e Non|diretto e non]]) in modo sistematico. L'obiettivo della visita in ampiezza è quello di visitare prima tutti i nodi vicini al nodo di partenza e quindi espandere gradualmente la ricerca ai nodi più lontani.

Quest'algoritmo di visita in ampiezza esplora il grafo *livello per livello*, visitando prima tutti i nodi a una distanza di un arco dal nodo di partenza, poi tutti i nodi a distanza due archi e così via. Questo garantisce che tutti i nodi raggiungibili dal nodo di partenza vengano visitati e che nessun nodo venga visitato più di una volta.

![[Pasted image 20230607110725.png|700]]

Esplora il grafo $G$ per scoprire tutti i vertici raggiungibili dal vertice di partenza (sorgente) $r$, calcolando la [[012 Grafi#^5e2482|distanza]] da $r$ ad ognuno dei vertici raggiungibili da $r$ in $G$.

L'algoritmo produce due **output**:
- Un "array" con i valori delle distanze di ogni nodo $w$ da $r$;
- Un [[013 Alberi#Albero Radicato|Albero Radicato]] $T$ che contiene tutti i vertici del grafo $G$ che sono raggiungibili mediante un cammino da $r$.

> [!summary] ‎Algoritmo ITERATIVO
>1. Si seleziona un nodo di partenza $v$ nel grafo e lo si segna come visitato;
>2. Si inserisce in $d[v]$ la distanza $0$;
>3. Si inserisce il nodo corrente in una [[022 Queue nel C#DEFINIZIONE|Coda]] (oppure una [[020 Liste nel C#DEFINIZIONE|Lista]]) ed in un albero $T$;
>4. Finché la coda non è vuota, si eseguono i passaggi successivi:
>    - Si rimuove il primo nodo $u$ dalla coda e lo si designa come nodo corrente;
>    - Si esaminano tutti i nodi adiacenti $w$ al nodo corrente $u$ che non sono ancora stati visitati. Esegui per ogni nodo non visitato $w$ i passaggi successivi: 
> 	   - Si segna $w$ come visitato;
> 	   - Si inserisce in $d[w]$ la distanza $d[u]+1$; 
> 	   - Si inserisce $w$ nella coda;
> 	   - Si inserisce $w$ e l'arco $(u,w)$ nell'albero $T$.

> [!example]- <font color="orange">Esempio</font>
>Abbiamo il seguente grafo non connesso. ![[Pasted image 20230607113244.png|500]]
>Supponiamo che l'algoritmo parta dal vertice $v = 1$. Gli archi rossi sono gli archi dell'albero prodotto dall'algoritmo $BFS(G, 1)$, gli archi tratteggiati sono gli archi del grafo G che non vengono inseriti nell'albero.
>
>![[Pasted image 20230607113339.png|700]]
>
>Dove i livello $L_i$ indica l'insieme dei nodi a distanza $i$ da $v$.

> [!quote]- Codice
>```C
>BFS(G, s){
>	// Imposta tutti i nodi come non visitati
>	for(ogni nodo v in V){
>		scoperto[v] = false;
>		parent[v] = NULL;
>	}
>	
>	Q <- s; // Inserisci il nodo di partenza s nella coda Q
>	d[s] = 0;
>	scoperto[s] = true;
>	T <- s; // Inserisci il nodo di partenza s nell'albero T
>	
>	while(Q non è vuota){
>		estrai il nodo v da Q;
>		
>		for(ogni nodo x adiacente al nodo v){
>			if(scoperto[x] == false){
>				scoperto[x] = true;
>				d[x] = d[v] + 1;
>				parent[x] = v;
>				T <- x, (v, x); // Inserisci il nodo x e l'arco (v, x) nell'albero T
>				Q <- x; // Inserisci il nodo x in Q
>			}
>		}	
>	}
>}
>```


---
### BFS nei grafi diretti
La visita BFS funziona anche in grafi diretti. Infatti anche in questo caso produce un albero $T$ contenente tutti i nodi dei vari livelli $L_i$.

L'albero $T$ ha la proprietà che esso contiene tutti e solo i nodi $x$ per cui nel grafo esiste un cammino *diretto* dal nodo sorgente. Tali nodi $x$ possono o non avere un cammino diretto da essi alla sorgente.

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
- [Michael Sambol (Youtube)](https://www.youtube.com/watch?v=HZ5YTanv5QE)