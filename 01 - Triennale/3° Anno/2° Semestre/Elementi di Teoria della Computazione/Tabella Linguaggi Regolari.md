- [[007 Linguaggio Regolare]]
- [[016 Linguaggio Non Regolare]]

| Regolari                                   | Motiv. Reg [^1]             | Non Regolari                           | Motiv. Non Reg. [^2] |
| ------------------------------------------ | --------------------------- | -------------------------------------- | -------------------- |
| $\{a^nb:n\ge0\}$                           | $(a)^*b$                    | $\color{#CC241D}\{a^nb^n:n\ge0\}$ [^3] | $a^pb^p$             |
| $\{0^kw0^k:k>0,w\in\{0,1\}^*\}$            | $0(0\cup1)^*0$              | $\{a^nb^{m}:n,m\ge0, n<m\}$            | $a^pb^{p+1}$         |
| $\{a^kba^k:k<4\}$                          | Finito                      | $\{xcx:x\in\{a,b\}^*\}$                | $a^pca^p$            |
| $\{a^kbc^{2n}:k,n>0\}$                     | $a^+b(cc)^+$                | $\{a^iba^{j}:i\neq j\}$                | $a^pba^{p+1}$        |
| $\{a^ib^j:i+j\text{ dispari}, i,j\geq 0\}$ | $a(aa)^*\cup (aa)^*(bb)^*b$ | $\{a^ib^j:i=2j,\ i,j\geq 0\}$          | $a^pb^{p/2}$         |
| $\{ww:w\in\{a\}^*\}$                       | $a^*$                       | $\{wcw:w\in\{a\}^*\}$                  | $a^pca^p$            |
| $\{0^nw0^n:n\geq0,w\in\{0,1\}^*\}$         | $0^*(0\cup1)^*0^*$          | $\{0^n0^{2n}:n\geq0\}$                 | $0^p0^{2p}$          |
| $\{0^n\ 0\ 0^n: n\geq 0\}\to 0^{2n+1}$     | $(00)^*0$                   |                                        |                      |


[^1]: Linguaggi finiti o espressioni regolari
[^2]: Stringa per il pumping lemma
[^3]: Questo è vero senza aver bisogno di una dimostrazione (anche all'esame)

%%
#corsi/informatica/elementi_teoria_computazione 
%%
