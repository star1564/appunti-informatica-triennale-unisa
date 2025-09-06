---
aliases: 
tags:
  - corsi/informatica/basi_di_dati
paragrafo: SQL
cssclasses:
  - 
---
## ALTRI COMANDI
### IS (NOT) NULL PER LE CONDIZIONI
Per verificare in una condizione se un valore è null si scrive in questo modo:
```SQL
valore IS NULL -- valore è nullo? -> T/F
```

Per vedere il contrario si scrive così:
```SQL
valore IS NOT NULL -- valore non è nullo? -> T/F
```

---
### BETWEEN
Per avere una operazione di confronto che verifica se un valore rientra tra un numero ed un altro, si usa **`BETWEEN`**.
```SQL
valore_da_verificare BETWEEN numero1 AND numero2 
-- numero1 <= valore_da_verificare <= numero2? -> T/F
```

Essenzialmente sostituisce questo tipo di scrittura:
```SQL
valore_da_verificare > numero1 AND valore_da_verificare < numero2;
```

Funziona per i tipi numerici, data e tempo.

> [!example]- <font color="orange">Esempio</font>
>```SQL
>potenza BETWEEN 100 AND 120;
>```
>
>```SQL
>dataS BETWEEN "2010-01-15" AND "2010-02-10";
>```
>
>```SQL
>orario BETWEEN "10:50:00" AND "13:00:00";
>```

---
### LIKE
Per trovare delle sottostringhe all'interno di una stringa si usa **`LIKE`**. Ha due caratteri jolly:
- `%` rimpiazza un qualsiasi numero di caratteri;
- `_` rimpiazza un solo carattere.

```SQL
stringa LIKE "Test_A%" 
-- Ritorna vero se la stringa comincia per "Test", ha un carattere qualsiasi, ha il carattere "A", ha un numero qualsiasi di caratteri
```

Funziona anche sulle date e gli orari.

> [!example]- <font color="orange">Esempio</font>
>Trovare tutti gli impiegati il cui indirizzo è a "Houston, Texas".
>```SQL
>SELECT FNAME, LNAME
>FROM EMPLOYEE
>WHERE ADDRESS LIKE "%HOUSTON, TEXAS%";
>```
>Trovare tutti gli impiegati nati il mese di febbraio del 1998.
>```SQL
>SELECT FNAME, LNAME
>FROM EMPLOYEE
>WHERE DATEOFBIRTH LIKE "1998-02-__";
>```

## QUERY COMPLESSE

> [!note]- Traccia
>![[Pasted image 20230110155022.png|600]]
>![[Pasted image 20230110155030.png|800]]

### ALIAS NELLE QUERY
Nelle [[Query]] si possono creare degli alias attraverso il comando `AS`. È molto utile per quando si lavora su più tabelle con nomi lunghi nella stessa query.

```SQL
SELECT column_names
FROM table_name AS alias_name;
```

> [!example]- <font color="orange">Esempio</font>
>```SQL
>SELECT a.Targa, p.Nome, ass.Nome   -- Abbiamo la targa dell'auto, il nome del proprietario e il nome dell'assicurazione
>FROM AUTO AS a, PROPRIETARI AS p, ASSICURAZIONI AS ass
>WHERE ...
>```
>Invece di:
>```SQL
>SELECT AUTO.Targa, PROPRIETARI.Nome, ASSICURAZIONI.Nome
>FROM AUTO, PROPRIETARI, ASSICURAZIONI
>WHERE ...
>```

> [!example]- <font color="orange">Esempio Query 1</font>
>```SQL
>SELECT  a.Targa, a.Marca
>FROM    AUTO AS a
>WHERE   (a.Cilindrata > 2000 OR a.Potenza > 120);
>```

