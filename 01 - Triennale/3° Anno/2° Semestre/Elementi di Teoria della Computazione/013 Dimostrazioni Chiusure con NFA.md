---
aliases:
  - Dimostrazione chiusura linguaggi unione (NFA)
  - Dimostrazione chiusura linguaggi concatenazione (NFA)
  - Dimostrazione chiusura linguaggi star (NFA)
  - Dimostrazione chiusura linguaggi reverse (NFA)
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: Automi Non Deterministici
cssclasses:
  - 
---
Qui vengono riportate tutte le dimostrazioni **possibili con gli [[009 Automi Finiti Non Deterministici|NFA]]** per le [[Chiusura di un_opearzione|chiusure]] delle operazioni su [[007 Linguaggio Regolare|linguaggi regolari]]. Alcune operazioni si possono dimostrare sia con DFA che NFA.

### Stati accettanti
Per dimostrare in maniera costruttiva attraverso gli NFA, si usa una trasformazione utile, ovvero si *usa un solo stato accettante* attraverso le epsilon-transizioni.

In fatti, basta inserire un singolo stato di accettazione "all'esterno" dell'NFA, collegare gli stati accettanti ad esso e trasformare gli stati accettanti nell'NFA in stati normali.

![[Pasted image 20240323105350.png|480]]

> [!example]- <font color="orange">Esempio</font>
>**NFA con 2 stati accettanti**:
>![[Pasted image 20240323104957.png]]
>
>**NFA *equivalente* con 1 stato accettante**:
>![[Pasted image 20240323105041.png]]


