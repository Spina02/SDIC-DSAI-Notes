- *On fire*: 8
    molto bella, me la sono pompata diverse volte prima che l'album uscisse
- *Crudele*: 8
    Hit old school, salmo mi fa impazzire quando fa sta roba così
- *Neurologia*: 8
    molto bella anche questa, classic salmo, base clamorosa
- *Sincero*: 6.5
    salmo che fa pop x_x, boh troppo "commerciale"
- *Bye bye*: 7
    molto bella la parte di salmo, ma kaos fa cagare
- *Bounce!*: 7.5
    Salmo tira dritto, base durissima, bounce!
- *Sangue amaro*: 7
    Troppo melodica, boh
- *Cartine corte*: 9
    clamorosa, veramente una grossa hit
- *Bitcoin*: 7.5
    hit, bella carica, mi picciono i cambi di stile
- *Il figlio del prete*: 7
    Carina, bella base, non mi ha lasciato troppo
- *Numeri primi*: 6
    Chill, non mi fa impazzire troppo la base, testo ok
- *Fuori controllo*: 7.5
    Bella carica, hit, forse un po' troppo spinta (bella pe luca agnelli)
- *Incapace*: 8
    carina, molto chill, stile "strano" per salmo
- *Conta su di me*: 8
    questa è l'altra __canzone commerciale__ dell'album, però carina anche que
- *Mauri*: 8.5/9
    pezzo introverso, veramente molto bello 
- *Titoli di coda*: 9.5
    base hit, testo hit



**5. Calcolo esplicito della derivata del prodotto**
Qui mancava il passaggio chiave:

$$
\frac{\partial(\Gamma^i_{\,km}A_i)}{\partial u^l}
=\underbrace{\frac{\partial\Gamma^i_{\,km}}{\partial u^l}}_{\text{variazione della connessione}}\,A_i
\;+\;
\underbrace{\Gamma^i_{\,km}}_{\text{connessione}}
\;\frac{\partial A_i}{\partial u^l},
$$

e analogamente

$$
\frac{\partial(\Gamma^i_{\,kl}A_i)}{\partial u^m}
=\frac{\partial\Gamma^i_{\,kl}}{\partial u^m}\,A_i
\;+\;
\Gamma^i_{\,kl}\,\frac{\partial A_i}{\partial u^m}.
$$

Sostituendo dentro $\Delta A_k$ otteniamo

$$
\Delta A_k
=\;
\dfrac12\,\Delta f^{lm}\,
\Bigl[
  \bigl(\tfrac{\partial\Gamma^i_{\,km}}{\partial u^l}\,A_i
        +\Gamma^i_{\,km}\,\tfrac{\partial A_i}{\partial u^l}\bigr)
  -
  \bigl(\tfrac{\partial\Gamma^i_{\,kl}}{\partial u^m}\,A_i
        +\Gamma^i_{\,kl}\,\tfrac{\partial A_i}{\partial u^m}\bigr)
\Bigr].
$$

---

**6. Uso del trasporto parallelo su $A_i$**
Poiché $A_i$ è trasportato parallelamente,

$$
\tfrac{\partial A_i}{\partial u^l}
=\;\dfrac{\delta A_i}{\partial u^l}
=\;\Gamma^n_{\,il}\,A_n,
\quad
\tfrac{\partial A_i}{\partial u^m}
=\;\Gamma^n_{\,im}\,A_n.
$$

Allora

$$
\Delta A_k
=\;
\dfrac12\,\Delta f^{lm}\,
\Bigl[
  \tfrac{\partial\Gamma^i_{\,km}}{\partial u^l}\,A_i
  -\tfrac{\partial\Gamma^i_{\,kl}}{\partial u^m}\,A_i
  +\Gamma^i_{\,km}\,\Gamma^n_{\,il}\,A_n
  -\Gamma^i_{\,kl}\,\Gamma^n_{\,im}\,A_n
\Bigr].
$$

---

**7. Scambio degli indici “morti” e fattorizzazione di $A_i$**
Nei due termini con prodotto di due connessioni scambiamo gli indici muti $i\leftrightarrow n$:

$$
\Gamma^i_{\,km}\,\Gamma^n_{\,il}\,A_n
\;=\;
\Gamma^n_{\,km}\,\Gamma^i_{\,nl}\,A_i,
\quad
\Gamma^i_{\,kl}\,\Gamma^n_{\,im}\,A_n
\;=\;
\Gamma^n_{\,kl}\,\Gamma^i_{\,nm}\,A_i.
$$

Si raccoglie $A_i$:

$$
\Delta A_k
=\;
\dfrac12\,A_i\,\Delta f^{lm}\,
\Bigl[
  \tfrac{\partial\Gamma^i_{\,km}}{\partial u^l}
  -\tfrac{\partial\Gamma^i_{\,kl}}{\partial u^m}
  +\Gamma^n_{\,km}\,\Gamma^i_{\,nl}
  -\Gamma^n_{\,kl}\,\Gamma^i_{\,nm}
\Bigr].
$$

---

**8. Definizione del tensore di curvatura**
La combinazione fra parentesi quadre è proprio il **tensore di Riemann–Christoffel**:

$$
\boxed{
R^i{}_{klm}
\;=\;
\tfrac{\partial\Gamma^i_{\,km}}{\partial u^l}
-\tfrac{\partial\Gamma^i_{\,kl}}{\partial u^m}
+\Gamma^n_{\,km}\,\Gamma^i_{\,nl}
-\Gamma^n_{\,kl}\,\Gamma^i_{\,nm}
}
$$

da cui infine

$$
\Delta A_k
=\;\dfrac12\;A_i\;R^i{}_{klm}\;\Delta f^{lm}.
$$

*(Nota bene: a volte lo si trova definito con segni opposti, a seconda della convenzione.)*

---

**9. Interpretazione geometrica**

* Se in una regione $R^i{}_{klm}=0$, allora $\Delta A_k=0$ per ogni piccola superficie, cioè il trasporto parallelo su un percorso chiuso non modifica $A_k$: lo spazio è **piatto** (euclideo o con $g_{ij}$ costante).
* Se invece $R^i{}_{klm}\neq0$, il risultato del trasporto dipende dal percorso: lo spazio è **curvo**, e $R^i{}_{klm}$ ne misura la curvatura intrinseca.
