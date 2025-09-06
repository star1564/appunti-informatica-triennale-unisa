---
aliases:
  - Adapter Design Pattern
  - Bridge Design Pattern
  - Composite Design Pattern
  - Facade Design Pattern
  - Proxy Design Pattern
  - Command Design Pattern
  - Observer Design Pattern
  - Strategy Design Pattern
  - Abstract Factory Design Pattern
  - Builder Design Pattern
tags:
  - corsi/informatica/ingegneria_software
paragrafo: Progettazione - Object Design
cssclasses:
  - 
---
>Nello sviluppo Object Oriented, i **Design Pattern** sono dei *template standardizzati per risolvere un vasto numero di problemi ricorrenti*. Descrivono oggetti comunicanti e classi.

Un design pattern è tipicamente composto da poche classi correlate tramite [[024 Riutilizzo#Delegazione|Delegazione]] ed [[015 Ereditarietà|Ereditarietà]] per fornire una soluzione robusta e modificabile. Tali classi possono essere *adattate e ridefinite* per lo specifico sistema che si vuole realizzare.

> [!success] [Consiglio di vedere questo sito](https://refactoring.guru/design-patterns/catalog)

Esistono tre tipologie di pattern.

![[Pasted image 20240119164643.png|1400]]

> [!tip]- Alcune Euristiche per i pattern
>![[Pasted image 20240119182631.png|600]]

### Pattern Strutturali
Si occupano delle modalità di composizione di classi e oggetti per formare strutture complesse.

Riducono il [[019 System Design#Coupling|Coupling]] tra due o più classi, incapsulano strutture complesse, introducono una classe astratta per permettere estensioni future.

### Pattern Comportamentali
Si occupano di algoritmi e dell'assegnazione delle responsabilità tra oggetti che collaborano tra loro. Caratterizzano i flussi di controllo complessi da seguire al runtime.

### Pattern Creazionali
Forniscono un'astrazione del processo di creazione degli oggetti. Aiutano a rendere un sistema indipendente dalle modalità di creazione, composizione e rappresentazione degli oggetti usati.

## Composite Pattern
>Il **Composite** è un pattern strutturale che compone gli oggetti in una *struttura ad [[013 Alberi|Albero]]* di ampiezza e profondità variabile. Permette ai client di poter utilizzare queste strutture come se fossero oggetti individuali.

![[Pasted image 20240119170759.png|600]]

Struttura:
- L'interfaccia **Componente** che *descrive le operazioni comuni agli elementi* semplici e complessi *dell'albero*.
- La **Foglia** è un elemento di base di un albero che non ha sotto-elementi. Di solito, *fanno la maggior parte del lavoro*, poiché non hanno nessuno a cui delegare il lavoro.
- Il **Contenitore** (*Composite*) è un elemento intermedio che *contiene sotto-elementi*: delle foglie o altri contenitori. Un contenitore non conosce le classi concrete dei suoi figli. Lavora con tutti i sotto-elementi solo attraverso l'interfaccia del componente. 
	- Quando riceve una richiesta, un contenitore delega il lavoro ai suoi sotto-elementi, elabora i risultati intermedi e poi restituisce il risultato finale al cliente.
- Il **Client** lavora con tutti gli elementi *attraverso l'interfaccia del componente radice*. Di conseguenza, il client può lavorare allo stesso modo con elementi semplici o complessi dell'albero.

## Facade Pattern
>Il **Facade** è un pattern strutturale che fornisce una *interfaccia unificata e semplificata* per accedere un *insieme di classi* di un sottosistema.

![[Pasted image 20240119172106.png]]

Struttura:
- La **Facade** fornisce un *accesso semplificato alle funzionalità del sottosistema*. Sa dove indirizzare la richiesta del cliente e come far funzionare tutte le parti mobili.
- È possibile creare una classe **Facade aggiuntiva** per evitare di inquinare una singola facade *con caratteristiche non correlate* che potrebbero renderla un'altra struttura complessa. Le Facade aggiuntive possono essere utilizzate sia dai client che da altre Facade.
- Il **Sottosistema complesso** è composto da *un certo numero di oggetti diversi*. Per far sì che tutti facciano qualcosa di significativo, è necessario addentrarsi nei dettagli dell'implementazione del sottosistema, come inizializzare gli oggetti nell'ordine corretto e fornire loro i dati nel formato appropriato.
	- Le classi del sottosistema non sono consapevoli dell'esistenza della facciata. Esse operano all'interno del sistema e lavorano direttamente con gli altri.
- Il **Client** utilizza la Facade invece di chiamare direttamente gli oggetti del sottosistema.
- Permette di costruire [[019 System Design#^5952da|architetture chiuse]].

## Adapter Pattern
>L'**Adapter** è un pattern strutturale che permette agli *oggetti con interfacce incompatibili* di collaborare.

![[Pasted image 20240119173039.png]]

È molto spesso utilizzato per utilizzare componenti o sistemi legacy, che non possono essere modificati o personalizzati.

Un Adapter è un oggetto speciale che *converte l'interfaccia di un oggetto* in modo che un altro oggetto possa comprenderla. Un adattatore *avvolge* uno degli oggetti per nascondere la complessità della conversione che avviene dietro le quinte. L'oggetto avvolto non è nemmeno a conoscenza dell'adattatore.

Struttura:
 - Il **Client** è una classe che vuole utilizzare un sistema che *non riesce a comprendere*.   
 - Il **Servizio** (*Adaptee*) è una classe di solito di *terze parti* o *legacy*. Il client non può usare direttamente questa classe perché ha un'interfaccia incompatibile.
 - L'interfaccia **Target** del client descrive un *protocollo* che le altre classi devono seguire per poter collaborare con il client.
 - L'**Adattatore** è una classe *in grado di lavorare con il servizio*: implementa l'interfaccia del client e avvolge l'oggetto del servizio. L'adattatore riceve le chiamate dal client tramite l'interfaccia del client e le traduce in chiamate all'oggetto servizio avvolto in un formato comprensibile.

![[Pasted image 20240119173657.png|700]]

## Bridge Pattern
>Un **Bridge** è un pattern strutturale che permette la *separazione dell'astrazione dall'implementazione* di un sistema. In questo modo, entrambe possono variare indipendentemente.

In pratica, permette di cambiare sia la parte astratta di un oggetto, che la sua implementazione senza che ciascun cambiamento influisca direttamente sull'altro.


> [!example]- <font color="orange">Esempio</font>
>Si supponga di avere un telecomando e vari dispositivi come televisori e lettori DVD. Il Bridge Pattern consente di cambiare sia il tipo di telecomando che il tipo di dispositivo senza modificarli direttamente. Si può passare da un telecomando con pulsanti a uno touch, o da una TV a una proiettore, senza dover riscrivere tutto il codice. 
>In questo modo, la parte che gestisce i controlli (telecomando) e la parte che gestisce l'azione (dispositivo) sono separate e possono evolvere indipendentemente.
>
>---
>Supponiamo di avere una applicazione che deve girare su diversi sistemi operativi.
>In questo caso abbiamo come:
>- Astrazione: la GUI dell'applicazione
>- Implementazione: le API del sistema operativo
>
>![[Pasted image 20240318110539.png|400]]
>
>L'oggetto di astrazione controlla l'aspetto dell'applicazione, delegando il lavoro effettivo all'oggetto di implementazione collegato. Le diverse implementazioni sono intercambiabili, purché seguano un'interfaccia comune, consentendo alla stessa GUI di funzionare in qualsiasi OS.
>
>Di conseguenza, è possibile modificare le classi della GUI senza toccare le classi relative all'API. Inoltre, per aggiungere il supporto a un altro sistema operativo è sufficiente creare una sottoclasse nella gerarchia delle implementazioni.

![[Pasted image 20240119175409.png]]

> [!success] [Consiglio di leggere questo](https://refactoring.guru/design-patterns/bridge)

### Differenze con l'Adapter
Vengono usati per nascondere i dettagli dell'implementazione sottostante. Ma hanno alcune differenze:
- L'adapter è attrezzato per la creazione di componenti non correlate che lavorano insieme
- Un bridge viene usato prima del design per permettere di poter variare astrazioni e implementazioni indipendentemente

## Proxy Pattern
>Il **Proxy** è un pattern strutturale che consente di fornire un *oggetto fittizio (proxy) al posto di un oggetto vero*. Un proxy controlla l'accesso all'oggetto originale, consentendo di eseguire qualcosa prima o dopo che la richiesta arrivi all'oggetto originale.

![[Pasted image 20240119175942.png|650]]

Un esempio di proxy sono gli [[016 Oggetti Remoti|Oggetti Remoti in Java]], dove:
- Lo [[016 Oggetti Remoti#^stub|Stub]] è il proxy;
- Lo [[016 Oggetti Remoti#^skeleton|Skeleton]] è l'oggetto vero.

Struttura:
- Il **Service Interface** (*Subject*) dichiara l'interfaccia del servizio. Il proxy deve seguire questa interfaccia per potersi camuffare da oggetto di servizio.
- Il **Servizio** (*Real Subject*) è una classe che fornisce la [[Logica di business]].
- La classe **Proxy** ha un campo di riferimento che punta a un oggetto servizio. Dopo che ha finito l'esecuzione, passa la richiesta a quell'oggetto.
- Il **Client** dovrebbe lavorare sia con i servizi che con i proxy attraverso la stessa interfaccia.

Possono essere usati per:
- Rappresentare oggetti in differenti spazi di memoria (*Proxy Remoti*)
- Rappresentare oggetti troppo costosi da creare o scaricare (*Proxy Virtuali*)
- Fornire accessi controllati a oggetti reali (*Proxy di Protezione*)

## Observer Pattern
>L'**Observer** è un pattern comportamentale che *attende il cambio di stato di un oggetto* e *notifica i suoi oggetti dipendenti* appena succede.

![[Pasted image 20240119180952.png]]

Funziona come il [[050 Tipi di destinazioni per Messaggi#Modello Publish-Subscribe|Pub-Sub nei messaggi]]. Dove gli Observer possono essere delle interfacce.

## Command Pattern
>Il **Command** è un pattern comportamentale che *trasforma una richiesta in un oggetto autonomo* contenente tutte le informazioni sulla richiesta. 

![[Pasted image 20240318113518.png|520]]

Questa trasformazione consente di passare le richieste come *argomenti di un metodo*, di ritardare o mettere in coda l'esecuzione di una richiesta e di supportare operazioni non annullabili.

Incapsula tutte le informazioni rilevanti utilizzate per avviare una azione o un evento.

- La classe **Sender** (*Invoker*) è responsabile dell'avvio delle richieste. Questa classe deve avere un campo per memorizzare un riferimento a un oggetto comando. Il mittente attiva tale comando, invece di inviare la richiesta direttamente al destinatario. Si noti che il mittente non è responsabile della creazione dell'oggetto comando.
- L'interfaccia **Command** di solito dichiara un solo metodo per eseguire il comando.
- I **Comandi Concreti** implementano vari tipi di richieste. Un comando concreto non dovrebbe eseguire il lavoro da solo, ma piuttosto passare la chiamata a uno degli oggetti della logica di business.
- La classe **Ricevitore** contiene una logica di business. Quasi ogni oggetto può fungere da ricevitore. La maggior parte dei comandi gestisce solo i dettagli di come una richiesta viene passata al ricevitore, mentre il ricevitore stesso svolge il lavoro effettivo.
- Il **Client** crea e configura oggetti di comando concreti. Il client deve passare tutti i parametri della richiesta, compresa l'istanza del ricevitore, nel costruttore del comando. Dopodiché, il comando risultante può essere associato a uno o più mittenti.

> [!example]- <font color="orange">Esempio</font>
>```Java
>// Receiver
>public class LightSwitcher {
>	public LightSwitcher() {}
>
>	public void switchLights(Light light) {
>		light.setSwitched(!light.getSwitched());
>	}
>}
>
>// Interfaccia Command
>public interface Command {
>	void execute();
>}
>
>// Un Comando Concreto
>public class SwitchLightsCommand implements Command {
>	private Light light;
>	private LightSwitcher lightSwitcher = new LightSwitcher();
>
>	public SwitchLightsCommand(Light light) {
>		this.light = light;
>	}
>
>	@Override
>	public void execute() {
>		lightSwitcher.switchLights(light);
>	}
>}
>
>// Un Sender (Invoker)
>public class Room {
>	Command command;
>
>	public Room() {}
>
>	public void setCommand(Command command) {
>		this.command = command;
>	}
>
>	public void executeCommand() {
>		command.execute();
>	}
>}
>
>// Client
>public static void main(String[] args){
>	Room livingRoom = new Room();
>	livingRoom.setCommand(
>		new SwitchLightsCommand(new Light())
>	);
>	livingRoom.executeCommand();
>}
>```


## Strategy Pattern
>La **Strategy** è un pattern comportamentale che consente di *definire una famiglia di algoritmi*, inserire ciascuno di essi in una classe separata e rendere i loro oggetti intercambiabili.

![[Pasted image 20240119181948.png|600]]

Questo permette di non far supportare tutti gli algoritmi se non ne abbiamo bisogno. Inoltre, nel caso avessimo bisogno di un nuovo algoritmo, lo possiamo aggiungere facilmente senza disturbare l'applicazione che utilizza l'algoritmo.

## Abstract Factory
>L'**Abstract Factory** è un pattern di creazione che consente di *produrre famiglie di oggetti correlati senza specificare le loro classi concrete*.

![[Pasted image 20240318120101.png|600]]

Si concentra su una famiglia di prodotti, dove i prodotti possono essere *semplici o complessi* e vengono immediatamente restituiti.
La fabbrica astratta *non nasconde il processo di creazione*.

## Builder
>Il **Builder** è un pattern di creazione che *consente di costruire oggetti complessi passo dopo passo*. Il pattern consente di produrre diversi tipi e rappresentazioni di un oggetto utilizzando lo stesso codice di costruzione.

I modelli di creazione nascondono all'utente il complesso processo di creazione. Viene restituito dopo la creazione come passaggio finale.

La costruzione del prodotto complesso cambia di volta in volta.

> [!example]- <font color="orange">Esempio</font>
>Si può creare un enorme costruttore direttamente nella classe base `House`, con tutti i possibili parametri che controllano l'oggetto casa. Questo però crea un problema.
>
>![[Pasted image 20240318120524.png]]
>
>Nella maggior parte dei casi, la maggior parte dei parametri *saranno inutilizzati*, rendendo le chiamate al costruttore piuttosto brutte. Ad esempio, solo una parte delle case ha una piscina, quindi i parametri relativi alle piscine saranno inutili nove volte su dieci.
>
>Il design pattern Builder suggerisce di estrarre il codice di costruzione dell'oggetto dalla sua classe e di spostarlo in oggetti separati, chiamati builder.
>
>![[Pasted image 20240318120757.png]]
>
>```Java
>House house = HouseBuilder
>				.buildWindows(4)
>				.buildDoors(2)
>				.buildRooms(4)
>				.buildGarage()
>				.getResult()
>
>// Equivalente a: new House(4, 2, 4, true, null, null, null, ...)
>// Nel builder, l'ordine non è importante, tranne per il getResult() che deve essere alla fine
>```

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
- https://refactoring.guru/design-patterns/catalog