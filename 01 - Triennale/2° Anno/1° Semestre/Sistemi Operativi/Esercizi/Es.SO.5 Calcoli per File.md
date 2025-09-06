---
tags:
  - corsi/informatica/sistemi_operativi
  - esercizi
---
Si hanno i seguenti dati:
- Abbiamo un hard disk con capienza di $2^{41}$ byte ed è formattato in *blocchi da 512 byte*;
- Abbiamo un file di nome `pluto` grande *300 kilobyte*.

Possiamo eseguire questi calcoli.

> [!warning]+ kilobyte in byte
> $n\ kilobyte$ (kb) equivalgono **APPROSSIMAMENTE** a $n\cdot 2^{10}\ byte$ (b).
>
> Inoltre: 8 bit = 1 byte.

#### [[012 Allocazione contigua di file|IN ALLOCAZIONE CONTIGUA]]
- **Numero di Blocchi in un FILE**:

$dimensione\ totale\ pluto\ = |pluto| = 300\ kb = \color{#61AFEF}300\cdot 2^{10}\ b$
$size\ blocco = 512\ b = \color{#61AFEF}2^9\ b$
$$\# blocchi\ dati = \textcolor{#61AFEF}{\frac{|pluto|}{size\ blocco}}=\frac{300\cdot 2^{10}\ b}{2^9\ b}= 300\cdot2 = 600$$

Quindi abbiamo una size dell'intero file di 600 blocchi.

---
#### [[013 Allocazione linkata di file|IN ALLOCAZIONE LINKATA]]
- **Numero di Blocchi in un DISCO**:

$size\ disco = \color{#61AFEF}2^{41}\ b$
$size\ blocco = 512\ b = \color{#61AFEF}2^9\ b$
$$\# blocchi\ dati\ DISCO = \textcolor{#61AFEF}{\frac{size\ disco}{size\ blocco}}=\frac{2^{41}\ b}{2^9\ b}= 2^{\textcolor{#CC241D}{32}}$$
$size\ puntatore = \textcolor{#CC241D}{32}\ bit = 4\ b$

L'**apice** del numero dei blocchi dati (32 nel nostro caso) equivale alla *size del puntatore (in bit)* che si trova in ogni blocco.

Quindi un blocco da $512\ b$ è composto da $508\ b$ per i dati ed il resto per il puntatore. 

![[Pasted image 20221011101716.png]]

- **Numero di Blocchi in un FILE**:

$dimensione\ totale\ pluto\ = |pluto| = 300\ kb = \color{#61AFEF}300\cdot 2^{10}\ b$
$size\ dati\ nel\ blocco = \color{#61AFEF}508\ b$
$$\# blocchi\ dati\ FILE = \textcolor{#61AFEF}{\Big\lceil\frac{|pluto|}{size\ dati\ nel\ blocco}\Big\rceil}=\Big\lceil\frac{300\cdot2^{10}\ b}{508\ b}\Big\rceil= 605$$

---
#### [[014 Allocazione indicizzata di file|IN ALLOCAZIONE INDICIZZATA]]
- **Numero di puntatori in un blocco indice**:

$\#blocchi\ dati\ file = \frac{|pluto|}{size\ blocco} = 600$
$size\ puntatore = 4\ b = 2^2\ b$

$$\# punatori\ in\ un\ blocco=\frac{size\ blocco}{size\ puntatore}=\frac{2^9\ b}{2^2\ b}=2^7=128$$

- **Numero di blocchi in un blocco indice (ESEMPIO $e$)**: 

Tenendo conto della seguente situazione ![[Pasted image 20221011145402.png|300]]
$\#blocchi\ dati\ pluto=600$
$\#puntatori\ in\ un\ blocco=128$

Si sottraggono i due blocchi ($b,c$): $600-2=598\ b$.
Si sottraggono i byte del blocco indice $d$: $598-128=470\ b$.
Si divide per eccesso questo numero per il numero di puntatori in un blocco: $\lceil\frac{470}{128}\rceil=\color{#CC241D}4$.

---
#### [[Allocazione FAT|FAT]]
- **Grandezza della FAT**:

$\#entry=\frac{size\ disco}{size\ blocco}=\frac{2^{41}\ b}{2^9\ b}=2^{32}\ b$
$size\ puntatore=32\ bit=4\ b$

$$|FAT|=2^{32}\ b\cdot4\ b=2^{32}\ b\cdot2^2\ b=2^{34}\ b$$


%%[[000 Indice SO|↖ Ritorna all'indice ↖]]%%