---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Numeri Complessi
cssclasses:
  - 
---
>La **Forma Trigonometrica** di un [[022 Numeri Complessi|numero complesso]] è una *rappresentazione* che consente di esprimere *qualsiasi numero complesso* $z$ esplicitandolo nella seguente [[MB - 00 Equazioni e Uguaglianze#Equazione|equazione]]:
>$$\color{#CC241D}z=\rho[\cos(\theta)+i\ \sin(\theta)]$$
>Dove:
>- $\rho$ è detto *Modulo* di $z$ ed è un numero reale non negativo $$\rho\geq0$$
>- $\theta$ è detto *Argomento* del numero complesso ed è un angolo in [[018 Circonferenza Goniometrica#Angoli Radianti|radianti]] che soddisfa una tra le seguenti condizioni $$-\pi<\theta\leq \pi\quad \text{oppure}\quad 0\leq\theta <2\pi$$
>- $i$ è l'[[022 Numeri Complessi#Unità Immaginaria|Unità Immaginaria]]

> [!example]+ <font color="orange">Esempio</font>
>- $2\sqrt[]{2}[\cos(\frac{\pi}{4})+i\ \sin(\frac{\pi}{4})]$
>- $3\left[ \cos\left( \frac{5}{6}\pi \right)+i\ \sin\left( \frac{5}{6}\pi \right) \right]$
>- $2\sqrt[]{3}[\cos(\frac{\pi}{2})+i\ \sin(\frac{\pi}{2})]$

## Piano Complesso (di Gauss)
Partendo dalla definizione di [[022 Numeri Complessi|numero complesso]], è possibile stabilire una *corrispondenza* tra i numeri complessi e i punti di un particolare piano cartesiano.
Ad ogni numero complesso corrisponde uno ed un solo punto nel *piano complesso (o di Gauss)*, il punto $P$ di coordinate cartesiane $(a,b)$.

![[Schermata 2022-03-25 alle 10.54.15.png|400]]

Quindi possiamo individuare il numero complesso $z=(a,b)$ anche conoscendo la misura del segmento $\overline{OP}=\rho$ e l'ampiezza dell'angolo $\theta$ che il segmento $\overline{OP}$ forma con il semiasse delle ascisse positive. ^91dce3

Secondo la definizione del [[019 Definizione di Seno e Coseno#Seno|seno]] e del [[019 Definizione di Seno e Coseno#Coseno|coseno]] risulta che:
$$a=\overline{OA}=\overline{OP}\cdot\cos(\theta)=\rho\cdot \cos(\theta)$$ 
$$b=\overline{OB}=\overline{OP}\cdot \sin(\theta)=\rho\cdot \sin(\theta)$$
Dove: $$\color{#CC241D}\rho=\sqrt{a^2+b^2}$$ $$\textcolor{#CC241D}{\begin{cases} \cos(\theta)=\frac{a}{\rho} \\ \sin(\theta)=\frac{b}{\rho} \end{cases}}\iff\begin{cases} \cos(\rho)=\frac{a}{\sqrt{a^2+b^2}} \\ \sin(\rho)=\frac{b}{\sqrt{a^2+b^2}} \end{cases}$$
$$\tan(\theta)=\frac{\sin(\theta)}{\cos(\theta)}=\frac{b/\rho}{a/\rho}=\frac{b}{a}$$
E quindi:
$$\color{#CC241D}\theta=\arctan\left( \frac{b}{a} \right)$$


Trovando il seno ed il coseno (oppure solo la [[020 Definizione di Tangente e Cotangente#Tangente|tangente]]), possiamo scoprire la misura in [[018 Circonferenza Goniometrica#Angoli Radianti|radianti]] di $\theta$. ^d6ccd5

### Dalla Forma Algebrica a quella Trigonometrica
Pertanto, partendo dalla [[023 Forma Algebrica di un Numero Complesso|forma algebrica]] di un numero complesso $z=(a,b)$, possiamo descriverne la rappresentazione trigonometrica nel seguente modo:$$a+ib=\rho\ \cos(\theta)+i[\rho\ \sin(\theta)]=\color{#CC241D}\rho[\cos(\theta)+i\ \sin(\theta)]$$
Si possono anche eseguire [[026 Operazioni in Forma Trigonometrica|certe operazioni attraverso le forme trigonometriche]].

---
# %%Esempi%%

> [!example]- <font color="orange">Esempio</font>
>$$z=1+i\sqrt{3}$$
>Scrivere il numero complesso $z$ nella sua forma trigonometrica corrispondente.
>
>Si ha che:
>- $a=1$
>- $b=\sqrt{3}$
>
>Allora:
>- $\rho=\sqrt{a^2+b^2=1^2+\sqrt{3}^2=1+3=4}=2$
>- $\tan(\theta)=\frac{\sin(\theta)}{\cos(\theta)}=\frac{b}{a}=\frac{\sqrt{3}}{1}=\sqrt{3}$
>
>Adesso si studia l'equazione in $(-\frac{\pi}{2}, \frac{\pi}{2})$, considerando $\theta=\frac{\pi}{3}$:
>$$z=2\left[\cos\left(\frac{\pi}{3}\right)+i\ \sin\left(\frac{\pi}{3}\right)\right]$$
>
>Infatti, si può notare che è corrispondente alla forma algebrica:
>$$z=2\left[\cos\left(\frac{\pi}{3}\right)+i\ \sin\left(\frac{\pi}{3}\right)\right]= 1+i\sqrt[]{3}$$



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
- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/numeri-complessi/2699-numeri-complessi-in-forma-trigonometrica.html)