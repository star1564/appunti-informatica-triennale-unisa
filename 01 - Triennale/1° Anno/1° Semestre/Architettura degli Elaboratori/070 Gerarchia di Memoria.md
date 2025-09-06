---
aliases:
  - Gerarchia di memoria
  - cache
tags:
  - corsi/informatica/architettura_degli_elaboratori
paragrafo: Memoria
cssclasses:
  -
---
La **Memoria** è il luogo dove vengono tenuti i programmi in esecuzione e i dati di cui essi necessitano.
La si può immaginare come un grande array unidimensionale di "celle in sequenza". Ogni elemento ha un *indirizzo* attraverso il quale si può accedere ad esso.

![[Pasted image 20211220094753.png|500]]

## TIPI DI ACCESSO

### ACCESSO CASUALE

Nelle **Memorie ad accesso casuale**, il tempo di accesso ai dati è indipendente dalla posizione in cui essi sono memorizzati.
Un esempio è la [[030 Memoria Centrale|Memoria Principale]] *(RAM, da Sistemi Operativi)*.

### ACCESSO SEQUENZIALE

Nelle **Memorie ad accesso sequenziale**, il tempo di accesso ai dati dipende dalla posizione in cui essi sono memorizzati.
Un esempio è la *memoria secondaria (dischi e nastri)*.

## TECNOLOGIE

### SRAM
*S*tatic *R*andom *A*ccess *M*emory. 
*Statica* perché l'informazione memorizzata è permanente fino a quando l'alimentazione è attiva.
*Costosa*, perché ciascun bit è memorizzato mediante 6 oppure 8 transistor.
*Rapido* tempo di accesso.

Una **2<sup>n</sup> X m SRAM** è un array di 2<sup>n</sup> celle, ciascuna di m bit.
- Si può accedere alle celle in *lettura* specificandone l'indirizzo.
- Si può accedere alle celle in *scrittura* specificandone l'indirizzo e il dato da scrivere.

Gli input sono:
- Indirizzo cella a cui accedere (n bit);
- Tre segnali di controllo di 1 bit:
	- *Chip select* (deve essere 1 per poter leggere o scrivere);
	- *Output enable* (deve essere 1 per poter leggere);
	- *Write enable* (deve essere 1 per poter scrivere);
- Dato da scrivere (m bit).

L'output di una 2<sup>n</sup> X m SRAM è il dato letto (m bit).

![[Pasted image 20211220100605.png|500]]

### DRAM
*D*ynamic *R*andom *A*ccess *M*emory.
*Dinamica* perché richiede un refresh periodico, cioè il dato va letto e riscritto continuamente.
*Economica*, perché ciascun bit è memorizzato mediante un solo transistor.
*Lento* tempo di accesso.

## PARAMETRI E GERARCHIA

La memoria è caratterizzata da diversi parametri:
- *Dimensione o Capacità*, misurata in multipli di byte, indica la quantità di dati memorizzabili.
- *Velocità o tempo di accesso*, indica l'intervallo di tempo tra la richiesta del dato e il momento in cui viene reso disponibile.
- *Consumo*, indica la potenza media assorbita.
- *Costo*, dipende dal volume di acquisto.

Però non è possibile avere un'unica memoria con tutte le caratteristiche ideali.

Possiamo organizzare una **Gerarchia** delle memorie dove: Le memorie più piccole, veloci e costose sono poste ai livelli alti, *vicino al processore*.
Mentre le memorie più grandi, lente e meno costose sono poste ai *livelli più bassi*.

Le memorie sono realizzate con tecnologie diverse seconda dei livelli in cui si trovano nella gerarchia.

![[Immagine 2021-12-20 102508.png]]

Quindi, più il livello della gerarchia è vicino al processore, minore è la sua capacità di memoria, ma maggiore è la velocità di accesso ai suoi dati.

Ciascun livello contiene un sottoinsieme dei dati del livello sottostante (di solito quelli usati più recentemente) e tutti i dati contenuti nei livelli soprastanti (di conseguenza il livello più basso contiene tutti i dati).

Quindi, le informazioni vengono di volta in volta *copiate tra livelli adiacenti della gerarchia*.

### PRINCIPIO DI LOCALITÀ
La gestione dei livelli avviene in base al **Principio di località**:
I dati utilizzati più spesso sono posti in memorie più veloci e più vicine al processore, mentre quelli meno utilizzati sono posti in memorie più lente e più lontane dal processore.

I dati possono essere spostati automaticamente tra i livelli attraverso canali di comunicazioni con l'*allocazione dinamica*.

#### LOCALITÀ TEMPORALE
Se un elemento di memoria (dato o istruzione) è stato letto, tenderà a essere letto nuovamente entro breve tempo. Caso tipico: le istruzioni e i dati entro un ciclo saranno letti ripetutamente.

#### LOCALITÀ SPAZIALE
Se un elemento di memoria (dato o istruzione) è stato letto, gli elementi i cui indirizzi sono vicini tenderanno a essere letti entro breve tempo. Caso tipico: gli accessi agli elementi di un array.

