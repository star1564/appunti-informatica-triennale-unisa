---
aliases:
  - Big O
  - O-grande
tags:
  - corsi/informatica/progettazione_algoritmi
paragrafo: Notazioni Asintotiche
cssclasses:
  - 
---
Siano $f$ e $g$ due [[011 Funzioni|funzioni]] dai naturali ai reali positivi, ovvero $f:n\in\mathbb{N}\to f(n)\in\mathbb{R}_+$, $g:n\in\mathbb{N}\to g(n)\in\mathbb{R}_+$.

>Diremo che **$\color{#CC241D}f(n)=O(g(n))$**, ovvero che la funzione $f(n)$ è $O(g(n))$, se e solo se esistono due costanti positive $c>0$ e $n_0\in \mathbb{N}$ tale che $$\color{#61AFEF}f(n)\leq c\cdot g(n),\quad\forall n\geq n_0$$

> [!tip]+ Definizione informale
> È vero che $f(n) = O(g(n))$ se la funzione $f(n)$ non cresce più velocemente della funzione $g(n)$.
>
> La notazione $O$ ne [[006 Insieme Illimitato e Limitato#Insieme Limitato Superiormente|limita superiormente]] la crescita e pertanto fornisce una indicazione della *velocità massima* dell'algoritmo. Si dice quindi che la notazione $O$ è il *limite superiore asintotico* della funzione $f(n)$.
>
>Graficamente, abbiamo che il grafico della funzione $f(n)$, dal punto $n_0$ in poi, si trova sempre *sotto* il grafico della funzione $c\cdot g(n)$, per qualche costante $c>0$.
>
>![[Pasted image 20230303160415.png|700]]

Dove, al posto di $g(n)$, si deve inserire una delle seguenti **espressioni $O$** (le funzioni più in alto sono quelle più lente a crescere, mentre quelle più in basso sono le più veloci):

| Espressione $O$       | Nome               |
| --------------------- | ------------------ |
| $O(1)$                | costante           |
| $O(log\ log\ n)$      | log log            |
| $O(log\ n)$           | logaritmico        |
| $O(\sqrt[c]{n}), c>1$ | sublineare         |
| $O(n)$                | lineare            |
| $O(n\ log\ n)$        | n log n            |
| $O(n^2)$              | quadratico         |
| $O(n^3)$              | cubico             |
| $O(n^k), k\geq1$      | polinomiale        |
| $O(2^n)$              | 2 con esponenziale |
| $O(3^n)$              | 3 con esponenziale |
| $O(a^n), a>3$         | esponenziale                   |
| $O(n!)$               | fattoriale         |
| $O(n^n)$              | n^n                |

^413852




---
### COME CALCOLARE LA COMPLESSITÀ $O$
#### Funzione Normale
Per ogni $k$ costante vale che $$\color{#CC241D}a_kn^k+a_{k-1}n^{k-1}+\cdots+a_1n+a_0 =O(n^k)$$
> [!example]- <font color="orange">Esempio con Funzione</font>
>Siano $f(n) = 2n^2+3n+6$, $g(n) = n^2$ e tentiamo di mostrare che $f(n) = O(n^2)$.
>Occorrerà quindi provare che esistono costanti $c>0$ e $n_0\in\mathbb{N}$ per cui $\color{#61AFEF}2n^2+3n+6\leq c\cdot n^2$, $\forall n\geq n_0$.
>
>A tal fine osserviamo che $2n^2+\textcolor{#FF6611}{3n}+6\leq 2n^2+\textcolor{#FF6611}{3n^2}+\textcolor{#00C575}{6}\leq 2n^2+3n^2+\textcolor{#00C575}{n^2}=\textcolor{orange}{6}n^2$, per ogni valore di $n$ per cui $n^2\geq\color{orange}6$, ovvero per ogni valore di $n\geq\color{#61AFEF}3$.
>Questo perché: 
>- $\textcolor{#FF6611}{3n}\leq\textcolor{#FF6611}{3n^2}$;
>- $\textcolor{#00C575}{6}\leq\textcolor{#00C575}{n^2}$ (con $n\geq3$).
>
>Quindi le costanti $c$ e $n_0$ che cercavamo sono $c=\color{orange}6$ e $n_0 = \color{#61AFEF}3$.
>Ovviamente, possono esistere anche altri valori di $c$ e $n_0$ per cui la disuguaglianza $f(n)\leq c\cdot n^2$, per ogni $n\geq n_0$ sia soddisfatta. Tuttavia, al fine di provare che $f(n)=O(n^2)$ ci basta aver provato che $c=6$ e $n_0=3$ vanno bene.
>---
>Quindi, se prendiamo in considerazione una funzione come $7n^{10}+8n^5+5=O(n^{10})$, la velocita di crescita della funzione *non è maggiore di quella $O$*. Infatti se consideriamo il rapporto tra le due funzioni otteniamo $$\frac{7n^{10}+8n^5+5}{n^{10}}=7+\frac{8}{n^5}+\frac{5}{n^{10}}$$ ed entrambi i termini $\frac{8}{n^5}$ e $\frac{5}{n^{10}}$ [[F04 Funzione Razionale-Frazione|tendono a zero al crescere]] di $n$, pertanto $8n^5$ e $5$ sono entrambi trascurabili rispetto a $n^{10}$, quando $n$ è molto grande.

> [!quote] Dimostrazione
> $a_kn^k+a_{k-1}n^{k-1}+\cdots+a_1n+a_0\ \leq |a_k|n^k+|a_{k-1}|n^{k-1}+\cdots+|a_1|n+|a_0|$
> $\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\leq |a_k|n^k+|a_{k-1}|n^k+\cdots+|a_1|n^k+|a_0|n^k$
> $\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\leq (|a_k|+|a_{k-1}|+\cdots+|a_1|+|a_0|)\cdot n^k=c\cdot n^k$

L'essenza di questa formula consiste nel fatto che, se vogliamo descrivere la velocità di crescita di un polinomio, *basterà concentrarci solo sul suo termine di grado massimo*, eliminando le costanti $a_k$ e i termini di grado minori di quello massimo.

> [!example]- <font color="orange">Esempio con Codice</font>
>Si vuole analizzare la complessità del seguente codice, che prende in input una sequenza $a=a[0]a[1]\cdots a[n-1]$ di $n$ numeri.
>```C
>int Algoritmo(int a[], int n){
>	int x = 0, y = 0;
>	for(int i = 0; i < n; i++){
>		for(int j = i; j < n; j++){
>			x = x + a[j];
>		}
>		y = y + i;
>	}
>	return x + y;
>}
>```
>
>Quando $i=0$, l'algoritmo esegue l'istruzione `x = x + a[j]` all'interno del secondo `for` per $n$ volte, quando $i=1$, l'algoritmo esegue quella istruzione per $n-1$ volte, quando $i=2$ lo esegue per $n-2$ volte, ecc.
>
>In totale il numero di operazioni effettuate sarà $$n+(n-1)+(n-2)+\cdots+2+1 = \sum_{i=1}^{n} i = \color{#61AFEF}\frac{n(n+1)}{2}$$
>Questa uguaglianza la si può provare con il [[046 Principio di Induzione Matematica#^2fedf4|principio di induzione]], oppure moltiplicando per $2$ e dimostrare il seguente: 
>$2\displaystyle\sum_{i=1}^{n} i = 1 +\quad 2\quad +\quad\quad 3 +\ \ \cdots +\ n +$
>$\quad\quad\quad\quad n+(n-1)+(n-2)+\cdots+1= (n+1)n$
>
>Il numero di operazioni `y = y + i` al di fuori del secondo `for` è $\color{#CC241D}n$, per cui il numero totale $T(n)$ di operazioni elementari che l'algoritmo esegue è pari a $$\textcolor{#61AFEF}{\frac{n(n+1)}{2}}+\color{#CC241D}n$$ per cui, applicando la definizione di $O$, possiamo dire che $T(n) = O(n^2)$.
>
>Questo perché $\frac{n(n+1)}{2}+n$ può essere semplificato così:
>$\frac{n(n+1)}{2}+n = \frac{(n^2+n)}{2}+n$
>$=\frac{(n^2+n+2n)}{2}=\frac{(n^2+3n)}{2}=\frac{1}{2}(n^2+3n)$
>Quindi, considerando solo il termine più grande, la funzione non cresce più di $n^2$.
>$\frac{1}{2}(n^2+3n)\leq\frac{1}{2}(n^2+3n^2)=\frac{1}{2}(4n^2)=\textcolor{orange}{2}\cdot n^2$, per ogni valore di $n$ per cui $n^2\geq\color{orange}2 = c$, ovvero per ogni valore di $n\geq\color{#61AFEF}\sqrt{2} = n_0$.

#### Funzione con Logaritmo
Per ogni costante $k$, è anche vero che (sfruttando le [[MB - 15 Logaritmi#Proprietà della Funzione Logaritmo|proprietà dei logaritmi]]) abbiamo $$\color{#CC241D}log(n^k)=O(n)$$
> [!example]- <font color="orange">Esempio con Funzione</font>
> $7n+8log\ n \leq 7n+8n = O(n)$
> ---
> $7n+8log\ n^3 \leq 7n+8\cdot3n = 7n+24n = O(n)$

> [!quote] Dimostrazione
>Proviamo che $log\ n = O(n)$, ovverosia proviamo che esiste $c > 0$, $n_0\geq0$  tale che $log\ n \leq cn$, per ogni $n \geq n_0$. Lo proveremo [[046 Principio di Induzione Matematica|per induzione]] su $n$, con costanti
>$c = 1 = n_0$. Per $n = 1$ abbiamo che $log\ 1 = 0 \leq 1$. Supposto vero $log n \leq n$, proviamo che $log(n + 1) \leq n + 1$. Avremo:
>
>$log(n + 1)\ \leq log(n+n)$
>$\quad\quad\quad\quad\quad = log(2n)$
>$\quad\quad\quad\quad\quad = log\ 2+log\ n$
>$\quad\quad\quad\quad\quad \leq 1+n$ (dall'ipotesi induttiva)

---
> [!summary]+ Tabella riassuntiva
> È utile ricordarsi le seguenti relazioni generali.
> 
>| Relazione                                       |
>| :-----------------------------------------------: |
>| $a_kn^k+a_{k-1}n^{k-1}+\cdots+a_1n+a_0 =O(n^k)$ |
>| $log(n^k)=k\cdot O(n)$                                 |
>| $log(n)=O(\sqrt{n})$                            |
>| $\sqrt{n}=O(n)$                                 |
>| $n^k = O(n^{k+1})$                              |
>| $n^k = O(2^n)$                                  |
>| $2^n=O(n!)$                                     |
>| $n!=O(n^n)$                                     |
>Le seguenti regole sono utili per confrontare le funzioni tra di loro.
>![[Pasted image 20230304122655.png|700]]



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