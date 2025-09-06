---
aliases: 
tags:
  - corsi/informatica/basi_di_dati
paragrafo: Forme Normali
cssclasses:
  - 
---
Dopo aver discusso le [[019 Qualità di un DB Relazionale|misure informali della progettazione]] delle basi di dati, definiamo ora uno **Strumento Formale** per l'analisi degli schemi di relazione che ci consentirà di descrivere in termini precisi i problemi che possono sorgere da questi.

Una **Dipendenza Funzionale (FD)** è un vincolo tra due insiemi di [[ER3_Attributo|attributi]] della base di dati.
Si supponga che il nostro schema di base di dati relazionale abbia $n$ attributi $A_1, A_2, ..., A_n$ e che l'intero database sia descritto da uno schema di relazione universale $R=\{A_1, A_2, ..., A_n\}$.

Una dipendenza funzionale, indicata con $\color{#CC241D}X ➔ Y$, tra due insiemi di attributi $X$ e $Y$ che sono [[002 Sottoinsiemi|Sottoinsiemi]] di $R$, specifica un vincolo sulle possibili tuple che possono formare una istanza di relazione $\color{#CC241D}r$ di $R$.

> [!tip] Importante
Il vincolo stabilisce che se $X ➔ Y$, allora $\forall t_1, t_2\in r: t_1[X] = t_2[X]$, allora deve valere $t_1[Y]=t_2[Y]$.
Questo significa che i valori della componente $Y$ di una tupla di $r$ *dipendono da* (o *sono determinati da*) i valori della componente $X$.
Alternativamente, i valori della componente $X$ di una tupla di $r$ *determinano univocamente* (o *funzionalmente*) i valori della componente $Y$.

Esiste quindi una dipendenza funzionale da $X$ a $Y$, dove:
- $Y$ è funzionalmente dipendente da $X$;
- $X$ è la parte sinistra della FD;
- $Y$ è la parte destra della FD.

Se un vincolo su $R$ stabilisce che non può esistere più di una tupla con un dato valore per $X$ in ogni istanza di relazione $r(R)$ (ovvero se $X$ è una chiave candidata di $R$) ciò implica che $X ➔ Y$ per ogni sottoinsieme $Y$ di attributi di $R$.

Si ha inoltre che: $X ➔ Y \not\Rightarrow Y ➔ X$.

> [!example]- <font color="orange">Esempio</font>
> La dipendenza funzionale è una proprietà della semantica degli attributi.
> ![[Pasted image 20230104093402.png]]
>- *FD1*: {`SSN`, `PNUMBER`} ➔ `HOURS`. Questo significa che le ore di lavoro sul progetto dipendono dalla coppia dell'SSN dell'impiegato e dal numero del progetto a cui sta partecipando.
>- *FD2*: `SSN` ➔ `ENAME`. Questo significa che l'SSN di un impiegato determina univocamente il suo nome.
>- *FD3*: `PNUMBER` ➔ {`PNAME`, `PLOCATION`}. Questo significa che il numero del progetto determina univocamente il suo nome e la sua locazione.

---
### FD DALLA SEMANTICA DEGLI ATTRIBUTI
Una FD è una proprietà dello schema di relazione $R$ e non di un particolare stato di relazione (o estensione legale $r$ di $R$).
Le estensioni di relazione $r(R)$ che soddisfano i vincoli di dipendenza funzionale sono dette *estensioni legali* di $R$.

Una FD non può essere inferita automaticamente da un'estensione di relazione $r$, ma deve essere definita esplicitamente da chi conosce la semantica degli attributi.

> [!example]- <font color="orange">Esempio</font>
> ![[Pasted image 20230104094946.png]]
> Si potrebbe pensare che `TEACHER` determina funzionalmente `COURSE` e che `COURSE` determina funzionalmente `TEXT`. 
> Ci sono però delle tuple che non seguono queste FD (`Smith` insegna due corsi, inoltre anche `Brown` insegna `Data Structures` e questo ha un testo diverso da quello definito da `Smith`), quindi giungiamo alla conclusione che:
> - `TEACHER` $\not➔$ `COURSE`;
> - `COURSE` $\not➔$ `TEXT`.

---
### REGOLE DI INFERENZA PER FD
Sia $F$ l'insieme delle dipendenze funzionali di uno schema di relazione $R$.
Il disegnatore dello schema specifica le dipendenze funzionali *semanticamente ovvie che si trovano in F*.

In tutte le istanze di relazioni legali, però, valgono numerose altre dipendenze funzionali, questo vengono chiamate **inferite** (o dedotte) da $F$.

L'insieme di tali dipendenze è detto *chiusura di F* ed è denotato da $F^+$.

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20230104100119.png]]
>- $F = \{$
>	- `SSN` ➔ {`ENAME`, `BADATE`, `ADDRESS`, `DNUMBER`},
>	- `DNUMBER` ➔ {`DNAME`, `DMGRSSN`}.
>$\}$
>
>- Possiamo inferire (dedurre):
>	- `SSN` ➔ {`DNAME`, `DMGRSSN`}, perché da `SSN` abbiamo `DNUMBER` da cui troviamo `DNAME` e `DMGRSSN`;
>	- `SSN` ➔ `SSN`;
>	- `DNUMBER` ➔ `DNAME`;
>	- ...

Una FD 0$X ➔ Y$ è *inferita da* un insieme di dipendenze $F$ su $R$ se $X ➔ Y$ vale in ogni stato di relazione $r$ che è estensione legale di $R$.

$\color{#CC241D}F \vDash X ➔ Y$ denota che la FD $X ➔ Y$ è inferita da $F$.

> [!note]- Regole di inferenza fondamentali e Notazioni
>![[Pasted image 20230104101902.png]]
>
>![[Immagine 2023-01-04 102136.png]]

---
### DIPENDENZA FUNZIONALE PIENA
Una dipendenza funzionale $X➔Y$ si dice **piena** se la rimozione di qualche attributo $A$ da $X$ implica che la dipendenza *non vale più*, ovvero:
$$\forall A\in X, (X-\{A\}) \not➔Y$$

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20230106113047.png]]
>- {`SSN`, `PNUMBER`} ➔ `HOURS` (FD1) è *piena*, infatti si ha che:
>	- `SSN` $\not➔$ `HOURS`;
>	- `PNUMBER` $\not➔$ `HOURS`.



---
### DIPENDENZA FUNZIONALE PARZIALE
Una dipendenza funzionale $X➔Y$ si dice **parziale** se la rimozione di qualche attributo $A$ da $X$ implica che la dipendenza *vale ancora*, ovvero:
$$\exists A\in X, (X-\{A\}) ➔Y$$

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20230106113047.png]]
>- {`SSN`, `PNUMBER`} ➔ `ENAME` (inferita) è *parziale*, infatti si ha che:
>	- `SSN` ➔ `ENAME`.

---
### DIPENDENZA FUNZIONALE TRANSITIVA
Una dipendenza funzionale $X➔Y$ in uno schema $R$ è una **Dipendenza Transitiva** se: 
- Esiste un insieme di attributi $Z$ che NON è sottoinsieme di alcuna chiave di $R$;
- Valgono $X➔Z$ e $Z➔Y$.


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