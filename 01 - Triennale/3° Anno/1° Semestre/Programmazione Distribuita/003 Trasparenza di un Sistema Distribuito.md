---
aliases: 
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Introduzione
cssclasses:
  - 
---
>Un [[001 Sistema Distribuito|Sistema Distribuito]] deve essere **Trasparente**, ovvero che i dettagli del sistema che offre le funzionalità operative sono *nascoste agli utenti*.

Questo permette al progettista o sviluppatore di lavorare in un ambiente che non fornisce informazioni sull'architettura del sistema, consentendo di ignorare la eterogeneità delle componenti, i fallimenti specifici di componenti, ecc. Quindi *aumenta la produttività* e permette il *riuso delle applicazioni*.

![[Pasted image 20230929144601.png]]

Ci sono diversi tipi di trasparenza che rientrano in tre livelli:
1. Livello di *Base*;
2. Livello di *Funzionalità*;
3. Livello di *Efficienza*.

Ogni tipo di trasparenza è molto spesso *dipendente* da un altro tipo di trasparenza dal livello inferiore (non accade per i livelli di base).

## Livelli di Base
### Trasparenza di Accesso
>La trasparenza di **Accesso** fa in modo che non ci si debba preoccupare delle differenze su come i dati *sono rappresentati* o come *vengono richiamati*, in modo che oggetti diversi possano lavorare insieme senza problemi. 

Questo significa che gli oggetti devono avere un modo comune per essere raggiunti, sia che si trovino in un nodo locale che su nodi remoti. In questo modo, è semplice spostare un oggetto da un nodo a un altro mentre il programma sta già funzionando.

### Trasparenza di Locazione
>La trasparenza di **Locazione** significa che non ci si preoccupa di *dove si trovi una componente all'interno del sistema*. Quindi si può usare un oggetto senza dover sapere la sua posizione fisica nel sistema. 

Questo è importante, perché ci permette di *spostare le componenti da un nodo all'altro* senza problemi. Altrimenti, dovremmo sempre tener conto di dove si trovano fisicamente le componenti.

## Livelli di Funzionalità

### Trasparenza di Migrazione
>La trasparenza di **Migrazione** serve a nascondere il fatto che il sistema può *spostare un oggetto da un nodo all'altro*, ma allo stesso tempo farlo rimanere raggiungibile e utilizzabile da altri oggetti. 

Questo è utile per migliorare le prestazioni dei sistemi, distribuendo il lavoro tra i nodi o riducendo il tempo di accesso a una parte del sistema. Può anche aiutare a prevedere problemi o a riorganizzare il sistema, ma richiede un controllo automatico da parte dell'infrastruttura del sistema. 

