---
aliases: 
tags:
  - corsi/matematica/probabilità_statistica
inglese: 
paragrafo: Distribuzioni V.A. Discrete
cssclasses:
  - 
---
>La [[024 Funzione di Distribuzione|distribuzione]] **Binomiale con parametri $(n,p)$** di una [[023 Variabili Aleatorie|Variabile Aleatoria]] $X$ rappresenta *il numero di successi ottenuti da $n$ [[009 Esperimento|esperimenti]] ripetuti in modo indipendente* su una [[028 Distribuzione di Bernoulli|Variabile Aleatoria di Bernoulli]].

Quindi, una variabile aleatoria di Bernoulli è semplicemente una variabile aleatoria binomiale di parametri $(1,p)$.

> [!note]+ Setting per avere un Esperimento Binomiale
> 1. Il numero di esperimenti ($n$) deve essere costante
> 2. Ci sono solamente due risultati possibili per ogni esperimento (Bernoulli):
> 	- Successo
> 	- Fallimento
> 3. La probabilità per un successo deve essere costante per ogni esperimento
> 4. Ogni esperimento deve essere indipendente


#### Densità Discreta
>La sua [[025 Variabili Aleatorie Discrete#Densità Discreta|Densità Discreta]] può essere espressa in questo modo:
>$$\color{#CC241D}p(x)={n\choose x}\cdot p^x (1-p)^{n-x}$$
>Dove: 
>- $x=0,1,\ldots,n$
>- $0\leq p\leq 1$ è una qualsiasi probabilità
> 
>Si ha che:
>- $x$ è il numero di successi
>- $n$ è il numero di esperimenti

Ogni sequenza di $n$ esiti contenenti $x$ successi e $n-x$ fallimenti si verifica con probabilità $p^x (1-p)^{n-x}$, in modo simile alla Bernoulli, per via dell'indipendenza degli esperimenti. 

Segue allora il fatto che ci sono ${n\choose x}$ differenti sequenze di $n$ esiti contenenti $x$ successi e $n-x$ fallimenti, essendoci esattamente ${n\choose x}$ modi differenti di scegliere le $x$ prove in cui si verifichino i successi.

Quindi, per [[008 Utilizzare il Principio Fondamentale del Calcolo Combinatorio|il principio fondamentale del calcolo combinatorio]] si moltiplicano questi due valori per ottenere $p(x)$.

> [!example]+ <font color="orange">Esempio</font>
>>Determinare la densità discreta della variabile aleatoria $X$ che descrive il numero di volte che esce testa in $n=5$ lanci indipendenti di una moneta non truccata.
>
>$$\begin{align}p(x)&={5\choose x}\cdot \left( \frac{1}{2} \right)^x\cdot\left( 1-\frac{1}{2} \right)^{5-x}\\ &={5\choose x}\cdot \left( \frac{1}{2} \right)^x\cdot\left( \frac{1}{2} \right)^{5-x} \\ &= \text{[proprietà degli esponenti con stessa base]} \\ &={5\choose x}\cdot \left( \frac{1}{2} \right)^{x+5-x} \\ &={5\choose x}\cdot \left( \frac{1}{2} \right)^5 \\ &={5\choose x}\cdot \frac{1}{2^5} \\ &={5\choose x}\cdot \frac{1}{32}\end{align}$$
>
>- $p(0)=\frac{1}{32}=0,031$
>- $p(1)=\frac{5}{32}=0,1562$
>- $p(2)=\frac{10}{32}=0,3125$
>- $p(3)=\frac{10}{32}=0,3125$
>- $p(4)=\frac{5}{32}=0,1562$
>- $p(5)=\frac{1}{32}=0,031$
>
>![[Pasted image 20240820122613.png|600]]

> [!note] Proprietà
> La densità discreta sarà inizialmente strettamente crescente e successivamente strettamente decrescente, con massimo in corrispondenza del più grande intero $k\le (n+1)p$.

#### Momento di Ordine $k$
Il suo [[026 Valore Atteso di una VA Discreta#Momento di Ordine $n$|Momento di Ordine k]] è il seguente:
$$\color{#CC241D}E[X^k]=np\cdot E[(Y+1)^{k-1}]$$

Dove $Y$ è una variabile aleatoria binomiale di parametri $(n-1, p)$.

> [!quote]+ Dimostrazione
> Si utilizza la definizione di momento di ordine
> $$\begin{align}E[X^k]&=\sum_{x:p(x)>0} \Big(\textcolor{#61AFEF}{x^k}\cdot \textcolor{#e86162}{p(x)}\Big)\\ &= \sum_{i=1}^{n} \left(\textcolor{#61AFEF}{i^k}\cdot \textcolor{#e86162}{{n\choose i}\cdot p^i(1-p)^{n-i}}\right)\end{align}$$
> 
> Utilizzando l'identità [[006 Coefficiente Binomiale#Altre|del coefficiente binomiale]] $$\color{#00C575}i{n\choose i}=n{n-1\choose i-1}$$
> e scomponendo $p^i$, si ottiene $$\begin{align}\sum_{i=1}^{n} \left(i^k\cdot {n\choose i}\cdot p^i(1-p)^{n-i}\right) &= \sum_{i=1}^{n} \left(i^{k-1}\cdot \textcolor{#00C575}{i {n\choose i}}\cdot \textcolor{#FF6611}{p^i}(1-p)^{n-i}\right)\\ &= \sum_{i=1}^{n} \left(i^{k-1}\cdot \textcolor{#00C575}{n {n-1\choose i-1}}\cdot \textcolor{#FF6611}{p\cdot p^{i-1}}(1-p)^{n-i}\right) \\ &= \text{[proprietà della sommatoria: moltiplicazione per costanti]} \\ &= \textcolor{#00C575}n\textcolor{#FF6611}p\cdot \left[ \sum_{i=1}^{n} \left(i^{k-1}\cdot {n-1\choose i-1}\cdot p^{i-1}(1-p)^{n-i}\right) \right]\\ &= \text{[si sostituisce } \textcolor{#fdaa39}{y=i-1} \text{]} \\ &= np\cdot \left[ \sum_{\textcolor{#fdaa39}{y=0}}^{\textcolor{#fdaa39}{n-1}} \left((\textcolor{#fdaa39}{y+1})^{k-1}\cdot {n-1\choose \textcolor{#fdaa39}{y}}\cdot p^{\textcolor{#fdaa39}{y}}(1-p)^{n-\textcolor{#fdaa39}{1-y}}\right) \right]\end{align}$$
> 
> Considerando $Y$ come un variabile aleatoria binomiale di parametri $(n-1,\ p)$, si ha che 
> $$\begin{align}E[X^k]&= np\cdot \left[ \sum_{y=0}^{n-1} \left(\underbracket{(y+1)^{k-1}}_{x^n}\cdot \underbracket{{n-1\choose y}\cdot p^{y}(1-p)^{n-1-y}}_{p(x)}\right) \right]\\ &=np\cdot E[(Y+1)^{k-1}]\end{align}$$

#### Valore Atteso
Il suo [[026 Valore Atteso di una VA Discreta|Valore Atteso]] è il seguente:
$$\color{#CC241D}E[X]=np$$

> [!quote]+ Dimostrazione
> Si pone $k=1$ nella formula del momento d'ordine $k$ $$E[X^k]=np\cdot E[(Y+1)^{k-1}]$$
> Quindi si ottiene $$E[X^1]=E[X]=np$$
> Così il valore atteso di successi che si verificano in $n$ prove indipendenti quando la probabilità di successo vale $p$, è pari a $np$.

#### Varianza
La sua [[027 Varianza di una VA Discreta|Varianza]] è la seguente:
$$\color{#CC241D}\text{Var}(X)=np\cdot(1-p)$$

> [!quote]+ Dimostrazione
> Si pone $k=2$ nella formula del momento d'ordine $k$ $$E(X^k)=np\cdot E[(Y+1)^{k-1}]$$
>
>Sapendo che $\color{#61AFEF}E[X]= np$, si ottiene $$\begin{align}E[X^2]&=np\cdot E[Y+1]\\ &= \text{[proprietà della linearità]} \\ &=np\cdot \left( E[Y]+1  \right) \\ &= \text{[} Y \text{ è una binomiale np con parametri } \textcolor{#00C575}{(n-1,\ p)} \text{]} \\ &= np\cdot[\textcolor{#61AFEF}{(n-1)\cdot p}+1]\end{align}$$
>
>Pertanto $$\begin{align}\text{Var}(X)&=E[X^2]-(E[X])^2\\ &=np\cdot [p\cdot(n-1)+1]-n^2p^2 \\ &= np\cdot[p\cdot(n-1)+1-np]\\ &=np\cdot [\cancel{ np }-p+1-\cancel{ np }]\\ &= np\cdot (1-p)\end{align}$$
> 
> Così la varianza del numero di successi che si verificano in $n$ prove indipendenti quando la probabilità di successo vale $p$, è pari a $np\cdot(1-p)$.
>
>Per $n=1$ la variabile aleatoria binomiale coincide con la variabile di Bernoulli. Infatti, si ha che $E[X]=p$ e $\text{Var}(X)=p\cdot(1-p)$.



___
[[000 Indice PS|↖ Ritorna all'indice ↖]]

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
- [Video (inglese), Statistics 110 - Binomiale np (Introduzione)](https://youtu.be/PNrqCdslGi4?list=PL2SOU6wwxB0uwwH80KTQ6ht66KWxbzTIo&t=2759)
- [Video (inglese), Statistics 110 - Binomiale np (Distribuzione con esempio)](https://youtu.be/k2BB0p8byGA?list=PL2SOU6wwxB0uwwH80KTQ6ht66KWxbzTIo&t=1942)
- [Video (inglese), Statistics 110 - Binomiale np (Valore Atteso)](https://youtu.be/LX2q356N2rU?list=PL2SOU6wwxB0uwwH80KTQ6ht66KWxbzTIo&t=1634)
- https://www.youtube.com/watch?v=nRuQAtajJYk&list=PL0KQuRyPJoe6KjlUM6iNYgt8d0DwI-IGR&index=25