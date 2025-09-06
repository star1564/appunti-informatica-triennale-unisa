---
aliases: 
tags:
  - corsi/informatica/basi_di_dati
paragrafo: SQL
cssclasses:
  - 
---
> [!tip] Usare l'outline dei titoli di Obsidian per trovare un argomento


### SINTASSI SQL
Una istruzione (statement) dell'SQL utilizza varie parole chiave e finiscono sempre con un `;`.

Le parole chiave non sono case sensitive, ma per migliore leggibilità si scrivono in maiuscolo.
Ogni parola chiave usa una riga nel codice sorgente, sempre per migliorare la leggibilità, ma come detto prima le istruzioni terminano con il punto e virgola.

Per commentare una riga nel codice si usa `-- Commento`, altrimenti si usa il classico `/* Commento */` che funziona su più righe.

> [!example]+ <font color="orange">Esempio</font>
>```SQL
>-- Query per recuperare dipendenti per stipendio oltre 1800
>/* Commento1
>Commento2
>*/
>SELECT nome, stipendio
>FROM dipendenti
>WHERE stipendio > 1800;
>```

---
### DATABASE
**Crea un nuovo database** (vuoto) con il nome `nome_database`.
```SQL
CREATE DATABASE nome_database;
```

**Elimina il database** con il nome `nome_database`.
```SQL
DROP DATABASE nome_database;
```

---
### TABELLE
**Crea una tabella** con il nome `nome_tabella` e gli si vanno a inserire un qualsiasi numero di attributi, specificando il loro tipo ed i vincoli.
```SQL
CREATE TABLE nome_tabella(
	nome_attributo1 tipo_dato vincolo,
	nome_attributo2 tipo_dato vincolo,
	nome_attributo3 tipo_dato vincolo,
	...
	nome_attributoN tipo_dato vincolo
);
```

> [!example]- <font color="orange">Esempio Creazione di una Tabella</font>
>```SQL
>CREATE TABLE IF NOT EXISTS dipendenti(
>	id int,
>	nome varchar(25),
>	cognome varchar(25),
>	data_assunzione date,
>	stipendio decimal
>);
>```
>Si usa il `IF NOT EXISTS` per creare la tabella se non esiste. Se esiste già viene aggiornata.
>
>| id (int) | nome (varchar(25)) | data_assunzione (date) | stipendio (decimal) |
>| -------- | ------------------ | ---------------------- | ------------------- |

**Elimina una tabella** dandogli il nome.
```SQL
DROP TABLE nome_tabella;
```

---
### VINCOLI (COSTRAINT)
#### NOT NULL
Poiché SQL consente che un attributo abbia valore **`null`**, se si vuole impedire ciò si usa il vincolo **`NOT NULL`**. 
Tale vincolo deve *sempre* essere specificato per la chiave primaria.

```SQL
CREATE TABLE IF NOT EXISTS dipendenti(
	id INT NOT NULL
);
```



#### CHIAVE PRIMARIA
La clausola **`PRIMARY KEY`** specifica uno o più attributi che fungeranno da chiave primaria.

```SQL
CREATE TABLE IF NOT EXISTS dipendenti( 
	id int NOT NULL,
	PRIMARY KEY (id)
);
```

#### UNIQUE
La clausola **`UNIQUE`** specifica una chiave candidata alternativa. Questo significa anche che ogni campo dell'attributo a cui si associa la clausola deve essere diverso ovvero *unico*.

```SQL
CREATE TABLE IF NOT EXISTS dipendenti( 
	id int NOT NULL,
	telefono varchar(10) NOT NULL UNIQUE,
	PRIMARY KEY (id)
);
```

#### DEFAULT
È anche possibile specificare un valore di default per un attributo, attraverso la clausola **`DEFAULT <value>`**, dopo la dichiarazione dell'attributo.
Senza tale clausola il valore di default di un attributo è null.

