---
aliases: 
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Socket
cssclasses:
  - 
---
La comunicazione tra client e server avviene attraverso la **scrittura e lettura di stream** associati con il [[014 Socket TCP|Socket]] e che permettono una facile interazione (gestita dal linguaggio) per poter trasmettere [[Oggetti]] tra client e server attraverso un meccanismo di [[025 Serializzazione|Serializzazione]].

>Gli *stream* utilizzati sono *`InputStream`* e *`OutputStream`*, da cui possiamo derivare le *importanti* sottoclassi **`ObjectInputStream`** che fornisce il meccanismo di **deserializzazione** quando si riceve un oggetto precedentemente **serializzato** con **`ObjectOutputStream`**. Gli oggetti che possono essere trasmessi su questo tipo di stream devono implementare l'interfaccia `Serializable`.


### Socket
Per creare un `Socket` bisogna specificare la coppia ([[Indirizzo IP]], [[032 Porte|Porta]]). Si può usare usare "*localhost*" come indirizzo IP per indicare che si usa il sistema locale.

Invece, per creare `ObjectInputStream` o `ObjectOutputStream` abbiamo bisogno di ottenere l'`InputStream` o l'`OutputStream` dal `Socket`. Per deserializzare (leggere) e restituire un oggetto (è necessario il cast) dallo stream si usa il metodo **`readObject()`**, il processo è analogo per la serializzazione (scrittura) con **`writeObject()`**.

Si usa **`close()`** su un oggetto `Socket` per chiudere la connessione.

```Java
Socket socket = new Socket("localhost", 9000);

ObjectInputStream in = new ObjectInputStream(socket.getInputStream());     // Deserializza (legge) gli oggetti
ObjectOutputStream out = new ObjectOutputStream(socket.getOutputStream()); // Serializza (scrive) gli oggetti

out.writeObject("Giovanni"); // Scrive sullo stream
```


### ServerSocket
Un `ServerSocket` si costruisce specificando una porta.
```java
ServerSocket servSock = new ServerSocket(1000);
Socket clientSock = servSock.accept();
```

I metodi principali sono:
- **`accept()`**, aspetta connessioni sul `ServerSocket` e le accetta, restituendo un `Socket` dedicato alla comunicazione. Il metodo è bloccante fino a quando non viene fatta una connessione;
- **`close()`**, chiude il `ServerSocket`.

---
> [!example]- <font color="orange">Esempio: HelloSocket</font>
>Nota che i due programmi devono essere eseguiti allo stesso tempo, prima il server e poi il client.
>
>**SERVER**: 
>```Java
>import java.io.EOFException;  
>import java.io.ObjectInputStream;  
>import java.io.ObjectOutputStream;  
>import java.net.ServerSocket;  
>import java.net.Socket;  
>import java.util.logging.Logger;
>
>public class Server {  
>    static Logger logger = Logger.getLogger("global");  
>  
>    public static void main(String[] args) {  
>       try {  
>          ServerSocket serverSocket = new ServerSocket(9000);  
>          logger.info("Socket istanziato, accetto connessioni...");  
>            
>          // Attende fino a quando non si connette un client  
>          Socket socket = serverSocket.accept();  
>  
>          // Un client si è connesso alla porta 9000  
>          // Viene indirizzato su `socket`          
>          logger.info("Accettata una connessione... attendo comandi.");  
>            
>          ObjectOutputStream outputStream = new ObjectOutputStream(socket.getOutputStream()); // Deserializza (legge) gli oggetti  
>          ObjectInputStream inputStream = new ObjectInputStream(socket.getInputStream());     // Serializza (scrive) gli oggetti  
>  
>          // Legge cosa ha ricevuto dallo stream          
>          String nome = (String) inputStream.readObject();  
>          logger.info("Ricevuto: " + nome);  
>  
>          // Scrive una risposta sullo stream  
>          // Verrà letta dal client          
>          outputStream.writeObject("Hello " + nome);  
>  
>          socket.close();  
>       } catch (EOFException e) {  
>          logger.severe("Problemi con la connessione\n\t==> " + e.getMessage());  
>       } catch (Throwable e) {  
>          logger.severe("Errore\n\t==> " + e.getMessage());  
>       }  
>    }  
>}
>```
>
>**CLIENT**:
>```Java
>import java.io.EOFException;  
>import java.io.ObjectInputStream;  
>import java.io.ObjectOutputStream;  
>import java.net.Socket;  
>import java.util.logging.Logger;
>
>public class Client {  
>    static Logger logger = Logger.getLogger("global");  
>  
>    public static void main(String[] args) {  
>       try {  
>          Socket socket = new Socket("localhost", 9000); // Si connette alla porta 9000 del localhost
>  
>          ObjectInputStream in = new ObjectInputStream(socket.getInputStream());     // Deserializza (legge) gli oggetti  
>          ObjectOutputStream out = new ObjectOutputStream(socket.getOutputStream()); // Serializza (scrive) gli oggetti  
>  
>          out.writeObject("Giovanni"); // Scrive "Giovanni" sullo stream  
>  
>          // Il Server ha ricevuto "Giovanni" e ha scritto "Hello Giovanni" sullo stream
>          // Legge "Hello Giovanni" dallo stream 
>          System.out.println(in.readObject());  
>  
>          socket.close();  
>       } catch (EOFException e) {  
>          logger.severe("Problemi con la connessione\n\t==> " + e.getMessage());  
>       } catch (Throwable e) {  
>          logger.severe("Errore\n\t==> " + e.getMessage());  
>       }  
>    }  
>}
>```

