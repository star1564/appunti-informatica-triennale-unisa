---
tags:
  - corsi/informatica/machine_learning
---
## Utilizzo pacchetti
I pacchetti di Python sono delle estensioni opzionali.

### Installazione pacchetti
Possono essere installati in due modi:
- Dal terminale
	- `pip install nome-pacchetto`
	- Oppure, se ci sono errori `python -m pip install nome-pacchetto`
- Dall'IDE ![[Pasted image 20240108133626.png]]

### Importazione nel programma
All'inizio del file si può importare una libreria con `import` e gli si può dare un nome personalizzato con `as`.

```python
import nomePacchetto

nomePacchetto.faiQualcosa()
```

```python
import nomePacchetto as pac

pac.faiQualcosa()
```

## NumPy
NumPy è una libreria per il *calcolo scientifico* in Python. Essa fornisce un supporto per **array multidimensionali** e **funzioni matematiche** adatte a operazioni complesse su dati. NumPy è ampiamente utilizzato in settori come l'analisi dei dati, il machine learning e la simulazione scientifica.

> [!tip] Installazione
>```bash
>pip install numpy
>```

### Utilizzo di NumPy

```python
# Importa NumPy
import numpy as np

# Creazione di un array
arr = np.array([1, 2, 3, 4, 5])

# Operazioni sugli array
arr_squared = arr ** 2
arr_sum = np.sum(arr)

# Creazione di array multidimensionali
matrix = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])

# Accesso agli elementi dell'array multidimensionale
element = matrix[1, 2]  # Ottiene l'elemento nella seconda riga e terza colonna

# Operazioni su array multidimensionali
matrix_transposed = np.transpose(matrix)
matrix_det = np.linalg.det(matrix)
```

### Funzioni statistiche
NumPy offre un ampio insieme di funzioni per eseguire operazioni statistiche su array multidimensionali.

| Funzione                                      | NumPy            |
| --------------------------------------------- | ---------------- |
| Minimo                                        | `numpy.min()`    |
| Massimo                                       | `numpy.max()`    |
| [[Media]]                                     | `numpy.mean()`   |
| [[Mediana]]                                   | `numpy.median()` |
| Derivazione Standard                          | `numpy.std()`    |
| [[036 Varianza di una VA Continua\|Varianza]] | `numpy.var()`    |

```python
import numpy as np

# Generate random number from normal distribution
normal_array = np.random.normal(5, 0.5, 10)

# Print content of normal_array
print(f"normal_array content: {normal_array}")

# Min
print(f"normal_array min: {np.min(normal_array)}")

# Max
print(f"normal_array max: {np.max(normal_array)}")

# Mean
print(f"normal_array mean: {np.mean(normal_array)}")

# Median
print(f"normal_array median: {np.median(normal_array)}")

# Standard Deviation
print(f"normal_array deviation: {np.std(normal_array)}")
```

## SciPy
SciPy è una libreria open-source per Python che fornisce funzioni specializzate per *problemi scientifici e tecnici*. Essa si basa su NumPy e fornisce funzionalità aggiuntive che sono spesso utili in ambiti quali ottimizzazione, **algebra lineare**, **statistica**, analisi dei segnali e altro ancora.

> [!tip] Installazione
>```bash
>pip install numpy
>```

### Utilizzo di SciPy in statistica
[[007 Combinazioni|Combinazioni]], [[003 Permutazioni|Permutazioni]].

```python
from scipy.special import comb
from scipy.special import perm

#Find combinations of 5, 2 values using comb(N, k)
com = comb(5, 2, exact = False, repetition=True)

#Print content of com
print(f"Combination value: {com}")

#Find permutation of 5, 2 values using perm(N, k)
per = perm(5, 2, exact = True)

#Print content of per
print(f"Permutation value: {per}")
```

## Pandas
Pandas è una libreria open-source per Python progettata per facilitare la *manipolazione e l'analisi dei dati*. Essa fornisce strutture dati flessibili e ad alte prestazioni, progettate per lavorare in modo efficiente con dati strutturati o etichettati. Due delle principali strutture dati di Pandas sono le **Serie** e i **DataFrame**.

> [!tip] Installazione
>```bash
>pip install pandas
>```

### Serie
Una **Serie** è una struttura dati unidimensionale simile a un array o una lista, ma con *etichette* (indici) associate a ciascun elemento.
Quando viene stampata, la Serie stampa l'indice con il valore ed il tipo che si trova nella serie.

Una serie non può avere più colonne