```SQL
CREATE TABLE IF NOT EXISTS dipendenti( 
	id int NOT NULL,
	mansione varchar(255) NOT NULL DEFAULT 'impiegato',
	PRIMARY KEY (id)
);
```

#### CHECK
Verifica (attraverso una condizione specificata) un campo in fase di modifica.

```SQL
CREATE TABLE IF NOT EXISTS dipendenti( 
	id int NOT NULL,
	stipendio decimal NOT NULL CHECK (stipendio >= 1200 AND stipendio <= 5000),
	PRIMARY KEY (id)
);
```
Quando si inserisce uno stipendio si controlla questo valore. Se il valore inserito rientra tra 1200 e 5000 questo viene accettato, altrimenti da un errore.

#### CHIAVE ESTERNA (FOREIGN KEY)
La clausola **`FOREIGN KEY`** specifica l'integrità relazionale.

```SQL
CREATE TABLE IF NOT EXISTS dipendenti( 
  id_dipendente int NOT NULL,
  nome_dip varchar(50) NOT NULL,
  PRIMARY KEY (id_dipendente)
);

CREATE TABLE IF NOT EXISTS clienti(
  id_cliente int NOT NULL,
  nome_cli varchar(50) NOT NULL,
  PRIMARY KEY (id_cliente)
);

CREATE TABLE IF NOT EXISTS rapporto_clienti(
  id_rapporto int NOT NULL,
  id_cliente int NOT NULL,
  id_dipendente int NOT NULL,
  PRIMARY KEY (id_rapporto),
  FOREIGN KEY (id_cliente) REFERENCES clienti(id_cliente), -- id_cliente in questa tabella si riferisce a id_cliente nella tabella clienti
  FOREIGN KEY (id_dipendente) REFERENCES dipendenti(id_dipendente) -- id_dipendente in questa tabella si riferisce a id_dipendente nella tabella dipendenti
);
```

![[Pasted image 20230109103230.png|600]]

---
### INSERIMENTO
Per inserire dati all'interno di una tabella si ha questa sintassi:

```SQL
INSERT INTO nome_tabella (colonna1, colonna2, ..., colonnaN)
VALUES (valore1, valore2, ..., valoreN);
```

La parte riguardante `(colonna1, colonna2, ..., colonnaN)` può anche essere omessa se si inseriscono bene i valori.

> [!example]- <font color="orange">Esempio</font>
>```SQL
>CREATE TABLE IF NOT EXISTS clienti(
>	id_cliente int NOT NULL,
>	nome_cli varchar(50) NOT NULL,
>	data_inizio date,
>	PRIMARY KEY (id_cliente)
>);
>
>INSERT INTO clienti (id_cliente, nome_cli, data_inizio)
>VALUES (0, "Xyz Impianti", "2015-03-15");
>```

#### AUTO INCREMENT
Si può automaticamente incrementare un valore intero ad ogni inserimento con la clausola `AUTOINCREMENT`.

```SQL
CREATE TABLE IF NOT EXISTS dipendenti( 
  id_dipendente int NOT NULL AUTO_INCREMENT,
  nome_dip varchar(50) NOT NULL,
  PRIMARY KEY (id_dipendente)
);

INSERT INTO dipendenti (nome_dip)
VALUES ("TestDipendente");
```

#### INSERIMENTO MULTIPLO
```SQL
INSERT INTO rubrica
VALUES 	("Rocca", "Forte", NULL, "2668123365", "2003-06-01"),
		("Panco", "Pillo", "pillowPanco@duecento.it", "333822158", "1976-02-11"),
		("Trota", "Rossa", "rossaTrota@quanto.com", "01222685", "1989-10-23");
```

---
### SELECT
Richiede dei dati da vedere.

```SQL
SELECT colonna1, colonna2, ..., colonnaN
FROM tabella1, tabella2, ..., tabellaN;
```

Al posto di mettere specifiche colonne, si può inserire `*` per selezionare tutte le colonne.

