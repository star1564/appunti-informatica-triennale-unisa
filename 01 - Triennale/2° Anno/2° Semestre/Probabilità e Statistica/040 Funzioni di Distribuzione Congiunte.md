---
aliases:
  - Funzione di Distribuzione Marginale
  - Variabili Aleatorie Assolutamente Continue
  - Densità Discreta Congiunta
tags:
  - corsi/matematica/probabilità_statistica
inglese:
  - Joint probability distribution
paragrafo: Leggi Congiunte delle V.A.
cssclasses:
  - 
---
>Date due [[023 Variabili Aleatorie|Variabili Aleatorie]] $X$ e $Y$, la **[[024 Funzione di Distribuzione|Funzione di Distribuzione]] Congiunta** è usata per descrivere il comportamento di due variabili aleatorie *considerate insieme*.
>
>Considerando una variabile aleatoria doppia $(X,Y)$, la funzione di distribuzione congiunta è $$\color{#CC241D}F(x,y)=\mathbb{P}(X\leq x, Y\leq y)=\mathbb{P}(\{X\leq x\} \cap \{Y\leq y\})$$
>con $x,y\in\mathbb{R}$.


![[Pasted image 20240821173051.png|1100]]

%%
###### Codice
```python
import numpy as np  
import matplotlib.pyplot as plt  
from mpl_toolkits.mplot3d import Axes3D  
  
# Dati per il primo grafico  
x_values = np.array([1, 2, 3, 4, 5])  
y_values = np.array([1, 2, 3, 4, 5])  
joint_probabilities = np.array([  
    [0.05, 0.10, 0.05, 0.00, 0.00],  
    [0.10, 0.20, 0.10, 0.05, 0.00],  
    [0.05, 0.10, 0.10, 0.05, 0.00],  
    [0.00, 0.05, 0.05, 0.10, 0.05],  
    [0.00, 0.00, 0.00, 0.05, 0.10]  
])  
X, Y = np.meshgrid(x_values, y_values)  
  
# Dati per il secondo grafico  
x = np.linspace(0, 10, 100)  
y = np.linspace(0, 10, 100)  
X2, Y2 = np.meshgrid(x, y)  
Z = np.exp(-0.1 * ((X2 - 5)**2 + (Y2 - 5)**2))  
  
# Creare la figura e i due sotto-grafici  
fig = plt.figure(figsize=(17, 8))  
  
# Primo grafico  
ax1 = fig.add_subplot(121, projection='3d')  
ax1.bar3d(X.flatten(), Y.flatten(), np.zeros_like(X.flatten()), 0.6, 0.6, joint_probabilities.flatten(), shade=True, color='g')  
ax1.set_xlabel('X', color='blue', fontsize='15')  
ax1.set_ylabel('Y', color='red', fontsize='15')  
ax1.set_zlabel('Probabilità congiunta')  
ax1.set_title('Continua', fontsize='20')  
  
# Secondo grafico  
ax2 = fig.add_subplot(122, projection='3d')  
ax2.plot_surface(X2, Y2, Z, cmap='viridis')  
ax2.contour(X2, Y2, Z, zdir='x', offset=0, cmap='Reds')  
ax2.contour(X2, Y2, Z, zdir='y', offset=10, cmap='Blues')  
ax2.set_xlabel('X', color='blue', fontsize='15')  
ax2.set_ylabel('Y', color='red', fontsize='15')  
ax2.set_zlabel('Probabilità Congiunta')  
ax2.set_title('Discreta', fontsize='20')  
  
plt.tight_layout()  
plt.show()
```
###### End Codice
%%

### Funzioni Marginali

>Dalla funzione di distribuzione congiunta $F(x,y)$, è possibile derivare le funzioni di distribuzione **marginali** per ciascuna delle variabili $X$ e $Y$. Queste funzioni marginali descrivono il comportamento *di una sola variabile aleatoria*, ignorando l'altra.

$$\lim\limits_{y \to \infty}F(x,y)=\mathbb{P}(X\leq x, Y<\infty)=\mathbb{P}(X\leq x)=F_X(x)$$
$$\lim\limits_{x \to \infty}F(x,y)=\mathbb{P}(X\leq \infty, Y<y)=\mathbb{P}(Y\leq y)=F_Y(y)$$

> [!example]- <font color="orange">Esempio</font>
>Supponiamo che $X$ e $Y$ siano due variabili aleatorie continue, ciascuna con una [[037 Distribuzione Uniforme Continua#Funzione di Distribuzione|funzione di distribuzione uniforme]] su un intervallo $[0,1]$.
>
>![[Pasted image 20240821143521.png]]
>
>- Il grafico a sinistra mostra la superficie tridimensionale che rappresenta la probabilità congiunta $F(x,y)$. Si tratta di una funzione crescente rispetto a entrambi i parametri $x$ e $y$.
>
>- Il grafico a destra mostra come le probabilità cumulative si distribuiscono lungo i valori di $x$ e $y$ rispettivamente, ignorando l'altra variabile.

%% 
###### Codice
```python
import numpy as np  
import matplotlib.pyplot as plt  
from mpl_toolkits.mplot3d import Axes3D  
  
x = np.linspace(0, 1, 100)  
y = np.linspace(0, 1, 100)  
X, Y = np.meshgrid(x, y)  
F = X * Y
  
# Grafico della funzione di distribuzione congiunta  
fig = plt.figure(figsize=(12, 6))  
ax = fig.add_subplot(121, projection='3d')  
ax.plot_surface(X, Y, F, cmap='viridis', edgecolor='k')  
ax.set_title('Funzione di Distribuzione Congiunta $F(x,y)$')  
ax.set_xlabel('X')  
ax.set_ylabel('Y')  
ax.set_zlabel('$F(x,y)$')  
  
# Grafico delle funzioni di distribuzione marginali  
fig.add_subplot(122)  
plt.plot(x, x, label='$F_X(x)$ (Marginale di $X$)', linestyle='-', color='blue')
plt.plot(y, y, label='$F_Y(y)$ (Marginale di $Y$)', linestyle='--', color='orange')
plt.title('Funzioni di Distribuzione Marginali')  
plt.xlabel('$x$ o $y$')  
plt.ylabel('Probabilità cumulativa')  
plt.legend()  
plt.grid(True)  
  
plt.tight_layout()  
plt.show()
```
###### End Codice
%%

## Densità Discreta Congiunta
>Se $X$ e $Y$ sono variabili aleatorie [[025 Variabili Aleatorie Discrete|discrete]], allora la *[[025 Variabili Aleatorie Discrete#Densità Discreta|densità discreta]] congiunta* di $(X,Y)$ è $$\color{#CC241D}p(x,y)=\mathbb{P}(X=x, Y=y)=\mathbb{P}(\{X = x\} \cap \{Y= y\})$$

Dove:
- $p(x,y)\geq 0,\ \forall x,y$
- $\sum_x\sum_y p(x,y)=1$

La funzione di distribuzione congiunta di $(X,Y)$ è:
$$\color{#61AFEF}F(x,y) = \mathbb{P}(X\leq x, Y\leq y)=\sum_{u\ :\ u\leq x}\ \sum_{v\ :\ v\leq y} p(u,v)$$

### Densità Discrete Marginali
Le densità discrete **marginali** di $X$ e $Y$ si ottengono da $p(x,y)$ in questi modi:
$$p_X(x) = \mathbb{P}(X = x)=\sum_{i}\ p(x,y_{i})$$
$$p_Y(y) = \mathbb{P}(Y = y)=\sum_{i}\ p(x_{i},y)$$

Inoltre, se $X$ e $Y$ sono variabili aleatorie discrete, che assumono valori $\{x_1,x_2,\ldots,x_n\}$ e $\{y_1,y_2,\ldots,y_n\}$ rispettivamente, è possibile riportare i valori della densità discreta congiunta in una *tabella*. Dove ai margini, sono riportate le densità discrete di $X$ e $Y$ ricavate con le due formule di sopra.

![[Pasted image 20240822110812.png]]

> [!example]- <font color="orange">Esempio</font>
>>>Un esperimento consiste nello scegliere a caso una sequenza booleana di lunghezza 5, avente 2 bit pari a 1. Sia $X$ la posizione in cui si trova il secondo bit pari a 1 e sia $Y$ la lunghezza della più lunga sottosequenza composta da bit pari a 0. Ricavare la densità congiunta di $(X, Y)$ e le densità discrete di $X$ e $Y$ .
>
>Lo spazio campionario è costituito da $|S|={5\choose 2} = 10$ sequenze equiprobabili. Quindi si ha che:
>
>![[Pasted image 20240822111107.png]]
>
>Per calcolare $p(x,y)$ si vede se esiste una sequenza con $X=x$ e $Y=y$.
>- $p(2,1)=\mathbb{P}(X=2,Y=1)=\mathbb{P}(\{X= 2\} \cap \{Y= 1\})=\mathbb{P}(\emptyset)=0$ 
>- $p(2,3)=\mathbb{P}(X=2,Y=3)=\mathbb{P}(\{X= 2\} \cap \{Y= 3\})=\mathbb{P}(\{\text{1100}\})=1/10$  
>- $p(3,2)=\mathbb{P}(X=3,Y=2)=\mathbb{P}(\{X= 3\} \cap \{Y= 2\})=\mathbb{P}(\{\text{01100, 10100}\})=2/10$  
>
>Le densità marginali di $X$ e $Y$ si ottengono calcolando le somme sulle righe e sulle colonne:
>$$p_X(x)=\sum_y p(x,y),\quad p_Y(y)=\sum_x p(x,y)$$
>
>- $p_X(2)=0+0+\frac{1}{10}=\frac{1}{10}$
>- $p_X(3)=0+\frac{2}{10}+0=\frac{2}{10}$
>- $p_Y(2)=0+\frac{2}{10}+\frac{2}{10}+\frac{2}{10}=\frac{6}{10}$

## Variabili Aleatorie Assolutamente Continue
>Diciamo che due variabili aleatorie $X$ e $Y$ sono **congiuntamente (assolutamente) [[034 Variabili Aleatorie Continue|continue]]** se esiste una funzione $f(x,y)$ integrabile, tale che per ogni sottoinsieme $\mathcal{C}$ dello spazio delle coppie di numeri reali risulti che:
>$$\mathbb{P}((X,Y)\in \mathcal{C})=\int\int_{(x,y)\in \mathcal{C}} f(x,y)\ dx\ dy$$

La funzione $f$ è detta *[[034 Variabili Aleatorie Continue#Funzione di Densità|funzione di densità]] congiunta* di $X$ e $Y$. Se $\mathcal{A}$ e $\mathcal{B}$ sono una qualsiasi coppi di sottoinsiemi di $\mathbb{R}$, allora per $\mathcal{C}=\{(x,y : x\in \mathcal{A}, y\in \mathcal{B}\}$ otteniamo che $$\mathbb{P}(X\in\mathcal{A}, Y\in\mathcal{B})=\int_\mathcal{B}\int_\mathcal{A} f(x,y)\ dx\ dy$$


___
[[000 Indice PS|↖ Ritorna all'indice ↖]]

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