Questa trasparenza si basa sulla [[#Trasparenza di Accesso|trasparenza di accesso]] e di [[#Trasparenza di Locazione|locazione]].

### Trasparenza di Replica
>Attraverso la trasparenza di **Replica**, il sistema *nasconde il fatto che una componente viene duplicata in diverse copie* chiamate "repliche" e collocate su altri nodi del sistema. Queste repliche forniscono gli stessi servizi della componente originale. 

Il sistema deve assicurarsi che lo stato di tutte le repliche *rimanga coerente* con quello della componente originale, in modo da garantire che le operazioni e i servizi mantengano la loro logica.

Le repliche servono a *migliorare le prestazioni*, duplicando le componenti in aree del sistema con *maggiore domanda per quei servizi*, riducendo così il tempo di accesso. Inoltre, le repliche consentono di scalare il sistema quando la richiesta di lavoro aumenta.

Questa trasparenza si basa sulla [[#Trasparenza di Accesso|trasparenza di accesso]] e di [[#Trasparenza di Locazione|locazione]].

### Trasparenza alla Persistenza
>La trasparenza alla **Persistenza** nasconde all'utente *il processo con cui il sistema mantiene un oggetto persistente* (cioè, memorizzato nella memoria a lungo termine) *quando non viene utilizzato attivamente*. 

Per migliorare le prestazioni del sistema, gli oggetti poco utilizzati vengono *de-attivati e spostati nella [[016 Memoria di Massa|Memoria Secondaria]]*, con il loro stato, tenendo solo un riferimento per riattivarli quando vengono richieste operazioni su di essi. Quando questo accade, l'oggetto *viene riportato nella [[030 Memoria Centrale|Memoria Principale]] e riattivato* per rispondere alla richiesta.

Gli oggetti che interagiscono con un oggetto "disattivato" *non notano la differenza* rispetto a quando interagiscono con un oggetto "attivo". 

Questo si basa sulla [[#Trasparenza di Locazione|trasparenza di locazione]], poiché l'accesso all'oggetto non dipende dalla sua posizione fisica, consentendo la riattivazione dell'oggetto anche su nodi diversi da quelli in cui è stato de-attivato.

### Trasparenza alle Transazioni
Un sistema distribuito coinvolge diverse risorse accessibili da vari nodi, il che significa che la concorrenza è comune. 

>La trasparenza alle **Transazioni** (o trasparenza di concorrenza) nasconde agli utenti *il processo di coordinamento che garantisce la coerenza degli oggetti* quando ci sono molte attività simultanee. Gli utenti, così come i progettisti e gli sviluppatori, non devono preoccuparsi delle complesse operazioni di coordinamento che assicurano la corretta esecuzione delle azioni, possono quindi pensare di essere gli unici utenti del sistema.

La gestione delle transazioni distribuite è complessa, ma delegarla al sistema semplifica notevolmente il lavoro degli sviluppatori di applicazioni. Avere la capacità di eseguire operazioni in modo transazionale è cruciale per evitare che una risorsa diventi inconsistente in caso di guasti o problemi.

Non si basa su alcun livello inferiore.

## Livelli di Efficienza
### Trasparenza alla Scalabilità
>La trasparenza alla **Scalabilità** garantisce che i progettisti e gli sviluppatori *non debbano preoccuparsi di come il servizio crescerà* per soddisfare le richieste in aumento.

Il sistema, tramite la [[#Trasparenza di Replica|trasparenza di replica]] e della [[#Trasparenza di Migrazione|migrazione]], gestirà automaticamente l'uso delle nuove risorse acquisite per affrontare il carico crescente.

### Trasparenza alle Prestazioni
>La trasparenza alle **Prestazioni** permette al progettista o allo sviluppatore *di non preoccuparsi dei dettagli tecnici usati per rendere il sistema efficiente* durante la fornitura dei servizi. 

Questo può includere azioni come il *bilanciamento del carico*, spostando parti del sistema da nodi sovraccarichi a quelli meno impegnati, riducendo la latenza tenendo le parti del sistema fisicamente vicine agli utenti che le utilizzano di più, o *gestendo la memoria* inattivando oggetti poco usati che possono essere riattivati se necessario. 

Questa trasparenza si basa sulla [[#Trasparenza di Migrazione|trasparenza della migrazione]], [[#Trasparenza di Replica|replica]] e [[#Trasparenza alla Persistenza|persistenza]].

### Trasparenza ai Malfunzionamenti
>La trasparenza ai **Malfunzionamenti** *nasconde agli oggetti eventuali problemi o guasti* (e, se necessario, il successivo recupero) *di altri oggetti con cui stanno interagendo*. Questa trasparenza vale anche per gli utenti del sistema, i quali non dovrebbero notare malfunzionamenti parziali all'interno del sistema, poiché il sistema stesso si occupa automaticamente di reindirizzare le richieste e fornire il servizio in un modo alternativo.

Questa trasparenza si basa sulla [[#Trasparenza di Replica|trasparenza di replica]], che permette di ripetere senza problemi operazioni che erano state iniziate su una copia di un oggetto, ora su un'altra copia. Inoltre, si appoggia alla [[#Trasparenza alle Transazioni|trasparenza sulle transazioni]]: operazioni complesse eseguite come parte di una transazione non vengono confermate (ovvero, non hanno effetto dato che non è avvenuto un "commit") se si verifica un guasto, quindi possono essere ripetute su un'altra copia dell'oggetto.


___
[[000 Indice PD|↖ Ritorna all'indice ↖]]

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