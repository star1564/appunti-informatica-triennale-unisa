---
aliases:
  - Classe Polinomiale
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: Complessità di Tempo Polinomiale
cssclasses:
  - 
---
>La **Classe $P$** è l'insieme dei linguaggi $L$ per i quali esiste una Macchina di Turing deterministica $M$ *con un solo nastro* che [[024 Macchine Decisionali|decide]] $L$ in *[[054 Tempo Polinomiale ed Esponenziale|tempo polinomiale]]* $O(n^k)$ per qualche $k\geq 1$, ovvero $$P=\bigcup_{k\geq 1}TIME(n^k)$$

> [!example]- <font color="orange">Esempio</font>
> $$L=\{0^k1^k\ |\ k\geq 0\}\in P$$

> [!info]+ Proposizione
> La classe $P$ è [[Chiusura di un_opearzione|chiusa]] rispetto al [[013 Complemento di un Insieme|complemento]].
> 
> Sia $L\in P$ , quindi esiste una Macchina di Turing $T$ che decide in tempo polinomiale se un input $x\in L$. Possiamo descrivere la seguente Macchina di Turing per vedere se $x\in\overline{L}$ [^1].
> 
> > [!note] Descrizione di $M$
> >Sull'input $x$:
> >1. Simula $T$ su input $x$
> >2. ➜ Se $T$ accetta $x$, $M$ rifiuta $x$
> >➜ Se $T$ rifiuta $x$, $M$ accetta $x$
>
>La macchina $M$ rientra ancora nel tempo polinomiale, quindi $\overline{L}\in P$.

[^1]: https://cs.stackexchange.com/questions/105254/prove-pspace-is-closed-under-complement

