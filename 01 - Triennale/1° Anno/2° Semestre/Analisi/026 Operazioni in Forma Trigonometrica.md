---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Numeri Complessi
cssclasses:
  - 
---
Si suppone di avere due [[022 Numeri Complessi|numeri complessi]] in [[025 Forma Trigonometrica di un Numero Complesso|forma trigonometrica]]:
- $z_1=\rho_2[cos(\theta_1)+i\ \sin(\theta_1)]$
- $z_2=\rho_2[\cos(\theta_2)+i\ \sin(\theta_2)]$

### Somma
Per la **Somma**, si è meglio trasformare i numeri in [[023 Forma Algebrica di un Numero Complesso|forma algebrica]] ed eseguire la [[024 Operazioni e Proprietà dei Numeri Complessi#Somma|somma]].
### Prodotto
>Il **Prodotto** tra due numeri complessi in forma trigonometrica è definito come:
>$$z_1\cdot z_2=\color{#CC241D}\rho_1\cdot\rho_2[\cos(\theta_1+\theta_2)+i\ \sin(\theta_1+\theta_2)]$$

### Divisione
>La **Divisione** tra due numeri complessi in forma trigonometrica è definita come:
>$$\frac{z_1}{z_2}=\frac{\rho_1[\cos(\theta_1)+i\ \sin(\theta_1)]}{\rho_2[\cos(\theta_2)+i\ \sin(\theta_2)]}=\color{#CC241D}\frac{\rho_1}{\rho_2}[\cos(\theta_1-\theta_2)+i\ \sin(\theta_1-\theta_2)]$$

### Potenza (Formula di De Moivre)
>La **Potenza** di un numero complesso in forma trigonometrica elevato ad un numero intero $\color{#61AFEF}n\in\mathbb{N}$ è definito come:
>$$z^n=z_1\cdot z_2\cdot\ \dots\ \cdot z_n =\color{#CC241D} \rho^n[\cos(n\cdot\theta)+i\ \sin(n\cdot\theta)]$$
### Radice
In generale si ha che: $$\color{#61AFEF}\sqrt[n]{z_1}=z_2\iff z_1 = z_2^n$$
>La **Radice** di un numero complesso si risolve attraverso questa formula risolutiva per $\sqrt[n]{z}$:
>$$\color{#CC241D}\sqrt[n]{ z }=\sqrt[n]{\rho}\ \Big[cos\Big(\frac{\theta+2k\pi}{n}\Big)+i\ sen\Big(\frac{\theta+2k\pi}{n}\Big)\Big]$$ 
>Con:
>- $k=0,1,...,n-1$

---
# %%Esempi%%

> [!example]- <font color="orange">Esempio</font>
>Calcolare la radice quadrata ($n=2$) di $z=1$.
>
>Si ha che:
>- $a=1$
>- $b=0$
>
>$$\rho=\sqrt{a^2+b^2=1^2+0^2=1^2}=1$$
>$$\tan(\theta)=\frac{b}{a}=\frac{0}{1}=0\iff \theta=0$$
>$$z=1[\cos(0)+i\ \sin(0)]$$
>$$z_k=\sqrt[n]{\rho}\ [cos(\frac{\theta+2k\pi}{n})+i\ sen(\frac{\theta+2k\pi}{n})]$$
>$$z_0=\sqrt[2]{1}[cos(0)+i\ sen(0)]=1(1+0)=1$$
>$$z_1=\sqrt[2]{1}[cos(2\pi)+i\ sen(2\pi)]=1(1+0)=1$$
>
>Quindi $\sqrt[2]{1}=1$.

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
- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/numeri-complessi/773-operazioni-con-i-numeri-complessi.html)