---
### PIÙ CONDIZIONI NELLA WHERE
Per eseguire delle query più avanzate si può espandere la [[024 Comandi SQL#WHERE|WHERE]] attraverso un qualsiasi numero di `AND`.

> [!example]- <font color="orange">Esempio Query 2</font>
>```SQL
>SELECT  a.Targa, a.Marca
>FROM    AUTO AS a, PROPRIETARI AS p
>WHERE   (a.Cilindrata > 2000 OR a.Potenza > 120) AND    -- Specificazione auto 
>		(p.CodF = a.CodF);                              -- Associazione auto al proprio proprietario
>```

> [!example]- <font color="orange">Esempio Query 3</font>
>```SQL
>SELECT  a.Targa, p.Nome
>FROM    AUTO AS a, PROPRIETARIO AS p, ASSICURAZIONI AS ass
>WHERE   (a.Cilindrata > 2000 OR a.Potenza > 120) AND
>		(a.CodF = ass.CodF) AND
>		(a.CodAss = ass.CodAss) AND
>		(ass.Nome = "SARA") AND
>		(ass.Sede = "Pisa");
>```

> [!example]- <font color="orange">Esempio Query 4</font>
>```SQL
>SELECT  a.Targa, p.Nome
>FROM    AUTO AS a, PROPRIETARIO AS p, ASSICURAZIONI AS ass, AUTOCOINVOLTE AS ac, SINISTRO AS s
>WHERE   (a.CodF = ass.CodF) AND
>		(a.CodAss = ass.CodAss) AND
>		(ass.Nome = "SARA") AND
>		(ass.Sede = "Pisa") AND
>		(a.Targa = ac.Targa) AND
>		(ac.CodS = s.CodS) AND
>		(s.Data = "2010-01-20");
>```

---
### CONTATORE COUNT
Per contare un certo numero di occorrenze di ricorrenze si usa **`COUNT(*)`**. Dopo la `WHEN` si inserisce **`GROUP BY`** per specificare come raggruppare i risultati.
Viene usato insieme agli alias. 
Se messo sulla riga di `SELECT`, nel risultato apparirà la colonna di `COUNT` con il numero di occorrenze avvenute nel gruppo specificato da `GROUP BY`.

> [!example]- <font color="orange">Esempio</font>
>Diciamo di avere questa situazione: ![[Pasted image 20230111113039.png]]
>Vogliamo sapere quante persone lavorano in ogni team.
>```SQL
>SELECT id_team, COUNT(*) AS contaImpiegati
>FROM   impiegati
>GROUP BY id_team;
>```
>![[Pasted image 20230111114236.png]]
>![[Pasted image 20230111113442.png]]
><center>Essenzialmente ha fatto un conteggio a parte per ogni team trovato</center>

> [!example]- <font color="orange">Esempio Query 5</font>
>```SQL
>SELECT   ass.Nome, ass.Sede, COUNT(*) AS numAuto
>FROM     AUTO AS a, ASSICURAZIONI AS ass
>WHERE    (a.CodAss = ass.CodAss)
>GROUP BY ass.CodAss;
>```
>![[Pasted image 20230110161400.png]]

> [!example]- <font color="orange">Esempio Query 6</font>
>```SQL
>SELECT   a.Targa, COUNT(*) AS numSin
>FROM     AUTO AS a, AUTOCOINVOLTE AS ac
>WHERE    (a.Marca = "Fiat") AND
>		 (a.Targa = ac.Targa)
>GROUP BY a.Targa;
>```
>![[Pasted image 20230110162039.png]]

---
### HAVING COUNT
La **`HAVING`** serve per selezionare eventi con una condizione (es. per ciascuna auto coinvolta in più di un sinistro). Si inserisce in questa clausola un `COUNT(*)`.
Si mette dopo la `WHERE` e l'eventuale `GROUP BY`.

> [!example]- <font color="orange">Esempio Query 7</font>
>```SQL
>SELECT   a.Targa, ass.Nome, COUNT(*) AS numSin
>FROM     AUTO AS a, ASSICURAZIONI AS ass, AUTOCOINVOLTE AS ac
>WHERE    (a.Targa = ac.Targa) AND
>		 (a.CodAss = ass.CodAss)
>GROUP BY a.Targa
>HAVING COUNT(*) > 1;
>```
>![[Pasted image 20230110162541.png]]

---
### SUM
La funzione **`SUM(colonna)`** può essere usata in due modi:
- Sommare tutti i valori di una colonna;
- Usare la `GROUP BY`.

> [!example]- <font color="orange">Esempio della somma di una colonna</font>
>```SQL
>-- Creazione tabella ed inserimento dati
>CREATE TABLE numeri (num INT);  
>
>INSERT INTO numeri(num) 
>VALUES (50), (100), (150), (200);
>
>SELECT * FROM numeri;
>```
>![[Pasted image 20230111111547.png]]
>Ora calcoliamo la somma totale della colonna `num` usando la `SUM()`.
>```SQL
>SELECT SUM(num) AS sommaNumeri
>FROM numeri;
>```
>![[Pasted image 20230111111709.png]]

> [!example]- <font color="orange">Esempio con GROUP BY</font>
>Diciamo di avere questa situazione: ![[Pasted image 20230111113039.png]]
>Vogliamo sapere lo stipendio totale di ogni team.
>```SQL
>SELECT id_team, SUM(stipendio) AS sommaStipendi
>FROM   impiegati
>GROUP BY id_team;
>```
>![[Pasted image 20230111113555.png]]
>![[Pasted image 20230111113442.png]]
><center>Essenzialmente ha fatto una somma a parte degli stipendi per ogni team trovato</center>

---
### AVG, MIN, MAX
Queste funzioni sono simili alla `SUM` (anche queste hanno bisogno del `GROUP BY`).
- **`AVG(colonna)`**, media di una colonna;
- **`MIN(colonna)`**, valore minimo di una colonna;
- **`MAX(colonna)`**, valore massimo di una colonna.

## QUERY ANNIDATE
Una **Query Annidata** è una query all'interno di un'altra query, ovvero quando si ha una `SELECT` all'interno della `SELECT` principale.

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20230111140024.png]]
>Vogliamo trovare tutti gli studenti che hanno un GPA maggiore della media. Ma non sappiamo quanto è la media.
>Normalmente si possono fare due query:
>```SQL
>SELECT AVG(GPA)
>FROM students; -- Risultato: 3.19
>```
>
>```SQL
>SELECT *
>FROM students
>WHERE GPA > 3.19;
>```
>
>Ma questo lo si può fare in una sola query annidata:
>```SQL
>SELECT *
>FROM students
>WHERE GPA > (
>			SELECT AVG(GPA)
>			FROM students);
>);
>```
>
>La sub-query restituisce una tabella con una singola colonna e riga. Mentre la query principale restituisce questa tabella.
>
>![[Pasted image 20230111140406.png]]

