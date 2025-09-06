---
tags:
  - corsi/informatica/machine_learning
---
Scikit-learn è una libreria open-source per il *machine learning* in Python. Essa fornisce una vasta gamma di strumenti per la modellazione statistica, l'apprendimento automatico e la visualizzazione dei dati. scikit-learn è progettato per essere semplice ed efficiente e si integra bene con altre librerie Python come [[Python 4 - Pacchetti Python|NumPy, SciPy e Matplotlib]].

> [!tip] Installazione
>```bash
>pip install scikit-learn
>```

Con questo è possibile eseguire l'addestramento e l'utilizzo di modelli.

> [!example] <font color="orange">Esempio</font>
>Per dimostrare l'utilizzo di questo pacchetto, supponiamo di avere un dataset in forma [[010 Fonti e Formati dei dati#^04aac2|CSV]] che contiene le caratteristiche di diverse case. Vogliamo creare un modello che ci permetta di prevedere, date le caratteristiche di una casa, qual è il prezzo a cui dovrebbe essere venduta.
>
>| SquareFeet | Bedrooms | Bathrooms | Neighborhood | YearBuilt | Price                |
>|------------|----------|-----------|--------------|-----------|----------------------|
>| 2126       | 4        | 1         | Rural        | 1969      | 215355.28361820139  |
>| 2459       | 3        | 2         | Rural        | 1980      | 195014.22162584803  |
>| 1860       | 2        | 1         | Suburb       | 1970      | 306891.0120763329   |
>| 2294       | 2        | 1         | Urban        | 1996      | 206786.78715332696  |
>| 2130       | 5        | 2         | Suburb       | 2001      | 272436.239065061    |
>


%%
[[000 Indice ML|↖ Ritorna all'indice ↖]]
%%