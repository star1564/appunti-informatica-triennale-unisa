---
aliases:
  - JDBC
cssclasses:
  - 
tags:
  - corsi/informatica/basi_di_dati
paragrafo: SQL
---


> [!tip]- File Finale Template
>```Java
>import java.sql.Statement;
>import java.sql.Connection;
>import java.sql.DriverManager;
>import java.sql.ResultSet;
>import java.sql.SQLException;
>
>public class Demo {
>	public static void main(String[] args) {
>		String nomeDB = "azienda_ceramica";
>		String username = "root";
>		String password = "password";
>		String url = "jdbc:mysql://localhost:3306/" + nomeDB;
>		
>		// VERIFICA SE LA LIBRERIA DRIVER SI TROVA NEL PROGETTO
>		try {
>			Class.forName("com.mysql.cj.jdbc.Driver");
>		} catch (ClassNotFoundException e) {
>			System.out.println("DB driver non trovato");
>			System.out.println(e);	
>		} 
>		
>		// INIZIA LA CONNESSIONE
>		Connection conn = null;
>		
>		try {
>			conn = DriverManager.getConnection(url, username, password);
>		} catch (SQLException e) {
>			System.out.println("Connessione Fallita");
>			System.out.println(e);
>		}
>		
>		// CODICE
>		Statement st = null;
>		ResultSet rs = null;
>		String sql; 
>		
>		
>		// sql = "query";
>		
>		try {
>			st = conn.createStatement();
>			rs = st.executeQuery(sql);
>			
>			
>			
>
>		} catch (SQLException e1) {
>			System.out.println("Errore Query");
>			System.out.println(e1);
>		}
>		
>		
>		
>		
>		// CHIUSURA CONNESSIONE
>		try {
>			rs.close();
>			st.close();
>			connection.close();
>			
>		} catch (SQLException e) {
>			System.out.println("Chiusura DB Fallita");
>			System.out.println(e);
>		}
>	}
>}
>```

**Java Data Base Connector** serve per utilizzare all'interno di un programma [[002 Java|Java]] un database MySql.

