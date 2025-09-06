---
aliases:
  - Grafo
  - Grafi
  - Componente connessa
  - Grafi Orientati
  - Cammino Hamiltoniano
cssclasses:
  - 
tags:
  - corsi/informatica/progettazione_algoritmi
paragrafo: Alberi e Grafi
---
### Definizione
>Un **Grafo** $G$ consiste di una coppia di insiemi $(V,E)$, dove $V$ è l'*insieme* dei **vertici** (o nodi) del grafo, mentre $E$ è l'*insieme* degli **archi** *tra coppie di vertici* del grafo.

![[Pasted image 20230607095850.png]]

Un grafo è *connesso* se per ogni coppia di vertici $i$ e $j$ esiste un cammino tra $i$ e $j$.

![[Pasted image 20230607103119.png|600]]
<center>Sinistra: connesso; Destra: non connesso</center>
^bf9a77

La *distanza tra due vertici* $i$ e $j$ è la lunghezza (misurata in numeri di archi) del più breve cammino tra $i$ e $j$ (se esiste). ^5e2482

## Grafi Diretti e Non
Gli archi di un grafo possono rappresentare sia relazioni "simmetriche" tra vertici (relazione valida in entrambi i sensi), che relazioni "asimmetriche" (relazione valida solo in uno specifico senso).

### Grafi Non Diretti (Non Orientati)
Per rappresentare relazioni simmetriche si usano **Grafi non Diretti**. In tali grafi gli archi non hanno direzioni e *non si fa differenza* tra l'arco dal vertice $u$ al vertice $v$ e l'arco dal vertice $v$ al vertice $u$.  ![[Immagine 2023-06-07 100326.png|350]] 
### Grafi Diretti (Orientati)
Per rappresentare relazioni asimmetriche si usano **Grafi Diretti** (*orientati*). In tali grafi gli archi hanno "direzioni" ed *esiste una differenza* tra l'arco dal vertice $u$ al vertice $v$ e l'arco dal vertice $v$ al vertice $u$. ![[Immagine 2023-06-07 100441.png|350]]

In un grafo diretto $G$ si possono invertire le direzioni degli archi. Questo nuovo grafo sarà chiamato il *reverse di $G$*, ovvero $G^R$. ^e5c3e8

![[Pasted image 20230607151817.png|500]]

> [!info]+ Cammino Hamiltoniano
> Un Cammino Hamiltoniano in un grafo orientato è un cammino (orientato) che passa per ogni vertice del grafo una e una sola volta.
> 
>![[Pasted image 20240511103008.png]]

^cb9deb



