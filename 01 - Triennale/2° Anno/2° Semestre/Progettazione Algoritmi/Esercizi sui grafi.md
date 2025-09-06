## TABELLA RIASSUNTIVA ALGORITMI SU GRAFI
Dato un grafo $G=(V,E)$, dove $V$ è il numero di vertici e $E$ è il numero di archi nel grafo, possiamo implementare i seguenti algoritmi:



| Algoritmo                                                        | Descrizione                                                                                                                                                                                                                                | Tecnica Utilizzata                                                                                | Complessità Temporale                                                                                    |
| ---------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| **[[014 Breadth First Search\|Breadth First Search]]**           | Esplora tutti i nodi di un grafo a partire da un nodo iniziale, visitando prima tutti i nodi adiacenti a quello attuale in ordine di profondità.                                                                                           | <font color="#61AFEF">Iterazione</font>                   | $O(V + E)$                                                                                               |
| **[[015 Depth First Search\|Depth First Search]]**               | Esplora tutti i nodi di un grafo a partire da un nodo iniziale, visitando prima tutti i nodi figli del nodo attuale in profondità prima di passare ai nodi fratelli.                                                                       | <font color="#61AFEF">Iterazione</font>/<font color="#FF6611">Ricorsione</font>                                                           | $O(V + E)$                                                                                               |
| **[[019 Algoritmo di Dijkstra\|Algoritmo di Dijkstra]]**         | Trova il percorso più breve da un nodo di partenza a tutti gli altri nodi in un grafo con pesi sugli archi non negativi.                                                                                                                   | <font color="#00C575">Greedy</font>                                                               | $O(E\log V)$ utilizzando una coda di priorità con un heap                                                |
| **[[021 Algoritmo di Prim\|Algoritmo di Prim]]**                 | Costruisce un [[020 Minimo Sottografo Connesso Ricoprente\|albero di copertura minimo]] in un grafo non orientato pesato, selezionando ad ogni passo l'arco di costo minimo che connette un nodo del MST a uno fuori.                      | <font color="#00C575"><font color="#00C575">Greedy</font></font>                                  | $O(E\log V)$ utilizzando una coda di priorità con un heap                                                |
| **[[022 Algoritmo di Kruskal\|Algoritmo di Kruskal]]**           | Costruisce un [[020 Minimo Sottografo Connesso Ricoprente\|albero di copertura minimo]] in un grafo non orientato pesato, selezionando ad ogni passo l'arco di costo minimo che non forma cicli con gli archi precedentemente selezionati. | <font color="#00C575">Greedy</font>                                                               | $O(E\log V)$ utilizzando una struttura dati di union-find per il controllo dei cicli.                    |
| **[[024 Algoritmo di Bellman-Ford\|Algoritmo di Bellman-Ford]]** | Trova il percorso più breve da un nodo di partenza a tutti gli altri nodi in un grafo con pesi sugli archi, anche negativi.                                                                                                                | <font color="yellow">Programmazione Dinamica</font>                                               | $O(V\cdot E)$                                                                                            |
| **[[025 Tecnica Backtracking\|Backtracking]]**                   | Metodo di risoluzione di problemi che si basa su una strategia di ricerca in cui le soluzioni parziali sono costruite ed estese passo dopo passo.                                                                                          | <font color="#FF6611"><font color="#FF6611"><font color="#FF6611">Ricorsione</font></font></font> | La complessità può variare notevolmente a seconda dell'implementazione specifica e del problema risolto. |
| **[[026 Branch and Bound\|Branch and Bound]]**                   | Algoritmo di ricerca esaustiva che organizza lo spazio degli stati di un problema di ottimizzazione in un albero.                                                                                                                          | <font color="#FF6611">Ricorsione</font>                                                           | La complessità può variare notevolmente a seconda dell'implementazione specifica e del problema risolto. |

## ESERCIZI 5
### `GrafoCiclo(G)`
>Progettare ed analizzare un algoritmo che, prendendo in input un grafo non orientato $G = (V,E)$, determina se $G$ contiene o meno cicli. La complessità dell'algoritmo deve essere $O(|V|)$, indipendentemente da $|E|$.

```C
GrafoCiclo(G){
	for(ogni nodo v in V){
		scoperto[v] = false;
	}
	
	inserisci il nodo di partenza s alla coda Q;
	scoperto[s] = true;
	parent[s] = NULL;
	
	while(Q non è vuota){
		estrai il nodo v da Q;
		for(ogni nodo x adiacente ad v){
			if(scoperto[x] == false){
				scoperto[x] = true;
				parent[x] = v;
				inserisci il nodo x in Q;
			}
			else {
				if(parent[v] != x)
					return true;
			}
		}	
	}
	return false;
}
```