### MIGRAZIONE DELLE INFORMAZIONI
Consideriamo *due livelli adiacenti* (livello superiore e inferiore). Possiamo dare le seguenti definizioni:
- **Blocco o Linea**: la più piccola quantità d'informazione che può essere trasferita tra due livelli adiacenti nella gerarchia.

- **Hit (Successo)**: Se il dato richiesto dal processore è contenuto in uno dei blocchi presenti nel livello superiore.
- **Miss (Fallimento)**: Se il dato richiesto dal processore non è contenuto in uno dei blocchi presenti nel livello superiore. In tal caso bisogna *accedere al livello inferiore* della gerarchia per recuperare il blocco contenente il dato richiesto e *copiarlo nel livello superiore*.
- **Tempo di Hit**: Tempo necessario per trovare il dato richiesto nel livello superiore.
- **Hit rate**: Frequenza di successo, ovvero la frazione degli accessi in cui troviamo il dato nel livello superiore $\frac{num.\ hit}{num.\ tentativi}$.
- **Miss rate**: Frequenza di fallimento, ovvero la frazione degli accessi in cui NON troviamo il dato nel livello superiore $\frac{num.\ miss}{num.\ tentativi}$.
-  **Penalità di Miss**: Tempo necessario per trovare il dato se questo non è presente nel livello superiore.

L'*hit rate* viene usato come *indice delle prestazioni* della gerarchia di memoria, perché il livello superiore della gerarchia è il più piccolo e costruito con componenti più veloci, quindi il tempo di hit sarà molto inferiore alla penalità di miss.

## MEMORIA CACHE

La **Cache** è una *memoria veloce* e di *piccole dimensioni* posta fra la CPU e la memoria principale.
La cache e la memoria principale formano una gerarchia di memoria.

Le prestazioni della memoria cache dipendono anche dalla sua posizione rispetto alla CPU:
- Su scheda madre;
- Su chip (maggiore efficienza).

![[Immagine 2021-12-20 105759.png|500]]

### CHACHE AD INDIRIZZAMENTO DIRETTO
Ad ogni indirizzo della memoria corrisponde *in modo univoco* una locazione della cache. La corrispondenza si trova con:
> **indirizzo nella cache = indirizzo della memoria modulo[^1] N<sub>B</sub>**

Dove N<sub>B</sub> indica il numero di blocchi nella cache.

Se N<sub>B</sub> è una *potenza di 2*, basta considerare i **log<sub>2</sub>N<sub>B</sub>** [[Bit Meno Significativo|bit meno significativi]] dell'indirizzo del blocco in memoria.

> [!example]+ <font color="orange">Esempio</font>
>- N<sub>B</sub> = 2, log<sub>2</sub>N<sub>B</sub> = 1, 1001**0** mod 2 = **0**; 1101**1** mod 2 = **1**.
>- N<sub>B</sub> = 4, log<sub>2</sub>N<sub>B</sub> = 2, 100**10** mod 4 = **10**; 110**11** mod 4 = **11**.
>- N<sub>B</sub> = 8, log<sub>2</sub>N<sub>B</sub> = 3, 10**010** mod 8 = **010**; 11**011** mod 8 = **011**.

Quindi, tutti i blocchi della memoria che hanno i log<sub>2</sub>N<sub>B</sub> meno segnificativi dell'indirizzo uguali vengono *mappati sullo stesso blocco di cache*.

![[Immagine 2021-12-20 162552.png]]

### STRUTTURA DELLA CACHE
Ciascun *blocco o linea* della cache è costituita da 4 campi:
- Indice (log<sub>2</sub>N<sub>B</sub> bit meno segnificativi dell'indirizzo);
- Validità (1 bit);
- Tag (2 bit più significativi dell'indirizzo);
- Dato (l'intero indirizzo).

![[Pasted image 20211220164029.png|500]]

Avremo una **hit** se nella linea di cache, il tag coincide con il *TAG* da cercare e *V = 1*.

#### IL TAG
Poichè ogni blocco della cache può contenere parole provenienti da diverse locazioni di memoria, per verificare se un dato presente nella cache *corrisponde ad una certa locazione di memoria* usiamo il campo **tag**, ottenuto dai 2 [[Bit Più Significativo|bit più significativi]].

![[Immagine 2021-12-20 163210.png]]

#### LA VALIDITÀ
Per capire se il dato contenuto in un blocco della cache è **valido** oppure no si ricorre ad un campo, detto *validità*:
- Se il bit validità è 1, il dato può essere letto;
- Se il bit validità è 0, il dato viene ignorato.

All'avvio la cache è vuota, pertanto tutti i bit di validità saranno 0.



[^1]: Modulo: [<font color="Orange">Matematica</font>] resto(a, b), [<font color="Orange">Linguaggio C</font>] a%b

___
[[000 Indice AdE|↖ Ritorna all'indice ↖]]

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