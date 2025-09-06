---
aliases:
  - Diagramma ER
tags:
  - corsi/informatica/basi_di_dati
paragrafo: Modello Entity-Relationship
cssclasses:
  - 
---
> [!summary]+ TRACCIA ESEMPIO
> Una compagnia è organizzata in DIPARTIMENTI. Ogni Dipartimento ha un nome, un numero ed un impiegato che lo gestisce. Bisogna tener traccia della data di insediamento del manager. Un dipartimento può avere più locazioni.
>
> Ogni dipartimento controlla una serie di PROGETTI. Ogni progetto ha un nome, un numero ed una singola locazione.
> 
> Per IMPIEGATO si tiene traccia di: nome, SSN, indirizzo, salario, sesso e data di nascita. Ogni impiegato lavora per un dipartimento e può lavorare su più progetti. Teniamo traccia del numero di ore settimanali che un impiegato spende su un progetto e del supervisore di ogni impiegato.
>
> Ogni impiegato ha una serie di PERSONE A CARICO. Per ogni persona a carico, registriamo: nome, sesso, data di nascita e parentela con l'impiegato.

Leggenda: <u><font color="yellow">Attributo Chiave</font></u>, <font color="green">Attributo Derivabile</font>, <font color="#FF6611">{Attributo Multiplo}</font>,  Attributo, <font color="CC241D">Attributo Composto</font>, <font color="lime">Relazione</font>.

Si identificano le [[ER1_Entità|entità]] ed i loro [[ER3_Attributo|attributi]] (**SOSTANTIVI**):

1. *DIPARTIMENTO*:
	- <u><font color="yellow">Nome</font></u>, <u><font color="yellow">Numero</font></u>, <font color="#FF6611">{Sedi}</font>, <font color="green">NumeroImpiegati</font>.
2. *PROGETTO*:
	-  <u><font color="yellow">Nome</font></u>, <u><font color="yellow">Numero</font></u>, Locazione.
3. *IMPIEGATO*:
	- <font color="CC241D">Nome</font>, <u><font color="yellow">SSN</font></u>, <font color="CC241D">Indirizzo</font>, Stipendio, DataNascita.
4. *PERS_A_CARICO* ([[ER9_Entità e Relazioni Deboli#^b5cc34|Entità Debole]]):
	- Sesso, DataNascita, <u><font color="yellow">Nome</font></u>, Parentela.

Ora identifichiamo le relazioni (**VERBI**):

1. *GESTISCE*: (Un Solo) Impiegato <font color="lime">{gestisce [da Data_Inserimento]}</font> (Un Solo) Dipartimento.
2. *LAVORA_PER*: (N) Impiegati <font color="lime">{lavorano per}</font> (Un Solo) Dipartimento.
3. *SUPERVISIONA* (Ricorsiva): (Un Solo) Impiegato <font color="lime">{supervisiona}</font> (N) Impiegati;
4. *LAVORA_SU*: (M) Impiegati <font color="lime">{lavorano [per h ore] su}</font> (N) Progetti;
5. *CONTROLLA*: (Un Solo) Dipartimento <font color="lime">{controlla}</font> (N) Progetti;
6. *HA_A_CARICO* ([[ER9_Entità e Relazioni Deboli#^3496ac|Relazione Debole]]): (Un Solo) Impiegato <font color="lime">{ha a carico}</font> (N) Persone.

![[Pasted image 20221008163943.png]]

> [!tip]+ Suggerimenti
> - Quando un [[ER4_Tipi di Attributi#^b63ded|attributo composto]] partecipa a qualsiasi relazione, questo diventa una entità.
> - Quando si parla di un elenco si specifica sempre il singolo oggetto.

___
[[000 Indice BD|↖ Ritorna all'indice ↖]]

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