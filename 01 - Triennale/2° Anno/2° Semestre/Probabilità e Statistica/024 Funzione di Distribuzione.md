---
aliases:
  - Cumulative Distribution Function
  - CDF
tags:
  - corsi/matematica/probabilità_statistica
paragrafo: Variabili Aleatorie
cssclasses:
  - 
---
>Data una [[023 Variabili Aleatorie|Variabile Aleatoria]] $X$, la **Funzione di Distribuzione** (di ripartizione, Cumulative Distribution Function, CDF) di $X$ è la seguente [[011 Funzioni|funzione]] $$\textcolor{#CC241D}{F(x)=\mathbb{P}(X\leq x)},\quad -\infty<x<\infty$$

Possiamo notare che, $X\leq x$ è un [[011 Evento|Evento]], quindi ha senso parlare della sua probabilità.
Quindi, la funzione di distribuzione $F(x)$ di una variabile aleatoria $X$ rappresenta la probabilità che $X$ assuma un valore minore o uguale a $x$, per ogni $x \in \mathbb{R}$.

### Calcolare la Funzione
Se la variabile è [[034 Variabili Aleatorie Continue|continua]], la CDF si ottiene con l'[[066 Integrali|integrale]], altrimenti, se è [[025 Variabili Aleatorie Discrete|discreta]], si ottiene con la [[Sommatoria|sommatoria]].

### Caratteristiche di una Funzione di Distribuzione
Queste proprietà contraddistinguono una funzione di distribuzione, nel senso che se una funzione $F(x)$ soddisfa tali proprietà allora esiste una variabile aleatoria che ammette $F(x)$ come funzione di distribuzione:
1. $F(x)$ è una [[015 Funzione Monotona|funzione monotona non decrescente]], ovvero $F(a)\leq F(b)$ se $a<b$
2. $\lim\limits_{x \to +\infty}F(x)=1$
3. $\lim\limits_{x \to -\infty}F(x)=0$
4. $F(x)$ è SEMPRE [[035 Funzione Continua|continua A DESTRA]], ovvero che $\lim\limits_{h \to 0^+}F(x+h)=F(x)$ per ogni $x\in \mathbb{R}$ fissato
5. $F(x)\geq 0, \forall x \in \mathbb{R}$
6. In base al [[023 Variabili Aleatorie#Tipi di variabili aleatorie|tipo di variabile aleatoria]], può essere continua o non

![[Pasted image 20250313100016.png|650]]

> [!note] Osservazione
> Ad ogni $x$ si aggiunge alla probabilità precedente la probabilità di $X\leq x$.
> 
> Formalizzazione nelle [[#Proprietà|proprietà]].


> [!example]- <font color="orange">Altri Esempi</font>
> ![[Pasted image 20240819111157.png]]

> [!quote]+ Dimostrazione
> 1. Dato $a<b$, allora $$\{X\leq b\}=\{X\leq a\}\cup \{a< X\leq b\}$$ con i due eventi a secondo membro incompatibili. Per la proprietà di additività finita: $$F(b)=\mathbb{P}(X\leq b)=\textcolor{#61AFEF}{\mathbb{P}(X\leq a)+\mathbb{P}(a<X\leq b)}\geq F(a)$$
>    ![[Pasted image 20250313112238.png|600]]
> 2. Notiamo che se $\{x_n\}_{n\geq 1}$ è una [[029 Successioni|successione]] di reali che cresce verso $\infty$, allora gli eventi $\{X\leq x_n\}$ formano una [[015 Successione di Eventi|successione crescente di eventi]] il cui limite è $\{X<\infty\}$. Quindi per la proprietà di continuità della probabilità, si ha $$\lim\limits_{x \to \infty}F(x)=\lim\limits_{n \to \infty}\mathbb{P}(X\leq x_n)=\mathbb{P}(X<\infty)=1$$
> 3. Simile alla 2, ma per $\mathbb{P}(X< -\infty)=0$.
> 4. Notiamo che se $\{x_n\}_{n\geq 1}$ è una successione decrescente che converge a $x\in\mathbb{R}$, allora $\{X\leq x_n\}$, con $n\geq 1$, costituisce una successione di eventi decrescente il cui limite coincide con $\{X\leq x_n\}$. Quindi, per la proprietà di continuità si ha $$\lim\limits_{h \to 0^+}F(x+h)=\lim\limits_{n \to \infty}\mathbb{P}(X\leq x_n) = \mathbb{P}(X\leq x)=F(x)$$





### Proprietà
1. $\color{#61AFEF}\mathbb{P}(a<X\leq b)=F(b)-F(a)$ per ogni $a<b$ 

Si ha che $\mathbb{P}(X<b)=\lim\limits_{n \to \infty}F(x_n)$ per ogni $b\in \mathbb{R}$ fissato e per ogni successione crescente $\{x_n\}_{n\geq 1}$ che [[032 Limiti di Successioni|converge]] a $b$. Quindi $\mathbb{P}(X<x)=\lim\limits_{h \to 0^-}F(x+h)$, da cui derivano le seguenti:

2. $\color{#61AFEF}\mathbb{P}(X\leq x)=F(x)$ per via della definizione di funzione di distribuzione
   
   ![[Pasted image 20250313112352.png|600]]
   
3. $\color{#61AFEF}\mathbb{P}(X> x)=1 - \mathbb{P}(X\leq x) = 1 - F(x)$
   
   ![[Pasted image 20250313112410.png|600]]
   
4. $\color{#61AFEF}\mathbb{P}(X< x)=\mathbb{P}(X\leq x^-)=F(x^-)$
   
   ![[Pasted image 20250313113139.png|600]]
   
5. $\color{#61AFEF}\mathbb{P}(X = x)=\mathbb{P}(X\leq x)-\mathbb{P}(X< x)=F(x)-F(x^-)$
	- Questo valore può essere $0$ se la funzione è continua in $x$
	- Questo valore può essere $>0$ se la funzione è discontinua
	
	![[Pasted image 20250313113601.png|600]]
	

6. $\color{#61AFEF}\mathbb{P}(a\leq X \leq b)=\mathbb{P}(X\leq b)-\mathbb{P}(X< a)=F(b)-F(a^-)$
   
   ![[Pasted image 20250313115326.png|600]]
# %%END%%

> [!example]- <font color="orange">Esempio</font>
>Sia $X$ una variabile aleatoria avente la seguente funzione di distribuzione.
>![[Pasted image 20240819112618.png]]
>
>Si possono calcolare le seguenti:
>- $\mathbb{P}(X<2)=F(2^-)= \frac{3}{4}$
>- $\mathbb{P}(X=1)=F(1)-F(1^-)= \frac{3}{4} - \frac{1}{2} = \frac{1}{4}$
>- $\mathbb{P}\left( X> \frac{1}{2} \right)= 1 - \mathbb{P}\left( X\leq \frac{1}{2} \right) =1 - F\left( \frac{1}{2} \right) = 1 - \frac{1}{4} = \frac{3}{4}$
>- $\mathbb{P}(1<X\leq 3)= \mathbb{P}(X\leq 3) - \mathbb{P}(X\leq1) = F(3) - F(1) = 1 - \frac{3}{4} = \frac{1}{4}$
>- $\mathbb{P}(1\leq X \leq 2)= \mathbb{P}(X\leq 2) - \mathbb{P}(X<1)= F(2) - F(1^-)= 1 - \frac{1}{2} = \frac{1}{2}$
>- $\mathbb{P}\left( \frac{X>1}{X> \frac{1}{2}}\right)=\frac{\mathbb{P}(X>1)}{\mathbb{P}\left( X> \frac{1}{2} \right)}= \frac{1-\mathbb{P}(X\leq1)}{3/4}=1-\frac{F(1)}{3/4}=\frac{1/4}{3/4}=\frac{1}{3}$



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
- [Video (inglese), Statistics 110 - Funzione di Distribuzione (CDF)](https://youtu.be/k2BB0p8byGA?list=PL2SOU6wwxB0uwwH80KTQ6ht66KWxbzTIo&t=660)
- [Video (inglese), Statistics 110 - Funzione di Distribuzione (Definizione e proprietà)](https://youtu.be/LX2q356N2rU?list=PL2SOU6wwxB0uwwH80KTQ6ht66KWxbzTIo&t=78)