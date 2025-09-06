---
aliases: 
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Java RMI
cssclasses:
  - 
---
Il **processo di creazione** che porta alla realizzazione di un programma in Java RMI è spesso diviso in due sottoprocessi: uno che procede verso lo sviluppo ed esecuzione del *server*, l'altro che va in direzione dello sviluppo e della esecuzione del *client*. Alcuni passi sono dipendenti dai precedenti, mentre altri possono proseguire indipendentemente.

![[Pasted image 20231014100247.png|500]]

Gli Stub e Skeleton sono creati attraverso `rmic`, mentre il server e client sono compilati attraverso `javac`. Sono chiamate automaticamente al runtime dall'IDE.

## (1) Definizione della Interfaccia Remota e delle Eccezioni
> La descrizione dei *servizi offerti dal server* da parte di un [[016 Oggetti Remoti|oggetto remoto]] è contenuta all'interno di una **Interfaccia Remota**, ovvero una [[018 Interfacce|Interfaccia]] che dichiara i metodi remoti. L'implementazione sarà a carico del server e totalmente nascosta al client.
^ffd46f

Una **Invocazione di Metodi Remoti** rappresenta l'invocazione di un metodo su un oggetto remoto (specificato nella interfaccia remota) e ha la *stessa sintassi* di una invocazione di un metodo locale.

