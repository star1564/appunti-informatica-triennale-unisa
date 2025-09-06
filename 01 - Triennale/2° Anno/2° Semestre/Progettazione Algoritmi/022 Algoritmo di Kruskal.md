---
aliases: 
tags:
  - corsi/informatica/progettazione_algoritmi
paragrafo: Alberi e Grafi
cssclasses:
  - 
---
> L'**Algoritmo di Kruskal** è un algoritmo di tipo [[011 Algoritmi Greedy|greedy]] utilizzato per trovare l'[[020 Minimo Sottografo Connesso Ricoprente|MST]] di un [[012 Grafi|Grafo]] non diretto e pesato.

> [!tip]+ Algoritmo di Kruskal
> La complessità di questo algoritmo è $O(|E|\log |E|)$, dove $|E|$ è il numero di archi.
> ![[013 Alberi#Foresta]]
> 
> 1. Ordina tutti gli archi del grafo in ordine crescente in base al loro peso;
> 2. Si crea un insieme di foreste separate inizialmente, in cui ogni foresta contiene un solo nodo del grafo;
> 3. Gli archi vengono considerati uno alla volta in ordine crescente di peso. Per ogni arco considerato:
> 	- Controlla se i due nodi che collega appartengono alla stessa foresta. 
> 		- Se i due nodi sono già nella stessa foresta, l'aggiunta di quell'arco creerebbe un ciclo. In tal caso, l'arco viene scartato. 
> 		- Altrimenti, l'arco viene accettato.  
> 4. Se l'arco viene accettato, si uniscono le due foreste di appartenenza dei due nodi collegati da quell'arco in una sola foresta. Questa unione viene eseguita collegando i due nodi.  
> 5. Si ripetono i passaggi 3-4 fino a quando non si sono considerati tutti gli archi.

^3198b4


> [!example]- <font color="orange">Esempio</font>
> Il seguente grafico ha questo ordinamento degli archi in base al loro costo:
> $(h,g)=1, (i,c)=2, (g,f)=2, (a,b)=4, (c,f)=4, (i,g)=6, (c,d)=7, (i,h)=7, (a,h)=8, (b,c)=8, (d,e)=9, (b,h)=11, (d,f)=14$
> 
> ![[Pasted image 20230608121154.png|500]]
> L'algoritmo esegue le seguenti scelte, passo passo, in cui rappresentiamo in blu gli archi considerati (ed aggiunti) ed in rosso quelli considerati ma non aggiunti in quanto creerebbero un ciclo.
> ![[Pasted image 20230608121330.png]]
> ![[Pasted image 20230608121350.png]]

---
### Implementazione efficiente
Per un'implementazione efficiente dell'algoritmo di Kruskal abbiamo bisogno di un sistema efficiente per stabilire se l'aggiunta di un arco $(u, v)$ all'albero MST crea un ciclo o meno. 

Seguendo all'algoritmo con foreste mostrato sopra, notiamo che se due nodi $u$ e $v$ *appartengono alla stessa [[012 Grafi#Componente Connessa|componente connessa]]*, allora si creerà un ciclo. Ovvero se: $$\text{insieme di nodi raggiungibili da } u \equiv \textcolor{#61AFEF}{C(u)=C(v)}\equiv \text{insieme di nodi raggiungibili da } v$$
Quindi, dato un grafo $G=(V, E)$ con $V=\{x_1,x_2,\dots\}$, dobbiamo gestire una collezione dinamica (*foresta*) $F=\{S_1, \dots, S_r\}$ di insiemi a due a due disgiunti, dove:
- Ogni insieme (*componente connessa* che via via l'algoritmo di Kruskal crea) $S_i\subseteq\{x_1,x_2,\dots\}$;
- Ogni insieme $S_i$ nella collezione $F$ ha un *rappresentante*, denotato $rap[S_i]$, che in generale è un elemento esso stesso in $S_i$.

Le operazioni che vogliamo supportare sono le seguenti:
- La ***`MAKE-SET(x)`*** *aggiunge* alla collezione $F$ un nuovo insieme $S=\{x\}$ il cui unico elemento è $x$. Vale che $rap[S]=x$. Poiché gli insiemi in $F$ devono essere disgiunti, $x$ non deve appartenere a *nessun altro insieme della collezione*;
- La ***`FIND-SET(x)`*** *ritorna il rappresentante* $rap[S_x]$, dove $S_x$ è quell'unico membro della foresta $F$ che contiene l'elemento $x$;
- La ***`UNION(x,y)`*** *unisce due insiemi* $S_x$ ed $S_y$ di $F$, che contengono gli elementi $x$ e $y$, in un nuovo insieme $S$, dove $S=S_x\cup S_y$. Viene assegnato qualche valore a $rap[S]$.

Le operazioni si possono implementare attraverso:
- **Liste con puntatori**;
- **Alberi** (più efficiente).

> [!example]- <font color="orange">Esempio</font>
>![[Immagine 2023-06-08 144724.png]]
>Le componenti connesse correttamente individuate si trovano nell'ultima riga.

> [!quote]- Codice
>```C
>Kruskal(G){
>	crea un nuovo MST di nome T;
>
>	for(ogni vertice v in V){
>		Make_Set(v);
>	}
>
>	ordina gli archi di E per costo non decrescente;
>
>	for(ogni arco (u, v) in E, in ordine di peso non decrescente){
>		if(Find_Set(u) != Find_Set(v)){
>			T <- T unito all'arco (u, v);
>			Union(u, v);
>		}
>	}
>
>	return T;
>}
>```



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