## Unione
Il seguente teorema è equivalente a quello sviluppato con i DFA [[008 Dimostrazioni Chiusure con DFA#Unione|precedentemente]], ma in questo caso si avrà una *costruzione più semplice*.

> [!quote]+ Dimostrazione per l'unione [Sipser - Teorema 1.45]
> >La classe dei linguaggi regolari è chiusa rispetto all'operazione di [[008 Unione tra Insiemi|unione]].
> 
> Ovvero, dati due linguaggi regolari $A_1$ e $A_2$, dobbiamo dimostrare che $$A_1 \text{ e } A_2 \text{ regolari } \Rightarrow A_1\cup A_2 \text{ regolare}$$
> > [!error] In generale NON è vero che se l'unione di due linguaggi è regolare allora i due linguaggi sono regolari
> >$$A_1\cup A_2 \text{ regolare}\not\Rightarrow A_1 \text{ e } A_2 \text{ regolari } $$
> #### Idea
> L'idea è prendere due NFA, $N_1$ ed $N_2$ per $A_1$ e $A_2$, e comporli in un nuovo NFA, $N$. La macchina $N$ deve accettare il suo input se $N_1$ o $N_2$ lo accetta.
> 
> ![[Pasted image 20240323110147.png|150]]
> 
>La nuova macchina ha un nuovo stato iniziale che si dirama negli stati iniziali delle vecchie macchine con delle $\epsilon$-transizioni. In questo modo, la nuova macchina ipotizza non deterministicamente quale delle due macchine accetta l'input. Se una di esse accetta l'input, anche $N$ lo accetterà.
>
>![[Pasted image 20240323110224.png|230]]
>
>#### Dimostrazione
>Sia $N_1=(Q_1,\Sigma,\delta_1,q_1,F_1)$ che riconosce $A_1$ ed $N_2=(Q_2,\Sigma,\delta_2,q_2,F_2)$ che riconosce $A_2$.
>Costruiamo $N=(Q,\Sigma,\delta,q_0,F)$ per riconoscere $A_1\cup A_2$.
>1. $Q=\{q_0\}\cup Q_1\cup Q_2$
>   Gli stati di $N$ sono tutti gli stati di $N_1$ ed $N_2$, con l'aggiunta di un nuovo stato iniziale $q_0$.
>2. Lo stato $q_0$ è lo stato iniziale di $N$.
>3. L'insieme degli stati accettanti è $F=F_1\cup F_2$.
>   Gli stati accettanti di $N$ sono tutti gli stati accettanti di $N_1$ ed $N_2$. In questo modo, $N$ accetta se $N_1$ accetta o $N_2$ accetta.
>4. Definiamo $\delta$ in modo che per ogni $q\in Q$ e ogni $a\in\Sigma_\epsilon$ (l'alfabeto con $\epsilon$), $$\delta(q,a)=\begin{cases} \delta_1(q,a)\quad q\in Q_1 \\ \delta_2(q,a)\quad q\in Q_2 \\ \{q_1,q_2\}\quad q=q_0 \land a=\epsilon \\ \emptyset\quad\quad\quad\ \  q=q_0\land a\neq\epsilon \end{cases}$$ Ovvero che su $q$ è definita solo la $\epsilon$-transizione, che porta nei due stati iniziali degli automi.

## Concatenazione
> [!quote]+ Dimostrazione per la concatenazione [Sipser - Teorema 1.47]
> >La classe dei linguaggi regolari è chiusa rispetto all'operazione di [[002 Operazioni su Linguaggi|concatenazione]].
> 
> Ovvero, dati due linguaggi regolari $A_1$ e $A_2$, dobbiamo dimostrare che $$A_1 \text{ e } A_2 \text{ regolari } \Rightarrow A_1\circ A_2 \text{ regolare}$$
> 
> #### Idea
> L'idea è prendere due NFA, $N_1$ ed $N_2$ per $A_1$ e $A_2$, e combinarli in un nuovo NFA $N$ come abbiamo fatto nel caso dell'unione, ma questa volta in modo diverso.
> 
> ![[Pasted image 20240323113036.png|400]]
> 
>Poniamo come stato iniziale di $N$ lo stato iniziale di $N_1$. Gli stati accettanti di $N_1$ hanno delle ulteriori $\epsilon$-transizioni che non deterministicamente permettono di diramarsi in $N_2$ ogni volta che $N_1$ è in uno stato accettante, indicando che ha trovato un pezzo iniziale dell'input che costruisce una stringa in $A_1$. Gli stati accettanti di $N$ sono solo gli stati accettanti di $N_2$.
>
>![[Pasted image 20240323113252.png|500]]
>
>Quindi, esso accetta quando l'input può essere diviso in due parti, la prima accettata da $N_1$ e la seconda da $N_2$. Possiamo pensare che $N$ non deterministicamente ipotizza dove effettuare la divisione.
>
>#### Dimostrazione
>Sia $N_1=(Q_1,\Sigma,\delta_1,q_1,F_1)$ che riconosce $A_1$ ed $N_2=(Q_2,\Sigma,\delta_2,q_2,F_2)$ che riconosce $A_2$.
>Costruiamo $N=(Q,\Sigma,\delta,q_1,F_2)$ per riconoscere $A_1\circ A_2$.
>1. $Q=Q_1\cup Q_2$
>   Gli stati di $N$ sono tutti gli stati di $N_1$ ed $N_2$.
>2. Lo stato $q_1$ è lo stato iniziale di $N_1$.
>3. L'insieme degli stati accettanti è $F_2$.
>4. Definiamo $\delta$ in modo che per ogni $q\in Q$ e ogni $a\in\Sigma_\epsilon$ (l'alfabeto con $\epsilon$), 
>	$$\delta(q,a)=\begin{cases} \delta_1(q,a)\quad\quad\quad\quad q\in Q_1\land q\not\in F_1 \\ \delta_1(q,a)\quad\quad\quad\quad q\in F_1\land a\neq\epsilon \\ \delta_1(q,a)\cup\{q_2\}\quad  q\in F_1\land a=\epsilon \\ \delta_2(q,a)\quad\quad\quad\quad q\in Q_2 \end{cases}$$


## Star
> [!quote]+ Dimostrazione per la star [Sipser - Teorema 1.49]
> >La classe dei linguaggi regolari è chiusa rispetto all'operazione [[003 Chiusura di Kleene|star]].
> 
> Ovvero, dato un linguaggio regolare $A_1$, dobbiamo dimostrare che $$A_1 \text{ regolare } \Rightarrow A_1^* \text{ regolare}$$
> 
> #### Idea
> Prendiamo un NFA $N_1$ per $A_1$ e lo modifichiamo per riconoscere $A_1^*$. L'NFA risultante accetterà il suo input quando esso può essere diviso in varie parti ed $N_1$ accetta ogni parte.
>
>![[Pasted image 20240323114622.png]]
>
>Possiamo costruire $N$ come $N_1$ con $\epsilon$-transizioni supplementari che dagli stati accettanti ritornano allo stato iniziale. In questo modo, quando l'elaborazione giunge alla fine di una parte che $N_1$ accetta, la macchina $N$ ha la scelta di tornare indietro allo stato iniziale per provare a leggere un'altra parte che $N_1$ accetta. Inoltre dobbiamo modificare $N$ in modo che accetti $\epsilon$, che è sempre un elemento di $A_1^*$.
>
>![[Pasted image 20240323115258.png|550]]
>
>#### Dimostrazione
>Sia $N_1=(Q_1,\Sigma,\delta_1,q_1,F_1)$ che riconosce $A_1$.
>Costruiamo $N=(Q,\Sigma,\delta,q_0,F)$ per riconoscere $A_1^*$.
>1. $Q=\{q_0\}\cup Q_1$
>   Gli stati di $N$ sono tutti gli stati di $N_1$ più un nuovo stato iniziale.
>2. Lo stato $q_0$ è il nuovo stato iniziale.
>3. L'insieme degli stati accettanti è $F=\{q_0\}\cup F_1$.
>   Gli stati accettanti sono i vecchi stati accettanti più il nuovo stato iniziale.
>4. Definiamo $\delta$ in modo che per ogni $q\in Q$ e ogni $a\in\Sigma_\epsilon$ (l'alfabeto con $\epsilon$), 
>	$$\delta(q,a)=\begin{cases} \delta_1(q,a)\quad\quad\quad\quad q\in Q_1\land q\not\in F_1 \\ \delta_1(q,a)\quad\quad\quad\quad q\in F_1\land a\neq\epsilon \\ \delta_1(q,a)\cup\{q_1\}\quad  q\in F_1\land a=\epsilon \\\{q_1\}\quad\quad\quad\quad\quad\  q=q_0\land a=\epsilon \\ \emptyset\quad\quad\quad\quad\quad\quad\ \ \  q= q_0\land a\neq\epsilon \end{cases}$$





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