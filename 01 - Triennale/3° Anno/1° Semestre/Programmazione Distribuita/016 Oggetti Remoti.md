---
aliases:
  - Stub
  - Skeleton
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Java RMI
cssclasses:
  - 
---
> Un **Oggetto Remoto** è un [[Oggetti|Oggetto]] i cui metodi possono essere acceduti *da un altro spazio di [[Indirizzo Fisico|indirizzamento]]* e potenzialmente da un'altra macchina. Ovvero, è un oggetto che si trova *sul server* a cui altri nodi possono accedervi attraverso una rete.

Il loro scopo è quello di poter utilizzare oggetti (attraverso i [[Metodi]]) su nodi differenti come se fossero tutti sulla stessa macchina in locale. Questo involve l'utilizzo della [[003 Trasparenza di un Sistema Distribuito#Trasparenza di Locazione|Trasparenza di Locazione]].

Viene adottato uno strato di software che viene utilizzato per nascondere al programmatore la maggior parte del lavoro necessario per permettere l'*invocazione remota di metodi*. Questo strato di software consiste di due componenti: lo *stub* e lo *skeleton*.

> Lo **Stub** è un oggetto che *si trova sul client* e che *rappresenta* l'oggetto che si trova sul server. Presenta ed espone gli stessi metodi che vengono esposti sul server. ^stub

> Lo **Skeleton** è l'oggetto che *si trova sul server* che comunica con lo stub. Ogni chiamata del client ai metodi remoti dello stub inizierà la comunicazione con lo skeleton, il quale *effettua l'invocazione del metodo* richiesto sull'oggetto del server e *restituisce il risultato* allo stub. ^skeleton

![[Pasted image 20231011172143.png]]

Lo stub conosce i metodi che sono disponibili sull'oggetto server attraverso una [[019 Processo di Creazione di programmi Java RMI#^ffd46f|Interfaccia Remota]].

In generale tutti i metodi (anche i getter) possono lanciare [[023 Gestione delle Eccezioni|Eccezioni]].

