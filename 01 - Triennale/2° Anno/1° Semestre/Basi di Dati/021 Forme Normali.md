---
aliases: 
tags:
  - corsi/informatica/basi_di_dati
paragrafo: Forme Normali
cssclasses:
  - 
---
La **Normalizzazione dei Dati** può essere vista come un processo che consente di decomporre [[010 Modello Relazionale|schemi di relazione]] non ottimali in schemi più piccoli, tali da garantire la mancanza di [[019 Qualità di un DB Relazionale#ANOMALIE DI MODIFICA|update anomalies]].

Si suppone qui che per ogni relazione venga dato un insieme di [[020 Dipendenze Funzionali|dipendenze funzionali]] e che ogni relazione abbia una chiave primaria.

Le forme normali forniscono:
- Un ambito formale per *analizzare* schemi di relazione, basato sulle chiavi e sulle dipendenze funzionali tra attributi;
- Una serie di *test* da condurre sugli schemi di relazione individuali, se un test fallisce, la relazione che viola il test deve essere decomposta in relazioni che individualmente superano il test.

Esistono diverse forme normali:
- Le *prime tre forme normali* (1NF, 2NF, 3NF) e la *Boyce-Codd* (BCNF): sono definite considerando solo i vincoli di dipendenza funzionale e di chiave;
- La *4NF* considera la dipendenza multivalued;
- La *5NF* considera la join dependency.

La forma normale di una relazione indica la *più alta condizione* di forma normale soddisfatta, e perciò indica il livello al quale è stata normalizzata.

> [!summary]- Riassunto delle prime 3 forme normali
>![[Pasted image 20230106120525.png]]

---
### PRIMA FORMA NORMALE (1NF)
La 1NF è stata definita per non consentire attributi [[ER4_Tipi di Attributi#^abce31|multivalued]], [[ER4_Tipi di Attributi#^b63ded|composti]] e le loro combinazioni.

Ora fa parte della normale definizione di relazione del modello relazionale.
Gi unici valori consentiti da questa forma sono quelli atomici (o indivisibili).

> [!example]- <font color="orange">Esempio 1</font>
>![[Pasted image 20230106111014.png]]
>- **PRIMO METODO (Migliore)**
>	L'idea è di rimuovere `DLOCATIONS` che viola la 1NF e porlo in una relazione separata insieme con la chiave primaria.
>	La chiave primaria di questa nuova relazione è la combinazione di `DNUMBER` e `DLOCATION`.
>	![[Immagine 2023-01-06 111156.png]]
>
>- **SECONDO METODO**
>	Un secondo metodo di normalizzare è di avere una tupla per ogni locazione. In tal caso la chiave primaria è `DNUMBER` e `DLOCATION`, ma *si ha ridondanza tra le tuple*.
>	![[Pasted image 20230106111418.png]]
>
>- **TERZO METODO**
>	Un terzo metodo consiste nello stabilire un massimo numero di valori per l'attributo `DLOCATION` (per esempio 3) e rimpiazzare l'attributo con i 3 attributi atomici `DLOCATION1`, `DLOCATION2` e `DLOCATION3`. Questo porta all'introduzione di *valori nulli*.

> [!example]- <font color="orange">Esempio 2</font>
>La 1NF proibisce anche attributi composti. In questo caso abbiamo `PROJS` = {`PNUMBER`, `HOURS`}.
>![[Pasted image 20230106112141.png]]
>`SSN` è la chiave primaria, mentre `PNUMBER` è la chiave primaria della relazione annidata.
>
>Un altro metodo sarebbe quello di rimuovere gli attributi composti e portarli in una nuova relazione e propagare la chiave primaria in essa.
>![[Pasted image 20230106112331.png]]

---
### SECONDA FORMA NORMALE (2NF)
È basata sul concetto di [[020 Dipendenze Funzionali#DIPENDENZA FUNZIONALE PIENA|dipendenza funzionale piena]].

Uno schema di relazione $R$ è in 2NF se:
- $R$ è in *prima forma normale*;
- Ogni attributo non primo $A$ in $R$ è *pienamente funzionalmente dipendente* dalla chiave primaria di $R$.

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20230106114541.png]]
>Gli attributi non primi sono i seguenti:
>- `HOURS` NON viola la 2NF, perché nella FD1 c'é la dipendenza parziale dalla chiave primaria {`SSN`, `PNUMBER`};
>- `ENAME`, *viola la 2NF*, perché nella FD2 c'é la dipendenza a solo una parte della chiave primaria, ovvero solo `SSN`;
>- `PNAME` e `PLOCATION`, *violano la 2NF*, perché nella FD3 c'é la dipendenza a solo una parte della chiave primaria, ovvero solo `PNUMBER`.
>
>Quindi `EMP_PROJ` è in 1NF, ma non in 2NF.
>Le dipendenze funzionali FD2 e FD3 rendono `ENAME`, `PNAME`, `PLOCATION` parzialmente dipendenti dalla chiave primaria {`SSN`, `PNUMBER`}.
>
>Se uno schema non è in 2NF, può essere ulteriormente normalizzato in un certo numero di relazioni in 2NF, in cui gli attributi non primi sono associati solo con la parte di chiave primaria da cui sono funzionalmente dipendenti pienamente.
>
>![[Pasted image 20230106115133.png]]
><center>Normalizzazione in 2NF della relazione EMP_PROJ</center>

---
### TERZA FORMA NORMALE (3NF)
È basata sul concetto di [[020 Dipendenze Funzionali#DIPENDENZA FUNZIONALE TRANSITIVA|dipendenza transitiva]].

Secondo la definizione di Codd, uno schema di relazione $R$ è in 3NF se:
- $R$ è in *seconda forma normale*;
- Nessun attributo non primo di $R$ è *transitivamente dipendente* dalla chiave primaria.

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20230106115840.png]]
>
>Abbiamo che `SSN` ➔ `DMGRSSN` è transitiva attraverso `DNUMBER`, infatti:
>- `SSN` ➔ `DNUMBER`;
>- `DNUMBER` ➔ `DMGRSSN`;
>- `DNUMBER` non è sottoinsieme della chiave di `EMP_DEPT`.
>
>Quindi è in 2NF ma non in 3NF.
>
>![[Immagine 2023-01-06 120138.png]]
><center>Normalizzazione in 3NF della relazione EMP_DEPT</center>

---
### FORMA NORMALE DI BOYCE-CODD (BCNF)
Uno schema $R$ è in BCNF se ogniqualvolta vale in $R$ una dipendenza funzionale $X➔A$, allora $X$ è una [[Superchiave]] di $R$.

![[Pasted image 20230106120755.png]]
<center>Una relazione in 3NF, ma non in BCNF</center>


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