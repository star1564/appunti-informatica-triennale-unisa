---
aliases: 
tags:
  - corsi/matematica/probabilità_statistica
inglese: 
paragrafo: Calcolo Combinatorio
cssclasses:
  - 
---
![[002 Calcolo Combinatorio#^5e55f1|Principio Fondamentale del Calcolo Combinatorio]]

In un problema di calcolo combinatorio, quando si devono considerare *molteplici passaggi indipendenti* che devono **avvenire simultaneamente**, si sfrutta il principio fondamentale del calcolo combinatorio.

- **Uso della moltiplicazione**: La moltiplicazione si utilizza quando si combinano [[021 Eventi Indipendenti|Eventi Indipendenti]], ovvero quando l'uscita di un evento non influenza l'uscita di un altro. Se si devono fare più scelte indipendenti, si moltiplica il numero di modi per ciascuna scelta.

- **Somma e sottrazione di combinazioni**: Spesso si utilizza la somma o sottrazione di diverse combinazioni binomiali per escludere o includere determinate configurazioni.

---
> [!example]+ <font color="orange">Esempio 1</font>
>>Una classe di tango argentino ha 22 studenti, 10 donne e 12 uomini. In quanti modi si possono formare *5 coppie* (ognuna composta da una donna e un uomo)?
>
>- Per formare le 5 coppie, prima bisogna selezionare 5 donne dal gruppo di 10. Vi sono ${10\choose 5}$ modi di selezionare 5 persone da 10 donne.
>- Analogamente, per formare le coppie, bisogna selezionare 5 uomini dal gruppo di 12. Vi sono ${12\choose 5}$ modi di selezionare 5 persone da 12 uomini.
>
>Dopo aver selezionato i 5 uomini e le 5 donne, bisogna formare le *5 coppie*. Il numero di modi in cui si possono accoppiare 5 donne con 5 uomini è dato da $5!$, cioè il numero di permutazioni di 5 elementi.
>
>Il numero totale di modi in cui si possono formare 5 coppie è dato dal prodotto delle combinazioni di donne, delle combinazioni di uomini e delle [[003 Permutazioni|permutazioni]] delle coppie.
>$${10\choose 5}{12\choose 5} 5! = 252 · 792 · 120 = 23. 950. 080$$
>
>![[bf2ed3b3-373e-447b-8f95-c1abf723f36f.png|800]]

> [!example]+ <font color="orange">Esempio 2</font>
>>Quanti comitati composti da 2 donne e 3 uomini si possono formare da un gruppo di 5 donne e 7 uomini?
>
>- Ci sono ${5\choose 2}$ possibili insiemi con 2 donne
>- Ci sono ${7\choose 3}$ possibili insiemi con 3 uomini
>
>Segue allora dal principio fondamentale del calcolo combinatorio che vi sono
>$${5\choose 2}{7\choose 3}=350$$
>comitati possibili formati da 2 donne e 3 uomini.
>
>![[d8acddef-5213-4990-a4b6-0191286e5302.png|500]]
>
>---
>>Quanti sono i comitati se 2 uomini che hanno litigato rifiutano di sedere insieme nel comitato?
>
>##### Passaggio 1: Selezione delle donne
>Scegliamo il numero di possibili insiemi con 2 donne: $$\color{#FF99FF}{5\choose 2}=10$$
>
>##### Passaggio 2: Selezione degli uomini con la restrizione
>- *Nessuno* dei due uomini litiganti è nel comitato - In questo caso, dobbiamo scegliere 3 uomini dai 5 uomini rimanenti (escludendo i due litiganti): $$\color{#00C575}{2\choose 0}{5\choose 3}={5\choose 3}=10$$
>- *Esattamente uno* dei due uomini litiganti è nel comitato - In questo caso, dobbiamo scegliere 1 uomo tra i 2 litiganti e 2 uomini tra i 5 rimanenti: $$\color{#FF6611}{2\choose 1}\cdot{5\choose 2}=2\cdot10=20$$
>
>Quindi, il numero totale di modi per scegliere i 3 uomini rispettando la restrizione è: $$\textcolor{#00C575}{5\choose 3}+\Bigg(\textcolor{#FF6611}{{2\choose 1}\cdot{5\choose 2}}\Bigg)=10+20=\color{#61AFEF}30$$
>
>##### Passaggio 3: Combinazione delle selezioni
>Ora, per ogni selezione di 2 donne, abbiamo 30 modi per scegliere gli uomini. Quindi il numero totale di comitati è: $$\textcolor{#FF99FF}{5\choose 2}\cdot\textcolor{#61AFEF}{30}=10\cdot30=300$$
>
>![[6c036141-c37f-4f85-aa63-600a21ce44d1.drawio.png|1100]]


> [!example]+ <font color="orange">Esempio 3</font>
>>Da un lotto di $N = 20$ pezzi costituito da 4 pezzi difettosi e 16 buoni si estraggono $n = 10$ pezzi a caso (campioni).
>>1. Quanti sono i possibili campioni?
>>2. Quanti sono i possibili campioni che non contengono pezzi difettosi?
>>3. Quanti sono i possibili campioni che contengono 1 pezzo difettoso e 9 buoni?
>>4. Quanti sono i possibili campioni che contengono almeno 2 pezzi difettosi?
>
>
>##### 1.
>Il numero totale di campioni possibili, ovvero il numero di modi in cui possiamo scegliere 10 pezzi da un totale di 20, è dato dal seguente coefficiente binomiale: $$\color{#61AFEF}{20\choose 10}=184. 756$$ 
>   ![[ce283829-ba1f-4072-b98f-88a75d19fa57.png|400]]
>##### 2. 
>Per trovare il numero di campioni che non contengono pezzi difettosi, dobbiamo scegliere tutti i 10 pezzi dai 16 pezzi buoni (escludendo i 4 difettosi). Il numero di questi campioni è dato da: $$\color{#00C575}{4\choose 0}{16\choose 10}={16\choose 10}=8. 008$$
>![[dacef4a4-0565-4758-a272-96dbb25623c7.png|600]]
>
>##### 3. 
>Per determinare il numero di campioni che contengono esattamente 1 pezzo difettoso e 9 pezzi buoni, scegliamo 1 pezzo dai 4 difettosi e 9 pezzi dai 16 buoni: $$\color{#FF6611}{4\choose 1}{16\choose 9}=45. 760$$
>![[8c1883e8-af75-41b3-b018-59e6fb2fe0a6.png|600]]
>##### 4. 
>Per trovare il numero di campioni che contengono almeno 2 pezzi difettosi, possiamo sottrarre dal numero totale di campioni quelli che non contengono nessun pezzo difettoso e quelli che ne contengono esattamente uno. Quindi: $$\textcolor{#61AFEF}{20\choose 10}-\textcolor{#00C575}{{4\choose 0}{16\choose 10}}-\textcolor{#FF6611}{{4\choose 1}{16\choose 9}}=130.988$$
>
>![[6722bac3-7801-4af2-83c1-82f0afab13e5.png|1200]]


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