---
aliases:
  - Spazio Vettoriale
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Aritmetica
cssclasses:
  - 
---
Sia $F$ un [[054 Strutture Algebriche#CAMPO $(A, +, cdot)$|Campo]] e sia $S\neq \emptyset$ un insieme.

>$S$ è uno **Spazio Vettoriale** sul campo $F$, dove gli elementi di $S$ si chiamano *Vettori ($\color{#61AFEF}\underline{u}$)* e gli elementi di $F$ si chiamano *Scalari ($\color{#61AFEF}\alpha, \beta, ...$)*, se e solo se:
>- $\forall \underline{u}, \underline{v}\in S$ è definita la loro somma $\color{#61AFEF}\underline{u} + \underline{v}\in S$;
>- $\forall \alpha \in F, \forall \underline{u}\in S$ è definito il loro prodotto $\color{#61AFEF}\alpha \cdot \underline{u}\in S$.

^da02d3

Se $S$ è uno spazio vettoriale devono valere le seguenti *Proprietà*: ^65f710
- $(S, +)$ è un [[054 Strutture Algebriche#GRUPPO ABELIANO|Gruppo Abeliano]];
- $\forall\alpha,\beta\in F, \forall\underline{u}\in S: (\alpha + \beta)\underline{u} = \alpha\underline{u} + \beta\underline{u}$;
- $\forall\alpha\in F, \forall\underline{u}, \underline{v}\in S: \alpha (\underline{u}+ \underline{v}) = \alpha\underline{u} + \beta\underline{v}$;
- $\forall\alpha,\beta\in F, \forall\underline{u}\in S: (\alpha\beta)\underline{u} = \alpha(\beta\underline{u})$;
- $1\in F, \forall \underline{u}\in S: 1\underline{u} = \underline{u}$.

In generale, con $F$ campo, abbiamo uno spazio vettoriale numerico se:
$$F^n = \{(x_1, x_2, ..., x_n)\ |\ x_1, x_2, ..., x_n \in F, \forall n \in \mathbb{N}\}$$

> [!example]- <font color="orange">Esempio</font>
>Abbiamo che:
>$S = \mathbb{R}^2 = \{(x,y)|x, y\in \mathbb{R}\}$
>
>Dobbiamo verificare per definizione che:
>- $\forall (x,y)\in \mathbb{R}^2: (x,y) + (a,b)=\color{#61AFEF}(x+a\ ,\ y+b)$.
>- $\forall (x,y)\in \mathbb{R}^2, \forall \alpha \in \mathbb{R}: \alpha(x,y) = \color{#61AFEF}(\alpha x\ ,\ \alpha y)$.
>
>Abbiamo uno spazio vettoriale su $\mathbb{R}$.
>Devono valere le [[#^65f710|proprietà]].
>- $(\mathbb{R}, +)$ è un [[054 Strutture Algebriche#GRUPPO ABELIANO|Gruppo Abeliano]]:
>	- $+$ **Associativa**: $\forall (x, y), (a, b), (h, k) \in \mathbb{R}^2:$ $((x,y)+(a,b))+(h,k)$ 
>	$= (x+a, y+b) + (h,k)$ 
>	$= ((x+a)+h, (y+b)+k)$
>	$= (x+(a+h), y+(b+k))$
>	$= (x,y)+((a,h)+(b,k))$
>	$= \color{#61AFEF}(x,y)+((a,b)+(h,k))$.
>	- $+$ **Commutativa**: $\forall (x, y), (a, b) \in \mathbb{R}^2:$ 
>	$(x,y)+(a,b)=(x+a,y+b)$
>	$=(a+x,b+y)=\color{#61AFEF}(a,b)+(x,y)$.
>	- Esiste un **Elemento Neutro** (Vettore Nullo): $\exists \underline{0} = (0,0) \in \mathbb{R}^2:$ $(0,0)+(x,y) = (0+x, 0+y) = \textcolor{#61AFEF}{(x,y)}, \forall (x,y) \in \mathbb{R}^2$.
>	- Ogni elemento possiede un "**Simmetrico**": $\forall \underline{u} = (x,y) \in \mathbb{R}^2, \exists (-x, -y) \in \mathbb{R}^2:$
>	$(-x, -y)+(x, y) = \color{#61AFEF}(0,0) = \underline{0}$.
>- $\color{#CC241D}\forall\alpha,\beta\in \mathbb{R}, \forall\underline{u}=(x,y)\in \mathbb{R}^2:$
>$(\alpha + \beta)\underline{u} = (\alpha + \beta)(x, y) = ((\alpha + \beta)x, (\alpha + \beta)y)$
>$=(\alpha x + \beta x, \alpha y + \beta y)=(\alpha x, \alpha y) + (\beta x, \beta y)$
>$\alpha (x,y) + \beta (x,y) = \color{#61AFEF}\alpha\underline{u} + \beta\underline{u}$.
>- $\color{#cc241d}\forall \alpha \in \mathbb{R}, \forall \underline{u}= (x,y), \underline{v}= (a,b)\in \mathbb{R}^2:$
>$\alpha(\underline{u}+\underline{v}) = \alpha((x,y)+(a,b))$
>$= \alpha(x+a, y+b) = (\alpha(x+a),\alpha(y+b))$
>$= (\alpha x + \alpha a, \alpha y + \alpha b) = (\alpha x,  \alpha y) + (\alpha a,  \alpha b)$
>$= \alpha (x,y) + \alpha (a,b) = \color{#61AFEF}\alpha \underline{u} + \alpha \underline{v}$.
>- $\color{#CC241D}\forall \alpha, \beta\in \mathbb{R}, \forall \underline{u} = (x,y)\in \mathbb{R}^2:$
>$(\alpha\beta)\underline{u} = (\alpha\beta)(x,y) = ((\alpha\beta)x,(\alpha\beta)y)=(\alpha(\beta x),\alpha(\beta y)$
>$= \alpha(\beta x,\beta y)=\alpha(\beta (x,y))=\color{#61AFEF}\alpha(\beta \underline{u})$.
>- $\color{#CC241D}1\in\mathbb{R}, \forall \underline{u} = (x,y)\in \mathbb{R}^2:$
>$1\cdot \underline{u}=1(x,y)=(1x,1y)=(x,y)=\color{#61AFEF}\underline{u}$.
>
>Abbiamo dimostrato che $S$ è uno Spazio Vettoriale su $\mathbb{R}$


### VETTORI FISSATI
Un vettore si dice **fissato in un punto ($P$)** (esempio $O$) in un piano (esempio $\pi$) se abbiamo un insieme dei *vettori del piano applicati in $O$*:
$$V_o^2 = \{\overrightarrow{OP}\ |\ P\in \pi\}$$
![[Pasted image 20220114095111.png]]

$V_o^2$ diventa uno spazio vettoriale su $\mathbb{R}$, con:
- la **somma**;
![[Pasted image 20220118112311.png|500]]
- il **prodotto** di uno scalare per un vettore:
$\color{#61AFEF}\alpha\cdot \overrightarrow{OA} = \overrightarrow{OD}$ dove $\overrightarrow{OD}$ ha la stessa direzione di $\overrightarrow{OA}$. Quindi:
$|\overrightarrow{OD}| = |\alpha|\cdot|\overrightarrow{OA}|$. Se lo scalare è positivo, il verso del vettore sarà lo stesso di $\overrightarrow{OA}$, altrimenti se è negativo avrà il verso opposto.
![[Pasted image 20220118113846.png|500]]

Quindi $(V_o^2, +, \cdot)$ è uno spazio vettoriale su $\mathbb{R}$.
___
[[000 Indice MD|↖ Ritorna all'indice ↖]]

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