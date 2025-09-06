---
tags:
  - corsi/informatica/machine_learning
---
## Regressione Lineare Semplice
![[008 Task di Regressione#^5e8207|regressione lineare semplice]]

In questo caso possiamo solamente usare due variabili, dove il target è `Price`. Quindi scegliamo come variabile feature `SquareFeet`.

> [!tip] Import
>```python
>import pandas as pd
>from sklearn.model_selection import train_test_split
>from sklearn.linear_model import LinearRegression
>from sklearn.metrics import mean_squared_error
>import numpy as np
>```

1. **Caricamento del Dataset**:
```python
# https://www.kaggle.com/datasets/muhammadbinimran/housing-price-prediction-data
dataset = pd.read_csv("housing_price_dataset.csv")
```

Questa linea carica il dataset dal file CSV specificato nella variabile `dataset`.

2. **Selezione delle Caratteristiche e del Target**:
```python
X = dataset['SquareFeet']   # Caratteristica
y = dataset['Price']        # Target
```

Qui vengono selezionate le colonne `SquareFeet` come caratteristica (variabile indipendente) e `Price` come target (variabile dipendente).
È uno standard in ML chiamare la X in maiuscolo e la Y in minuscolo.

3. **Divisione del Dataset in Set di Addestramento e Test**:
```python
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
```
   
Questa linea divide il dataset in [[027 Perdita di Dati#Suddivisione Casuale dei Dati|set di addestramento]] (`X_train`, `y_train`) e [[027 Perdita di Dati#Suddivisione Casuale dei Dati|set di test]] (`X_test`, `y_test`). Il 20% del dataset viene utilizzato per il test, mentre l'80% rimanente viene utilizzato per l'addestramento del modello.

La parte `random_state` controlla la casualità della suddivisione del dataset in set di addestramento e test. Se si esegue lo script più volte con lo stesso valore di `random_state`, *si otterrà la stessa suddivisione del dataset* in set di addestramento e test. Questo è utile per confrontare i risultati tra diverse esecuzioni.

Per ottenere una suddivisione casuale, *omettere il parametro* `random_state`.

4. **Creazione del Modello di Regressione Lineare**:
```python
model = LinearRegression()
```
   Viene creato un oggetto di regressione lineare utilizzando la classe `LinearRegression` di scikit-learn.

5. **Addestramento del Modello**:
```python
model.fit(X_train, y_train)
```
   Il modello viene addestrato sui dati di addestramento utilizzando il metodo `fit`.

6. **Previsioni sul Set di Test**:
```python
y_pred = model.predict(X_test)
```
   Il modello viene utilizzato per fare previsioni sulla variabile target (`Y_pred`) utilizzando il set di test (`X_test`).

7. *Opzionale. Calcolo dell'Errore Quadratico Medio (MSE)*:
```python
mse = mean_squared_error(y_test, y_pred)
print(f'Errore quadratico medio: {mse}')
```
   L'errore quadratico medio tra le previsioni (`Y_pred`) e i valori effettivi (`Y_test`) viene calcolato e stampato.

8. **Previsione su nuovi dati**:
```python
new_data = pd.DataFrame({'SquareFeet': [2500]})
predicted_price = model.predict(new_data)
```
   Infine, il modello viene utilizzato per fare una previsione su nuovi dati, in questo caso, un valore `SquareFeet` di 2500.

9. *Opzionale. Arrotonda il risultato (oppure eseguire qualche altra operazione) e stampalo*:
```python
rounded_price = np.round(predicted_price[0], 2)  
print(f'Previsione del costo per SquareFeet = 2500: {rounded_price}')
```
   La funzione `round()` di python non funziona su un array di numpy, quindi si utilizza la `numpy.round()`.

> [!tip]- Codice
>```python
>import pandas as pd  
>from sklearn.model_selection import train_test_split  
>from sklearn.linear_model import LinearRegression  
>from sklearn.metrics import mean_squared_error  
>import numpy as np  
>  
># Carica il dataset  
># https://www.kaggle.com/datasets/muhammadbinimran/housing-price-prediction-data
>dataset = pd.read_csv("housing_price_dataset.csv")  
>  
># Seleziona le colonne SquareFeet e Price  
>X = dataset[['SquareFeet']]  
>y = dataset[['Price']]  
>  
># Dividi il dataset in set di addestramento e set di test  
>X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)  
>  
># Crea un modello di regressione lineare  
>model = LinearRegression()  
>  
># Addestra il modello  
>model.fit(X_train, y_train)  
>  
># Fai previsioni sul set di test  
>y_pred = model.predict(X_test)  
>  
># Calcola l'errore quadratico medio  
>mse = mean_squared_error(y_test, y_pred)  
>print(f'Errore quadratico medio: {mse}')  
>  
># Effettua una previsione su nuovi dati (ad esempio, SquareFeet = 2500)  
>new_data = pd.DataFrame({'SquareFeet': [2500]})  
>predicted_price = model.predict(new_data)  
>  
># Arrotonda il risultato e stampalo  
>rounded_price = np.round(predicted_price[0], 2)  
>print(f'Previsione del costo per SquareFeet = 2500: {rounded_price}')
>```

## Regressione Lineare Multipla
![[008 Task di Regressione#^efffdc|regressione lineare multipla]]

Il codice è molto simile a quello precedente.

1. In questo caso *non possiamo utilizzare la colonna `Neighborhood` perché contiene stringhe*. Quindi **rimuoviamo l'intera colonna**:
```python
# Rimuovi la colonna con le stringhe  
fixedDataset = dataset.drop('Neighborhood', axis=1)
```

Il metodo `drop()` di pandas rimuove una specifica colonna e ritorna il nuovo DataFrame. Il parametro `axis=1` indica che si sta rimuovendo una colonna.
Quindi, la variabile `fixedDataset` conterrà tutte le colonne del DataFrame originale tranne quella chiamata `Neighborhood`.

2. Si prendono come caratteristiche **tutte le colonne tranne `Price`**:
```python
# Seleziona tutte le variabili tranne 'Price' come caratteristiche (X)  
X = fixedDataset.drop('Price', axis=1)  
  
# Seleziona 'Price' come target (Y)  
y = fixedDataset['Price']
```

Queste colonne rimanenti costituiranno le variabili indipendenti che saranno utilizzate per addestrare il modello di regressione lineare multipla.

3. **Previsione sui nuovi dati**:
```python
new_data = pd.DataFrame({  
    'SquareFeet': [2500],
    'Bedrooms': [3], 
    'Bathrooms': [2],
    'YearBuilt': [2000]
})
predicted_price = model.predict(new_data)
```

> [!tip]- Codice
>```python
>import numpy as np  
>import pandas as pd  
>from sklearn.model_selection import train_test_split  
>from sklearn.linear_model import LinearRegression  
>from sklearn.metrics import mean_squared_error  
>  
># Carica il dataset  
># https://www.kaggle.com/datasets/muhammadbinimran/housing-price-prediction-data
>dataset = pd.read_csv("housing_price_dataset.csv")  
>  
># Rimuovi la colonna con le stringhe  
>fixedDataset = dataset.drop('Neighborhood', axis=1)  
>  
># Seleziona tutte le variabili tranne 'Price' come caratteristiche (X)  
>X = fixedDataset.drop('Price', axis=1)  
>  
># Seleziona 'Price' come target (y)  
>y = fixedDataset[['Price']]  
>  
># Dividi il dataset in set di addestramento e set di test  
>X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)  
>  
># Crea un modello di regressione lineare  
>model = LinearRegression()  
>  
># Addestra il modello  
>model.fit(X_train, y_train)  
>  
># Fai previsioni sul set di test  
>y_pred = model.predict(X_test)  
>  
># Calcola l'errore quadratico medio  
>mse = mean_squared_error(y_test, y_pred)  
>print(f'Errore quadratico medio: {mse}')  
>  
># Effettua una previsione su nuovi dati (ad esempio, SquareFeet=2500, Bedrooms=3, Bathrooms=2, YearBuilt=2000)  
>new_data = pd.DataFrame({  
>    'SquareFeet': [2500],  
>    'Bedrooms': [3],  
>    'Bathrooms': [2],  
>    'YearBuilt': [2000]  
>})  
>predicted_price = model.predict(new_data)  
>  
>rounded_price = np.round(predicted_price[0], 2)  
>print(f'Previsione del costo: {predicted_price[0]}')
>```



%%
[[000 Indice ML|↖ Ritorna all'indice ↖]]
%%