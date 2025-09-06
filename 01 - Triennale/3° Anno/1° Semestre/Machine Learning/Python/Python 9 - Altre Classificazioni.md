---
tags:
  - corsi/informatica/machine_learning
---
## K-Neighbors Classifier
> [!tip] Import
>```python
>from sklearn.neighbors import KNeighborsClassifier
>```

```python
model_knn = KNeighborsClassifier(n_neighbors=7)  
model_knn.fit(X_train, y_train)  
y_pred_knn = model_knn.predict(X_test)
```

## Random Forest
> [!tip] Import
>```python
>from sklearn.ensemble import RandomForestClassifier
>```

```python
model_rf = RandomForestClassifier(n_estimators=30, random_state=42)  
model_rf.fit(X_train, y_train)  
y_pred_rf = model_rf.predict(X_test)
```

%%
[[000 Indice ML|↖ Ritorna all'indice ↖]]
%%