#### WHERE
Alla SELECT si possono aggiungere delle condizioni.
> [!example]- <font color="orange">Esempio</font>
>```SQL
>CREATE TABLE IF NOT EXISTS dipendenti( 
>  id_dipendente int NOT NULL AUTO_INCREMENT,
>  nome_dip varchar(50) NOT NULL,
>  PRIMARY KEY (id_dipendente)
>);
>
>SELECT nome_dip
>FROM dipendenti
>WHERE id_dipendente >= 4;
>```

#### CONDIZIONI AND - OR - NOT
Si possono inserire più di una condizione quando si fa una WHERE oppure una CHECK e le si possono mettere in [[003 OR (MMI)|OR]] oppure in [[002 AND (MMI)|AND]] tra di loro.
Alle condizioni si può aggiungere un [[004 NOT (MMI)|NOT]].

> [!example]- <font color="orange">Esempio</font>
>```SQL
>SELECT nome_dip
>FROM dipendenti
>WHERE NOT id_dipendente >= 4 AND nome_dip = "Test";
>```

---
### UPDATE
La clausola `UPDATE` serve per aggiornare una tabella.

```SQL
UPDATE tabella
SET colonna1 = valore1, colonnaN = valoreN
WHERE condizione;
```

Bisogna fare attenzione a non omettere `WHERE condizione` (modifica tutte le colonne della tabella!).

> [!example]- <font color="orange">Esempio</font>
>```SQL
>CREATE TABLE IF NOT EXISTS rubrica(
>  nome varchar(25) NOT NULL,
>  cognome varchar(25),
>  email varchar(35),
>  telefono varchar(15) NOT NULL,
>  compleanno date,
>  PRIMARY KEY (telefono)
>);
>
>INSERT INTO rubrica
>VALUES 	("Rocca", "Forte", NULL, "2668123365", "2003-06-01"),
>		("Panco", "Pillo", "pillowPanco@duecento.it", "333822158", "1976-02-11"),
>		("Trota", "Rossa", "rossaTrota@quanto.com", "01222685", "1989-10-23");
>
>
>UPDATE rubrica
>SET email = "roccaforte@regist.net"
>WHERE nome = "Rocca" AND cognome = "Forte";
>
>SELECT * FROM rubrica;
>```

---
### DELETE
La clausola `DELETE` serve per cancellare un record in una tabella.

```SQL
DELETE FROM tabella 
WHERE condizione;
```

Bisogna fare attenzione a non omettere `WHERE condizione` (cancella tutto da una tabella!).

> [!example]- <font color="orange">Esempio</font>
>```SQL
>INSERT INTO rubrica
>VALUES 	("Errore", "Palese", NULL, "000000000", NULL);
>
>DELETE FROM rubrica
>WHERE nome = "Errore";
>```

L'auto increment continua a contare anche se l'ultimo incremento è eliminato (eliminato = 7, prossimo incremento = 8).

---
### TRUNCATE
Ripulisce una tabella, aggiornando anche l'auto increment a zero. È più veloce della delete.

```SQL
TRUNCATE TABLE tabella; 
```



---
### JOIN
Si esegue la [[018 Operazioni di Join|JOIN]] tra due tabelle.

```SQL
SELECT campi_da_mostrare
FROM tabella1
/*TIPO*/ JOIN tabella2 ON condizione;
```