> [!example]- <font color="orange">Esempio: Registro con client shell e server</font>
>Nota che i due programmi devono essere eseguiti allo stesso tempo, prima il server e poi il client.
>
>**RECORD REGISTRO**:
>```Java
>import java.io.Serial;  
>import java.io.Serializable;  
>  
>public class RecordRegistro implements Serializable {  
>    private String nome;  
>    private String indirizzo;  
>  
>    @Serial  
>    private static final long serialVersionUID = -2844154862186918410L;  // Generato casualmente
>  
>    public RecordRegistro(String nome, String indirizzo) {  
>       this.nome = nome;  
>       this.indirizzo = indirizzo;  
>    }  
>    public String getNome() {  
>       return nome;  
>    }  
>    public String getIndirizzo() {  
>       return indirizzo;  
>    }  
>}
>```
> **SERVER REGISTRO**:
>```Java
>import java.io.EOFException;  
>import java.io.IOException;  
>import java.io.ObjectInputStream;  
>import java.io.ObjectOutputStream;  
>import java.net.ServerSocket;  
>import java.net.Socket;  
>import java.util.HashMap;  
>import java.util.logging.Logger;  
>  
>public class ServerRegistro {  
>    static Logger logger = Logger.getLogger("global");  
>  
>    public static void main(String[] args) {  
>       // Crea una HashMap dove la chiave è il Nome  
>       HashMap<String, RecordRegistro> hash = new HashMap<String, RecordRegistro>();  
>  
>       Socket socket = null;  
>       logger.info("In attesa...");  
>  
>       try {  
>          ServerSocket serverSocket = new ServerSocket(7000);  
>  
>          while (true) {  
>             // Attende collegamenti  
>             socket = serverSocket.accept();  
>  
>             // Riceve un collegamento  
>             ObjectInputStream inStream = new ObjectInputStream(socket.getInputStream());  
>  
>             // Legge cosa ha ricevuto  
>             RecordRegistro record = (RecordRegistro) inStream.readObject();  
>  
>             // Se viene inviato un nome E un indirizzo allora è un comando di inserimento  
>             // Altrimenti è un comando di ricerca             
>             if (record.getIndirizzo() != null) {  
>                // Comando di inserimento  
>                // Inserisci nell'HashMap il nome e il record                
>                hash.put(record.getNome(), record);  
>                logger.info("Inserito: " + record.getNome() + ", " + record.getIndirizzo());  
>             } else {  
>                // Comando di ricerca  
>                // Cerca il registro in base al nome (null se non viene trovato)                
>                RecordRegistro res = hash.get(record.getNome());  
>                ObjectOutputStream outStream = new ObjectOutputStream(socket.getOutputStream());  
>  
>                logger.info("Esito ricerca per " + record.getNome() + ": " + res);  
>  
>                // Scrive sullo stream  
>                outStream.writeObject(res);  
>                outStream.flush();  
>             }  
>             socket.close();  
>          }  
>       } catch (EOFException e) {  
>          logger.severe("Problemi con la connessione\n\t==> " + e.getMessage());  
>       } catch (Throwable e) {  
>          logger.severe("Errore\n\t==> " + e.getMessage());  
>       } finally {  
>          // Chiusura del socket  
>          try {  
>             if(socket != null)  
>                socket.close();  
>          } catch (IOException e) {  
>             logger.severe("Errore\n\t==> " + e.getMessage());  
>             System.exit(0);  
>          }  
>       }  
>    }  
>}
>```
>**CLIENT SHELL**:
>```Java
>import java.io.*;  
>import java.net.Socket;  
>import java.util.logging.Logger;  
>  
>public class ClientShell {  
>    static Logger logger = Logger.getLogger("global");  
>  
>    static final String ERRORMSG = "Comando non trovato.\n\t==> Comandi disponibili: quit, inserisci, cerca.";  
>    static BufferedReader in = null;  
>  
>    /**  
>     * Chiede all'utente di inserire qualcosa
>     * @param prompt cosa chiedere all'utente  
>     * @return quello che ha scritto l'utente  
>     */    
>     private static String ask(String prompt) throws IOException {  
>       System.out.print(prompt + " ");  
>       return (in.readLine());  
>    }  
>  
>    public static void main(String[] args) {  
>       String host = "localhost";  
>       String cmd; // Comando ricevuto  
>       in = new BufferedReader(new InputStreamReader(System.in));  
>  
>       try {  
>          // Fino a quando non si riceve "quit"  
>          while (!(cmd = ask(">>>")).equals("quit")) {  
>             if (cmd.equals("inserisci")) {  
>                // Crea un record  
>                System.out.println("Inserire i dati.");  
>                String nome = ask("Nome: ");  
>                String indirizzo = ask("Indirizzo: ");  
>  
>                RecordRegistro r = new RecordRegistro(nome, indirizzo);  
>  
>                // Manda il record al server  
>                Socket socket = new Socket(host, 7000);  
>                ObjectOutputStream sock_out = new ObjectOutputStream(socket.getOutputStream());  
>                sock_out.writeObject(r);  
>                sock_out.flush();  
>  
>                System.out.println("Ho inserito: " + r.getNome());  
>  
>                socket.close();  
>             } else if (cmd.equals("cerca")) {  
>                // Ricerca un record in base al nome  
>                System.out.println("Inserire il nome per la ricerca.");  
>                String nome = ask("Nome: ");  
>  
>                RecordRegistro r = new RecordRegistro(nome, null);  
>  
>                // Manda il record (impostato per la ricerca) al server  
>                Socket socket = new Socket(host, 7000);  
>                ObjectOutputStream sock_out = new ObjectOutputStream(socket.getOutputStream());  
>                sock_out.writeObject(r);  
>                sock_out.flush();  
>  
>                // Ottieni la risposta dal server  
>                ObjectInputStream sock_in = new ObjectInputStream(socket.getInputStream());  
>                RecordRegistro result = (RecordRegistro) sock_in.readObject();  
>  
>                // Stampa il risultato  
>                if (result != null) {  
>                   System.out.println("Indirizzo: " + result.getIndirizzo());  
>                } else {  
>                   System.out.println("Record assente");  
>                }  
>  
>                socket.close();  
>             } else {  
>                System.out.println(ERRORMSG);  
>             }  
>          }  
>       } catch (Throwable e) {  
>          logger.severe("Errore\n\t==> " + e.getMessage());  
>       }  
>  
>       System.out.println("Bye bye");  
>    }   
>}
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