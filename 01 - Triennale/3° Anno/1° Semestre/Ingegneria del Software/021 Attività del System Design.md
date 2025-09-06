---
aliases:
  - Modello di System Design
  - Obbiettivi di Progettazione
  - Matrice degli Accessi
  - Decomposizione in sottosistemi
tags:
  - corsi/informatica/ingegneria_software
paragrafo: Progettazione - System Design
cssclasses:
  - 
---
Le prime due attività fanno *scegliere gli obbiettivi di progettazione* e *trasformano gli oggetti ottenuti* dall'[[015 Object Modeling|Object Modeling]] in [[019 System Design#Sottosistema|sottosistemi]].

Il resto delle attività si focalizza sulla *progettazione degli obbiettivi scelti*.

Le attività del [[019 System Design|System Design]] sono le seguenti.

![[Pasted image 20240119132501.png|600]]

Seguendo queste attività si ottiene un **Modello di System Design**, che verrà scritto all'interno del [[022 System Design Document|System Design Document]].

## 1 - Identificare gli Obbiettivi di Progettazione

>La definizione degli obiettivi di progettazione è il primo passo della progettazione di un sistema. Identifica le *qualità del sistema su cui gli sviluppatori devono concentrarsi*. 
>Molti **obiettivi di progettazione** possono essere dedotti dai [[008 Requisiti#Requisiti Non Funzionali|Requisiti Non Funzionali]] o dal [[002 Sistemi, Modelli e Viste#^ae7aa9|Dominio di Applicazione]].

È importante dichiararli esplicitamente, poiché ogni decisione di progettazione importante deve essere presa in modo *coerente*, seguendo lo stesso insieme di **criteri**.

I criteri più importanti si trovano in cinque gruppi.

> [!note]- Criteri di Performance
>Questi criteri includono qualità riguardo allo *spazio utilizzato e la velocità*.
>
>- **Tempo di risposta**: il tempo necessario per riconoscere una richiesta dell'utente dopo che questa è stata inviata, indicando la rapidità con cui viene fornita una prima risposta o azione in risposta alla richiesta.
>- **Velocità di elaborazione (Throughput)**: la capacità di un sistema di completare o gestire un determinato numero di attività o operazioni in un periodo di tempo fisso, indicando la sua efficienza nell'esecuzione delle funzioni assegnate.
>- **Memoria**: La memoria fisica richiesta per il funzionamento del sistema.

> [!note]- Criteri di Dependability
>Questi criteri includono qualità riguardo allo *sforzo necessario per minimizzare crash di sistema e risolvere le loro conseguenze*.
>
>- **Robustness**: capacità di sopravvivere a input non validi da parte dell'utente.
>- **Reliability**: differenza tra il comportamento specificato e quello osservato.
>- **Availability**: percentuale di tempo in cui il sistema può essere utilizzato per svolgere le normali attività.
>- **Tolleranza ai guasti**: capacità di operare in condizioni errate.
>- **Sicurezza**: capacità di resistere ad attacchi malevoli.
>- **Safety**: capacità di evitare di mettere in pericolo vite umane, anche in presenza di errori e guasti.

> [!note]- Criteri di Costo
>Questi criteri includono qualità riguardo al *costo dello sviluppo, deployment e amministrazione del sistema*.
>
>- **Costo di sviluppo**: costo di sviluppo del sistema iniziale.
>- **Costo di deployment**: costo dell'installazione del sistema e della formazione degli utenti.
>- **Costo di aggiornamento**: costo di traduzione dei dati dal sistema precedente (retrocompatibilità).
>- **Costo di manutenzione**: costo necessario per la correzione dei bug e per i miglioramenti del sistema.
>- **Costo di amministrazione**: costo necessario per amministrare il sistema dopo l'installazione.

> [!note]- Criteri di Manutenzione
>Questi criteri includono qualità riguardo alla *difficoltà per la modifica o manutenzione del sistema*.
>
>- **Estensibilità**: quanto è facile aggiungere funzionalità o nuove classi al sistema?
>- **Modificabilità**: quanto è facile cambiare la funzionalità del sistema?
>- **Adattabilità**: quanto è facile adattare il sistema a diversi domini applicativi?
>- **Portabilità**: quanto è facile portare il sistema su piattaforme diverse?
>- **Leggibilità**: quanto è facile capire il sistema leggendo il codice?
>- **Tracciabilità dei requisiti**: quanto è facile mappare il codice su requisiti specifici?

> [!note]- Criteri per gli End User
>Questi criteri includono qualità *desiderabili all'utente*.
>
>- **Utilità**: quanto, il sistema, supporta il lavoro dell'utente?
>- **Usabilità**: quanto è facile per l'utente utilizzare il sistema?

### Trade-off tra Criteri
Quando si definiscono gli obbiettivi, solo un piccolo insieme di questi criteri possono essere presi simultaneamente. Quindi è necessario *dare priorità a certi criteri* rispetto ad altri.

> [!tip]- Spazio vs. velocità
>- Se il software non soddisfa i requisiti di tempo di risposta o di velocità, è possibile utilizzare più memoria per velocizzare il software (ad esempio, caching, maggiore ridondanza). 
>- Se il software non soddisfa i vincoli di spazio di memoria, i dati possono essere compressi a scapito della velocità.

> [!tip]- Tempi di consegna vs. funzionalità
>- Se lo sviluppo è in ritardo rispetto alla tabella di marcia, il project manager può consegnare in tempo una funzionalità inferiore a quella specificata o consegnare l'intera funzionalità in un momento successivo. 
>- Il software a contratto di solito pone maggiore enfasi sulla funzionalità, mentre i progetti di software off-the-shelf pongono maggiore enfasi sulla data di consegna.

> [!tip]- Tempi di consegna vs. qualità
>Se i test sono in ritardo rispetto alla tabella di marcia, il project manager può consegnare il software in tempo con i bug noti (ed eventualmente fornire una patch successiva per risolvere i bug più gravi), oppure consegnare il software in un secondo momento con meno bug.

> [!tip]- Tempo di consegna vs. personale
>- Se lo sviluppo è in ritardo, il project manager può aggiungere risorse al progetto per aumentare la produttività. Nella maggior parte dei casi, questa opzione è disponibile solo all'inizio del progetto: l'aggiunta di risorse di solito riduce la produttività mentre il nuovo personale viene formato o aggiornato.
>- Da notare che l'aggiunta di risorse aumenterà anche il costo dello sviluppo.

## 2 - Identificare i sottosistemi (decomposizione in sottosistemi)
La **decomposizione** consiste nel ridurre la complessità andando a decomporre il sistema *in parti piu piccole*.

Trovare i sottosistemi durante la progettazione del sistema è simile a trovare gli oggetti durante l'[[015 Object Modeling|analisi]].

> [!tip] Euristiche per trovare i sottosistemi
>- Assegnare gli oggetti identificati in un caso d'uso allo stesso sottosistema.
>- Creare un sottosistema dedicato agli oggetti utilizzati per spostare i dati tra i sottosistemi.
>- Ridurre al minimo il numero di associazioni che attraversano i confini del sottosistema.
>- Tutti gli oggetti dello stesso sottosistema devono essere funzionalmente correlati.

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20240119123342.png]]

