---
aliases:
  - Traduzione verso il modello logico
tags:
  - corsi/informatica/basi_di_dati
paragrafo: Modello Relazionale
cssclasses:
  - 
---
Analizziamo i passi di un algoritmo per la mappatura di uno [[007 Modello ER|schema ER]] in uno [[010 Modello Relazionale|schema Relazionale]] utilizzando come esempio la base di dati `AZIENDA`.

![[IMG_20221208_112851.jpg|950]]

> [!success]- Risultato Finale
>![[IMG_20221208_154031 1.jpg|950]]

---
### 1. MAPPATURA DELLE ENTITÀ FORTI
Per ogni tipo di entità (forte) $E$ nello schema ER, si costruisca una tabella (relazione) $R$ che contenga tutti gli [[ER4_Tipi di Attributi#^9c4be0|attributi semplici]] di $E$.

Di un [[ER4_Tipi di Attributi#^b63ded|attributo composto]] si inseriscano solo gli attributi componenti semplici.

Si scelga come [[ER5_ Attributi Chiave|chiave principale]] per $R$ uno degli attributi chiave di $E$. 
- Se la chiave scelta per $E$ è composta, l'insieme di attributi semplici che la compongono formeranno nel complesso la chiave primaria di $R$.
- Se, durante la progettazione concettuale, sono state identificate più chiavi per $E$, quelle non scelte per diventare chiave primaria si mantengono per specificare le chiavi secondarie.

Nel nostro esempio si costruiscono le relazioni `IMPIEGATO`, `DIPARTIMENTO` e `PROGETTO`.
Essi comprendono gli attributi semplici delle stesse entità nello schema ER.

Come chiavi principali vengono scelte:
- *`SSN`* per `IMPIEGATO`;
- *`NUMERO_D`* per `DIPARTIMENTO`;
- *`NOME_P`* per `PROGETTO`.

![[Pasted image 20221208155802.png|500]]

---
### 2. MAPPATURA DELLE ENTITÀ DEBOLI
Per ogni tipo di [[ER9_Entità e Relazioni Deboli#^b5cc34|entità debole]] $W$ dello schema ER collegato ad una entità $E$, si costruisca una tabella $R$ e si inseriscano tutti gli attributi semplici (o componenti semplici degli attributi composti) di $W$ come attributi di $R$.

Inoltre si inseriscano come attributi di [[011 Vincoli del Modello Relazionale#^79e163|chiave esterna]] di $R$ gli attributi di chiave primaria della entità proprietaria $E$.
La chiave primaria di $R$ è data dalla combinazione delle chiavi primarie delle entità proprietà (come $E$) e dalla chiave parziale di $W$.

Se esiste un tipo di entità debole $E_2$, il cui proprietario è a sua volta un tipo di entità debole $E_1$, allora $E_1$ dovrebbe essere mappato prima di $E_2$ in modo da determinare prima la sua chiave primaria.

Per sapere qual è l'attributo chiave originale di quello esterno, alla fine si collega la chiave esterna alla chiave originale con una freccia.

Nel nostro esempio si costruisce la relazione `PERSONA_A_CARICO`, che corrisponde alla omonima relazione debole nello schema ER e si inseriscono gli attributi.
Si inserisce la chiave primaria *`SSN`* della relazione `IMPIEGATO` (che corrisponde al tipo di entità proprietario) come attributo di chiave esterna. Questo può essere rinominato a `SSN_I` per non confonderlo con quello originale.

![[Pasted image 20221208155943.png|500]]

---
### 3. MAPPATURE DEI TIPI DI RELAZIONI
#### RELAZIONI BINARIE 1:1
Per ogni tipo di relazione $R$ [[ER7_Tipi Relazioni#^220e06|binaria 1:1]] nello schema ER si individuino le relazioni $S$ e $T$ corrispondenti ai tipi di entità che partecipano a $R$.

L'approccio più semplice è quello *basato su chiavi esterne*:
- Si scelga una delle relazioni (per esempio $S$) e si inserisca al suo interno come chiave esterna la chiave primaria di $T$.
	- È meglio scegliere nel ruolo di $S$ un tipo di entità con [[007 Modello ER#VINCOLI DI PARTECIPAZIONE|partecipazione totale]].
- Si inseriscano come attributi di $S$ tutti gli attributi semplici (o componenti semplici di attributi composti) che si trovano nella relazione $R$.

Nel nostro esempio si deve mappare il tipo di relazione 1:1 `DIRIGE`. Scegliamo al posto di $S$ `DIPARTIMENTO` ed al posto di $T$ `IMPIEGATO`.

![[Pasted image 20221208123059.png]]

Si inserisce la chiave primaria di `IMPIEGATO` (*`SSN`*) all'interno di `DIPARTIMENTO` e lo chiameremo *`SSN_DIR`*. Inseriamo inoltre l'attributo *`DATA_INIZIO_DIR`*.

Avremo quindi il seguente:
![[Pasted image 20221208160052.png|500]]

#### RELAZIONI BINARIE 1:N
Per ogni tipo di relazione $R$ [[ER7_Tipi Relazioni#^4ad5e9|binaria 1:N]] (o anche [[ER7_Tipi Relazioni#^27ba3d|binaria N:1]]) nello schema ER si individuino le relazioni $S$ e $T$ corrispondenti ai tipi di entità che partecipano a $R$.

L'approccio più semplice è quello *basato su chiavi esterne*:
- Si inserisca in $S$ come chiave esterna la chiave primaria della relazione $T$.
	- Viene scelto come $S$ l'entità che si trova al lato con $N$, mentre $T$ è l'altra relazione (con cardinalità $1$).
- Si inseriscano come attributi di $S$ tutti gli attributi semplici (o componenti semplici di attributi composti) che si trovano nella relazione $R$.

Nel nostro esempio, si mappano i tipi di relazione 1:N seguenti: 
- **`LAVORA_PER`**: $S$ è `IMPIEGATO`, mentre $T$ è `DIPARTIMENTO`, quindi si inserisce in $S$ la chiave primaria di $T$ (*`NUMERO_D`*) e la rinominiamo *`N_D`*.

![[Pasted image 20221208152221.png|650]]

- **`CONTROLLA`**: $S$ è `PROGETTO`, mentre $T$ è `DIPARTIMENTO`, quindi si inserisce in $S$ la chiave primaria di $T$ (*`NUMERO_D`*) e la rinominiamo *`NUM_D`*.

![[Pasted image 20221208152623.png|660]]

- **`SUPERVISIONE`**: In questo caso si ha che $S=T$ dove $S$ e $T$ sono `IMPIEGATO`, quindi si riscrive in `IMPIEGATO` la sua chiave primaria sotto un nome diverso, ovvero *`SUPER_SSN`*.

![[Pasted image 20221208153029.png|400]]

Avremo quindi il seguente:
![[Pasted image 20221208161419.png|800]]

#### RELAZIONI BINARIE N:M
L'unica possibilità per le relazioni [[ER7_Tipi Relazioni#^365d02|N:M]] è quello di avere una *relazione di relazioni* (tabella di relazioni) dove si inserisce l'attributo chiave di $S$, l'attributo chiave di $T$ e gli eventuali attributi della relazione in una singola tabella.

Nel nostro esempio abbiamo `LAVORA_SU`. Quindi creiamo la tabella `LAVORA_SU` e ci inseriamo *`SSN_I`* (chiave di `IMPIEGATO`), *`N_P`* (chiave di `PROGETTO`) e l'attributo `ORE` della relazione.

![[Pasted image 20221208162825.png]]

---
### 4. MAPPATURE DEGLI ATTRIBUTI MULTIVALORE
Per ogni [[ER4_Tipi di Attributi#^abce31|attributo multivalore]] $A$ si costruisca una nuova relazione $R$. Questa comprenderà un attributo corrispondente ad $A$, più l'attributo di chiave primaria dell'entità da cui deriva questo attributo.

Per esempio si ha l'attributo multivalore `Sedi`. Si crea una nuova tabella `SEDI_DIP` dove ci sarà la chiave di `DIPARTIMENTO` (*`NUMERO_D`*) ed il valore dell'attributo stesso (*`SEDE_D`*).

![[Pasted image 20221208163752.png]]

---
> [!success]- Risultato Finale
>![[IMG_20221208_154031 1.jpg|950]]

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