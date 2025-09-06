---
tags:
  - corsi/matematica/analisi
  - esercizi
---
Negli integrali non ci possono essere soluzioni che hanno 
$$
\sqrt[a]{x^b},\quad \text{con } b>a
$$

> [!example]- <font color="orange">Esempio</font>
>$$\sqrt[4]{x^5}\iff\sqrt[4]{x\cdot x^4}\iff x\sqrt{ x }$$

---
È possibile inserire costanti all'interno di un integrale se lo si moltiplica anche all'esterno (non vale per le $x$):

$$
\int \frac{ax}{bx}\,dx\iff c\int c\cdot \frac{ax}{bx}\,dx\iff \frac{c}{c}\int \frac{c}{c}\cdot \frac{ax}{bx}\,dx
$$

Questo è utile per trovarsi negli integrali immediati.

---
Quando si ci vuole ritrovare in uno di questi due integrali immediati:
$$\int \textcolor{#61AFEF}{\frac{1}{\sqrt{1-x^2}}} \, dx=\textcolor{#CC241D}{\arcsin(x)+c}$$

$$\int \textcolor{#61AFEF}{\frac{1}{1+x^2}} \, dx=\textcolor{#CC241D}{\arctan(x)+c}$$
Ma non si ha $1$ al di sotto si divide per la costante tutto il denominatore per farlo diventare $1$.

> [!example]- <font color="orange">Esempio</font>
>$$\begin{align}\int \frac{1}{\sqrt{ 9-25x^2 }}\,dx &= \int \frac{1}{\sqrt{ 9\left( \frac{\cancel{ 9 }}{\cancel{ 9 }}-\frac{25x^2}{9} \right) }}\,dx \\ &= \int \frac{1}{\sqrt{ 9\left( 1-\frac{25}{9}x^2 \right) }}\,dx\\ &= \int \frac{1}{3\sqrt{1-\left( \frac{5}{3}x \right)^2}}\,dx\\ &=\int \frac{\frac{1}{3}}{\sqrt{1-\left( \frac{5}{3}x \right)^2}}\,dx\\&=\frac{1}{5}\int \frac{\frac{5}{3}}{\sqrt{1-\left( \frac{5}{3}x \right)^2}}\,dx\\&=\frac{1}{5}\arcsin\left( \frac{5}{3} x \right)\end{align}$$