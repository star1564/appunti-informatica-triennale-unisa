---
aliases:
tags:
  - corsi/matematica/analisi
paragrafo: Matematica di Base
cssclasses:
  - 
---
>Un'**Equazione Fratta** è un particolare tipo di [[MB - 00 Equazioni e Uguaglianze|equazione]] caratterizzata dalla presenza di *denominatori in cui compare l'[[MB - 00 Equazioni e Uguaglianze#Variabili (Incognite)|Incognita]]* ed è *riducibile* alla forma:
>$$\color{#CC241D}\frac{N(x)}{D(x)}=0$$
>
>Questa viene detta *forma normale delle equazioni fratte*, in cui $N(x)$ e $D(x)$ sono espressioni matematiche che dipendono dall'incognita $x$.
>
>Per essere in forma normale, l'equazione deve esserci:
>1. Un'unica frazione al primo membro
>2. Zero al secondo membro

> [!example]+ <font color="orange">Esempio</font>
>In forma normale: $$\frac{x+1}{x}=0$$
>
>*Non* in forma normale: $$\frac{1}{x} + \frac{1}{x+1}=1$$

## Metodo risolutivo delle Equazioni Fratte

1. Imporre le **[[042 Campo di Esistenza|Condizioni di Esistenza]]**. Normalmente, con le [[MB - 04 Equazioni di Primo Grado|equazioni di primo grado]] e di [[MB - 08 Equazioni di Secondo Grado|secondo grado]], basta imporre che *ciascun denominatore presente nell'equazione sia diverso da zero*. 
   $$CE:\begin{cases} \text{denominatore}_1 \neq 0 \\ \dots \\ \text{denominatore}_n \neq 0 \end{cases}$$

2. Ridurre l'equazione fratta *in forma normale*. Questo lo si fa:
	- Trasportando tutti i termini a sinistra dell'uguale
	- Svolgendo le operazioni necessarie in modo che al primo membro si presenti un unico rapporto

3. Applicando il [[MB - 01 Principi di Equivalenza delle Equazioni#^e15b0b|secondo principio di equivalenza delle equazioni]], si *cancella il denominatore* moltiplicando entrambi i membri per $D(x)$ $$D(x)\cdot \frac{N(x)}{D(x)}=D(x)\cdot 0$$
4. *Risolvere* l'equazione $$N(x)=0$$
5. Bisogna stabilire se le soluzioni dell'equazione $N(x)=0$ *soddisfano le condizioni di esistenza*. Tutte le soluzioni di $N(x)=0$ che appartengono all'insieme di esistenza dell'equazione originale sono dette **soluzioni accettabili**.


## Possibili soluzioni
In base al numero di soluzioni si possono classificare le equazioni fratte in:
- **Equazioni Determinate**: quando esiste un *numero finito di soluzioni* dell'equazione $N(x)=0$ che soddisfano le condizioni di esistenza.
- **Equazioni Indeterminate**: quando l'equazione $N(x)=0$ ammette *infinite soluzioni* che soddisfano le condizioni di esistenza.
- **Equazioni Impossibili**: quando l'equazione $N(x)=0$ *non ammette soluzioni* oppure se tutte le sue soluzioni non rispettano le condizioni di esistenza.



---
[[000 Indice Analisi|↖ Ritorna all'indice ↖]]

Altri collegamenti: 
- [YouMath](https://www.youmath.it/lezioni/algebra-elementare/equazioni/3737-equazioni-fratte.html)
- [YouMath - Primo Grado](https://www.youmath.it/lezioni/algebra-elementare/equazioni/79-equazioni-fratte-di-primo-grado-ad-unincognita.html)
- [YouMath - Secondo Grado](https://www.youmath.it/lezioni/algebra-elementare/equazioni/80-equazioni-di-secondo-grado-fratte-ad-unincognita.html)