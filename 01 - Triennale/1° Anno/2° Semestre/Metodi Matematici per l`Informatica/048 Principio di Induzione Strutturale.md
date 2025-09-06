---
aliases: 
tags:
  - corsi/matematica/metodi_matematici_informatica
paragrafo: Dimostrazioni per induzione
cssclasses:
  - 
---
>Le proprietà di insiemi e strutture *definite ricorsivamente* (come le [[030 Ricorsione di Stringhe|stringhe]] e gli [[037 Ricorsione di Alberi|alberi]]) possono essere stabilite attraverso il **Principio di induzione Strutturale**.

Per provare che un enunciato della forma $$\color{#61AFEF}(\forall a\in A)\ P(a)$$ è vero, dove $A$ è un insieme di oggetti definito ricorsivamente e $P$ è una funzione proposizionale avente come dominio $A$, basta provare i seguenti due enunciati.

>**BASE**: Mostrare che l'enunciato $P$ è vero per tutti gli elementi nell'insime specificati dal Passo Base della definizione ricorsiva di $A$.
>
>**PASSO INDUTTIVO**: Mostrare che se l'enunciato $P$ è vero per ciascuno degli elementi usati per costruire nuovi elementi nel Passo Ricorsivo della definizione, allora l'enunciato $P$ è vero per questi nuovi elementi.

Nella prova si utilizzano molto spesso _insieme_ sia il principio di induzione strutturale che quello [[046 Principio di Induzione Matematica|matematico]].

> [!question]- Quando bisogna usare il Principio di Induzione Strutturale?
> Il Principio di Induzione Strutturale lo si usa quando si deve dimostrare una doppia [[005 Equivalenza tra Insiemi|Doppia Inclusione]], ed il primo elemento è un Insieme definito Ricorsivamente.
> Precisamente quando abbiamo la seguente ipotesi: $$\textcolor{#61AFEF}{Insieme\ Definito\ Ricorsivamente}\subseteq Insieme\ Normale$$
>
> Si vuole dimostrare che un elemento $x\in Insieme\ Definito\ Ricorsivamente$ appartiene anche all'insieme definito normalmente.
>
> Oppure quando si vuole semplicemente provare che un insieme definito in maniera ricorsiva è corretto.

> [!question]- Quando, invece, bisogna usare il Principio di Induzione Matematica?
> Il Principio di Induzione Matematica lo si usa quando si deve dimostrare una doppia doppia inclusione, ed il primo elemento è un Insieme definito Normalmente.
> Precisamente quando abbiamo la seguente ipotesi: $$\textcolor{#61AFEF}{Insieme\ Normale }\subseteq Insieme\ Definito\ Ricorsivamente$$
>
> Si vuole dimostrare che un elemento $x\in Insieme\ Normale$ appartiene anche all'insieme definito ricorsivamente.
>
> Oppure quando si vuole semplicemente provare che un insieme definito in maniera normale è corretto.

> [!example]- <font color="orange">Esempio su un insieme di interi</font>
> Sia A l'insieme ricorsivamente definito come segue.
>
> > Passo Base: $1\in A$.
> > Passo Ricorsivo: Se $x\in A$, allora $x+2\in A$.
>
> È possibile dimostrare, utilizzando il principio di induzione strutturale e il principio di induzione, che $A$ è l'insieme dei numeri interi positivi dispari:
>
> > $$A=\{n\in\mathbb{N}\ |\ \exists k\geq0:n=2k+1\}$$
>
> Per procedere, dobbiamo dimostrare la doppia inclusione.
>
> -   Dimostriamo, utilizzando il _Principio di Induzione Strutturale_, che: $$A\subseteq \{n\in\mathbb{N}\ |\ \exists k\geq0:n=2k+1\}$$
>
> Osserviamo che il nostro enunciato può essere riformulato come segue: $\forall x\in A$ esiste un intero non negativo $k$ tale che $x=2k+1$ ($*$).
>
> **BASE**: L'enunciato ($*$) è vero per l'elemento specificato dal passo base della definizione ricorsiva di $A$, poiché tale elemento è 1 e $1=2\cdot 0+1$. Il passo base è dimostrato.
>
> **PASSO INDUTTIVO**: Sia $w\in A$ un elemento costruito mediante il passo ricorsivo della definizione di $A$. Per definizione esiste $x\in A$ tale che $w=x+2$. Per ipotesi induttiva, esiste un intero non negativo $k$ tale che $x=2k+1$.
> Quindi risulta $$w=x+2=(2k+1)+2=2(k+1)+1$$
> Siccome $k+1$ è un intero non negativo, anche il passo induttivo dell'enunciato ($*$) risulta dimostrato.
>
> L'enunciato ($*$) è dimostrato in base al principio di induzione strutturale.
>
> -   Dimostriamo, utilizzando il _Principio di Induzione_, che: $$\{n\in\mathbb{N}\ |\ \exists k\geq0:n=2k+1\}\subseteq A$$
>
> Dobbiamo dimostrare che ogni intero non negativo pari multiplo di 3 si trova in $A$.
>
> Osserviamo che il nostro enunciato può essere riformulato come segue: Per ogni intero non negativo $n$ risulta $2n+1\in A$ ($*$).
>
> **BASE**: Per $n=0$ risulta $2n+1=1\in A$. Quindi il passo base dell'enunciato ($*$) è dimostrato.
>
> **PASSO INDUTTIVO**: Sia $k\geq0$. Supponiamo che si abbia $$2k+1\in A\quad (**)$$
> Proviamo che $2(k+1)+1\in A$.
> Per il passo ricorsivo della definizione di $A$ e l'ipotesi induttiva $(**)$, otteniamo $2(k+1)+1 = (2k+1)+2\in A$.
> Con $(2k+1)=x$ abbiamo che $x+2\in A$ e quindi il passo induttivo è dimostrato.
>
> L' enunciato (\*) è dimostrato in base al principio di induzione.

## Induzione strutturale su stringhe
Sia $P(w)$ una funzione proposizionale sulle stringhe.

Quindi $w$ è un elemento dell'insieme $\Sigma^*$ delle stringhe su un alfabeto $\Sigma$.
Vogliamo usare il principio di Induzione Strutturale per dimostrare che $P(w)$ è vera per ogni stringa $w$ in $\Sigma^*$, dobbiamo eseguire il:

>**PASSO BASE**: Mostrare che $P(\lambda)$ è vera.
>
>**PASSO INDUTTIVO**: Assumere che $P(w)$ è vera, dove $w\in\Sigma^*$ è una stringa.
>Mostrare che, se $x\in\Sigma$, allora $P(wx)$ è vera.

> [!example]- <font color="orange">Esempio su un insieme di interi</font>
> L'insieme $\{a\}^*$ delle stringhe sull'alfabeto ${a}$ è definito ricorsivamente come segue:
> Passo Base: $\lambda\in \{a\}^*$.
> Passo Ricorsivo: Se $x\in \{a\}^*$, allora $xa\in \{a\}^*$.
>
> Sia $A = \{a^n|n \in \mathbb{N}\}$, dove $\mathbb{N}$ è l'insieme dei numeri interi non negativi. Dimostrare che $A = \{a\}^*$.
>
> L'equivalenza delle due definizioni, cioè l'uguaglianza degli insiemi, viene provata usando la doppia inclusione.
>
> -   Proviamo che $\{a\}^*\subseteq A$ attraverso il _Principio di Induzione Strutturale_.
>
> Quindi consideriamo una stringa $w\in\{a\}^*$.
> **BASE**: Se $w=\lambda$, allora $w=\lambda = a^0\in A$ e il passo base è dimostrato.
>
> **PASSO INDUTTIVO**: Sia $w\in\{a\}^*$ (qui l'insieme possibile di stringhe) e sia $w\neq \lambda$. Per la definizione ricorsiva di $\{a\}^*$ abbiamo che $w=xa$, con $x\in \{a\}^*$. Possiamo dunque applicare l'ipotesi induttiva su $x$ e quindi $x\in A$, cioè $x=a^n, n\in \mathbb{N}$. Questo implica che $w=xa=a^na=a^{n+1}$, utilizzando la definizione ricorsiva di potenza di una stringa. Quindi $w=a^{n+1}\in A$ per definizione ricorsiva di $A$, essendo $n+1\in \mathbb{N}$.
> Avendo provato il passo induttivo, per il principio di induzione strutturale, $\{a\}^*\subseteq A$.
>
> -   Proviamo ora che $A=\{a^n\ |\ n\in\mathbb{N}\}\subseteq\{a\}^*$ con il _Principio di Induzione Matematico_. Precisamente sia $w=a^n\in A, n\in\mathbb{N}$ e dimostriamo che $a^n\in \{a\}^*$.
>
> **PASSO BASE**: Sia $n=0$. Allora, per il passo base della definizione di potenza di una stringa, $a^n=a^0=\lambda$ e sappiamo per il passo base della definizione ricorsiva di $\{a\}^*$ che $\lambda\in\{a\}^*$.
> Quindi il passo base è dimostrato.
>
> **PASSO INDUTTIVO**: Sia $k\geq0$ e supponiamo che $a^k\in\{a\}^*$. Dimostriamo che $a^{k+1}\in\{a\}^*$.
> Per definizione ricorsiva di potenza, $a^{k+1}=a^ka$. Siccome per ipotesi induttiva, $a^k\in\{a\}^*$, allora, applicando il passo ricorsivo della definizione ricorsiva di $\{a\}^*$, anche $a^ka=a^{k+1}$ appartiene all'insieme $\{a\}^*$.
> Avendo provato il passo induttivo, per il principio di induzione matematica, abbiamo provato che $A\subseteq\{a\}^*$.
>
> Dalle due inclusioni segue che $A=\{a\}^*$.

---

> [!example]- <font color="orange">Esempio su un albero</font>
> In un albero binario, un nodo si dice pieno se ha esattamente due figli. Mostrare la seguente affermazione $S(T)$.
>
> > $S(T)$: "Per ogni albero binario pieno, il numero di foglie è uguale al numero di nodi pieni più uno".
>
> È possibile dimostrarlo, utilizzando il principio di induzione strutturale.
> Possiamo notare che l'affermazione si può riscrivere come
> $$S(T): \color{#CC241D}f(T)=n(T)+1$$
> Dove $n(T)$ rappresenta i nodi interni di $T$, mentre $f(T)$ rappresenta le foglie di $T$.
>
> **BASE**: Sia $T=(\{r\},\emptyset)$ un albero dotato di un solo nodo. In questo caso $f(T)=1, n(T)=0$.
> Allora $f(T)=n(T)+1=0+1=1$.
> Il passo base è dimostrato.
>
> **PASSO INDUTTIVO**: Sia $T=(V,E)$ un albero binario pieno con $E\neq\emptyset$.
> Questo è costituito a partire da due alberi binari $T_1=(V_1, E_1)$ e $T_2=(V_2, E_2)$.
> Poiché $T_1$ e $T_2$ sono alberi binari, possiamo applicare l'ipotesi induttiva e quindi $S(T_1)$ e $S(T_2)$ sono vere.
> $$f(T_1)=n(T_1)+1\ \land\ f(T_2)=n(T_2)+1$$
> Poiché $T$ è pieno, allora per la definizione ricorsiva dei nodi interni di un albero abbiamo che $$\color{#61AFEF}n(T)=n(T_1)+n(T_2)+1$$
> Quindi:
> $\textcolor{#CC241D}{f(T)}=f(T_1)+f(T_2)=\textcolor{#61AFEF}{n(T_1)+1+n(T_2)}+1=\textcolor{#CC241D}{n(T)+1}$
> Allora $S(T)$ è vera ed il passo induttivo è dimostrato.
>
> L'affermazione è dimostrata in base al principio di induzione strutturale.

___
[[000 Indice MMI|↖ Ritorna all'indice ↖]]

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