Ci sono tre tipi di `JOIN` principali:
- **`INNER JOIN`**, vengono mostrate solo quelle righe che rendono la condizione vera; ![[Pasted image 20230109131824.png]]
- **`LEFT JOIN`**, attacca alla tabella1 le righe della tabella2 che rendono la condizione vera; ![[Pasted image 20230109131839.png]]
- **`RIGHT JOIN`**, attacca alla tabella2 le righe della tabella1 che rendono la condizione vera; ![[Pasted image 20230109131943.png]]

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20230109134730.png]]
><center>Dipendenti (Tabella A)</center>
>
>![[Pasted image 20230109134755.png]]
><center>Uffici (Tabella B)</center>
>
>**INNER JOIN**
>```SQL
>SELECT dipendenti.id_dipendente, dipendenti.nome, dipendenti.data_assunzione, dipendenti.stipendio, dipendenti.telefono,
>	dipendenti.id_ufficio, ufficio.nome_ufficio
>FROM dipendenti
>INNER JOIN ufficio ON dipendenti.id_ufficio = ufficio.id_ufficio;
>```
>![[Pasted image 20230109134856.png]]
><center>Mostra solo le righe che rendono la condizione vera</center>
>
>**LEFT JOIN**
>```SQL
>SELECT dipendenti.id_dipendente, dipendenti.nome, dipendenti.data_assunzione, dipendenti.stipendio, dipendenti.telefono,
>	dipendenti.id_ufficio, ufficio.nome_ufficio
>FROM dipendenti
>LEFT JOIN ufficio ON dipendenti.id_ufficio = ufficio.id_ufficio;
>```
>![[Pasted image 20230109134935.png]]
><center>Leonardo Valli non ha un id_ufficio, quindi viene mostrato nella LEFT JOIN</center>
>
>**RIGHT JOIN**
>```SQL
>SELECT dipendenti.id_dipendente, dipendenti.nome, dipendenti.data_assunzione, dipendenti.stipendio, dipendenti.telefono,
>	dipendenti.id_ufficio, ufficio.nome_ufficio
>FROM dipendenti
>RIGHT JOIN ufficio ON dipendenti.id_ufficio = ufficio.id_ufficio;
>```
>![[Pasted image 20230109135228.png]]
><center>L'ufficio Risorse Umane non è assegnato a nessun dipendente</center>

---
### ALTER TABLE
La `ALTER TABLE` permette la modifica della struttura di una tabella. Si possono eseguire varie operazioni:
- Aggiungere una nuova colonna: 
```SQL
ALTER TABLE tabella 
ADD nuova_colonna tipo_dati vincoli;
```

- Cambiare la posizione di una colonna: 
```SQL
ALTER TABLE tabella
MODIFY colonna_da_cambiare tipo_dati 
AFTER colonna_nome;
```

- Aggiungere vincoli ad una colonna:
```SQL
ALTER TABLE tabella
ADD VINCOLO (colonna1, ..., colonnaN);
```

- Rimuovere una colonna:
```SQL
ALTER TABLE tabella
DROP COLUMN colonna;
```

- Cambiare il tipo di dato di una colonna:
```SQL
ALTER TABLE tabella
MODIFY colonna nuovo_tipo_dati;
```

- Rinominare una tabella:
```SQL
ALTER TABLE tabella
RENAME nuovo_nome;
```

> [!example]- <font color="orange">Esempio</font>
>**NUOVA COLONNA**
>```SQL
>ALTER TABLE ufficio 
>ADD prova varchar(50) NOT NULL;
>```
>
>**SPOSTAMENTO COLONNA**
>```SQL
>ALTER TABLE ufficio 
>MODIFY prova varchar(50)
>AFTER id_ufficio;
>```
>
>**AGGIUNTA VINCOLO**
>```SQL
>ALTER TABLE ufficio 
>ADD UNIQUE (prova);
>```
>
>**RIMOZIONE COLONNA**
>```SQL
>ALTER TABLE ufficio
>DROP COLUMN prova;
>```
>
>**MODIFICA DEL TIPO DI DATO DI UNA COLONNA**
>```SQL
>ALTER TABLE ufficio 
>MODIFY prova int;
>```
>
>**RINOMINAZIONE TABELLA**
>```SQL
>ALTER TABLE ufficio
>RENAME uffici;
>```

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
- https://www.youtube.com/playlist?list=PLP5MAKLy8lP_zsv6uijqYTIjdVEYgWf44