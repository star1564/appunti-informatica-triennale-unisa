---
tags:
  - corsi/matematica/analisi
  - corsi/informatica/programmazione_strutture_dati
---
## Con $x\to\pm \infty$
### 
$$\lim_{ x \to \pm\infty } \frac{a}{x}=0, \quad \forall a \in \mathbb{R}$$
$$\lim_{ x \to \pm\infty } \frac{x}{a}=\pm \infty, \quad \forall a \in \mathbb{R}$$


---
### 
Quando si ha il limite a $\color{#61AFEF}\pm\infty$ e una [[038 Forme Indeterminate dei Limiti|forma indeterminata]] $\color{#61AFEF}\frac{\infty}{\infty}$ si può raccogliere la $x$ con grado più grande del *denominatore* e semplificare.

> [!example]- <font color="orange">Esempio</font>
>$$\begin{align*}
\lim\limits_{x \to \pm\infty}\frac{x^2-2x+1}{\textcolor{#61AFEF}{x^2}+1}
&=\lim\limits_{x \to \pm\infty}\frac{\textcolor{#61AFEF}{\cancel{ x^2 }}\left( \frac{x^2}{\textcolor{#61AFEF}{x^2}}-\frac{2x}{\textcolor{#61AFEF}{x^2}}+\frac{1}{\textcolor{#61AFEF}{x^2}} \right)}{\textcolor{#61AFEF}{\cancel{ x^2 }}\left( \frac{x^2}{\textcolor{#61AFEF}{x^2}}+\frac{1}{\textcolor{#61AFEF}{x^2}} \right)} \\
&=\lim\limits_{x \to \pm\infty}\frac{\left( \frac{\cancel{ x^2 }}{\cancel{ x^2 }}-\frac{2\cancel{ x }}{x^\cancel{ 2 }}+\frac{1}{x^2} \right)}{\left( \frac{\cancel{ x^2 }}{\cancel{ x^2 }}+\frac{1}{x^2} \right)}\\
&=\lim\limits_{x \to \pm\infty}\frac{\left( 1-\frac{2}{x}+\frac{1}{x^2} \right)}{\left( 1+\frac{1}{x^2} \right)}\\
&=[\text{F-L-1}]\\
&=\frac{1-0+0}{1+0}=1
\end{align*}$$



## Con $x\to a$
###

![[Pasted image 20250211140642.png]]

$$\begin{align}
-(a^+) = (-a)^- \\
-(a^-) = (-a)^+
\end{align}$$
> [!example]- <font color="orange">Esempio</font>
>$$-(2^+)=-2.1=(-2^-)$$
>$$-(2^-)=-1.9=(-2^+)$$
>$$-(2)=-2$$

---
### 

$$
\begin{align}
(a^\pm + b)=(a+b)^\pm \\
(a^\pm - b)=(a-b)^\pm
\end{align}
$$


> [!example]- <font color="orange">Grafici</font>
>![[Pasted image 20250211142003.png]]


> [!example]- <font color="orange">Esempio</font>
>$$\begin{align}5-2^+ &= -(2^+ - 5) \\&=-(-3^+) \\&= 3^-\end{align}$$



> [!tip]- Usare una calcolatrice per le somme/sottrazioni
>- $2^+\simeq 2.1$
>- $2^-\simeq 1.9$



---
### 
$$(a^-)^2=(a^2)^-;\quad (a^+)^2=(a^2)^+$$
$$(-a^-)^2=(a^2)^+;\quad (-a^+)^2=(a^2)^-$$
Con esponente dispari, si lascia il segno negativo, ad esempio $(-a^-)^3=(-a^3)^+$, ma funziona allo stesso modo.

---
### 
Quando si considera il limite di un numero reale in uno dei due estremi all'interno di una funzione frazione: 
- Al *numeratore* si sostituisce il *numero normale*
- Al *denominatore* si sostituisce il *numero all'estremo*

$$
\lim_{ x \to a^\pm } \frac{x}{x}=\frac{a}{a^\pm}
$$


> [!example]- <font color="orange">Esempio</font>
>$$\lim\limits_{x \to 1^\pm}\frac{x}{x+5}=\frac{1}{1^\pm+5}=\begin{cases}\frac{1}{1^+ +5}\to \frac{1}{6^+} \\\frac{1}{1^- +5}\to \frac{1}{6^-} \\\end{cases}$$



---
