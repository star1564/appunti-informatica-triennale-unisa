---
aliases: [CRC, Checksum]
tags:
  - corsi/informatica/reti_calcolatori
paragrafo: Livello Data Link
cssclasses:
  - 
---
Il trasferimento al [[006 Livello Fisico|Livello Fisico]] può comportare molti errori. 
Per rendere possibile la **rilevazione degli errori** nel [[008 Livello Data Link|Livello Data Link]] ci sono due metodi principali.
### Checksum
> Il **checksum** è una tecnica di controllo degli errori utilizzata per verificare l'integrità dei dati trasmessi attraverso un canale di comunicazione. 
> In particolare, il checksum *viene calcolato sul contenuto dei dati* e viene aggiunto come campo *nell'[[008 Livello Data Link#^8afe36|header]] di ogni frame* durante il processo di incapsulamento (o framing) nel livello data-link.

Il checksum viene calcolato *sommando tutti i byte dei dati trasmessi*. Questo valore viene poi aggiunto al frame come campo nell'header.

Alla destinazione, *si effettua nuovamente il calcolo del checksum* e lo si *compara* con il valore posto nell'header. Se i valori coincidono, il corpo del pacchetto è corretto.

![[e58765bf-e175-48e0-b3bf-aecd455c1591 2.png]]

---
### Cyclic Redundancy Code (CRC)
> Il **CRC** (*Cyclic Redundancy Check*) è una tecnica di controllo degli errori più avanzata rispetto al checksum.

In esso, i dati da trasmettere vengono considerati come un *polinomio* e viene utilizzato un algoritmo matematico basato sulla divisione polinomiale (con *polinomio generatore* calcolato tramite i *campi di Galois*) per generare un valore di controllo (*CRC code*) che viene aggiunto al trailer di ogni frame durante il processo di incapsulamento (o framing) nel livello data-link.

Quando il frame viene ricevuto, il destinatario calcola il CRC code utilizzando lo *stesso algoritmo* utilizzato dal mittente e confronta il risultato ottenuto con il CRC code presente nel trailer del frame. Se i due valori coincidono, allora si presume che il frame sia stato trasmesso correttamente. Se invece i due valori sono diversi, allora il frame è stato corrotto durante la trasmissione e viene considerato come un errore di trasmissione.

![[Pasted image 20230324170531.png]]

> [!info] Campo di Galois
> Il **Campo di Galois** è un [[054 Strutture Algebriche|Campo]] $(+,\cdot)$ con un certo numero di elementi su cui sono definite due operazioni aritmetiche che godono della proprietà commutativa ed associativa. Le operazioni vengono effettuate seguendo l'aritmetica binaria.

#### Come Calcolare il CRC
Dato un <font color="#FF6611">polinomio messaggio</font> $\color{#FF6611}M$ ed un <font color="#00C575">polinomio generatore</font> $\color{#00C575}G$, vogliamo creare un <font color="#b0bc64">codice CRC</font>.

- Convertire i due polinomi in binario:
	- Sostituire le occorrenze di $x^i$ con $1$ in posizione $i$;
	- Se $x^i$ non esiste, allora questo significa che $x^i = 0$, pertanto mettere $0$ in posizione $i$.
	- $x^0$ è la costante più a destra.
- Trovare il grado massimo $\color{#61AFEF}n$ di $\color{#00C575}G$.
- Aggiungere a destra di $\color{#FF6611}M$, $\color{#61AFEF}n$ zeri.
- Eseguire una divisione polinomiale, $\textcolor{#FF6611}{M}:\textcolor{#00C575}{G}$.

> [!example]- <font color="orange">Esempio</font>
>`(Cliccare sulle immagini per ingrandire)`
>
>$\color{#FF6611}M=x^{9}+x^{8}+x^{7}+x^{4}+x^{2}+1$
>$\color{#00C575}G=x^5+x^2+1$
>
>1. Conversione in binario ed aggiunta degli zero alla fine: ![[Pasted image 20230425175425.png|800]]
>
>2. Prendere la posizione $i$ dell'uno più a sinistra e fare la sottrazione $k=i-n$. Il risultato sarà $1$ alla posizione $k$: ![[Pasted image 20230425175441.png|800]]
>
>3. Mettere sotto gli $n$ bit più a sinistra il polinomio generatore $G$ ed eseguire la "divisione" (ovvero uno [[005 XOR (MMI)|XOR]] bit a bit). Se, a partire dal bit $1$ più a sinistra, si hanno meno di $n+1$ bit, fai scendere a destra il numero necessario di bit per farlo arrivare ad $n+1$:![[Pasted image 20230425175455.png|800]]
>
>4. Ripetere il passo 2 con il bit risultato dalla "divisione" precedente: ![[Pasted image 20230425175510.png|800]]
>5. Ripetere il passo 3: ![[Pasted image 20230425175525.png|800]]
>
>6. Ripetere i passi precedenti fino a quando si troverà un <font color="#CC241D">resto</font> $\color{#CC241D}R$. Per ottenere il CRC, sostituire gli ultimi $n$ bit $0$ con le ultime $n$ cifre del resto. ![[Pasted image 20230425180035.png|800]]
>7. Se si vuole, si può **provare** che il CRC sia corretto, dividendolo per il polinomio generatore $G$. Se il resto è $0$, il CRC è corretto, altrimenti è errato. ![[Pasted image 20230425180125.png|800]]

___
[[000 Indice RC|↖ Ritorna all'indice ↖]]
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