---
> [!question]- Come descrivere un grafo $G=(V,E), |V|=n, |E|=m$ ad un calcolatore?
>***Metodo 1 - [[058 Matrici|Matrice]] di Adiacenza***
>Una matrice di adiacenza è una matrice binaria con $n$ righe ed $n$ colonne, (una riga ed una colonna per ciascun vertice del grafo) in cui l'elemento $A[i,j]$ nella riga $i$-esima e colonna $j$-esima di $A$ è uguale a $1$ se e solo se esiste nel grafo $G$ l'arco tra i vertici $i$ e $j$.
>
>![[Pasted image 20230607101201.png|550]]
>- Caratteristiche:
>	- Lo spazio richiesto è $\Theta(n^2), n=|V|$;
>	- Verificare se c'è un arco tra i vertici $i$ e $j$ perde tempo $\Theta(1)$;
>	- Identificare tutti gli archi del grafo perde tempo $\Theta(n^2)$.
>
>***Metodo 2 - [[020 Liste nel C#DEFINIZIONE|Liste]] di Adiacenza***
>Le liste di adiacenza di un grafo consistono in un insieme di $n$ liste a puntatori, una per ogni vertice del grafo. La lista associata al generico nodo $i$ contiene tutti i nodi $j$ che sono connessi al nodo $i$ mediante un arco. Tali vertici $j$ sono detti vicini (o adiacenti) del nodo $i$ nel grafo.
>
>![[Pasted image 20230607102444.png|550]]
>- Caratteristiche:
>	- Lo spazio richiesto è $\Theta(n+m), n=|V|, m=|E|$;
>	- Verificare se c'è un arco tra i vertici $i$ e $j$ perde tempo $\Theta(grado(i))$, dove il grado di $i$ è il numero di nodi adiacenti al nodo $i$;
>	- Identificare tutti gli archi del grafo perde tempo $\Theta(n+m)$;
>	- Un po' di nomenclatura sui grafi.

---
### Grafo Fortemente Connesso
>Un grafo *diretto* $G$ è detto **Fortemente Connesso** se e solo se ogni coppia dei suoi nodi *è mutualmente raggiungibile*, ovvero se per ogni sua coppia di nodi $u, v$, esiste un cammino diretto da $u$ e $v$ ed un cammino diretto da $v$ ad $u$.

![[Pasted image 20230607142628.png|600]]

Per sapere se un grafo è fortemente connesso si usa il seguente algoritmo.

> [!summary] Algoritmo: FortementeConnesso(Nodo, Grafo)
> 1. Scegli un nodo arbitrario $s$ in $G$;
> 2. Esegui [[014 Breadth First Search|BFS]]($s$) in $G$ oppure [[015 Depth First Search|DFS]]($s$) in $G$;
> 3. Esegui BFS($s$) nel [[012 Grafi#^e5c3e8|reverse]] $G^R$ oppure DFS($s$) in $G^R$;
> 4. Ritorna `True` se e solo se tutti i nodi di $G$ sono stati raggiunti in entrambe le esecuzioni precedenti.

---
### Componente Connessa
>Una **Componente Connessa** di un grafo è un sottoinsieme di nodi del grafo che *sono tutti collegati tra di loro tramite un percorso che parte dal nodo specificato*. In altre parole, una componente connessa rappresenta un gruppo di tutti e soli i nodi raggiungibili da un determinato nodo.

![[Pasted image 20230608143443.png|500]]

---
### Componente Fortemente Connessa
>In un grafo *diretto*, una **Componente Fortemente Connessa** è un insieme di nodi in cui ogni nodo può essere raggiunto da ogni altro nodo tramite un percorso diretto. In altre parole, se prendiamo due nodi all'interno di una componente fortemente connessa, esisterà sempre un percorso diretto che li collega.

> [!summary] Algoritmo
> 1. Esegui l'algoritmo DFS (o BFS) *su ogni nodo non visitato* nel grafo per creare un elenco ordinato dei nodi in base all'ordinamento decrescente dei tempi di completamento DFS. Questo può essere fatto visitando ogni nodo e segnando il *tempo di scoperta e il tempo di completamento della ricorsione*.
> 2. Inverti la direzione di tutti gli archi.    
> 3. Esegui nuovamente l'algoritmo DFS (o BFS) sul grafo inverso su ogni nodo non visitato, ma stavolta nell'ordine inverso dell'elenco ordinato dei nodi ottenuto nel passaggio 1. Durante l'esecuzione di DFS, ogni chiamata ricorsiva raggiungerà una componente fortemente connessa.

[[Esercizi sui grafi#Componenti Fortemente Connesse 1|↪Esercizio componenti fortemente connesse]]

---
### Eccentricità di un nodo
L'**Eccentricità** di un vertice $u$ è definita come $$\color{#61AFEF}e(u)=\begin{cases} \infty\quad\quad\quad\quad\quad\quad\quad\quad\quad\text{se } G \text{ non è connesso} \\ \max\{d(u,v)\}, \forall v\in V \quad\ \text{altrimenti} \end{cases}$$
Ovvero è la massima distanza tra il nodo $u$ e tutti gli altri nodi $v$ del grafo $G=(V,E)$ non diretto.

> [!example]- <font color="orange">Esempio</font>
> ![[Pasted image 20230610135324.png]]

---
### Diametro e Raggio di un grafo
Dato un grafo $G=(V,E)$ non diretto, il suo **Diametro** $D(G)$ è definito come la *distanza massima* tra due vertici di $G$.

$$D(G)=\begin{cases} \infty\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\text{se } G \text{ non è connesso} \\ \max\{d(u,v)\}, \forall u,v\in V \quad\ \text{altrimenti} \end{cases}$$

Quindi è la massimo tra tutte le eccentricità dei nodi di $G$.
$$\color{#61AFEF}D(G)=max\{e(u)\ |\ u\in V\}$$

Invece, il **Raggio** di un grafo $G=(V,E)$ è definito come $$\color{#61AFEF}R(G)=min\{e(u)\ |\ u\in V\}$$
> [!example]- <font color="orange">Esempio</font>
> ![[Pasted image 20230610135440.png]]

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