Questo algoritmo utilizza il BFS, durante la quale viene salvato il parent di ogni nodo. Se un nodo $x$ viene esplorato da un suo adiacente $v$, il parent di $x$ sarà $v$.
Se in una successiva esplorazione, due nodi già esplorati $i$ e $j$ sono adiacenti e notiamo che il parente di $i$ è diverso da $j$, allora c'è un ciclo.

La complessità è $O(|V|)$ perché l'algoritmo si ferma non appena trova un ciclo.

---
### `CamminoPassantePer(G, u, v, w)`
>Dato un grafo non diretto $G = (V,E)$, e tre vertici $u, v,w \in V$ , descrivere ed analizzare un algoritmo che ritorna `True` se esiste un cammino in $G$ che parte da $u$ ed arriva a $v$, passando per $w$, ritorna `False` altrimenti. Il cammino può anche contenere vertici ripetuti.

```C
CamminoPassantePer(G, u, v, w){
	for(ogni nodo v in V){
		scoperto[v] = false;
	}
	
	inserisci w in una coda Q;
	scoperto[w] = true;
	
	while(Q non è vuota){
		estrai il nodo x da Q;
		for(ogni nodo y adiacente ad x){
			if(scoperto[y] == false){
				scoperto[y] = true;
				inserisci il nodo y in Q;
			}
		}	
	}
	
	if(scoperto[u] == true && scoperto[v] == true){
		return true;
	} else {
		return false;
	}
}
```

Questo algoritmo utilizza il BFS, che esplora G in ampiezza a partire dal nodo $w$ e scopre tutti i cammini verso gli altri nodi. Essendo $G$ non diretto, se esiste un cammino da $w$ a $x$, allora lo si può percorrere al contrario per andare da $x$ a $w$. Quindi se $w$ riesce a raggiungere $u$ e riesce a raggiungere $v$, allora esiste un cammino passante per $w$.

![[Pasted image 20230609163617.png|350]]

BFS ha complessità $\Theta(|V|+|E|)$.

---
### `EsisteCamminoIncrociato(G, u, v)`
>Dato un grafo diretto $G = (V,E)$, e due vertici $u, v \in V$ , descrivere ed analizzare un algoritmo che ritorna `True` se esiste un cammino in $G$ che parte da $u$ ed arriva a $v$, e contemporaneamente esiste un cammino in $G$ che parte da $v$ ed arriva a $u$, ritorna `False` altrimenti.

```C
EsisteCamminoIncrociato(G, u, v){
	T1 = BFS(G, u);
	T2 = BFS(G, v);
	// Andrebbe anche bene il DFS, dato che non viene chiesto il cammino minimo
	if(v si trova in T1 && u si trova in T2)
		return true;
	else
		return false;
}
```

BFS($G$, $u$) esplora in ampiezza tutti i nodi a partire da $u$ e crea un albero $T1$ nel quale ci saranno tutti i nodi raggiungibili da $u$.
BFS($G$, $v$) esplora in ampiezza tutti i nodi a partire da $v$ e crea un albero $T2$ nel quale ci saranno tutti i nodi raggiungibili da $v$.

Se $v$ si trova nell'albero $T1$ e $u$ si trova nell'albero $T2$, allora esistono due cammini $u\to v$ e $v\to u$.

BFS ha complessità $\Theta(|V|+|E|)$.

---
### `TrovaPozzo(G)`
>![[Pasted image 20230609165221.png]]

Quindi, un nodo per essere un pozzo deve:
- avere nessun arco uscente;
- avere $n-1$ archi entranti.

![[Pasted image 20230609165905.png]]

Si cerca una riga $i$ con tutti $0$ e che abbia colonna $A_{ji}=1$ con $\forall j\neq i$ ($n-1$ nodi entranti).

```C
TrovaPozzo(G){
	n = |V|; // |V| è il numero di nodi in G
	i = 0;
	j = 0;

	// Ricerca di un nodo senza archi uscenti
	while(i < n && j < n){
		if(A[i][j] == 1){
			i++; // Prossima riga
			j = 0; // Resetta j
		}
		else
			j++; // Controlla prossima colonna
	}

	// Ogni nodo ha almeno un arco uscente
	if(i > n) 
		return false;

	// Controlla se ci sono n-1 archi entranti
	for(j = 0; j < n; j++){
		if(j != i && A[j][i] == 0)
			return false;
	}

	return true;
}
```

