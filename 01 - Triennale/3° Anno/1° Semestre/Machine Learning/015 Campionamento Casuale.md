---
aliases:
- Campionamento casuale semplice
- Campionamento stratificato
- Campionamento pesato
- Campionamento a serbatoio
- Campionamento per importanza
tags:
  - corsi/informatica/machine_learning
paragrafo: Training Data
cssclasses:
  - 
---
Per ottenere modelli più affidabili, le tecniche di [[014 Campionamento Non Probabilistico|Campionamento Non Probabilistico]] non sono le più indicate, infatti in queste si ha sempre la potenziale presenza di [[Bias]] ed altri problemi.

>Il **campionamento probabilistico** comprende tutte le tecniche di selezione dati che *si basano proprio sulla definizione di criteri probabilistici*. In questo tipo di campionamento non ci sono molti problemi di bias.

Tra le tecniche più importanti possiamo trovare 
- *Campionamento casuale semplice (Simple Random Sampling)*
- *Campionamento stratificato (Stratified Sampling)*
- *Campionamento pesato (Weighted Sampling)*
- *Campionamento a serbatoio (Reservoir Sampling)*
- *Campionamento per importanza (Importance Sampling)*

## Campionamento Casuale Semplice
>La tecnica di **Campionamento Casuale Semplice**, anche nota come "Simple Random Sampling", prevede la *selezione dei campioni in maniera casuale*, assegnando *uguale probabilità di selezione ad ogni elemento* della popolazione.

![[Pasted image 20231215172738.png|500]]

> [!success] Vantaggi
> - Facile da implementare
> - Semplicità nella definizione dei criteri di selezioni

> [!failure] Svantaggi
> - Classi rare possono non essere scelte
> - Problematico con carenza di dati
> - Non tiene conto delle correlazioni tra i dati
> - Può essere costoso con popolazioni molto grandi

> [!example]- <font color="orange">Esempio</font>
>Supponiamo di voler acquisire informazioni sulle preferenze musicali degli studenti universitari. Abbiamo una popolazione di 1000 studenti e vogliamo sceglierne 100.
>
>Con questo tipo di campionamento , seguiamo i seguenti passi:
>- Assegniamo un identificativo ad ognuno dei 1000 studenti;
>- Definiamo una probabilità di selezione uguale per ognuno di essi;
>- Utilizziamo un estrattore di numeri casuali per definire i 100 campioni da scegliere.

## Campionamento Stratificato
>La tecnica di **Campionamento Stratificato**, anche nota come "Stratified Sampling", è una tecnica che mira a superare le limitazioni del campionamento semplice, in cui *la popolazione viene inizialmente suddivisa in gruppi distinti*. Successivamente, *si effettua il campionamento in modo separato e indipendente* da ciascun gruppo.

![[Pasted image 20231215173403.png|700]]

Questo approccio si rivela estremamente utile quando si lavora con popolazioni:
- Eterogenee;
- Che *presentano diverse classi rare*;
- Con membri che manifestano caratteristiche ben definite.

Il Campionamento Stratificato consente di ottenere *dataset bilanciati e rappresentativi della complessità della realtà*, garantendo una copertura accurata di ciascun strato della popolazione, migliorando così la precisione complessiva del campionamento.

> [!success] Vantaggi
> - Rappresentatività
> - Precisione nel campionamento
> - Efficienza
> - Permette di fare confronti tra gruppi

> [!failure] Svantaggi
> - Difficile scegliere il criterio di raggruppamento
> - In alcuni casi, è impossibile dividere la popolazione in gruppi
> - Complesso da implementare
> - Costi maggiori
> - Analisi molto complesse

> [!example]- <font color="orange">Esempio</font>
>Supponiamo di voler condurre un'indagine sulla soddisfazione dei clienti di un grande centro commerciale. La popolazione è composta da clienti di diverse fasce di età: giovani (18-25 anni), adulti (26-40 anni) e anziani (oltre i 40 anni).
>
>Dividiamo la popolazione in gruppi , prendendo in considerazione l'età, e selezioniamo 50 campioni da ogni strato:
>- Strato 1: clienti giovani
>- Strato 2: adulti
>- Strato 3: anziani
>
>Per ogni strato, selezioniamo i 50 campioni applicando una selezione casuale

## Campionamento Pesato
>La tecnica di **Campionamento Pesato**, anche nota come "Weighted Sampling", si basa sull'*assegnazione di un peso a ciascun elemento o classe della popolazione*, considerando la distribuzione complessiva della popolazione.

![[Pasted image 20231215174130.png]]

Principali caratteristiche di questa tecnica:
- **Definizione dei Pesi**: i pesi vengono assegnati *in base alla distribuzione della popolazione*, riflettendo la probabilità di selezione di ciascun campione.
- **Rappresentazione di Probabilità**: un *peso più elevato* corrisponde a una *probabilità maggiore di essere scelto* durante il campionamento.

Questa metodologia si dimostra particolarmente efficace in presenza di popolazioni che:
- Presentano classi di dimensioni notevolmente diverse;
- Contengono sottopopolazioni rare;
- Richiedono l'ottimizzazione dell'uso delle risorse disponibili.

Inoltre, il campionamento pesato offre una *soluzione* al problema del *bias di non risposta*, ovvero la mancata partecipazione di alcune entità campionate.

> [!success] Vantaggi
> - Rappresentatività
> - Precisione nel campionamento
> - Flessibilità del campione
> - Permette di gestire situazioni in cui abbiamo carenza di dati