```python
import pandas as pd

# Definition of a series of floats
serie_1 = pd.Series([1., 2., 3.])

# Print content of serie_1 with index
print("serie_1:")
print(serie_1)

# Definition of a series of floats with custom indexes
serie_2 = pd.Series([1., 2., 3.], index=['a', 'b', 'c'])

# Print content of serie_2
print("serie_2:")
print(serie_2)
```

![[Pasted image 20240108140123.png|190]]
### DataFrame
Un **DataFrame** è una struttura dati *bidimensionale* simile a un foglio di calcolo o a una tabella [[023 Definizioni SQL|SQL]]. Può essere visto come un *contenitore di Serie*, ognuna delle quali può contenere dati di tipo diverso.

```python
import pandas as pd  
  
# Creazione di un DataFrame da un dizionario  
data = {
	'Nome': ['Alice', 'Bob', 'Charlie'],  
	'Età': [25, 30, 35],  
	'Città': ['Roma', 'Milano', 'Napoli']
}
  
df = pd.DataFrame(data)  
  
print("df completo:")  
print(df)  
  
print("\nAccesso ai nomi:")  
print(df['Nome'])
```

![[Pasted image 20240108141046.png]]
### Lettura e Scrittura di Dati

Pandas supporta la lettura e la scrittura di dati in vari formati come [[010 Fonti e Formati dei dati#^04aac2|CSV]], Excel, SQL, e altro ancora.

```python
import pandas as pd

# Lettura di un file CSV
df = pd.read_csv('dati.csv')

# Scrittura in un file Excel
df.to_excel('output.xlsx', index=False)
```

### Operazioni di Selezione e Filtraggio

```python
# Selezione basata su condizioni
df_filtrato = df[df['Età'] > 30]

# Utilizzo del metodo loc per l'accesso basato sugli indici
df.loc[df['Città'] == 'Roma', 'Nome']
```

### Operazioni Statistiche e Aggregazioni

```python
# Calcolo della media per una colonna
media_età = df['Età'].mean()

# Gruppo per una colonna e calcolo di statistiche
statistiche_per_città = df.groupby('Città').agg({'Età': ['mean', 'min', 'max']})
```

## Matplotlib
Matplotlib è una libreria di *visualizzazione di dati* in Python. Essa offre una vasta gamma di opzioni per creare **grafici**, **diagrammi** e **visualizzazioni di dati**. Matplotlib è spesso utilizzato insieme a NumPy, SciPy e Pandas per visualizzare dati in modo efficace.

> [!tip] Installazione
>```bash
>pip install matplotlib
>```

### Creazione di un Grafico Lineare

```python
import matplotlib.pyplot as plt  
import matplotlib.style as style  
  
# Usa uno stile  
style.use('ggplot')  
  
# Dati linea 1  
x1 = [5, 8, 10]  
y1 = [12, 16, 6]  
  
# Dati linea 2  
x2 = [6, 9, 11]  
y2 = [6, 15, 7]  
  
# Creazione di un grafico lineare con due linee  
plt.plot(x1, y1)  
plt.plot(x2, y2, linewidth=5)  
  
# Aggiunta di etichette agli assi  
plt.xlabel('Asse X')  
plt.ylabel('Asse Y')  
  
# Aggiunta di un titolo al grafico  
plt.title('Grafico Lineare')  
  
# Visualizzazione del grafico  
plt.show()
```

![[Pasted image 20240108142423.png]]
### Creazione di un Diagramma a Barre

```python
import matplotlib.pyplot as plt

# Dati
categorie = ['A', 'B', 'C', 'D']
valori = [3, 7, 2, 5]

# Creazione di un diagramma a barre
plt.bar(categorie, valori, color='blue')

# Aggiunta di etichette agli assi
plt.xlabel('Categorie')
plt.ylabel('Valori')

# Aggiunta di un titolo al grafico
plt.title('Diagramma a Barre')

# Visualizzazione del grafico
plt.show()
```

![[Pasted image 20240108141757.png]]

### Plottaggio di punti

```python
import matplotlib.pyplot as plt
import numpy as np

# Dati
x = np.random.rand(50)
y = np.random.rand(50)

# Creazione di un grafico a dispersione
plt.scatter(x, y, color='red', marker='o', label='Punti casuali')

# Aggiunta di etichette agli assi
plt.xlabel('Asse X')
plt.ylabel('Asse Y')

# Aggiunta di un titolo al grafico
plt.title('Grafico a Dispersione')

# Aggiunta di una legenda
plt.legend()

# Visualizzazione del grafico
plt.show()
```

![[Pasted image 20240108142653.png]]




%%
[[000 Indice ML|↖ Ritorna all'indice ↖]]
%%