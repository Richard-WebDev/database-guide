# 03 — Insiemi

[← Capitolo precedente](02-astrazione-linguaggi.md) · [Indice](README.md) · [Capitolo successivo →](04-modello-relazionale.md)

---

## Perché studiare gli insiemi

Il modello relazionale si basa sulla teoria degli insiemi: una tabella è un **insieme di tuple**. Quindi non ci possono essere righe duplicate e l'ordine delle righe non ha significato. Anche gli operatori dell'algebra relazionale (unione, intersezione, differenza, prodotto cartesiano) vengono da qui.

## Cos'è un insieme

Un **insieme** è una collezione di elementi che rispetta due regole:

1. **Nessun duplicato**: lo stesso elemento non può comparire due volte.
2. **Nessun ordine**: non esiste un "primo" o un "ultimo" elemento.

---

## Come si descrive un insieme

### Forma esplicita

Si elencano uno per uno tutti gli elementi.

```
A = {1, 7, 3}
B = {Rosso, Giallo}
```

### Forma implicita

Si descrive la proprietà che caratterizza gli elementi.

```
C = {x | x è un numero pari maggiore di 0} = {2, 4, 6, 8, ...}
```

---

## Simbologia

Negli esempi: `A = {1, 2, 3, 4}`.

| Simbolo | Nome | Significato | Esempio |
|---|---|---|---|
| ∈ | appartenenza | l'elemento appartiene all'insieme | 3 ∈ A |
| ∉ | non appartenenza | l'elemento non appartiene all'insieme | 10 ∉ A |
| ⊆ | sottoinsieme | **ogni** elemento di B è anche in A (B ⊆ A) | {1, 2} ⊆ A |
| ⊄ | non sottoinsieme | esiste almeno un elemento di B che non è in A | {1, 9} ⊄ A |
| ⊂ | sottoinsieme proprio | B ⊆ A, ma B ≠ A: A ha almeno un elemento in più | {1, 2} ⊂ A |
| ∪ | unione | tutti gli elementi di A e di B, senza duplicati | |
| ∩ | intersezione | solo gli elementi presenti in entrambi | |
| − | differenza | gli elementi di A che non sono in B | |
| \|A\| | cardinalità | numero di elementi dell'insieme | \|A\| = 4 |
| × | prodotto cartesiano | tutte le coppie possibili | |

> **Nota bene:** ogni insieme è sottoinsieme di se stesso (`A ⊆ A`), perché ogni suo elemento appartiene ad A. Non ne è però sottoinsieme *proprio*.

### Unione, intersezione, differenza

Con `A = {1, 3, 5}` e `B = {1, 3, 7, 8}`:

```
A ∪ B = {1, 3, 5, 7, 8}     (il 1 e il 3 compaiono una volta sola)
A ∩ B = {1, 3}
A − B = {5}                 (5 è in A ma non in B)
B − A = {7, 8}              (la differenza NON è commutativa)
```

### Cardinalità

Per un insieme è il numero di elementi. Nel modello relazionale, la cardinalità di una relazione è il **numero di tuple** (righe) che contiene.

---

## Prodotto cartesiano

Il prodotto cartesiano `A × B` contiene **tutte le coppie possibili** prendendo un elemento da A e uno da B, senza saltarne nessuna.

Con `A = {1, 2}` e `B = {a, b}`:

- prendo 1 da A e lo combino con a → `(1, a)`
- prendo 1 da A e lo combino con b → `(1, b)`
- prendo 2 da A e lo combino con a → `(2, a)`
- prendo 2 da A e lo combino con b → `(2, b)`

```
A × B = {(1,a), (1,b), (2,a), (2,b)}
```

Il numero di coppie è il prodotto delle cardinalità: `|A × B| = |A| · |B|` (qui 2 · 2 = 4).

Il prodotto cartesiano si può fare anche tra più di due insiemi: in quel caso si ottengono **tuple** (terne, quaterne…) invece di coppie.

---

[← Capitolo precedente](02-astrazione-linguaggi.md) · [Indice](README.md) · [Capitolo successivo →](04-modello-relazionale.md)
