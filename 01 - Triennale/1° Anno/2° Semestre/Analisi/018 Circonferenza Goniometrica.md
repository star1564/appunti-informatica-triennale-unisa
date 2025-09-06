---
aliases:
  - Angoli Radianti
  - Angoli Notevoli
tags:
  - corsi/matematica/analisi
paragrafo: Funzioni Trigonometriche
cssclasses:
  - 
---

>La **Circonferenza Goniometrica** è una *circonferenza di raggio 1*, disegnata nel [[012 Grafici di Funzioni|Piano Cartesiano]], con *centro* nell'origine degli assi, ovvero $\color{#ed7d31}O=(0,0)$.

![[Pasted image 20240928105346.png|500]]

Con la circonferenza goniometrica possiamo rappresentare un **angolo** qualsiasi e associarlo a un punto sulla circonferenza stessa.

![[Pasted image 20240928103019.png|500]]


Per farlo si prendono in considerazione tre aspetti:
- Il <font color="#ed7d31">vertice</font> dell'angolo è l'origine $O$ 
- Il <font color="#cc0099">primo lato</font> dell'angolo è il semiasse positive delle $x$ (da $O$ a $(1,0)$)
- Spostando il <font color="#70ad47">secondo lato</font> in senso antiorario, si può <font color="#00b0f0">creare un angolo orientato</font> $\color{#00b0f0}\alpha$ di qualsiasi ampiezza tra $0^\circ$ e $360^\circ$

![[Pasted image 20240928105046.png|500]]

Considerando questo andamento antiorario, si possono individuare quattro **quadranti** all'interno della circonferenza, che sono separati dagli assi delle ordinate e delle ascisse. Sono numerati da $1$ a $4$.

![[Pasted image 20240928114026.png|500]]

## Angoli Radianti
>Un **radiante** è un'unità di misura degli angoli. Si ottiene facendo il *rapporto tra la lunghezza di un arco e il raggio* della circonferenza su cui l'arco si trova.

Considerare: 
- Una circonferenza di centro $O$ con un raggio $r>0$ 
- Un angolo $\alpha$ al centro della circonferenza
- Un arco $AB$ di lunghezza $\color{#ed7d31}l$

![[Pasted image 20220321152254.png|400]]

L'ampiezza dell'angolo $\alpha$ **in radianti** è data dal rapporto tra la lunghezza dell'arco $AB$ e la misura del raggio della circonferenza, ovvero:
$$a^{rad}=\frac{l}{r}$$

### Rappresentare gli Angoli con i Radianti
>Gli angoli espressi in radianti si possono scrivere in **funzione di $\pi$** (pi greco). Il valore di $\pi$ è usato perché rappresenta la *metà della circonferenza goniometrica*, ovvero l'angolo piatto, che equivale a $\color{#61AFEF}180^\circ$.  

Questi sono i quattro angoli che sono separati da un angolo da $90^\circ$:
- $90^\circ$ (angolo retto): corrisponde a $\frac{\pi}{2}$ radianti
- $180^\circ$ (angolo piatto): corrisponde a $\pi$ radianti
- $270^\circ$ (angolo piatto): corrisponde a $\frac{3}{2}\pi$ radianti
- $360^\circ$ (angolo giro): corrisponde a $2\pi$ radianti

In generale, per convertire un angolo in gradi in radianti, si usa la formula:
   $$\text{Angolo in radianti} = \frac{\text{Angolo in gradi} \cdot \pi}{180^\circ}$$

### Angoli Notevoli

- $0^\circ=0$ radianti
- $30^\circ=\frac{\pi}{6}$ radianti
- $45^\circ=\frac{\pi}{4}$ radianti
- $60^\circ=\frac{\pi}{3}$ radianti
- $90^\circ=\frac{\pi}{2}$ radianti
- $120^\circ=\frac{2\pi}{3}$ radianti
- $135^\circ=\frac{3\pi}{4}$ radianti
- $180^\circ=\pi$ radianti
- $270^\circ=\frac{3\pi}{2}$ radianti
- $360^\circ=2\pi$ radianti

![[Pasted image 20220325123421.png|600]]


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
- [YouMath](https://www.youmath.it/formulari/65-formulari-di-trigonometria-logaritmi-esponenziali/142-circonferenza-trigonometrica.html)