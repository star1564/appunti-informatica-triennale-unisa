---
aliases: Mock-Up
tags:
  - corsi/informatica/ingegneria_software
paragrafo: Ciclo di Vita del Software
cssclasses:
  - 
---
>Un **[[002 Sistemi, Modelli e Viste#Modello|Modello]] del [[003 Ciclo di Vita del Software|Ciclo di Vita del Software]]** è una caratterizzazione descrittiva o prescrittiva di *come un sistema software viene o dovrebbe essere sviluppato*.

Devono definire l'organizzazione del lavoro e varie le linee guida riguardo la gestione totale del progetto.

Esistono vari modelli.

## Modello a Cascata (Waterfall)
> Il **Modello a Cascata** è un modello *sequenziale lineare*, ovvero una progressione sequenziale a cascata di fasi, senza ricicli per controllare al meglio i tempi e costi. È consigliato utilizzare questo modello *quando i requisiti sono ben definiti*.

In questo modello:
- Si definiscono e si **separano le varie fasi** e le attività del processo, *rendendo nullo l'overlap fra le fasi*;
- Ogni fase raccoglie un insieme di attività, tecnologie e skill del personale per poter *completare le suddette attività*;
- Alla fine di ogni fase, produce degli **elaborati intermedi** che verranno utilizzati come *input per la fase successiva*;
	- I prodotti ottenuti da una fase *non possono essere modificati* durante il processo di elaborazione delle fasi successive.

![[Pasted image 20240116150041.png|600]]


> [!success] Vantaggi
> Facile da comprendere e da applicare.

> [!failure] Svantaggi
> - L'interazione con il cliente avviene solo all'inizio e alla fine del ciclo.
> - I requisiti del clienti potrebbero non essere molto chiari fino alla fine del ciclo.
> - Se il prodotto non ha soddisfatto tutti i requisiti alla fine del ciclo, è necessario iniziare daccapo l'intero processo.


### Fasi specifiche al modello a Cascata
| Fase                               | Output                                                                                     | Descrizione                                                                                            |
| ---------------------------------- | ------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------ |
| **Studio di Fattibilità**          | *Documento di fattibilità* <br>(definizione problema, scenari, costi e tempi)              | Una valutazione preliminare dei costi e dei requisiti ottenuta attraverso il cliente                   |
| **Analisi dei Requisiti**          | *Documento di specifica dei requisiti*<br>(manuale utente, piano di acceptance test)<br>   | Analisi completa dei bisogni dell'utente e del dominio del problema                                    |
| **Progettazione**                  | *Documento di specifica del progetto*<br>(possibile uso di linguaggi per la progettazione) | Definizione di una opportuna struttura del software e scomposizione del sistema in componenti e moduli |
| **Sviluppo e Test di unità**       | *Moduli sviluppati*                                                                        | Ogni modulo viene codificato nel linguaggio scelto e testato in isolamento                             |
| **Integrazione e Test di sistema** | *Prodotto da installare*                                                                   | Composizione dei moduli nel sistema globale e verifica del corretto funzionamento                      |
| **Deployment**                     | *Software installato e attivo*                                                             | Distribuzione e gestione del software presso l'utenza                                                  |
| **Manutenzione**                   | -                                                                                          | Gestione dell'evoluzione del software                                                                  |

Quasi tutte le fasi sono solitamente uguali per tutti gli altri modelli CVS.

### Modello a Cascata V&V con Feedback (Iterativo)
>Il **Modello a Cascata Iterativo** aggiunge al modello a cascata classico dei *ricicli*. Ovvero, alla fine di ogni fase, viene eseguita una verifica:
>- Se questa verifica viene superata, si passa alla fase successiva.
>- Altrimenti si ritorna alla fase precedente.

![[Pasted image 20240116151123.png|600]]

## Modello a V
>Il **Modello a V** è un tipo di modello in cui il processo viene eseguito in modo sequenziale seguendo una forma a "V", in cui *le attività di sinistra sono collegate a quelle di destra*. Se si trova un errore in una fase a destra, si *riesegue* il pezzo collegato a sinistra.

![[Pasted image 20230926113007.png|650]]

## Modello a Prototipi
>Il **Modello a Prototipi** è un approccio nel quale viene creato un *prototipo preliminare*. Questo prototipo *aiuta a comprendere i requisiti o a valutare la fattibilità* di un approccio ed è la realizzazione di una prima implementazione. Il prototipo è anche un mezzo con il quale si interagisce con il committente per accettarsi di aver ben compreso le sue richieste.

![[Pasted image 20230926114357.png|650]]

Dopo l'utilizzo del prototipo, si passa alla produzione della versione definitiva del sistema utilizzando spesso un modello di sviluppo a cascata.

In questo modello si utilizzano spesso:
- **Mock-ups**: una *interfaccia utente fittizia* che consente di definire con completezza e senza ambiguità i requisiti finali.
  ![[Pasted image 20240118104528.png|500]] ^920367
- **Breadboards**: implementazione di sottoinsiemi di *funzionalità critiche* del sistema, concentrandosi su aspetti come carichi elevati o tempi di risposta.

I due tipi di prototipazione sono:
- **Throw-Away**: utilizzata per ottenere una migliore comprensione dei requisiti del prodotto da sviluppare. L'idea è *iniziare lo sviluppo dalla parte dei requisiti meno chiari e compresi*.
- **Esplorativa**: serve per sviluppare il prodotto finale a partire da una descrizione approssimativa, lavorando a stretto contatto con il committente. In questo caso, l'obiettivo è *iniziare lo sviluppo dalla parte dei requisiti meglio compresi*.

## Modello a Sviluppo Evolutivo
>Lo **Sviluppo Evolutivo** è una metodologia *iterativa* di sviluppo del software che si basa sull'idea di sviluppare il software in *piccoli passi successivi*, ciascuno dei quali aggiunge funzionalità o miglioramenti al sistema esistente. 

Il ciclo di sviluppo si ripete più volte, consentendo una continua evoluzione del software in base alle esigenze degli utenti e alle scoperte fatte durante il processo di sviluppo.

![[Pasted image 20240116153330.png|600]]

> [!failure] Svantaggi
>- Perdita di visibilità del processo
>- Spesso il prodotto finito è scarsamente strutturato
>- Perdita di visibilità del processo da parte del management

## Modello a Sviluppo Incrementale
>Lo **Sviluppo Incrementale** è un approccio *incrementale* che affronta la difficoltà di creare un intero sistema software in un'unica volta, specialmente in progetti di grandi dimensioni. Suddivide il sistema in *incrementi* o sottosistemi, che vengono implementati, testati, rilasciati e mantenuti *in tempi diversi*, permettendo di consegnare il prodotto in *più rilasci*, affrontando le sfide finanziarie e logistiche, ed è particolarmente utile per progetti complessi. 

![[Pasted image 20230926115815.png|600]]

Le fasi iniziali del processo sono completamente realizzate, e l'integrazione dei nuovi sottosistemi con quelli esistenti diventa una parte fondamentale del processo.

> [!success] Vantaggi
>- Anticipare Funzionalità: consente di offrire al committente alcune funzionalità fin dall'inizio, soddisfacendo i requisiti di alta priorità.
>- Rilasci Graduali: ogni incremento corrisponde al rilascio di una parte delle funzionalità. I requisiti prioritari vengono rilasciati prima, riducendo il rischio di un fallimento completo del progetto.  
>- Testing Approfondito: i rilasci iniziali fungono da prototipi, aiutando a identificare requisiti per gli incrementi successivi. I servizi prioritari sono soggetti a test più approfonditi.

### Differenza tra Iterativo ed Incrementale
La principale differenza tra i due modelli è il modo in cui vengono affrontate le iterazioni. 

- Nel modello iterativo, le iterazioni coinvolgono lo sviluppo, il test e il miglioramento delle funzionalità esistenti. 
- Nel modello incrementale, le iterazioni coinvolgono l'aggiunta di nuove parti o incrementi al sistema esistente. 

Entrambi gli approcci sono utilizzati per affrontare i cambiamenti dei requisiti e per fornire valore in modo incrementale, ma seguono strategie leggermente diverse per farlo.

![[Pasted image 20230926120628.png|550]]

## Modello a Spirale
>Il **Modello a Spirale** è un modello che fornisce supporto per la *Gestione del Rischio*. Nella sua rappresentazione grafica, assomiglia a una *spirale* con molte curve. Il numero esatto di curve della spirale è sconosciuto e può variare da progetto a progetto. Ogni curva della spirale è chiamata *fase del processo*.

È fondato sul riciclo e ogni giro della spirale rappresenta una fase del processo che prevede la scoperta, la valutazione e il trattamento esplicito dei rischi.

![[Pasted image 20230926121124.png|600]]

È un *Meta-Modello*, cioè un modello per costruire un altro modello, dove si possono usare più modelli.

___
[[000 Indice IS|↖ Ritorna all'indice ↖]]

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