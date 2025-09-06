---
aliases: 
tags:
  - corsi/informatica/programmazione_strutture_dati
paragrafo: Programmazione Modulare
cssclasses:
  - 
---
Abbiamo bisogno di un [[016 Introduzione agli ADT|ADT]], e dell'[[019 Pseudo-Generici|Item pseudo-generico]].

## TRACCIA ESEMPIO
Si tiene conto della seguente traccia:
Specificare e realizzare un ADT "Playlist" che dovrà fornire operatori per:
1. Creare una playlist (indicata da un nome e una lista di canzoni):
	- Una canzone è caratterizzata dal titolo, dal nome del cantante e dalla durata della canzone in secondi.
3. Inserire una canzone in testa ad una playlist;
4. Rimuovere una canzone da una playlist dato il titolo.

## I MODULI
I moduli che si utilizzeranno saranno Playlist, Song, List, Item-song.
![[Pasted image 20220408131647.png|500]]

<u><font color="61AFEF">Playlist</font></u> [<font color="61AFEF">CONTENITORE OGGETTI</font>] servirà per conservare una struttura con un <font color="#d809eb">ADT Lista</font> ed un nome, <font color="61AFEF">conterrà</font> delle canzoni. 
Le funzioni saranno quelle per: inizializzare una Playlist; stampare una Playlist; aggiungere e rimuovere canzoni <font color="CC241D">create</font>; eventuali richieste (ordinamento canzoni).

<u><font color="CC241D">Song</font></u> [<font color="CC241D">RITORNA-INFORMAZIONI E CREAZIONE OGGETTI</font>] servirà per conservare una struttura con il nome, l'artista e la durata delle canzoni.
Le funzioni saranno quelle per: <font color="CC241D">creare</font> una nuova canzone, associando le informazioni <font color="lime">raccolte</font> in item-song; <font color="CC241D">restituire le informazioni</font> per delle canzoni già create.

<u><font color="lime">Item-song</font></u> [<font color="lime">RACCOLTA INFORMAZIONI</font>] servirà a <font color="lime">raccogliere</font> le informazioni dall'utente (nome, artista, durata) con scanf (o altri metodi) e farle associare direttamente ad una nuova canzone dalla funzione di <font color="CC241D">creazione</font> in Song (chiamata alla funzione di creazione di Song).
Darà all'<font color="#d809eb">ADT Lista</font> il tipo Item da usare. Le funzioni saranno quelle di: <font color="lime">raccolta</font> informazioni con chiamata ad associazione; stampa di tutte le informazioni di una canzone; comparazione tra due item.

## CODICE
### PLAYLIST
#### PLAYLIST.H
```C
#include "song.h"

//Puntatore alla struttura
typedef struct playlist *Playlist;

//Prototipi Funzioni
...
```

#### PLAYLIST.C
```C
#include "song.h"
#include "playlist.h"
#include "list.h"
#include <stdlib.h>
#include <string.h>

//Struttura
struct playlist{
	char *name;
	List songs;
};

//Funzioni
...
```

#### CREAZIONE PLAYLIST (createPlaylist)
```C
Playlist createPlaylist(char *name){
	Playlist p = malloc(sizeof(struct playlist));
	p->name = strdup(name);
	//Restituisce un puntatore a una copia della stringa
	p->songs = newList();
	return p;
}
```

#### AGGIUNTA DI UNA CANZIONE ALLA PLAYLIST (addSong)
```C
void addSong(Playlist p, Song s){
	addHead(p->songs, s);
}
```

#### RIMOZIONE DI UNA CANZONE DALLA PLAYLIST (removeSong)
```C
Song removeSong(Playlist p, char *name){
	Song canzone_elim = initSong(name, "", 0);
	return removeListItem(p->songs, canzone_elim);
}
```

#### RITORNA LE INFORMAZIONI DELLA PLAYLIST
```C
char *name(Playlist p){
	return strdup(p->name);
}
```

### SONG
#### SONG.H
```C
//Puntatore alla struttura
typedef struct song *Song;

//Prototipi Funzioni
...
```

#### SONG.C
```C
#include <song.h>
#include <list.h>
#include <stdlib.h>
#include <string.h>

//Struttura
struct song{
	char *title;
	char *artist;
	int duration;
};

//Funzioni
...
```

#### INIZIALIZZAZIONE CANZONE (initSong)
```C
Song initSong(char *title, char *artist, int duration){
	Song s = malloc(sizeof(struct song));
	s->title = strdup(title);
	s->artist = strdup(artist);
	s->duration = duration;
	return s;
}
```

#### RITORNA LE INFORMAZIONI DELLA PLAYLIST
```C
char *title(Song s){
	return strdup(s->title);
}

char *artist(Song s){
	return strdup(s->artist);
}

int duration(Song s){
	return s->duration;
}
```

### ITEM-SONG
#### INPUT ITEM
Prendi in input dall'utente tutti i dati necessari per creare una funzione (titolo, artista, durata). Alla fine si crea una canzone con le informazioni ottenute.
```C
Item inputItem(){
	//Prendi informazioni
	return initSong(title, artist, duration);
}
```

#### OUTPUT ITEM
Crea una formattazione per stampare tutte le informazioni della canzone.
```C
void outputItem(Item item){
	Song s = item;
	printf("%s\n", title(s));
	printf("%s\n", artist(s));
	printf("%d\n", duration(s));
}
```

#### COMPARE ITEM
In questo caso si deve fare una comparazione tra due stringhe.
```C
int cmpItem(Item a1, Item a2){
	Song s1 = a1;
	Song s2 = a2;
	return strcmp(title(s1), title(s2));
}
```

___
[[000 Indice PSD|↖ Ritorna all'indice ↖]]

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