Una query annidata si mette nella clausola `WHERE` e si possono confrontare i risultati in due modi, che dipendono dal tipo di risultato della sub-query.

Una sub-query può essere:
- A singolo valore (con una sola colonna ed una sola riga);
	- In questo caso si possono usare gli operatori di confronto normali (come `<` e `>`).
- Una lista di valori.
	- In questo caso si possono usare gli operatori `IN`, `NOT IN`, `ANY`, `ALL`, `EXISTS` e `NOT EXISTS`.

### OPERATORE IN - NOT IN
L'operatore **`IN`** controlla se un certo valore *si trova* nella tabella restituita dalla sub-query. 
Se questo valore si trova, lo inserisce nel risultato della query principale.

> [!example]- <font color="orange">Esempio</font>
>```SQL
>SELECT AVG(number_of_students)
>FROM classes
>WHERE teacher_id IN (
>		SELECT id
>		FROM teachers
>		WHERE subject = "English" OR subject = "History");
>```
>La query annidata ritorna una tabella di tutti i professori che insegnano inglese o storia.
>La query principale confronta *per ogni* `teacher_id` se questo si trova nella tabella della sub-query. Se si trova, lo inserisce nel risultato finale.

La **`NOT IN`** è l'opposto: se *non si trova* nella tabella della sub-query, lo inserisce nel risultato della query principale.

### OPERATORE ANY
L'operatore **`ANY`** controlla se *una condizione* su un certo valore *è vera per qualche valore* della tabella restituita dalla sub-query.

Può anche essere combinato con gli operatori di confronto unari.

> [!example]- <font color="orange">Esempio</font>
>Trovare tutti i nomi degli impiegati, tranne quelli con il salario minimo.
>```SQL
>SELECT  LNAME, ENAME
>FROM    EMPLOYEE
>WHERE   SALARY > ANY(SELECT SALARY
>					 FROM EMPLOYEE);
>```

### OPERATORE ALL
L'operatore **`ALL`** controlla se *una condizione* su un certo valore *è vera tutti i valori* della tabella restituita dalla sub-query.

Può anche essere combinato con gli operatori di confronto unari.

> [!example]- <font color="orange">Esempio</font>
>Trovare tutti i nomi degli impiegati il cui salario è maggiore del salario di tutti gli impiegati del dipartimento 5.
>```SQL
>SELECT  LNAME, ENAME
>FROM    EMPLOYEE
>WHERE   SALARY > ALL(SELECT SALARY
>					 FROM EMPLOYEE
>					 WHERE DNO = 5);
>```

### OPERATORE EXISTS - NOT EXISTS
L'operatore **`EXISTS`** serve per verificare se il risultato di una query annidata *NON è vuota*.

Invece, **`NOT EXISTS`** serve per verificare se il risultato di una query annidata *è vuota*.

> [!example]- <font color="orange">Esempio</font>
>Ritrovare il nome degli impiegati che NON hanno persone a carico.
>```SQL
>SELECT  FNAME, LNAME
>FROM    EMPLOYEE
>WHERE   NOT EXISTS(SELECT *
>				   FROM DEPENDENT
>				   WHERE SSN = ESSN);
>```
>
>In questo caso nel risultato della query principale si inseriscono gli impiegati dove la sub-query ritorna un risultato vuoto.


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
- [Query Annidate](https://learnsql.com/blog/sql-nested-select/)