L'oggetto client di oggetti remoti server utilizza *esclusivamente* l'interfaccia remota dell oggetto, non la sua implementazione. Questo garantisce che le funzionalità remote risultino *astratte* verso il client e *disaccoppia le due implementazioni* (permettendo, ad esempio, l'evoluzione dei metodi dell'oggetto server senza modificare quello dell'oggetto client).

Prima di definire un oggetto remoto, si deve necessariamente definire una interfaccia remota per l'oggetto attraverso `java.rmi.Remote`. Inoltre, ogni metodo remoto deve dichiarare esplicitamente l'[[023 Gestione delle Eccezioni|eccezione]] `java.rmi.RemoteException`, utilizzata anche per segnalare al programmatore che si sta invocando un metodo remoto.

> [!example]- <font color="orange">Esempio: Interfaccia Remota</font>
>```Java
>public interface Hello extends java.rmi.Remote {
>	String saluta(String chi) throws java.rmi.RemoteException;
>}
>```

## (2) Implementazione del Server
Un oggetto remoto in Java deve essere una istanza di una classe che *derivi* da `java.rmi.UnicastRemoteObject` e che implementi una o più *interfacce remote*.

Il fatto che il server derivi da `UnicastRemoteObject` implica che il [[Costruttore di Oggetti|Costruttore]] del server **debba essere sempre scritto** (anche se *vuoto*).

In generale una IDE, come [Netbeans](https://netbeans.apache.org/download/nb15/), compila automaticamente la classe Server.

### Servizio di Naming
A questo punto è necessario che l'oggetto remoto possa essere *accessibile dai client* (all'interno di *tutto il sistema distribuito*) che ne vogliono invocare i servizi. 

> A tale scopo, Java RMI propone un **servizio di naming** chiamato **`rmiregistry`**. Permette di poter reperire il riferimento ad un oggetto remoto di cui *si sa solamente il nome* (ovvero un identificativo). Questo realizza parzialmente la [[003 Trasparenza di un Sistema Distribuito#Trasparenza di Locazione|Trasparenza di Locazione]].

Questo programma deve essere lanciato *prima* di eseguire l'oggetto server, in quanto il server (tipicamente) tra le prime operazioni farà in modo di *registrarsi* presso il servizio di naming con una etichetta. Il package **`java.rmi.Naming`** mette a disposizione diversi metodi per registrare un oggetto:
- **`lookup(String regURL)`**, ricerca dell'oggetto (se non viene trovato lancia una eccezione);
- `bind(...)` / `unbind(...)` / **`rebind(String name, Object obj)`**, registrazione dell'oggetto;
- **`list()`**, elenco degli oggetti registrati.

I client che vogliono accedere ai servizi di un oggetto remoto, fanno una *richiesta al registry* per ottenere un riferimento remoto da usare per le invocazioni remote. Il client accede al servizio attraverso una specifica che segue l'[[002 URL|URL]] (sotto forma di `"rmi://nomehost/nomeoggetto"`).

Il servizio di naming deve essere lanciato dalla directory dove si trova il file `.class` dello stub, dell'oggetto remoto e della interface remota.

Infine si deve registrare il server (l'oggetto remoto) nel sistema.

> [!tip]- Come iniziare il processo `rmiregistry`
>1. Andare nella directory dove si trovano i `.class` inerenti al server, al client ed all'interfaccia nel progetto;
>2. Aprire un terminale in questa directory;
>3. Copiare il path della jdk (`C:\Program Files\Java\jdk-1.8`) più il programma necessario (`\bin\rmiregistry`);
>4. Eseguire dal terminale aperto il programma `rmiregistry` incollando il path:
>	- Windows (cmd): `"C:\Program Files\Java\jdk1.8.0\bin\rmiregistry"`; 
>	- Linux/Mac: `⁓/.jdks/jdk1.8.0/bin/rmiregistry`.

> [!example]- <font color="orange">Esempio: Server</font>
>Per i nostri scopi definiamo un nuovo `SecurityManager` che garantisce una [[017 Java RMI#Sicurezza in Java|politica di sicurezza]] `AllPermission`.
>Dopo aver inizializzato il servizio di naming eseguire `HelloImpl`.
>```Java
>import java.rmi.*;
>import java.rmi.server.UnicastRemoteObject; 
>import java.security.Permission;
>import java.util.logging.Logger;
>
>public class HelloImpl extends UnicastRemoteObject implements Hello {
>    private static final long serialVersionUID = -4469091140865645865L; // Generato a caso
>    static Logger logger = Logger.getLogger("global");
>    
>    public HelloImpl() throws RemoteException {} // Costruttore vuoto
>
>	// Implementazione del metodo nell'interfaccia
>    public String saluta(String chi) throws RemoteException {
>        logger.info("Sto salutando " + chi);
>        return "Ciao " + chi + "!"; 
>    }
>    
>    public static void main(String[] args) throws RemoteException { 
>        // SecurityManager per AllPermission 
>        // Richiede JDK < 17!
>        System.setSecurityManager(new SecurityManager() {
>            @Override
>            public void checkPermission(Permission perm) {}
>            @Override
>            public void checkPermission(Permission perm, Object context) {}
>        });
>        
>        try {
>            logger.info("Creo l'oggetto remoto..."); 
>            HelloImpl obj = new HelloImpl(); 
>            
>            logger.info("... ne effettuo il rebind...");
>            // Inserisco nel servizio di naming l'oggetto con il nome "HelloServer"
>            Naming.rebind("HelloServer", obj); 
>	        
>            logger.info("... Pronto!");
>        } catch (Exception e) { 
>            logger.severe("Errore\n\t==> " + e.getMessage());
>        }
>    }
>}
>```
>

## (3) Implementazione del Client
L'implementazione del client non richiede molte modifiche rispetto ad una eventuale applicazione scritta in locale. 

Si deve essenzialmente risolvere il problema della *localizzazione dell'oggetto remoto*, effettuata tramite i servizi di **`java.rmi.Naming`** che permettono di ottenere un riferimento remoto ad un oggetto server. Questo accade attraverso il metodo **`lookup(String url)`**.

A questo punto, l'invocazione dei metodi remoti procede in maniera tradizionale, con la sola differenza che si deve gestire la eccezione `RemoteException` che viene lanciata da tutti i metodi remoti.

Anche qui l'IDE compila la classe Client.

> [!example]- <font color="orange">Esempio: Client</font>
>```Java
>import java.rmi.*;
>import java.util.logging.Logger;
>public class HelloClient {
>    static Logger logger = Logger.getLogger("global");
>    
>    public static void main(String[] args) { 
>        try {
>	        logger.info("Sto cercando l'oggetto remoto..."); 
>	        Hello obj = (Hello) Naming.lookup("rmi://localhost/HelloServer");  
>	        
>	        logger.info("... Trovato! Invoco metodo...");
>	        String risultato = obj.saluta("Pippo");
>	        
>	        System.out.println("Ricevuto:" + risultato); // Output: Ciao Pippo!
>        } catch (Exception e) { 
>	        logger.severe("Errore\n\t==> " + e.getMessage()); 
>        }
>
>    }
>}
>
>```



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