---
aliases: 
  - Rendering
  - BRDF
  - Riflessione
tags:
  - corsi/informatica/grafica_interattivita
paragrafo: Illuminazione
cssclasses:
  - 
---
>La **Radianza** ($L$) indica la quantità di energia luminosa che *emerge* da una specifica *piccola area* di una sorgente in una particolare *direzione*.

![[Pasted image 20240313200636.png|500]]

Abbiamo:
- Il [[004 Illuminazione#^afdfab|flusso]] - la quantità totale di energia luminosa che fuoriesce.
- L'*area proiettata* ($dA$)- come l'"impronta" della piccola area sulla superficie che stiamo considerando. 
- L'*angolo solido* ($\theta$) - descrive il "cono" di luce che si propaga da quella piccola area.

Quindi, la Radianza combina questi tre aspetti:
- Dice quanta potenza luminosa (flusso) viene emessa da una *specifica piccola zona* (area proiettata) della sorgente luminosa.
- Considera anche la *direzione* in cui viaggia questa luce (angolo solido).

La simulazione dei singoli fotoni è computazionalmente costosa per la creazione di immagini. Quindi, la computer grafica utilizza **raggi** (ray) che rappresentano il *percorso della luce*.

![[Pasted image 20240313201202.png|500]]

### Radiosità e Irradianza
Supponiamo di avere una parete in una stanza.

>La **Radiosità** ($B$) è la *quantità totale di <u>energia luminosa</u> che lascia* ogni *unità quadrata* della superficie della parete in *tutte le direzioni*. 

È quindi la somma di tutti i valori di radianza che lasciano ogni piccola area della parete.

>L'**Irradianza** ($E$) è la *quantità totale di <u>energia luminosa</u> che colpisce* ogni *unità quadrata* della superficie della parete da *tutte le direzioni*. 

Considera la luce proveniente dal sole, i riflessi di altri oggetti, ecc.

![[Pasted image 20240313210845.png|500]]

### Riflessione
>Quando la luce colpisce un oggetto, una parte di essa *rimbalza*. Questo fenomeno è chiamato **Riflessione**.

![[Pasted image 20240313202932.png|500]]

Mette in relazione la radianza riflessa con l'irradianza in entrata.

La quantità di luce che rimbalza dipende dalle *proprietà del materiale* e dall'*angolo* della luce in entrata.

#### BRDF (Funzione di distribuzione della riflettanza bidirezionale)
È un'equazione complessa che rappresenta la quantità di luce riflessa in diverse direzioni. 

Considera:
- **Direzione della luce in entrata ($\omega_i$)** - L'angolo con cui la luce colpisce una superficie.
- **Direzione della luce in uscita ($\omega_r$)** - Le diverse direzioni in cui la luce riflessa rimbalza.
- **Proprietà del materiale ($f$)** - Consente di capire come il materiale stesso influisce sulla riflessione (ad esempio, lucido o ruvido).

È difficile da utilizzare nella pratica, quindi si ricorre spesso a modelli semplificati:
- [[006 Illuminazione Locale#^f6b835|Diffusa]]
- [[006 Illuminazione Locale#^14b133|Speculare]]
- Un *mix* tra diffuso e speculare, che riflette la luce in modo più ampio (si pensi a una superficie lucida).

Questi modelli semplificati vengono solitamente *combinati* per creare una rappresentazione più realistica del modo in cui la luce si riflette dalle diverse superfici.

### Equazione della Radianza
>La **Radianza ($L(p, \omega)$)** indica la quantità totale di potenza luminosa *che viaggia in una direzione specifica ($\omega$)* dal punto ($p$).

In pratica, la potenza luminosa totale percepita in un punto specifico $p$ proviene da due fonti:
   - Luce emessa *direttamente dall'oggetto stesso*
   - Luce che *rimbalza su altri oggetti* e raggiunge quel punto

È possibile rintracciare la fonte luminosa che ha colpito un certo punto attraverso il [[007 Ray Tracing|ray tracing]].

### Rendering
>Diciamo di voler creare un'immagine bidimensionale (come una foto) di una scena. Questo *processo di estrazione di una vista specifica* da tutte le informazioni sulla luce si chiama **rendering**.

![[Pasted image 20240313205817.png|500]]

È come se fosse un'istantanea gigante che include l'intensità, la direzione e il colore della luce per ogni singolo punto nello spazio.

___
[[000 Indice Grafica|↖ Ritorna all'indice ↖]]

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