> [!example]- <font color="orange">Esempio sull'utilizzo con socket</font>
> Lancia prima `ImpiegatoSkeleton` e poi `Client`.
> 
>```Java
>public interface Impiegato {  
>    public String getNome() throws Throwable;  
>    public String getID() throws Throwable;  
>    public int getStipendio() throws Throwable;  
>    public int aumentaStipendio(int diQuanto) throws Throwable;  
>}
>```
>
>```Java
>/** Questo è il modello dell'oggetto che si trova sul server */
>public class ImpiegatoServer implements Impiegato {  
>    private String nome;  
>    private String ID;  
>    private int stipendio;  
>  
>    public ImpiegatoServer(String nome, String ID, int stipendio) {  
>       this.nome = nome;  
>       this.ID = ID;  
>       this.stipendio = stipendio;  
>    }  
>  
>    public String getNome() throws Throwable {  
>       return nome;  
>    }  
>  
>    public String getID() throws Throwable {  
>       return ID;  
>    }  
>  
>    public int getStipendio() throws Throwable {  
>       return stipendio;  
>    }  
>  
>    public int aumentaStipendio(int diQuanto) throws Throwable {  
>       if (diQuanto > 0)  
>          stipendio += diQuanto;  
>       return stipendio;  
>    }  
>}
>```
>
>```Java
>import java.io.IOException;  
>import java.io.ObjectInputStream;  
>import java.io.ObjectOutputStream;  
>import java.net.Socket;  
>  
>public class ImpiegatoStub implements Impiegato {  
>    Socket socket;  
>    ObjectInputStream in;  
>    ObjectOutputStream out;  
>  
>    public ImpiegatoStub(String host) throws Throwable {  
>       socket = new Socket(host, 9000);  
>       out = new ObjectOutputStream(socket.getOutputStream());  
>       in = new ObjectInputStream(socket.getInputStream());  
>    }  
>  
>    // Metodo locale per la chiusura del socket con lo skeleton  
>    public void close() {  
>       try {  
>          socket.close();  
>       } catch (IOException e) {  
>          System.out.println("Errore con la chiusura del socket");  
>       }  
>    }  
>  
>    public String getNome() throws Throwable {  
>       out.writeObject("getNome");  
>       out.flush();  
>       return (String) in.readObject();  
>    }  
>  
>    public String getID() throws Throwable {  
>       out.writeObject("getID");  
>       out.flush();  
>       return (String) in.readObject();  
>    }  
>  
>    public int getStipendio() throws Throwable {  
>       out.writeObject("getStipendio");  
>       out.flush();  
>       return in.readInt();  
>    }  
>  
>    public int aumentaStipendio(int diQuanto) throws Throwable {  
>       out.writeObject("aumentaStipendio");  
>       out.writeInt(diQuanto);  
>       out.flush();  
>       return in.readInt();  
>    }  
>}
>```
>
>```Java
>import java.io.EOFException;  
>import java.io.IOException;  
>import java.io.ObjectInputStream;  
>import java.io.ObjectOutputStream;  
>import java.net.ServerSocket;  
>import java.net.Socket;  
>  
>public class ImpiegatoSkeleton extends Thread {  
>    ImpiegatoServer impiegatoServer;  
>  
>    public ImpiegatoSkeleton(ImpiegatoServer server) {  
>       this.impiegatoServer = server;  
>    }  
>  
>    public static void main(String[] args) {  
>       ImpiegatoServer impiegato = new ImpiegatoServer("Mario Rossi", "01923", 30000);  
>       ImpiegatoSkeleton skeleton = new ImpiegatoSkeleton(impiegato);  
>       skeleton.start();  
>    }  
>  
>    public void run() {  
>       Socket socket = null;  
>       String metodo;  
>       int parametro;  
>       System.out.println("Attendo connessioni...");  
>  
>       try {  
>          ServerSocket serverSocket = new ServerSocket(9000);  
>          socket = serverSocket.accept();  
>  
>          System.out.println("Accettata una connessione... attendo comandi.");  
>          ObjectInputStream inputStream = new ObjectInputStream(socket.getInputStream());  
>          ObjectOutputStream outputStream = new ObjectOutputStream(socket.getOutputStream());  
>  
>          while (true) {  
>             metodo = (String) inputStream.readObject();  
>             System.out.println("Comando richiesto: " + metodo);  
>  
>             if (metodo.equals("getNome")) {  
>                outputStream.writeObject(impiegatoServer.getNome());  
>                outputStream.flush();  
>             } else if (metodo.equals("getID")) {  
>                outputStream.writeObject(impiegatoServer.getID());  
>                outputStream.flush();  
>             } else if (metodo.equals("getStipendio")) {  
>                outputStream.writeInt(impiegatoServer.getStipendio());  
>                outputStream.flush();  
>             } else if (metodo.equals("aumentaStipendio")) {  
>                parametro = inputStream.readInt();  
>                outputStream.writeInt(impiegatoServer.aumentaStipendio(parametro));  
>                outputStream.flush();  
>             } else break;  
>          }  
>       } catch (EOFException e) {  
>          System.out.println("Terminata la connessione");  
>       } catch (Throwable t) {  
>          System.out.println("Errore: " + t);  
>       } finally {  
>          try {  
>             if (socket != null)  
>                socket.close();  
>          } catch (IOException e) {  
>             System.out.println("Errore: " + e);  
>             System.exit(0);  
>          }  
>       }  
>    }  
>}
>```
>
>```Java
>import java.util.logging.Logger;  
>  
>public class Client {  
>    static Logger logger = Logger.getLogger("global");  
>  
>    public static void main(String[] args) {  
>       try {  
>          ImpiegatoStub impiegato = new ImpiegatoStub("localhost");  
>          System.out.println("Nome: " + impiegato.getNome());  
>          System.out.println("ID: " + impiegato.getID());  
>          System.out.println("Stipendio: " + impiegato.getStipendio());  
>  
>          System.out.println("Aumentiamo lo stipendio di 1000");  
>  
>          System.out.println("Ora lo stipendio è di: " + impiegato.aumentaStipendio(1000));  
>          impiegato.close();  
>  
>       } catch (Throwable t) {  
>          logger.severe("Errore: " + t.getMessage());  
>       }  
>    }  
>}
>```

Si ha una generazione automatica di stub e skeleton attraverso [[017 Java RMI|Java RMI]].

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
- [Wikipedia](https://en.wikipedia.org/wiki/Distributed_object)