> [!warning]- Il diagramma dei sottosistemi deve essere UNICO
> Non si deve separare in più diagrammi e deve includere tutte le interazioni tra i diversi sottosistemi.


## 3 - Affrontare gli Obiettivi di Progettazione
Qui eseguiamo attività inerenti agli obbiettivi di progettazione. Infatti, bisogna affrontare dei problemi in tutto il sistema quando si deve decomporre un sistema.

Queste sono le problematiche.

### Mapping Hardware/Software
In questa fase si decidono le *piattaforme hardware e software necessitate dal sistema*. Una volta decise queste piattaforme, si dovrà eseguire la mappatura tra hardware e software.

Molti sistemi complessi necessitano di lavorare su più di un computer interconnessi da rete. L'uso di più computer può ottimizzare le performance e permettere l'utilizzo del sistema a più utenti distribuiti sulla rete.

Affrontare i problemi di mappatura hardware/software spesso porta alla definizione di sottosistemi aggiuntivi dedicati allo *spostamento dei dati da un nodo all'altro*, alla gestione della concorrenza e ai problemi di affidabilità. 

Una volta decise le piattaforme, è necessario mappare le **componenti** ed i **nodi** per gestire problemi di concorrenza. Si utilizza un [[UML 8 - Diagramma di Deployment|Deployment Diagram]].