> [!info]- Link per installare MySql e JDBC
>1. [Video per installare MySql](https://www.youtube.com/watch?v=u96rVINbAUI)
>Nota che si può già installare il connettore dal programma di installazione di MySql (Available Products → MySQL Connectors → JDBC). In questo caso il jar si trova al seguente path `C:\Program Files (x86)\MySQL\Connector J X.X\mysql-connector-j-X.X.XX.jar`
>
>2. [Editor per MySql consigliato (beekeeper studio CE)](https://github.com/beekeeper-studio/beekeeper-studio/releases)
>
>3. [Video per installare ed utilizzare JDBC nei progetti java](https://www.youtube.com/watch?v=sBQ3JL9nu5U)
>4. [Link per il download del connettore (scegliere "Platform Independent" per ottenere solo il .jar), salta se si ha già scaricato dal passo 1](https://www.mysql.com/products/connector/)

> [!note]- Nota per i DB su linux
> Da mia esperienza è meglio mettere il nome preciso delle tabelle e database, perché potrebbe essere case sensitive.
> Per esempio, se si ha la tabella `dipendente`, non scrivere `DIPENDENTE` o `Dipendente`.

---
### CONNETTERSI AD UN DATABASE
#### AGGIUNGERE IL CONNETTORE
Prima di tutto bisogna **aggiungere il connettore** al progetto java: 
1. Tasto desto sul nome del progetto java;
2. `New` > `Folder`;
3. Chiamare la cartella "lib";
4. Inserire il jar nella cartella "lib";
5. Tasto desto sul nome del progetto java;
6. `Proprieties`;
7. Andare su `Libraries`, evidenziare `Classpath` e fare `Add JARs`; ![[Pasted image 20230121142151.png]]
8. Cercare il connettore e selezionarlo; ![[Pasted image 20230121142318.png]]
9. Applicare e chiudere le proprietà.

#### CODICE CONNESSIONE
Per connettere un programma java ad un database si usa la classe **`Connection`** in questo modo:
```Java
public class Demo {
	public static void main(String[] args) {
		String nomeDB = "nomeDB";        // Nome del database
		String username = "root";        // Username per accedere
		String password = "password";    // Password per accedere (lasciare "" se vuota)
		
		String url = "jdbc:mysql://localhost:3306/" + nomeDB;   // Connessione al DB locale
		
		// VERIFICA SE LA LIBRERIA DRIVER SI TROVA NEL PROGETTO
		try {
			Class.forName("com.mysql.cj.jdbc.Driver");
		} catch (ClassNotFoundException e) {
			System.out.println("DB driver non trovato");
			System.out.println(e);	
		} 
		
		// INIZIA LA CONNESSIONE
		Connection connection = null;
		try {
			connection = DriverManager.getConnection(url, username, password);
		} catch (SQLException e) {
			System.out.println("Connessione Fallita");
			System.out.println(e);
		}
		
		// CODICE
		
		System.out.println("Connessione OK");
		
		
		// CHIUSURA CONNESSIONE
		try {
			connection.close();
		} catch (SQLException e) {
			System.out.println("Chiusura DB Fallita");
			System.out.println(e);
		}
	}
}

```

---
### UTILIZZARE LE QUERY
Per poter utilizzare le query bisogna creare degli oggetti con le seguenti classi:
- `Statement`: permette di costruire le query;
- `ResultSet`: è il risultato della query;
- `String sql`: è la stringa che contiene la vera e propria query (in linguaggio sql).

Implementiamo le interazioni con il database nella sezione `CODICE` descritta prima (dove si trova "Connessione OK").

> [!warning] Attenzione agli import!
> Tutti gli import riguardanti l'sql devono cominciare con **`java.sql.`**. 
> Un esempio sbagliato è `java.beans.Statement`, uno corretto è `java.sql.Statement`.


Diciamo di voler selezionare tutti gli elementi della seguente tabella (`numero_telefono`).
![[Pasted image 20230121144415.png]]
Lo faremo in questo modo:

```Java
// CODICE
Statement st;
ResultSet rs;
String sql; 

sql = "SELECT * FROM numero_telefono;";     // Query

try {
	st = connection.createStatement();  // Creazione query
	rs = st.executeQuery(sql);          // Ottenimento risultati query

	// Stama il risultato
	System.out.println("Numero Tel." + "\t" + "Codice Settore");    // Titoli tabella

	while (rs.next() == true) { 
		System.out.println(rs.getString("numero") + "\t" + rs.getString("codiceSettore")); 
	}
} catch (SQLException e1) {
	System.out.println("Errore Query");
	System.out.println(e1);
}

// CHIUSURA CONNESSIONE
try {
	rs.close(); // Si aggiunge
	st.close(); // Si aggiunge
	connection.close();
	
} catch (SQLException e) {
	System.out.println("Chiusura DB Fallita");
	System.out.println(e);
}
```

Risultato:
```markup
Numero Tel.    Codice Settore
03756939696	   1
03818447846	   2
03402436293	   3
03936998878	   3
03814716551	   4
```

Si usa `rs.next()` per passare alla prossima riga del risultato.

Per ottenere quello che si trova nel campo di una riga si usa `rs.getString(ColonnaQuery)`. Questo converte quello che trova in stringhe.
Se si vuole ottenere direttamente un tipo di dato esistono metodi specifici.

#### FORMARE QUERY PIÙ LUNGHE
Si usa la proprietà della concatenazione del `+` delle stringhe in java.

Bisogna ricordarsi di mettere uno *spazio* alla fine di ogni stringa (tranne l'ultima), altrimenti si avrà un errore della query!
```Java
sql = 	"SELECT * " + 
		"FROM numero_telefono " +
		"WHERE codiceSettore = 3;";
```

Risultato:
```markup
Numero Tel.    Codice Settore
03402436293	   3
03936998878	   3
```

#### QUERY DI AGGIORNAMENTO
Quando si devono utilizzare le query come la [[024 Comandi SQL#INSERIMENTO|INSERT]] si utilizza (al posto del result test) il seguente codice:

```Java
Statement st;
sql = "INSERT INTO dipendente "
	+ "VALUES ('" + nome + "', '" + cognome + "');";

try {
	st = connection.createStatement();  // Creazione query
	st.executeUpdate(sql);   // Esecuzione query
} catch (SQLException e) {
	System.out.println("Errore Query");
	System.out.println(e);
}
```

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
