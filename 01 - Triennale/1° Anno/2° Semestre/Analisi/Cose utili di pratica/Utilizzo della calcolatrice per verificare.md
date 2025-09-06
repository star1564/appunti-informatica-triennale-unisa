---
tags:
  - esercizi
  - corsi/matematica/analisi
---
> [!note] Calcolatrice usata
> [Casio FX-570ES Plus](https://www.manualeduso.it/casio/fx-570es-plus/manuale)

> [!warning] La calcolatrice non mostra i calcoli eseguiti, ma solo il risultato
> Usare questo risultato per verificare le soluzioni trovate.


## Cose Generali
### Modalità
Il menù delle modalità si apre con `MODE`. Per Analisi, sono utili tre modalità:
- `COMP` (1) - Calcolatrice normale
- `CMPLX` (2) - Numeri complessi
- `EQN` (5) - Equazioni

### Variabili
La calcolatrice dispone di otto variabili preimpostate A, B, C, D, E, F, X, e Y. 
È possibile assegnare valori alle variabili e anche utilizzarle nei calcoli.

![[Pasted image 20250213115629.png]]

### Solve
`SOLVE` utilizza la legge di Newton per approssimare la soluzione di equazioni. 
Può essere utilizzato solo in modalità `COMP`.

Ad esempio, per $\frac{X+6}{X^2+2X-1}$ (premendo `SHIFT` + `SOLVE` dopo averla inserita), chiede qual è il valore della $X$.


## Equazioni
Passare in modalità `EQN` e selezionare il tipo di equazione:
- $anX+bnY=cn\iff ax+by=c$
- $anX+bnY+cnZ=dn\iff ax+by+cz=d$
- $aX^2+bX+c=0\iff ax^2+bx+c=0$
- $aX^3+bX^2+cX+d=0\iff ax^3+bx^2+cx+d=0$

Inserire i coefficienti nella tabella e premere ` =`  per la risoluzione. In caso di più risultati, come $x_1, x_2$, è possibile scorrerli con le frecce su e giù.


> [!tip] Disequazioni
> È possibile riutilizzare questa modalità anche per calcolare delle disequazioni, in quanto queste sono delle equazioni.
> 
> Bisogna ovviamente considerare anche il segno della soluzione.


## Derivate
Passare in modalità `COMP`.
Premere `SHIFT` + `Tasto Integrale` ed inserire come $f(x)$ la funzione da derivare, mentre per $x=\_$ inserire $0$.
Verificare se il risultato della calcolatrice è uguale alla derivata ottenuta quando $x=0$.


## Numeri Complessi
Passare in modalità `CMPLX`.
È possibile utilizzare sia la forma aritmetica $(a+bi)$ che quella trigonometrica $(\rho\ \angle\ \theta)$ per introdurre numeri complessi. Per inserire la $i$ si preme `ENG`.

È utile impostare i gradi in radianti: 
- `SHIFT` + `MODE` (SETUP)
- `Rad` per passare ai radianti
- `Deg` per passare ai gradi

> [!note] Forma trigonometrica
> $$\rho\ \angle\ \theta \iff \rho[\cos(\theta)+i \sin(\theta)]$$


Si possono eseguire tutte le operazioni tra numeri complessi, tra cui: addizione, sottrazione, divisione, moltiplicazione, coniugato

I risultati dei calcoli di numeri complessi vengono visualizzati in base all'impostazione del formato del numero complesso nel menu di impostazione. È anche possibile convertire da una forma all'altra avendo come risultato un numero complesso e premendo `SHIFT` + `2` (CMPLX) e selezionare il formato voluto (aritmetico=4, trigonometrico=3, coniugato=2).