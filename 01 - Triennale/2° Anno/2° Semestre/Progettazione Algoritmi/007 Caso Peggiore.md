---
aliases: 
tags:
  - corsi/informatica/progettazione_algoritmi
paragrafo: 
cssclasses:
  - 
---
Si userà come strumento di [[006 Analisi di Algoritmi|Analisi di Algoritmi]] il **Caso Peggiore**, ovvero il tempo usato dall'algoritmo nella situazione più "*sfavorevole*" in input.

La complessità di tempo di un algoritmo $A$, su input di taglia $n$, che si denota con $T(n)$, sarà: 
> $T(n)$: Il massimo tempo richiesto da $A$, dove il massimo è calcolato su tutti i possibili input di taglia $n$.

Ciò darà un [[006 Insieme Illimitato e Limitato#Insieme Limitato Superiormente|limite superiore]] alla complessità dell'algoritmo $A$, su tutti i possibili input, ovvero che $A$ non richiederà mai tempo superiore a $T(n)$ su di un qualsivoglia input di taglia $n$.

Indicheremo con [[003 Notazione O|O(g(n))]] il tempo nel caso peggiore di un algoritmo. Infatti:
- *Caso Peggiore*: Se diciamo che la complessità $T(n)$ di un algoritmo $A$ è $\color{#61AFEF}O(g(n))$, vuol dire che, avendo qualsivoglia input di dimensione $n$, l'algoritmo $A$ non richiede mai tempo superiore a $c\cdot g(n)$, per produrre il suo output, per $c$ opportuna e $n$ sufficientemente grande. Quindi non esistono input per cui l'algoritmo richiede tempo $> c\cdot g(n)$.
![[Pasted image 20230303160415.png|600]]
- *Caso Migliore*: Se diciamo invece che la complessità $T(n)$ di un algoritmo $A$ è $\color{#61AFEF}\Omega(g(n))$, vuol dire che $A$ richiede almeno tempo $c\cdot g(n)$, per produrre il suo output, per $c$ opportuna e $n$ sufficientemente grande. Quindi esiste almeno un input di taglia $n$ su cui $A$ richiede tempo $c\cdot g(n)$.
![[Pasted image 20230311093016.png|600]]
- *Caso Mediano*: $\color{#61AFEF}\Theta(g(n))$.

---
### CALCOLARE IL CASO PEGGIORE NEGLI ALGORITMI
#### Operazioni elementari
```C
x = y + z; // Tempo costante = c
```

Il costo delle istruzioni semplici (come assegnazione, lettura/scrittura di variabili, ...) è $O(1)$, ovvero una costante che *non dipende dall'input*;

In questo caso abbiamo un totale di $\color{#CC241D}c = O(1)$. 

#### Cicli
```C
for(i = 1; i <= n; i++) // Tempo lineare = n
	x = y + z; // Tempo costante = c
```

Il costo delle iterazioni `FOR, WHILE` è pari alla *somma su tutte le iterazioni del costo di ogni iterazione*.

In questo caso si deve contare quante volte viene eseguito il corpo del ciclo: viene eseguito $n$ volte.
Moltiplichiamo la complessità del ciclo per la complessità del corpo. 
Abbiamo quindi $\color{#CC241D}c\cdot n=O(n)$.


#### Cicli Annidati
```C
for(i = 1; i <= n; i++) // Tempo lineare = n
	for(j = 1; j <= n; j++) // Tempo lineare = n
		x = y + z; // Tempo costante = c
```

In questo caso moltiplichiamo la complessità di ogni ciclo. 
Abbiamo un totale di $\color{#CC241D}c\cdot n\cdot n = c\cdot n^2 = O(n^2)$.

#### Istruzioni Sequenziali
```C
1) a = a + b; // Tempo costante = c_1

2) for(i = 1; i <= n; i++) // Tempo lineare = n
		x = y + z; // Tempo costante = c_2

3) for(j = 1; j <= n; j++) // Tempo lineare = n
		c = d + e; // Tempo costante = c_2
```

Per delle istruzioni sequenziali, bisogna *sommare le complessità di ogni istruzione*. 
Abbiamo quindi $\color{#CC241D}c_1 + c_1 n + c_2 n=n+n=2n=O(n)$.

#### Istruzioni Condizionali
Il costo delle istruzioni `IF ... ELSE` è pari al tempo per effettuare il test (tipicamente $O(1)$) più $O($costo della *alternativa più costosa*$)$.

```C
if(condizione){
	// Diciamo che qui si trova qualcosa con complessità O(n)
}
else {
	// Diciamo che qui si trova qualcosa con complessità O(n^2)
}
```

In questo caso, consideriamo la condizione con la complessità peggiore, ovvero $\color{#CC241D}O(n^2)$.


---

> [!example]- <font color="orange">Esempio</font>
>*Input*:
>- Sequenza di interi $a = a[0]a[1]\dots a[n-1]$;
>- Numero $x$.
>
>*Output*:
>- La posizione $i$ se $x$ si trova in $a$;
>- $-1$ altrimenti.
>
>```C
>Scopri(a, n, x){
>	i = 0;
>	while(i < n && a[i] != x)
>		i++;
>	
>	if(i = n) 
>		return -1;
>	else 
>		return i;
>}
>```
>
>È chiaro che a seconda del caso che $x$ appaia o meno in $a$ (ed in quale posizione di $a$ esso appaia), l'algoritmo `Scopri(a, n, x)` può eseguire un qualunque numero di operazioni comprese tra $1$ ed $n$. Potremmo quindi dire che la complessità $T(n) = O(n)$.
>
>Però abbiamo deciso di calcolare la complessità in base al caso peggiore, quindi $T(n)=\Theta(n)$, in quanto nel caso peggiore (ad esempio nel caso in cui $x$ non appare in $a$) l'algoritmo esegue $\Omega(n)$ operazioni.

> [!example]- <font color="orange">Esempio</font>
>```C
>Alg(n){
>	x = 0;
>	if(n è pari)
>		return 0;
>	else {
>		for(i = 1; i < n+1; i++)
>			x++;
>		return x;
>	}
>}
>```
>
>Esiste un numero infinito di input su cui l'algoritmo esegue *un solo passo*. 
>Ciò che possiamo dire è che l'algoritmo non esegue mai più di $O(n)$ passi *per ogni possibile input*, e che esegue almeno $n$ passi *per certi input*.


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