La classe $P$ gioca un ruolo centrale nella teoria della complessità ed è importante perché:
1. $P$ corrisponde, con una certa approssimazione, alla classe di problemi che sono *realisticamente risolubili* mediante programmi su computer reali. Quindi $P$ è una classe rilevante dal punto di vista pratico.
2. $P$ è una classe robusta dal punto di vista matematico. Infatti, per via del [[053 Relazioni di complessità tra modelli#Multinastro|teorema 7.8]], *$P$ è invariante* per tutti i modelli di computazione che sono polinomialmente equivalenti alla Macchina di Turing deterministica a nastro singolo.


> [!tip]+ Algoritmo in tempo polinomiale
>Per mostrare che un algoritmo può essere eseguito in tempo polinomiale $O(n^k)$ da $M$, su input di lunghezza $n$, dobbiamo:
>1. Fornire un [[006 Insieme Illimitato e Limitato#Insieme Limitato Superiormente|limite superiore]] al numero dei passi eseguiti dall'algoritmo che sia *polinomiale in $n$*
>2. Mostrare che ogni passo può essere eseguito in tempo *polinomiale in $n$* da $M$ o da qualsiasi "ragionevole" modello di computazione deterministico equivalente
>
>Infatti, esiste $h\in\mathbb{N}$ tale che ogni passo può essere eseguito in tempo $O(n^h)$. Se il numero dei passi eseguiti dall'algoritmo è $O(n^c)$, l'algoritmo potrà essere eseguito in tempo $O(n^{h+c})$, su input di lunghezza $n$.

> [!note]+ Precisazione slla "composizione di polinomi"
>In generale questa frase si riferisce alla seguente situazione:
>>$N$ è un algoritmo che, su un input di lunghezza $n$, simula una Macchina di Turing $M_1$ con complessità di tempo polinomiale $O(n^k)$, poi sull'output simula una Macchina di Turing $M_2$ con complessità di tempo polinomiale $O(m^t)$.
>
>$$N:w\to\boxed{M_1}\to\boxed{M_2}$$
>
>Si conclude che $N$ è un algoritmo con complessità di tempo polinomiale.
>
>Infatti, il numero di passi di $N$ su un input di lunghezza $n$ è uguale al numero di passi di $M_1$ su tale input più il numero di passi di $M_2$ sull'output di $M_1$.
>La *lunghezza dell'output* di $M_1$ non è necessariamente $n$ ma *è limitata dalla complessità in tempo di $M_1$*.
>
>Per $t(n)\geq n$, qualsiasi macchina che opera in tempo $t(n)$ può visitare al massimo $t(n)$ celle, perché una macchina può esplorare al massimo una nuova cella ad ogni passo della propria esecuzione.
>
>Se l'input di $M_1$ (e di $N$) ha lunghezza $n$, il corrispondente output di $M_1$ ha lunghezza $O(n^k)$.
>
>- Il numero di passi di $M_1$ su input di lunghezza $n$ è $O(n^k)$. Quindi la computazione di $M_1$ su un input $x$ di lunghezza $n$ è eseguita in $q(n)$ passi, per qualche polinomio $q$.
>
>- Il numero di passi di $M_2$ sull'output $y$ di $M_1$, di lunghezza $|y|\leq q(n)$, è $P(q(n))$ per qualche polinomio $P$.
>  
> Quindi il numero di passi di $N$ su input di lunghezza $n$ è polinomiale.

> [!example]- <font color="orange">Esempio PATH</font>
> Diciamo che $PATH$ è l'insieme di tutti i [[012 Grafi#Grafi Diretti (Orientati)|Grafi Orientati]] in cui c'è un cammino da $s$ verso $t$.
> $$PATH=\{\langle G,s,t \rangle\ |\ G \text{ è un grafo orientato in cui c'è un cammino da }s\text{ verso }t\}$$
> 
> >$PATH\in P$
>
>Una generazione esaustiva dei cammini di $G$ con un algoritmo di forza bruta condurrebbe ad un algoritmo di complessità esponenziale. Infatti, se $V$ è l'insieme dei nodi in $G$, bisogna generare tutti i sottoinsiemi di $V$ di cardinalità maggiore o uguale di $2$. Tale numero è $O(2^{|\langle G,s,t \rangle|})$.
>
>Il seguente algoritmo $M$ (ovvero la Macchina di Turing equivalente ad $M$) decide $PATH$ in tempo deterministico polinomiale.
>
>> [!note] Descrizione di $M$
>> Sull'input $\langle G,s,t \rangle$, dove $G$ è un grafo con nodi $s$ e $t$:
>> 1. Marca il nodo $s$
>> 2. Ripete questa operazione finché nessun nuovo vertice viene marcato:
>> 3. $\quad$Scansiona tutti gli archi di $G$. Se trova un arco $(a,b)$ che va da un nodo $a$ marcato ad un nodo $b$ non marcato, marca il nodo $b$.
>> 4. Se $t$ è marcato, accetta. Altrimenti rifiuta.
>
>- Le fasi 1 e 4 vengono eseguite una volta.
>- Sia $m$ il numero dei vertici di $G$. Il passo 3 viene eseguito al massimo $m$ volte perché viene eseguito ogni volta che viene marcato un nuovo nodo in $G$ e ne posso marcare al massimo $m$.
>
>Quindi il numero totale dei passi è al massimo $1+1+m=O(m)=O(m^1)$, che è polinomiale nella lunghezza dell'input.

> [!example]- <font color="orange">Esempio RELPRIME</font>
>Diciamo che $RELPRIME$ è composto dalle codifiche di due numeri interi positivi $x$ e $y$ che sono *relativamente primi* ([[047 Numeri Coprimi|coprimi]]).
>
>$$RELPRIME=\{\langle x,y \rangle\ |\ x,y \text{ sono interi positivi relativamente primi}\}$$
>
>>$RELPRIME \in P$
>
>L'algoritmo che permette di provare che $RELPRIME \in P$ è basato sull'[[044 Algoritmo delle Divisioni Successive|Algoritmo di Euclide]] per calcolare il [[045 Massimo Comun Divisore|Massimo Comun Divisore]] $MCD(x, y)$ di due numeri interi non negativi $x, y$.
>
>> [!note] Descrizione di $R$
>> Sull'input $\langle x,y \rangle$, dove $x$ e $y$ sono numeri naturali in binario:
>> 1. Simula $MCD$ su $\langle x,y \rangle$
>> 2. Se il risultato è 1, accetta. Altrimenti rifiuta.
>
>$R$ (ovvero la Macchina di Turing equivalente ad $R$) decide $RELPRIME$ in tempo deterministico polinomiale.


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