> [!failure] Svantaggi
> - Complesso calcolare i vari pesi
> - Maggiore difficoltà nell'analisi dei risultati
> - Possibile presenza di bias
> - Gestione dei pesi molto complessa

> [!example]- <font color="orange">Esempio</font>
>Supponiamo di voler creare un dataset attingendo da una popolazione reale che presenta campioni classificati come rossi ed altri come blu.
>
>Sappiamo che, nella popolazione abbiamo una proporzione tra rossi e blu pari a $25/75$. Inoltre, sappiamo che nel mondo reale, sia rossi che blu hanno la stessa
>probabilità di essere scelti.
>
>In questo caso, il campionamento pesato ci permette di ottenere un dataset equilibrato
>
>Possiamo assegnare peso $\color{#61AFEF}1$ ad un esempio blu, mentre per i rossi, essendo un terzo di quelli blu, diamo un peso pari a $\color{#CC241D}3$, in modo tale da avere una probabilità di scelta maggiore.

## Campionamento a Serbatoio
>La tecnica di **Campionamento a Serbatoio**, anche nota come "Reservoir Sampling", è un approccio utilizzato in *situazioni dinamiche* in cui la popolazione è in *costante cambiamento*, come nel caso di uno streaming di dati nell'[[003 Apprendimento in modo Incrementale|Apprendimento Online]].

![[Pasted image 20231215174900.png|700]]

Il procedimento coinvolge la definizione di un *serbatoio*, che può assumere la forma di un array, un set, o una struttura simile. 

Inizialmente, questa struttura *viene riempita con un numero prefissato* di elementi. Man mano che nuovi elementi diventano disponibili, quelli presenti nel serbatoio *vengono rimpiazzati* seguendo un criterio predefinito.

Attraverso questa tecnica, è possibile *eseguire un campionamento in cui ogni elemento ha la stessa probabilità di essere selezionato*, anche in assenza di informazioni sulle dimensioni esatte della popolazione.

> [!success] Vantaggi
> - Adatto a popolazioni dinamiche
> - Efficienza del campionamento
> - Rappresentatività per streaming data
> - Adatta a popolazioni di grandissime dimensioni

> [!failure] Svantaggi
> - Complessità elevata
> - Applicazione limitata
> - Possibilità di avere distorsioni nella selezione dei campioni
> - Difficile gestione dei criteri di selezione

> [!example]- <font color="orange">Esempio</font>
>Supponiamo di voler definire un dataset di tweet provenienti da uno stream.
>Data la natura della sorgente , non possiamo conoscere quanti tweet abbiamo a disposizione e, ovviamente , non possiamo memorizzarli tutti in memoria.
>Quindi, non conosciamo la probabilità di scelta di ogni tweet.
>
>Il campionamento a serbatoio ci viene in aiuto.
>- Inizialmente, inseriamo i primi $k$ tweet nel nostro serbatoio
>- Per ogni $n$-esimo elemento, generiamo un numero casuale $i$, tale che $$1 < i < n$$
>- Se $i$ è compreso tra $1$ e $k$, allora rimpiazziamo l'elemento i simo nel serbatoio con l'elemento $n$-esimo. In caso contrario, non facciamo nulla.

## Campionamento per Importanza
Questa è una delle tecniche più conosciute e rilevanti, non solo nell'ambito del Machine Learning.

>La tecnica di **Campionamento per Importanza**, anche nota come "Importance Sampling", è impiegata quando si desidera *campionare una [[024 Funzione di Distribuzione|distribuzione]] complessa sfruttando un'altra distribuzione più facile da campionare*.

![[Pasted image 20231215180025.png|700]]

Immaginiamo di avere un campione $x$ da una distribuzione $P(x)$, che può essere *costosa, lenta o difficile da campionare direttamente*. 
Introduciamo un'altra distribuzione $Q(x)$, *più agevole da campionare*. 

Con il campionamento per importanza, è possibile estrarre $x$ da $Q(x)$ e pesare il campione con $$\frac{P(x)}{Q(x)}$$
In questo contesto, $Q(x)$ è comunemente denominata **proposal distribution** o **importance distribution**.

> [!success] Vantaggi
> - Efficiente con distribuzioni difficili da campionare
> - Adattabilità a diversi scenari di analisi
> - Possibilità di estendere l'intervallo di campionamento per includere valori rari o di interesse

> [!failure] Svantaggi
> - Difficoltà nella scelta della importance distribution
> - Varianza delle stime
> - Computazionalmente complesso
> - Distorsione nelle stime se le distribuzioni sono troppo diverse tra loro.


> [!example]- <font color="orange">Esempio</font>
>L'esempio più classico è legato ai modelli di [[002 Apprendimento con Supervisione Umana#Per Rinforzo|Reinforcement Learning]] basati su policy. Consideriamo il caso in cui si vuole aggiornare la policy del modello.
>
>Possiamo cercare di stimare i valori della nuova policy, ma calcolare il totale delle ricompense ottenute facendo un'azione può essere lento e costoso, in quanto si devono tener conto di tutti i possibili scenari.
>
>Tuttavia, se la nuova policy è molto simile alla vecchia, possiamo calcolare le ricompense totali della vecchia policy e ripesare queste ricompense secondo la nuova policy definita.

___
[[000 Indice ML|↖ Ritorna all'indice ↖]]

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