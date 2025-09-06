---
tags:
  - corsi/informatica/machine_learning
---
> [!tip] Import
>```python
>from sklearn.tree import DecisionTreeClassifier
>```

1. **Caricamento del dataset CSV**:
```python
# https://www.kaggle.com/datasets/uciml/iris  
data = pd.read_csv('Iris.csv')  
data = data.drop(columns=['Id'], axis=1)
```

2. **Preparazione dei dati**:
```python
# Creazione delle variabili dipendenti e indipendenti
X = data.drop('Species', axis=1)
y = data['Species']

# Suddivisione del dataset in set di addestramento e test
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
```

5. **Creazione del Decision Tree Classifier**:
```python
# Creazione dell'istanza del Decision Tree Classifier
dt_classifier = DecisionTreeClassifier(random_state=42)

# Addestramento del modello
dt_classifier.fit(X_train, y_train)

# Predizione sui dati di test
y_pred = dt_classifier.predict(X_test)
```

6. *Opzionale. Valutazione del modello*:
```python
from sklearn.metrics import classification_report

# Stampa del report di classificazione
classification_rep = classification_report(y_test, y_pred)
print("\nReport di classificazione:")
print(classification_rep)
```

7. **Predizione sui nuovi dati**:

```python
# Crea nuovi dati
new_data = pd.DataFrame({  
    'SepalLengthCm': [4],  
    'SepalWidthCm': [3.5],  
    'PetalLengthCm': [1.2],  
    'PetalWidthCm': [0.1]  
})  

# Effettua la predizione  
prediction = dt_classifier.predict(new_data)  
print(prediction[0])
```

> [!tip]- Codice
>```python
>import pandas as pd  
>from sklearn.model_selection import train_test_split  
>from sklearn.tree import DecisionTreeClassifier  
>from sklearn.metrics import classification_report  
>  
># https://www.kaggle.com/datasets/uciml/iris  
>data = pd.read_csv('Iris.csv')  
>data = data.drop(columns=['Id'], axis=1)  
>  
># Creazione delle variabili dipendenti e indipendenti  
>X = data.drop('Species', axis=1)  
>y = data['Species']  
>  
># Suddivisione del dataset in set di addestramento e test  
>X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)  
>  
># Creazione dell'istanza del Decision Tree Classifier  
>dt_classifier = DecisionTreeClassifier(random_state=42)  
>  
># Addestramento del modello  
>dt_classifier.fit(X_train, y_train)  
>  
># Predizione sui dati di test  
>y_pred = dt_classifier.predict(X_test)  
>  
># Stampa del report di classificazione  
>classification_rep = classification_report(y_test, y_pred)  
>print("\nReport di classificazione:")  
>print(classification_rep)  
>
># Crea nuovi dati
>new_data = pd.DataFrame({  
>    'SepalLengthCm': [4],  
>    'SepalWidthCm': [3.5],  
>    'PetalLengthCm': [1.2],  
>    'PetalWidthCm': [0.1]  
>})
>
># Effettua la predizione  
>prediction = dt_classifier.predict(new_data)  
>print(prediction[0])
>```


%%
[[000 Indice ML|↖ Ritorna all'indice ↖]]
%%