### Gestione dei Dati
In questa fase si decide *come e dove conservare i dati [[033 Java Persistence API|persistenti]]*.
Vanno identificati gli **oggetti persistenti** ed il tipo di infrastruttura da usare per memorizzarlo.

Questo porta alla selezione del [[DBMS]] e alla creazione di un altro sottosistema dedicato alla gestione dei dati persistenti.

Dato che molte applicazioni girano attorno alla funzionalità di conservazione/modifica di dati, l'accesso a questi deve essere *veloce e senza corruzioni*.

### Controllo degli Accessi
In questa fase si decide *chi può accedere a dati protetti e come può farlo*. Questi controlli devono essere consistenti in tutto il sistema.

È possibile rappresentare le politiche di accesso tramite una **matrice di accesso** in tre modi:
- **Tabella di accesso globale**: ogni *cella* della matrice contiene una tupla `(attore, classe, operazione)`. Se questa è presente per una determinata classe ed operazioni, l'accesso è consentito. Quindi le righe rappresentano gli attori, le *colonne* rappresentano le classi, mentre le *celle* (**diritti di accesso**) listano quali *operazioni* possono essere eseguite da un determinato attore in una determinata classe.
- **Access Control List** (ACL): ogni classe ha una lista che contiene una coppia `(attore, operazione)` che specifica se l'attore può accedere a quella determinata operazione della classe a cui la ACL appartiene.
- **Capability**: una capability associa una coppia `(classe, operazione)` ad un attore. Una capability consente a un attore di accedere a un oggetto della classe descritta nella capability. Negare una capacità equivale a negare l'accesso. 



> [!example]- <font color="orange">Esempio</font>
> ![[Pasted image 20240316095336.png|800]]
> 
>Matrice di accesso per un sistema bancario (tabella di accesso globale). 
>
>- I `Teller` possono eseguire piccole transazioni ed esaminare i saldi di un *`Account`*. 
>- I `Manager` possono eseguire transazioni più grandi sugli *`Account`* e accedere alle statistiche delle *`LocalBranch`*, oltre alle operazioni accessibili ai `Teller`. 
>- Gli `Analist` possono accedere alle statistiche di tutte le filiali (*`Corporation`* e *`LocalBranch`*), ma non possono eseguire operazioni a livello di conto.

### Controllo del Flusso
In questa fase si sceglie un un *tipo di controllo del flusso*. Questo può impattare diversi sottosistemi, dato che si deve gestire la [[Concorrenza]].

Esistono tre tipi di controllo di flusso:
- **Procedure-driven control**: le operazioni rimangono in attesa di un input dell'utente ogni volta che hanno bisogno di elaborare dati. Questo tipo è usato in sistemi di legacy e di tipo procedurale.
- **Event-driven control**: un ciclo principale aspetta il verificarsi di un evento esterno. Quando l'evento diventa disponibile, la richiesta viene direzionata all'opportuno oggetto.
- **[[023 Thread|Thread]]**: versione della procedure-driven con la gestione della concorrenza. Il sistema può creare un numero di threads ed assegnare ciascuno di questi ad un evento.

### Condizioni al Boundary
In questa fase si specifica *come il sistema sarà inizializzato, spento e come gestirà le eccezioni*. Si specificano attraverso [[011 Scenari e Casi d_Uso#Caso d'Uso|casi d'uso]] per:
- **Inizializzazione**
- **Terminazione**
- **Fallimento**

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