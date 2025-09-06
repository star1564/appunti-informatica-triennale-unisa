---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Limiti di Successioni
cssclasses:
  - 
---

## Teoremi generali sulle Successioni
### Teorema dell'unicità del Limite
>Una [[032 Limiti di Successioni#Convergente|successione convergente]] ha un solo limite e questo è unico.


> [!success]- Dimostrazione (pL 65-66 EAM1)
> Supponiamo per assurdo che esistano due limiti distinti per la successione $a_n$, ovvero 
> $$\lim\limits_{n \to \infty}a_n=a, \quad \lim\limits_{n \to \infty}a_n=b,\quad a\neq b$$
> Poniamo $\epsilon = \frac{|a-b|}{2}$. Si ha che: 
> $$\exists \nu_1: |a_n -a| < \epsilon,\quad \forall n > \nu_1$$
> $$\exists \nu_2: |a_n -b| < \epsilon,\quad \forall n > \nu_2$$
> Ponendo $\nu=\max(\nu_1, \nu_2)$, le relazioni sopra descritte valgono contemporaneamente e si ha (utilizzando $|x_1+x_2|\leq |x_1| + |x_2|$):
> $$\begin{align*} |a-b| = |(a-a_n) + (a_n -b)| &\leq |a-a_n| + |a_n-b|  \\ &= |a_n - a|+|a_n-b|< \epsilon +\epsilon \\ &= |a-b| \end{align*}$$
> Abbiamo così trovato che $|a-b| < |a-b|$, che è assurdo.

### Limitatezza delle Successioni Convergenti
>Ogni [[032 Limiti di Successioni#Convergente|successione convergente]] è [[030 Successioni Limitate ed Illimitate#Successione Limitata|limitata]].

> [!success]- Dimostrazione (pL 67-68 EAM1)
> Supponiamo che $a_n$ converga ad $a$ e scegliamo $\epsilon = 1$. Per via della definizione di limite di una successione convergente, esiste un indice $\nu$ per cui $|a_n-a|<1, \forall n>\nu$.
> Quindi si ha (utilizzando $|x_1+x_2|\leq |x_1| + |x_2|$):
> $$|a_n| = |(a_n-a)+a|\leq |a_n-a| +|a| <1 +|a|,\quad n>\nu$$
> Ma allora, $\forall n \in \mathbb{N}$, si ha:
> $$|a_n| \leq M = \max(|a_1|, |a_2|, \dots, |a_\nu|, 1+|a|)$$

## Teoremi di Confronto
Qui si scrivono alcune relazioni tra limiti e ordinamento.

### Teorema della Permanenza del Segno
>Se $\lim\limits_{n \to +\infty}a_n=a>0$ allora esiste $\nu\in\mathbb{R}$ tale che $\forall n>\nu$ risulta che $a_n>0$.

> [!success]- Dimostrazione (pL 71 EAM1)
> Dato che $a>0$, possiamo scegliere $\epsilon = \frac{a}{2}$. Esiste quindi un numero $\nu$ per cui $|a_n-a| < \frac{a}{2}$ per ogni $n>\nu$. Ciò equivale a $-\frac{a}{2}<a_n-a< \frac{a}{2}$. In particolare abbiamo:
> $$a_n>a- \frac{a}{2} = \frac{a}{2} > 0, \quad \forall n > \nu$$


### Corollario 4.1
>Se $\lim\limits_{n \to +\infty}a_n=a$, e se $a_n\geq 0, \forall n$, allora anche $a\geq 0$

> [!success]- Dimostrazione (pL 71 EAM1)
> Se per assurdo fosse $a<0$, il [[#Teorema della Permanenza del Segno|teorema della permanenza del segno]], applicato alla successione $-a_n$, comporterebbe che $a_n< 0$ per $n$ grande.

### Corollario 4.2
>Se $\lim\limits_{n \to +\infty}a_n=a$ e $\lim\limits_{n \to +\infty}b_n=b$, e se $a_n\geq b_n, \forall n$, allora $a\geq b$

> [!success]- Dimostrazione (pL 71 EAM1)
> Applicare il corollario [[#Corollario 4.1|precedente]] alla successione $a_n-b_n$.

### Teorema dei Carabinieri
>Siano $a_n, b_n, c_n$ tre successioni verificanti la relazione $$a_n\leq c_n\leq b_n,\quad \forall n\in\mathbb{N}$$
>Se $\lim\limits_{n \to +\infty}a_n=\lim\limits_{n \to +\infty}b_n=a$, allora anche la successione $c_n$ è convergente ed il suo limite è $\lim\limits_{n \to +\infty}c_n=a$.

> [!success]- Dimostrazione (pL 72 EAM1)
> Per ipotesi, per ogni $\epsilon>0$:
> $$\exists \nu_1: |a_n -a| < \epsilon,\quad \forall n > \nu_1$$
> $$\exists \nu_2: |b_n -a| < \epsilon,\quad \forall n > \nu_2$$
> Ricordiamo che le disuguaglianze con il valore assoluto si possono anche scrivere come:
> $$a-\epsilon < a_n < a+\epsilon$$
> $$a-\epsilon < b_n < a+\epsilon$$
> Quindi, se $n>\nu=\max(\nu_1,\nu_2)$, risulta che:
> $$a-\epsilon < a_n \leq c_n \leq b_n < a + \epsilon$$

## Altri Teoremi

### Teorema sulle Successioni Monotone
>Ogni [[031 Successioni Monotone|successione monotona]] ammette limite. In particolare, ogni successione monotona è limitata e convergente, cioè ammette limite finito.



> [!success]- Dimostrazione per le monotone crescenti (pL 79 EAM1)
> Consideriamo il caso di una successione $a_n$ [[031 Successioni Monotone#Successioni Monotone Crescenti|crescente]] e [[030 Successioni Limitate ed Illimitate#Successione Limitata|limitata]]. 
> Posto $l=\sup(a_n)$, fissato $\epsilon > 0$, per le proprietà dell'[[007 Estremo Inferiore e Superiore di un insieme#Estremo Superiore|estremo superiore]] esiste un $\nu \in \mathbb{N}$ tale che 
> $$l-\epsilon < a_\nu$$
> Per $n>\nu$ risulta $a_\nu\leq a_n$, dunque:
> $$l-\epsilon < a_\nu \leq l< l+\epsilon$$
> da cui $$\lim\limits_{n \to +\infty} a_n = l$$
> 
> Consideriamo il caso di una successione $a_n$ crescente e [[030 Successioni Limitate ed Illimitate#Successione Illimitata|illimitata superiormente]]. 
> Fissato $M>0$ esiste $\nu\in \mathbb{N}$ tale che $a_l> M$. Dato che $a_n$ è crescente, per ogni $n>\nu$ risulta 
> $$a_n\geq a_\nu > M$$
> da cui $$\lim\limits_{n \to +\infty}a_n=+\infty$$
> 
> In modo analogo si trattano i casi relativi a [[031 Successioni Monotone#Successioni Monotone Decrescenti|successioni decrescenti]].


### Teorema di Bolzano-Weierstrass
>Sia $a_n$ una successione [[030 Successioni Limitate ed Illimitate#Successione Limitata|limitata]]. Allora esiste almeno una sua [[031 Successioni Monotone#Successioni Estratte|estratta]] che [[032 Limiti di Successioni#Convergente|converge]].

> [!success]- Dimostrazione (pL 86-87 EAM1)
> Per ipotesi, la successione $a_n$ è limitata, pertanto esistono due costanti $A,B\in\mathbb{R}$ tali che 
> $$A\leq a_n\leq B,\quad \forall n\in \mathbb{N}$$
> Suddividiamo l'intervallo $[A,B]$ mediante il punto di mezzo $$C= \frac{A+B}{2}$$
> Consideriamo i due intervalli $[A,C], [C,B]$. Almeno uno dei due intervalli contiene termini della successione $a_n$ per infiniti indici. Ovvero, dato che l'insieme $\mathbb{N}$ dei numeri naturali è infinito, risulta anche che è infinito almeno uno tra i due sottoinsiemi di $\mathbb{N}$:
> $$\left\{n\in\mathbb{N} \mid a_n\in [A,C] \right\}$$
> $$\left\{n\in\mathbb{N} \mid a_n\in [C,B] \right\}$$
> Sia ad esempio $[A,C]$ (oppure $[C,B]$) il sotto-intervallo che contiene termini della successione $a_n$ per infiniti indici e indichiamolo genericamente con il simbolo $[A_1,B_1]$, essendo:
> $$A\leq A_1,\quad B_1\leq B,\quad B_1-A_1= \frac{B-A}{2}$$
> Suddividiamo l'intervallo $[A_1, B_1]$ mediante il punto di mezzo $$C_1= \frac{A_1+B_1}{2}$$
> Per lo stesso motivo indicato in precedenza, almeno uno dei due intervalli $[A_1, C_1], [C_1, B_1]$ contiene termini della successione $a_n$ per infiniti indici e indichiamo tale intervallo con il simbolo $[A_2, B_2]$. Risulta:
> $$A_1\leq A_2,\quad, B_2\leq B_1,\quad B_2-A_2= \frac{B_1-A_1}{2}= \frac{B-A}{2^2}$$
> $$A\leq A_k\leq A_{k+1} < B_{k+1} \leq B_k \leq B, \quad \forall k\in\mathbb{N},\quad (\star)$$
> $$B_k-A_k= \frac{B-A}{2^k}, \quad \forall k\in \mathbb{N}$$
> Inoltre l'intervallo $[A_k, B_k]$ contiene i termini della successione $a_n$ per infiniti indici.
> In particolare, l'intervallo $[A_1, B_1]$ contiene termini della successione $a_n$. Quindi esiste il primo intero $n_1$ tale che $a_{n_{1}}\in [A_1, B_1]$. Per lo stesso motivo esiste un primo intero $n_2$ fra tutti i numeri naturali più grandi di $n_1$, per cui $a_{n_{2}}\in [A_2, B_2]$. 
> Iterando i casi $k=1,2$ già trattati, con $k=3,4,5,\dots$ determiniamo una successione [[031 Successioni Monotone#Successioni Monotone Strettamente Crescenti|strettamente crescente]] di interi:
> $$n_1< n_2<n_3<\dots<n_k<n_{k+1}<\dots$$
> per cui $a_{n_k}\in [A_k, B_k],\quad \forall k \in \mathbb{N}$.
> Dato che $B_k - A_k=\frac{B-A}{2^k}$, abbiamo quindi che:
> $$A_k\leq a_{n_k}\leq B_K = A_k+ \frac{B-A}{2^k},\quad \forall k \in \mathbb{N} \quad (\star\star)$$
> Per la $(\star)$, la successione $A_k$ (ed anche la $B_k$) è monotona e limitata. Per il [[#Teorema sulle Successioni Monotone|teorema delle successioni monotone]], $A_k$ ammette limite finito per $k\to +\infty$. Indichiamo con $l\in\mathbb{R}$ il valore di tale limite.
> Dato che $\frac{B-A}{2^k}\to 0$ per $k\to +\infty$, sia il primo che l'ultimo membro della $(\star\star)$ convergono ad $l$ per $k\to +\infty$.
> Dal [[#Teorema dei Carabinieri|teorema dei carabinieri]] si ottiene allora la conclusione $$\lim\limits_{k \to +\infty}a_{n_k}=l$$

### Criterio del Rapporto per Successioni
>Sia $a_n$ una successione a termini positivi. Definiamo la successione dei rapporti $\frac{a_{n+1}}{a_n}$. Se la successione ha il seguente limite $$\lim\limits_{n \to +\infty}\frac{a_{n+1}}{a_n}=l$$
>Allora:
>- $0\leq l< 1\Rightarrow a_n\to 0$;
>- $l>1\Rightarrow a_n$ è [[032 Limiti di Successioni#^c2f332|divergente]];
>- $l=1$ non possiamo dire nulla su $a_n$.


___
[[000 Indice Analisi|↖ Ritorna all'indice ↖]]

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
