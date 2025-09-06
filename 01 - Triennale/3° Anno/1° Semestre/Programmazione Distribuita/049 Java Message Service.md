---
aliases:
  - JMS
  - MOM
  - Oggetti Amministrati
  - Message-Driven Beans
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Java Message Service
cssclasses:
  - 
---
## Messaggi e MOM
>Quando si parla di **messaggistica**, si intende una *comunicazione [[Asincrono|asincrona]] e non vincolata tra componenti*. 

^f9c102

Il **[[005 Middleware|Middleware]] Orientato ai Messaggi** (*MOM*) è un software che consente lo scambio di messaggi asincroni tra sistemi eterogenei, che può essere visto come un [[Buffer]] tra i sistemi che producono e consumano messaggi.

Le terminologie sono le seguenti:
- Il componente *mittente* del messaggio è chiamato un **producer**; ^0dc7a0
- Il componente *destinatario* del messaggio è chiamato un **consumer**; ^1e8c7a
- Quando un messaggio viene inviato, il *software (MOM)* che ottiene e conserva il messaggio è chiamato un **provider** (o *broker*); 
	- La *posizione in cui il messaggio viene conservato* si chiama **destinazione**. ^447443

Qualsiasi componente interessato a un messaggio in quella particolare destinazione può consumarlo.

![[Pasted image 20231117112932.png|Architettura MOM|650]]

## JMS
>In Java EE, l'API che si occupa di questi concetti si chiama **Java Message Service** (*JMS*). Si tratta di un insieme di interfacce e classi che consentono di connettersi a un provider, *creare un messaggio*, *inviarlo* e *riceverlo*.

Quando viene eseguito in un [[021 Container Java EE|contenitore EJB]], i **Message-Driven Beans** (*MDB*) possono essere utilizzati per ricevere messaggi in modo container-managed.

L'architettura di messaggistica è composto da:
- Un *provider*: JMS *non trasporta fisicamente i messaggi*, è solo un'API. Infatti richiede un provider incaricato di gestire i messaggi;
- **Clients**: è qualsiasi applicazione o componente Java che *produce o consuma un messaggio* da/verso il provider (come i producer o consumer);
- **Messaggi**: gli oggetti che i client inviano o ricevono al/dal provider; ^240a9a
- **Oggetti Amministrati**: un broker di messaggi deve fornire gli oggetti amministrati al client (fabbriche di connessioni e destinazioni) attraverso le ricerche [[025 Risorse e JNDI|JNDI]] o l'[[@Inject|iniezione]].

![[Pasted image 20231117114145.png|500]]

### Oggetti Amministrati
>Gli **Oggetti Amministrati** in JMS sono utilizzati per *facilitare la configurazione e la gestione delle risorse di messaggistica* all'interno di un'applicazione. Questi oggetti forniscono un'astrazione dalla complessità della configurazione della *connessione ai sistemi di messaggistica*, consentendo agli sviluppatori di concentrarsi sullo sviluppo dell'applicazione senza dover gestire dettagli specifici dell'infrastruttura sottostante.

Il provider di messaggi consente di configurare questi oggetti e li rende disponibili nello spazio dei nomi [[025 Risorse e JNDI|JNDI]]. Come i data-source [[026 JDBC|JDBC]], gli oggetti amministrati vengono creati una sola volta. 

I due tipi di oggetti amministrati sono:
- *Fabbriche di connessioni*: Utilizzati dai client per creare una connessione a una destinazione.
- *Destinazioni*: Punti di distribuzione dei messaggi che ricevono, trattengono e distribuiscono i messaggi. Le destinazioni possono essere code ([[050 Tipi di destinazioni per Messaggi|P2P]]) o argomenti ([[050 Tipi di destinazioni per Messaggi|Pub-Sub]]).

### Message-Driven Beans
>I **Message-Driven Beans** (*MDB*) sono *consumatori di messaggi asincroni* che vengono eseguiti all'interno di un contenitore EJB. Gli MDB sono [[042 Tipi di Session Beans#^68ea9a|stateless]], quindi il contenitore EJB *ne può avere numerose istanze*, in esecuzione simultanea, per elaborare i messaggi provenienti da vari producer. 

Anche se sembrano bean stateless, *le applicazioni client non possono accedere direttamente agli MDB*. L'unico modo per comunicare con un MDB è *inviare un messaggio alla destinazione* che l'MDB sta ascoltando. In generale, gli MDB sono in ascolto di una destinazione ([[050 Tipi di destinazioni per Messaggi|coda o pub-sub]]) e, quando arriva un messaggio, **lo consumano e lo elaborano**. 

Possono anche *delegare la logica di business ad altri [[042 Tipi di Session Beans|Session Bean Stateless]]* in modo sicuro e [[046 Transazioni EJB|transazionale]]. Essendo stateless, gli MDB non mantengono lo stato attraverso invocazioni separate, da un messaggio ricevuto al successivo.

È annotato da ***`@MessageDriven(configs)`*** a cui vengono passati dei `@ActivationConfigProperty` che specificano i dettagli della destinazione e del suo tipo. Deve inoltre implementare il metodo **`onMessage(Message message)`** che si attiva appena viene ricevuto un messaggio.

```Java
import javax.ejb.ActivationConfigProperty;
import javax.ejb.MessageDriven;
import javax.jms.JMSException;
import javax.jms.Message;
import javax.jms.MessageListener;
import javax.jms.TextMessage;

@MessageDriven(activationConfig = {
	@ActivationConfigProperty(
			propertyName = "destinationLookup",
			propertyValue = "jms/javaee7/Topic"
	),
	@ActivationConfigProperty(
			propertyName = "destinationType",
			propertyValue = "javax.jms.Topic"
	)
})
public class MyMessageDrivenBean implements MessageListener {
	public MyMessageDrivenBean() {}

	@Override
	public void onMessage(Message message) {
		// Controlla la console di glassfish per l'output
		try {
			if (message instanceof TextMessage) {
				TextMessage textMessage = (TextMessage) message;
				String messageContent = textMessage.getText();
				System.out.println("Received message: " + messageContent);
			} else {
				System.out.println("Received non-text message");
			}
		} catch (JMSException e) {
			e.printStackTrace();
		}
	}
}
```


### Anatomia di un Messaggio
Un **Messaggio** (implementato da `javax.jms.Message`) è un oggetto che incapsula certe informazioni. È diviso in tre parti:
- Un *Header*: contiene le informazioni basilari per identificare ed inviare il messaggio;
- *Proprietà*: sono delle coppie nomi-valori che l'applicazione può impostare o leggere. Permettono anche il filtraggio dei messaggi attraverso il loro valore;
- Un *Body*: contiene il vero e proprio messaggio che può essere di vari tipi (testo, bytes, oggetti, ecc).


![[Pasted image 20231117124031.png|500]]
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