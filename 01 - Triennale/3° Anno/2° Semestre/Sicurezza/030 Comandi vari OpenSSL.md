---
aliases:
  - Base64
tags:
  - corsi/informatica/sicurezza
paragrafo: SSL/TLS
cssclasses:
  - 
---
## Comando `enc`

> [!summary] Comando [enc](https://www.openssl.org/docs/man1.1.1/man1/enc.html)
> Viene utilizzato per cifrare e decifrare i dati attraverso diversi algoritmi di cifratura simmetrici (a blocchi e a flusso) con chiavi basate su password (inserite nel terminale o prese da un file).
>```shell
>openssl enc [options]
>```
>- `-e` - Cifra i dati
>- `-d` - Decifra i dati
>- `-(ciphername)` - Specifica l'algoritmo di cifratura, con lunghezza della chiave (opzionale) e modalità operativa (opzionale) all'interno del nome
>	- L'unico [[014 Cifrari a Stream|Stream Cipher]] supportato è [[016 Altri tipi di Cifrari a Stream#RC4|RC4]]
>- `-in [filetext]` - Specifica il file di input da cui prendere il testo in chiaro (è possibile cifrare qualsiasi tipo di file, anche immagini)
>- `-out [filetext]` - Specifica il file di output dove inserire il testo cifrato
>- `-k [key]` - Specifica la chiave usata dall'algoritmo di cifratura, se non viene specificata OpenSSL deriverà questa chiave da una password
>- `-pass [password]` - la sorgente della password, i valori possibili sono 
>	- `pass:[password]`, la password viene scritta nel terminale
>	- `pass:[file]`, la password si trova in un file
>- `-base64` - applica la codifica *Base64* prima o dopo le operazioni crittografiche

> [!info]+ Base64
> Base64 è un sistema di decodifica/codifica per dati binari, che utilizza **64 simboli stampabili**:
> 
> ![[Pasted image 20240615174551.png|500]]
> 
> Un file in input viene processato in **blocchi da 24 bit**. Ciascun blocco è suddiviso in **gruppi da 6 bit**. Viene considerato il valore decimale di ciascun gruppo di bit da 6, tale valore rappresenterà un indice nella tabella di codifica Base 64.
> 
> ![[Pasted image 20240615174937.png|700]]
> 
> Se il numero totale di bit da processare non è un multiplo di 24, allora viene aggiunto il **padding** (bit nulli alla fine).

> [!example]+ <font color="orange">Esempio</font>
>Codifica (con [[013 Advanced Encryption Standard|AES]]):
>```shell
>openssl enc -e -aes-256-cbc -base64 -in "FileInChiaro.txt" -out "FileCifrato.txt" -pass pass:P1pp0B4ud0
>```
>
>Decodifica (con [[013 Advanced Encryption Standard|AES]]):
>```shell
>openssl enc -d -aes-256-cbc -base64 -in "FileCifrato.txt" -out "FileDecifrato.txt" -pass pass:P1pp0B4ud0
>```

## Comando `genrsa`

> [!summary] Comando [genrsa](https://www.openssl.org/docs/man1.0.2/man1/genrsa.html)
> Viene utilizzato per generare una chiave privata [[017 RSA|RSA]].
>```shell
>openssl genrsa [options] [numbits]
>```
>
>1. `options`:
>	- `-des`, `-des3`, `-aes(bits)`, ... - Specifica l'algoritmo di cifratura per la generazione delle chiavi
>	- `-out [filekey]` - Specifica il file di output per la chiave privata (in formato `.pem`)
>	- `-passout [password]` - Specifica la password usata per la cifratura della chiave
>
>2. `numbits` rappresenta la lunghezza della chiave RSA (modulo). Di default è 2048 bit.

> [!example]+ <font color="orange">Esempio</font>
>Generazione di una chiave privata attraverso una password (cifratura):
>```shell
>openssl genrsa -out "rsaprivatekey.pem" -passout pass:P1pp0B4ud0 -aes128 1024
>```
>


## Comando `rsa`
> [!summary] Comando [rsa](https://www.openssl.org/docs/man1.0.2/man1/openssl-rsa.html)
>Viene usato per gestire le chiavi RSA, come la conversione, la visualizzazione e l'ottenimento di una chiave pubblica da una chiave privata.
>```shell
>openssl rsa [options]
>```
>
>- `-in [filekey]` - Specifica il file di input della chiave
>- `-out [filekey]` - Specifica il file di output della chiave
>- `-pubout` - Estrae la chiave pubblica
>- `-passin [password]` - Specifica la password usata per la decifratura della chiave

> [!example]+ <font color="orange">Esempio</font>
>Ottenimento della chiave pubblica da una privata (cecifratura):
>```shell
>openssl rsa -in "rsaprivatekey.pem" -passin pass:P1pp0B4ud0 -pubout -out "rsapublickey.pem"
>```


### Comando `rsautl`

> [!summary] Comando [rsautl](https://www.openssl.org/docs/man1.0.2/man1/openssl-rsautl.html)
>Viene usato per firmare, verificare, cifrare e decifrare i dati utilizzando con una chiave RSA.
>```shell
>openssl rsautl [options]
>```
>Cifrare e decifrare:
>- `-encrypt` - Cifra i dati
>- `-decrypt` - Decifra i dati
>- `-in [filetext]` - Specifica il file di input da cui prendere il testo in chiaro o cifrato
>- `-out [filetext]` - Specifica il file di output in cui scrivere il risultato
>- `-inkey [file]` - Specifica il file della chiave 
>	- *pubblica* per cifrare
>	- *privata* per decifrare
>- `-pubin` - Specifica che l'input è una chiave pubblica RSA (usata per la cifrare)
>
>Firmare e verificare:
>- `-sign` - Firma i dati in input con la *chiave privata*. Manda in output il risultato firmato (in un file `.bin`)
>- `-verify` - Verifica i dati in input con la *chiave pubblica*. Se la verifica ha successo allora manda in output il risultato in chiaro


> [!example]+ <font color="orange">Esempio - Cifratura e decifratura</font>
>Cifratura:
>```shell
>openssl rsautl -encrypt -pubin -inkey "rsapublickey.pem" -in "testoInChiaro.txt" -out "testoCifrato.txt"
>```
>
>Decifratura:
>```shell
>openssl rsautl -decrypt -inkey "rsaprivatekey.pem" -in "testoCifrato.txt" -out "testoDecifrato.txt"
>```


> [!example]+ <font color="orange">Esempio - Firma e verifica</font>
>Firma:
>```shell
>openssl rsautl -sign -inkey "rsaprivatekey.pem" -in "testoInChiaro.txt" -out "rsasign.bin"
>```
>
>Verifica:
>```shell
>openssl rsautl -verify -pubin -inkey "rsapublickey.pem" -in "rsasign.bin" -out "testoInChiaro.txt"
>```


## Comando `dhparam`

> [!summary] Comando [dhparam](https://www.openssl.org/docs/man1.0.2/man1/openssl-dhparam.html)
> Viene utilizzato per manipolare i file per i parametri di [[018 Protocollo Diffie-Hellman|Diffie-Hellman]].
>```shell
>openssl dhparam [options] [numbits]
>```
>
>1. `options`:
>	- `-in [fileparams]` - Specifica il file in input da cui leggere i parametri
>	- `-out [fileparams]` - Specifica il file in output dove scrivere i parametri
>	- `-inform [arg]` - Specifica il formato di input, dove `arg` può essere `DER` o `PEM`
>	- `-outform [arg]` - Specifica il formato di output, dove `arg` può essere `DER` o `PEM`
>	- `-text` - Stampa i parametri Diffie-Hellman in formato testuale
>	- `-2` - Genera i parametri usando 2 come valore del generatore (default se `numbits` è presente)
>	- `-5` - Genera i parametri usando 5 come valore del generatore
>
>2. `numbits` rappresenta la lunghezza in bit dei parametri da generare. Di default è 2048 bit.

> [!example]+ <font color="orange">Esempio</font>
>Con questo comando vengono generati i parametri pubblici per DH e vengono salvati nel file. 
>```shell
>openssl dhparam -out "dhparams.pem" -2 1024
>```

> [!question]+ Parametri pubblici DH
> Si assuma che `dhparams1.pem` contenga i parametri pubblici DH $p_1$ e $g_1$. Si assuma inoltre che `dhparams2.pem` contenga i parametri pubblici DH $p_2$ e $g_2$. Siano Utente1 e Utente 2 degli utenti.
> 
> Sia Utente1 che Utente2 devono usare `dhparams1.pem` e `dhparams2.pem` per generare la propria coppia di chiavi.

## Comando `pkey`
> [!summary] Comando [pkey](https://www.openssl.org/docs/man1.0.2/man1/openssl-pkey.html)
> Viene utilizzato per gestire le chiavi pubbliche e private per i diversi algoritmi (RSA, DSA, ecc.). Possono anche essere convertite tra le varie forme.
>```shell
>openssl pkey [options]
>```
>
>- `-in [filekey]` - Specifica il file di input della chiave
>- `-out [filekey]` - Specifica il file di output della chiave
>- `-inform [arg]` - Specifica il formato di input, dove `arg` può essere `DER` o `PEM`
>- `-outform [arg]` - Specifica il formato di output, dove `arg` può essere `DER` o `PEM`


> [!example]+ <font color="orange">Esempio - Esportazione di chiavi pubbliche</font>
>Gli utenti devono scambiarsi le loro rispettive chiavi pubbliche. 
>
>Ciascun utente deve estrarre la propria chiave pubblica e memorizzarla in un apposito file:
>```shell
># Utente 1
>openssl pkey -in "privatekey1.pem" -pubout -out "publickey1.pem"
>
># Utente 2
>openssl pkey -in "privatekey2.pem" -pubout -out "publickey2.pem"
>```
>
>Questo lo si può fare anche con il comando `rsa`, ma in questo caso è più "generico", in quanto le chiavi non devono essere per forza RSA.

### Comando `pkeyutl`
> [!summary] Comando [pkeyutl](https://www.openssl.org/docs/man1.0.2/man1/openssl-pkeyutl.html)
> Viene utilizzato per eseguire operazioni su chiavi pubbliche.
>```shell
>openssl pkeyutl [options]
>```
>- `-encrypt` - Cifra i dati utilizzando una chiave pubblica
>- `-decrypt` - Decifra i dati utilizzando una chiave privata
>- `-in [filetext]` - Specifica il file di input da cui verranno letti i dati
>- `-out [filetext]` - Specifica il file di output dove verrà scritto il risultato
>- `-inkey [file]` - Specifica il file della chiave
>- `-pubin` - Specifica che l'input è una chiave pubblica
> 
>Derivare un segreto:
>- `-derive` - Deriva un secreto condiviso attraverso la `peerkey`
>- `-peerkey [file]` - Il file peerkey, utilizzato dalla `derive`
> 
>Verificare una firma:
>- `-sign` - Firma i dati in input con la *chiave privata*. Manda in output il risultato firmato (in un file `.bin`)
>- `-verify` - Verifica i dati di input attraverso la *chiave pubblica* con quelli del file della firma ed indica se la verifica ha avuto un esito positivo o negativo
>- `-sigfile [file]` - Specifica il file firmato

> [!example]+ <font color="orange">Esempio - Cifratura e Decifratura</font>
>Cifratura:
>```shell
>openssl pkeyutl -encrypt -pubin -inkey "pub.pem" -in "plain.txt" -out "cipher.txt"
>```
>
>Decifratura:
>```shell
>openssl pkeyutl -decrypt -inkey "priv.pem" -in "plain.txt" -out "cipher.txt"
>```


> [!example]+ <font color="orange">Esempio - Calcolo del segreto condiviso</font>
>Sia l'Utente 1 che l'Utente 2 eseguono i seguenti comandi per ottenere la chiave condivisa, composta da 1014 bit.
>```shell
># Utente 1
>openssl pkeyutl -derive -inkey "dhkey1.pem" -peerkey "dhpub2.pem" -out "segreto1.bin"
>
># Utente 2
>openssl pkeyutl -derive -inkey "dhkey2.pem" -peerkey "dhpub1.pem" -out "segreto2.bin"
>```

> [!example]+ <font color="orange">Esempio - Firma e verifica</font>
>Firma:
>```shell
>openssl pkeyutl -sign -inkey "privkey.pem" -in "plain.txt" -out "firma.bin"
>```
>Verifica:
>```shell
>openssl pkeyutl -verify -pubin -inkey "pubkey.pem" -sigfile "firma.bin" -in "plain.txt"
>```

> [!example]+ <font color="orange">Esempio - Con i comandi Hash specifici</font>
>Firma **dell'hash**:
>```shell
>openssl sha1 -sign "rsaprivatekey.pem" -out "rsasign.bin" "testoInChiaro.txt"
>```
>
>Verifica, se ha successo, il file in ingresso viene correttamente decifrato:
>```shell
>openssl sha1 -verify "rsapublickey.pem" -signature "rsasign.bin" "testoInChiaro.txt"
>```


## Comando `dgst`

> [!summary] Comando [dgst](https://www.openssl.org/docs/man1.0.2/man1/openssl-dgst.html)
>Viene utilizzato per calcolare e verificare gli [[019 Algoritmi di Hashing Crittografici|Hash]] (digest) dei dati.
>```shell
>openssl dgst [options] [file]
>```
>
>1. `options`:
>	- `-(hashname)` - Specifica il nome della funzione hash da usare per il calcolo del digest
>		- digest può essere uno degli algoritmi mostrati dal comando `openssl list --digest-commands`
>	- `-out [file]` - Specifica il file in cui scrivere l'output della funzione
>	- `-hmac [key]` - Crea un hashed MAC usando una determinata chiave
>	- Firma e verifica:
>		- `-sign [filekey]` - Firma un file con la *chiave privata*. Manda in output il risultato firmato (in un file `.bin`)
>		- `-verify [filekey]` - Verifica la firma attraverso la *chiave pubblica* descritta in `file` ed indica se la verifica ha avuto un esito positivo o negativo
>		- `-signature [file]` - Il vero file con la firma da verificare
>
>> [!warning] Ci sono anche comandi con i nomi degli hashname
> Ad esempio `openssl sha256` invece di `openssl dgst -sha256`
>
>2. `file`: uno o più file su cui deve essere applicata la funzione hash

> [!example]+ <font color="orange">Esempio - Calcolo dell'hash</font>
>```shell
>openssl dgst -sha256 -out "hash.txt" "file.txt"
>```

> [!question]+ Cosa può fare `dgst`
> - Può essere usato per calcolare l'hash di *uno o più* file *con uno specifico algoritmo* (SHA, MD5, ecc...)
> 	- Se vengono passati più file, viene calcolato un hash separato per ciascun file
> - Può essere usato per calcolare l'[[020 Message Authentication Code#HMAC|HMAC]] di *uno o più* file
> - Può essere usato per *verificare una firma*
> - Può essere usato insieme al comando `cmp` per verificare se due file portano ad una *collisione*

> [!example]+ <font color="orange">Esempio - Verifica firma con una coppia di chiavi RSA</font>
>```shell
>openssl dgst -sha256 -verify "pub.pem" -signature "firma.bin" "plain.txt"
>```


## Comando `rand`
> [!summary] [rand](https://www.openssl.org/docs/man1.0.2/man1/openssl-rand.html)
> Viene utilizzato per generare [[021 Pseudo-casualità|numeri pseudo-casuali]]. Avviene attraverso un Probabilistic Random Bit Generator (PRBG).
>```shell
>openssl rand [options] [numbyte]
>```
>
>1. `options`:
>	- `-rand [file(s)]` - Specifica i file usati come seme per il generatore
>	- `-out [file]` - Specifica il file in cui scrivere i dati generati
>	- `-base64` - I dati generati sono codificati in formato Base64
>	- `-hex` - I dati generati sono codificati in formato esadecimale
>
>2. `numbyte`: il numero di byte che si intende generare

> [!info]+ Sempre multiplo di 4
>```shell
>openssl rand -base64 6
>```
>
>Quando i dati generati sono convertiti in Base64, la stringa prodotta avrà sempre una lunghezza multipla di quattro.
>Se si desidera una stringa pseudocasuale la cui lunghezza non sia multipla di quattro, è necessario "accorciare" tale stringa mediante altri strumenti.
>
>```shell
>openssl rand -base64 39 | cut -c1-39
>```
>- Genera una stringa di 39 byte casuali in base64, che risulterà in una stringa più lunga
>- Taglia e ritorna i primi 39 caratteri della stringa risultante, ignorando il resto

> [!question] Può essere usato per generare stringhe di caratteri stampabili
> Attraverso `-base64`.


___
[[000 Indice Sicurezza|↖ Ritorna all'indice ↖]]

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