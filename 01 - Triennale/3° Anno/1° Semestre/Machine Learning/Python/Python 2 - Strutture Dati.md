---
tags:
  - corsi/informatica/machine_learning
---
Le strutture dati in Python forniscono modi efficaci per organizzare e manipolare dati.

### Verifica di Appartenenza
```python
elemento_presente = 4 in contenitore     # Restituisce True se l'elemento è presente nel contenitore
elemento_assente = 8 not in contenitore  # Restituisce True se l'elemento è assente nel contenitore
```

## Liste
Le liste in Python sono *sequenze di elementi modificabili*. Si possono aggiungere, rimuovere, modificare e ottenere elementi.

Si creano con le parentesi quadre.

```python
# Creazione di una lista
lista = [1, 2, 3, 4, 5]

# Aggiungi elemento
lista.append(6)

# Modifica elemento
lista[2] = 10

# Ottieni elemento
elemento = lista[3]

# Rimuovi l'elemento 4
lista.remove(4)

# Rimuovi l'elemento nella posizione 2
lista.pop(2)
```

## Tuple
Le tuple sono *sequenze immutabili* di elementi. Non è possibile modificarle direttamente.

Si creano con le parentesi tonde.

```python
# Creazione di una tupla
tupla = (1, 'due', 3.0)

# Ottieni elemento
elemento = tupla[1]
```

## Dizionari
I dizionari sono strutture *chiave-valore*, ideali per il recupero efficiente dei dati tramite una chiave unica.

Si creano con le parentesi graffe.

```python
# Creazione di un dizionario
dizionario = {
	'chiave1': 'valore1', 
	'chiave2': 'valore2'
}

# Aggiungi/Modifica elemento
dizionario['chiave3'] = 'valore3'

# Ottieni elemento
valore = dizionario['chiave2']

# Rimuovi elemento
del dizionario['chiave1']
```

## Insiemi (Set) in Python
Gli [[001 Insieme|insiemi]] in Python sono collezioni di elementi unici, senza un ordine definito. Gli insiemi sono molto utili quando si vogliono gestire elementi unici o eseguire operazioni di insieme come unione, intersezione, differenza, ecc.

```python
insieme1 = {1, 2, 3, 4, 5}
insieme2 = set([3, 4, 5, 6, 7])
insieme_vuoto = set()  # Crea un insieme vuoto

insieme1.add(6)  # Aggiunge l'elemento 6 all'insieme
insieme1.remove(3)  # Rimuove l'elemento 3 dall'insieme

unione = insieme1.union(insieme2)  # Unione di due insiemi
intersezione = insieme1.intersection(insieme2)  # Intersezione di due insiemi
differenza = insieme1.difference(insieme2)  # Differenza tra due insiemi

insieme1.discard(2)  # Rimuove l'elemento 2 se presente, altrimenti non fa nulla
```


%%
[[000 Indice ML|↖ Ritorna all'indice ↖]]
%%