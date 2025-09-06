---
tags:
  - corsi/informatica/machine_learning
---
> [!success] Video consigliati
>- [One Hot Encoder - Ryan Nolan Data](https://www.youtube.com/watch?v=rsyrZnZ8J2o)
>- [Ordinal Encoder - Ryan Nolan Data](https://www.youtube.com/watch?v=15uClAVV-rI)


Ci sono due tipi di encoder.

## Ordinal Encoder
L'Ordinal Encoder è una tecnica particolarmente utile quando si trattano variabili *categoriche ordinate*, dove l'ordine tra le categorie ha un significato.

![[Pasted image 20240109174333.png|600]]

> [!tip] Import
>```python
>from sklearn.preprocessing import OrdinalEncoder
>```

Supponiamo di avere un dataset con una colonna categorica ordinata "Size": `Small`, `Medium`, `Large`

```python
# Ottieni il dataset dal csv
dataset = pd.read_csv("my_dataset.csv")

# Small, Medium, Large
sizes = dataset['size'].unique() 
```

Si inizializza l'encoder e si trasformano i dati.

```python
# Crea l'encoder
encoder = OrdinalEncoder(categories=[sizes])

# Modifica il dataset originale con la colonna codificata
dataset['size'] = encoder.fit_transform(dataset[['size']])
```

> [!tip]- Codice
>```python
>import pandas as pd  
>from sklearn.preprocessing import OrdinalEncoder  
>from sklearn.model_selection import train_test_split  
>from sklearn.linear_model import LinearRegression  
>  
># Ottieni il dataset dal csv  
># https://www.kaggle.com/datasets/muhammadbinimran/housing-price-prediction-data
>dataset = pd.read_csv("housing_price_dataset.csv")  
>  
># Ottieni tutti i valori di 'Neighborhood'  
>neighborhoods = dataset['Neighborhood'].unique()  
>  
># Effettua l'encoding  
>encoder = OrdinalEncoder(categories=[neighborhoods])  
>  
># Modifica il dataset originale con la colonna codificata  
>dataset['Neighborhood'] = encoder.fit_transform(dataset[['Neighborhood']])  
>  
># Seleziona tutte le variabili tranne 'Price' come caratteristiche (X)  
>X = dataset.drop('Price', axis=1)  
>  
># Seleziona 'Price' come target (y)  
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
># Effettua una previsione su nuovi dati (ad esempio, SquareFeet=2500, Bedrooms=3, Bathrooms=2, YearBuilt=2000)  
>new_data = pd.DataFrame({  
>    'SquareFeet': [1500],  
>    'Bedrooms': [1],  
>    'Bathrooms': [1],  
>    'Neighborhood': [2],  
>    'YearBuilt': [2020]  
>})  
>predicted_price = model.predict(new_data)  
>  
>print(f'Previsione del costo per \n{new_data}\n{predicted_price[0]}')
>```

## One-Hot Encoder
Il One-Hot Encoder è un'altra tecnica, adatta per variabili categoriche *senza un ordine* intrinseco.

![[Pasted image 20240109181853.png|800]]

> [!tip] Import
>```python
>from sklearn.preprocessing import OneHotEncoder
>```

Supponiamo di avere un dataset con una colonna categorica non ordinata, ad esempio una variabile "City" con categorie come `Jacksonville`, `Miami`, `Orlando` e `Tampa`.

```python
# Ottieni il dataset dal csv  
dataset = pd.read_csv("my_dataset.csv")

# Crea l'encoder
encoder = OneHotEncoder(handle_unknown='ignore', sparse_output=False).set_output(transform='pandas')

# Crea il dataframe modificato, questo dataset contiene solamente le nuove colonne
modified_data = encoder.fit_transform(dataset[['city']])
```

Nell'encoder, si sono aggiunti dei parametri riguardo al suo comportamento:
- Se ci sono valori sconosciuti, si ignorano
- Si trasforma il risultato in un array di pandas. `sparse_output` aiuta con la trasformazione.

![[Pasted image 20240109180648.png]]

Modifichiamo adesso il dataset originale, andando ad aggiungere le nuove colonne e rimuovendo la colonna city.

```python
dataset = pd.concat([dataset, modified_data], axis=1).drop(columns=['city'])
```

> [!tip]- Codice
>```python
>import pandas as pd  
>from sklearn.preprocessing import OneHotEncoder  
>from sklearn.model_selection import train_test_split  
>from sklearn.linear_model import LinearRegression  
>  
># Ottieni il dataset dal csv
># https://www.kaggle.com/datasets/muhammadbinimran/housing-price-prediction-data
>dataset = pd.read_csv("housing_price_dataset.csv")  
>  
># Effettua l'encoding  
>encoder = OneHotEncoder(handle_unknown='ignore', sparse_output=False).set_output(transform='pandas')  
>  
># Crea il dataframe modificato  
>modified_data = encoder.fit_transform(dataset[['Neighborhood']])  
>  
># Modifica il dataset originale con la colonna codificata  
>dataset = pd.concat([dataset, modified_data], axis=1).drop(columns=['Neighborhood'])  
>  
># Seleziona tutte le variabili tranne 'Price' come caratteristiche (X)  
>X = dataset.drop('Price', axis=1)  
>  
># Seleziona 'Price' come target (y)  
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
># Effettua una previsione su nuovi dati (ad esempio, SquareFeet=2500, Bedrooms=3, Bathrooms=2, YearBuilt=2000)  
>new_data = pd.DataFrame({  
>    'SquareFeet': [1500],  
>    'Bedrooms': [1],  
>    'Bathrooms': [1],  
>    'YearBuilt': [2020],  
>    'Neighborhood_Rural': [0],  
>    'Neighborhood_Suburb': [1],  
>    'Neighborhood_Urban': [0]  
>})  
>predicted_price = model.predict(new_data)  
>  
>print(f'Previsione del costo per \n{new_data}\n{predicted_price[0]}')
>```


%%
[[000 Indice ML|↖ Ritorna all'indice ↖]]
%%