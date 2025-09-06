---
tags:
  - corsi/informatica/machine_learning
---
[Python](https://www.python.org/) è un linguaggio interpretato, ad alto livello e versatile. È consigliabile l'utilizzo dell'IDE [Pycharm](https://www.jetbrains.com/pycharm/).

## Sintassi di Base
### Commento

```python
# Questo è un commento

"""
Questo è un commento
su più linee
"""
```

### Dichiarazione delle Variabili
In Python, non è necessario dichiarare esplicitamente il tipo di variabile.

```python
nome = "My Name"
eta = 21
```

### Strutture di Controllo
Le strutture di controllo come `if`, gli `else if` e gli `else` hanno una sintassi simile, ma **Python utilizza l'indentazione per definire i blocchi di codice**, a differenza delle parentesi graffe in Java e C.

```python
if condizione:
    # codice
elif altra_condizione:
    # altro codice
else:
    # codice nel caso in cui nessuna condizione sia vera
```

### Cicli
Anche nei cicli si utilizza l'indentazione.

```python
for elemento in sequenza:
    # codice

while condizione:
	# codice
```

### Funzioni
Le funzioni possono essere create tramite la parola chiave **`def`**.

```python
def somma(a, b):
    return a + b

# Chiamata
risultato = somma(3, 5)
```

## Tipi
I tipi principali in Python sono:
- **Tipi Numerici**
	- *Interi* (`42`)
	- *Virgola Mobile* (`3.14`)
	- *Complessi* (`2 + 3j`)
- **Tipi di Sequenza**
	- *Stringhe* (`"Hello"`)
	- *Altri Contenitori* (liste, array, ecc.)
- **Booleani** (`True`, `"False"`)
- **Nullo** (`None`)

### Conversione di tipi
È possibile convertire tipi attraverso le funzioni della libreria standard.

```python
intero = int("42")  # Converti una stringa in un intero
testo = str(3.14)   # Converti un numero in una stringa
virgola_mobile = float("57.2")   # Converti un numero in un float
```

### Ottenere il tipo della variabile
Si utilizza la funzione `type()`.

```python
print(type(intero))          # Output: <class 'int'>
print(type(testo))           # Output: <class 'str'>
print(type(virgola_mobile))  # Output: <class 'float'>
```

## Stringhe
In Python, le stringhe sono sequenze di caratteri e vengono manipolate con diverse funzioni integrate.

```python
# Creazione Stringhe
stringa1 = "Hello"
stringa2 = 'World'
stringa3 = """Una stringa
su più
righe"""

# Concatenazione di Stringhe
saluto = stringa1 + " " + stringa2

# Ottiene un carattere dato un indice
primo_carattere = stringa1[0]

# Ottiene la lunghezza della stringa (intero)
lunghezza = len(saluto)

maiuscole = saluto.upper()  # Converte la stringa in maiuscolo
minuscole = saluto.lower()  # Converte la stringa in minuscolo
```

### Slicing delle Stringhe
È possibile ottenere dei caratteri a partire da una posizione (*inclusa*) ad un altra posizione (*esclusa*).

```python
# Ottiene i caratteri dalla posizione 1 (inclusa) alla posizione 4 (esclusa)
sottostringa = saluto[1:4]
```
### Formatted String
Le formatted string, indicate dall'uso del prefisso `f` prima delle virgolette, sono un modo potente e leggibile per *incorporare variabili all'interno di stringhe*.

```python
nome = "Alice"
eta = 25

presentazione = f"Ciao, mi chiamo {nome} e ho {eta} anni."

# Output: Ciao, mi chiamo Alice e ho 25 anni.
```

### Altre Funzioni Utili
```python
# Conta quante volte appare la lettera 'l' nella stringa
conteggio = saluto.count('l') 

# Restituisce la posizione della prima occorrenza della lettera 'e'
posizione = saluto.find('e')   

# Sostituisci un carattere o una sottostringa
nuova_stringa = stringa1.replace('l', 'L')
```

## Librerie IO Standard
Python ha una vasta librerie standard che semplificano molte attività comuni.
### Print
Stampa sul terminale una variabile.

```python
print("Hello, World!")

numero = 1
print(numero)
```

### Input
In Python, la funzione `input()` viene utilizzata per acquisire l'input dell'utente da tastiera. La sintassi di base è semplice:

```python
nome = input("Inserisci il tuo nome: ")
print("Ciao, " + nome + "!")
```

La funzione `input()` **restituisce una stringa**, che rappresenta l'input immesso dall'utente. L'argomento opzionale fornito alla funzione `input()` è una stringa di prompt che viene visualizzata all'utente prima che questi inserisca i dati.

Se è necessario trattare l'input come un numero intero o in virgola mobile, sarà necessario convertirlo utilizzando le funzioni `int()` o `float()`, ad esempio:

```python
eta = int(input("Inserisci la tua età: "))
```

%%
[[000 Indice ML|↖ Ritorna all'indice ↖]]
%%