L'algoritmo analizza all'inizio la matrice di adiacenza e scarta i nodi che hanno un arco uscente. Se non sono stati trovati archi uscenti per un nodo, allora questo potrebbe essere un pozzo.
Si vede se nella colonna $i$-esima ci sono tutti $1$ tranne alla colonna $i$-esima. Se l'algoritmo raggiunge la fine del codice, allora il nodo $i$ è un pozzo.

I due cicli richiedono entrambi $\Theta(n)$, quindi la complessità totale è $\Theta(n)$.

---
### `PossibilitàRimozioneArco(G)`
![[Pasted image 20230609180544.png]]
Si può notare che si può rimuovere un arco $e$ e mantenere la connessione solamente se esiste un ciclo in $G$.

![[Pasted image 20230609180742.png|300]]

L'algoritmo è quindi praticamente lo stesso della [[#`GrafoCiclo(G)`|GrafoCiclo(G)]].


---
### Itera su BFS
>![[Pasted image 20230609175649.png|600]]

![[Pasted image 20230610100107.png|800]]

---
### Itera su DFS
>![[Pasted image 20230610100314.png|600]]

![[Pasted image 20230610102519.png|600]]

---
### Componenti Fortemente Connesse 1
![[Pasted image 20230610104541.png|800]]

[[012 Grafi#Componente Fortemente Connessa|↪Componenti Fortemente Connesse]]

1. Esegui DFS su $G$ su ogni nodo non esplorato. Mentre lo facciamo, salviamo per ogni nodo $v\in V$:
- $i(v)$ = istante nel quale viene scoperto il nodo $v$;
- $f(v)$ = istante nel quale finisce la ricorsione sul nodo $v$.

Cominciamo dal nodo $c$ all'istante 1. 
$c$ vede $d$ e $g$, ma entriamo in $d$ prima, all'istante 2.
$d$ vede $h$, ci entriamo all'istante 3.
$h$ non vede nessun'altro che non è stato già visitato, termina la ricorsione all'istante 4.
Torniamo a $d$, ma non vede nessun'altro che non è stato già visitato, termina la ricorsione all'istante 5.
Torniamo a $c$, entriamo in $g$ all'istante 6.
$g$ vede $f$, ci entriamo all'istante 7.
$f$ non vede nessun'altro che non è stato già visitato, termina la ricorsione all'istante 8.
Torniamo a $g$, ma non vede nessun'altro che non è stato già visitato, termina la ricorsione all'istante 9.
Torniamo a $c$, ma non vede nessun'altro che non è stato già visitato, termina la ricorsione all'istante 10.

A questo punto non abbiamo ancora visitato tutti i nodi del grafo, quindi utilizziamo la DFS sul nodo $b$ all'istante 11.
$b$ vede $e$, ci entriamo all'istante 12.
$e$ vede $a$, ci entriamo all'istante 13.
$a$ non vede nessun'altro che non è stato già visitato, termina la ricorsione all'istante 14.
Torniamo a $e$, ma non vede nessun'altro che non è stato già visitato, termina la ricorsione all'istante 15.
Torniamo a $b$, ma non vede nessun'altro che non è stato già visitato, termina la ricorsione all'istante 16.

Ora sono stati visitati tutti i nodi del grafo $G$. Questi sono gli alberi ottenuti:

![[Pasted image 20230610105055.png|500]]

2. Trova il reverse $G^R$ del grafo $G$: ![[Pasted image 20230610105940.png]]
3. Esegui DFS su $G^R$. Però adesso prendiamo in ordine *discendente*, un nodo con istante $f(v)$ più grande.

Il nodo $b$ ha l'istante di terminazione 16 tra i nodi non ancora visitati, quindi eseguiamo DFS su $b$: $b\to a\to c$. Questa è una componente fortemente connessa di $G$.

Il nodo $c$ ha l'istante di terminazione 10 tra i nodi non ancora visitati, quindi eseguiamo DFS su $c$: $c\to d$. Questa è una componente fortemente connessa di $G$.

Il nodo $g$ ha l'istante di terminazione 9 tra i nodi non ancora visitati, quindi eseguiamo DFS su $c$: $g\to f$. Questa è una componente fortemente connessa di $G$.

Il nodo $h$ ha l'istante di terminazione 4 tra i nodi non ancora visitati, quindi eseguiamo DFS su $h$, ma da questo non si possono raggiungere altri nodi non visitati. Questo nodo è una componente fortemente connessa di $G$.

![[Pasted image 20230610110924.png|500]]

---
### `CalcolaDiametroRaggio(G)`
> Si dia un algoritmo che, dato in input un grafo $G=(V, E)$ non diretto, calcoli $Diametro(G)$ e $Raggio(G)$.

![[Pasted image 20230610141453.png]]
<center>Grafo Esempio</center>

```C
CalcoloDiametroRaggio(G){
	// Calcolo delle distanze con BFS
	for(ogni nodo v in V){
		inizializza una coda Q;
		Q <- v;
		d_v[v] = 0;
		e(v) = 0;
		scoperto_v[v] = true;
		inizializza un albero T_v(V_v, E_v);

		while(Q != vuoto){
			estrai x da Q;
			for(ogni nodo y adiacente ad x){
				if(scoperto_v[y] == false){
					d_v[y] = d_v[x] + 1;
					Q <- y;
					T_v <- z, (x, z);

					// Calcola il massimo di e(v)
					if(e(v) < d_v[y]){
						e(v) = d_v[y];
					}
				}
			}
		}
	}
		
	// Controllo se il grafo è connesso, quindi va bene qualsiasi v in V
	if(V != V_v){
		return (infinito, infinito);
	}

	// Calcolo diametro e raggio
	diam = max{e(v) | v in V};
	ragg = min{e(v) | v in V};

	return (diam, ragg);
}
```

L'algoritmo esegue una BFS su ogni nodo $v\in V$ e salva per ogni nodo la sua eccentricità $e(v)$.
Viene controllato se il grafo è connesso e nel caso non lo fosse, ritorna $(\infty, \infty)$.
Calcola il valore massimo tra tutti gli $e(v)$ e il valore minimo.

BFS ha complessità $\Theta(|V|+|E|)$. Vengono eseguite $|V|$ BFS, quindi la complessità totale sarà $\Theta(|V|(|V|+|E|)=\Theta(|V|^2+|E|)$.

## ESERCIZI 6
### Dijkstra
![[Pasted image 20230612100937.png]]
```C
Dijkstra(G=(V,E), s){
	Visitati = {s};
	d[s] = 0;
	parent[s] = null;

	// V-Visitati (Insieme di nodi non visitati)
	for(ogni nodo x in V-Visitati){
		d[x] = infinito;
		parent[x] = NULL;
	}

	while(Visitati != V){
		curr = min(d[v], per ogni nodo v in V-Visitati); // Nodo non visitato con distanza minore
		for(ogni nodo u in V-Visitati adiacente a curr, con arco (curr, u)){
			temp_dist_u = d[curr] + peso_arco(curr, u);
			
			if(temp_dist_u < d[u]){
				d[u] = temp_dist_u;
				parent[u] = curr;
			}
		}

		Visitati <- curr;
	}
}
```

La correttezza dell'algoritmo si basa sul fatto che il cammino che va da $s$, passa per $w$ ed arriva ad $y$ sia quello di costo minore.

![[019 Algoritmo di Dijkstra#^d47736|ss]]

---
### Itera Dijkstra

![[Pasted image 20230612104110.png|800]]

![[Pasted image 20230612104042.png|700]]

---
### `CamminoMinimoUsaArco(G, (a, b))`
![[Pasted image 20230612110351.png|800]]

SOLO PUNTO 1:
Si aggiunge il seguente codice alla fine dell'algoritmo di Dijkstra scritto in codice [[#Dijkstra|qui sopra]].

```C
CamminoMinimoUsaArco(G, (a, b)){
	// ...

	for(ogni nodo x in V-Visitati){
		// ...
	}

	while(Visitati != V){
		// ...
	}

	// Check utilizzo arco dato
	if(parent[b] == a)
		return true;
	else
		return false;
}
```

---
### Prim
Scrivere l'algoritmo di Prim e descriverne il suo funzionamento.

```C
Prim(G, s){
	// Inserisci in una coda a PRIORITÀ MIN Q tutti i nodi in V;
	for(ogni nodo v in V)
		Q <- v;
	
	for(ogni nodo v in Q){
		d[v] = infinito;
		parent[v] = NULL;
	}

	d[s] = 0;
	Decrease_Key(Q, x, d[s]);

	while(Q non è vuota){
		u = Extract_Min(Q); // Rimuovi il nodo con d[...] minore
		for(ogni nodo x adiacente ad u){
			if(x in Q && costo(u, x) < d[x]){
				d[x] = costo(u, x);
				parent[x] = u;
				Decrease_Key(Q, x, d[x]); // Cambia la priorità
			}
		}
	}
}
```

L'algoritmo di Prim è simile a quello di Dijkstra, ma differisce nel calcolo della distanza dei nodi.

In prim la distanza viene calcolata a partire dal nodo parente a quello attuale.

%%
Tag Materia: #corsi/informatica/progettazione_algoritmi 
%%
