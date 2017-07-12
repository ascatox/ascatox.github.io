---
layout: post
title: Javascript ES6 Introduzione
published: true
categories: Sviluppo software
tags:
- Javascript
---
![Jekyll]({{site.baseurl}}/assets/es6-logo-low.jpg)
## Da ES5 a ES6
Javascript è un linguaggio di programmazione che riscuote sempre maggior successo in questi ultimi periodi, come testimoniano moltissimi sondaggi come quelle che ogni anno fornisce il celebre sito [StackOverflow](https://insights.stackoverflow.com/survey/2017#technology).<!--more-->

La versione attuale di Javascript maggiormente diffusa ed utilizzata al momento è [EcmaScript 5](https://it.wikipedia.org/wiki/ECMAScript) approvata in via definitiva nel 2009.<br/>

Nel [giugno del 2015](http://www.ecma-international.org/news/Publication%20of%20ECMA-262%206th%20edition.htm) la versione EcmaScript 6 è stata approvata ed è diventata **ufficiale**.
Purtroppo ancora oggi il supporto a questa versione del linguaggio Javascript nei principali browser, non avviene ancora in maniera piena, va leggermente meglio nel mondo *Server* con [Node Js](http://nodejs.org).
Per chiarirsi le idee qui c'è una [mappa](http://kangax.github.io/compat-table/es6/) che mostra l'adozione di ES6, fermo restando che esistono dei software come [Babel](https://babeljs.io) che permettono una conversione, [transpiling](https://www.stevefenton.co.uk/2012/11/compiling-vs-transpiling/) in gergo tecnico da ES6 a ES5, rendendo il passaggio più semplice ed indolore.
## ES6 Concetti di base
In questo blog vorrei provare a far conoscere al meglio i concetti principali (core) che ES6 porta con se, cercando di illustrare le differenze con la precedente versione.
Per quelli che vogliono approfondire fin da subito consiglio l'ottimo libro **Exploring ES6**, che trovate a partire da questo [indirizzo](http://exploringjs.com/es6.html).

### Dichiarazione delle variabili let e const al posto di var
La prima novità di rilievo che ES6 porta con se, è un nuovo modo di dichiarare variabili in Javascript.
In ES5 l'unica parola chiave per la dichiarazione era `var`, adesso invece possiamo usare `let` e `const`.
#### Dichiarazione con let
`let` funziona in modo molto simile a `var` ma la variabile dichiarata ha lo *scope a livello di blocco (block scoped)*, cioè la variabile esiste solo all'interno del blocco dove è stata dichiarata. Questa è una notevole differenza rispetto a `var` che invece aveva uno *scope a livello di funzione (function scoped)*.

Questa differenza è molto importante ed ha notevoli impatti su come il codice deve essere scritto ed impedisce di fatto una conversione *"automatica"* da `var` a `let` nel codice JS già implementato.

```javascript
function order(x, y) {
if (x > y) { // (A)
 let tmp = x;
 x = y;
 y = tmp;
}
console.log(tmp===x); // ReferenceError: tmp is not defined
return [x, y];
}
```
Come si vede dall'esempio sopra, la variabile `tmp` esiste solo nel blocco all'interno dell'istruzione di `if`, al di fuori prendiamo un'eccezione `ReferenceError` se proviamo ad accederci.

#### Dichiarazione con const
In questa versione di JS abbiamo anche la possiblità di dichiarare variabili che una volta inizializzate *non possono essere modificate successivamente*, attraverso la parola chiave `const`.

Il funzionamento della visibilità è del tutto identico a `let`, l'unica differenza sta nel poterla riassegnare una volta inizializzata.

```javascript
const jolly;
// SyntaxError: missing = in const declaration

const val = 123;
val = 456;
// TypeError: val is read-only
```

#### Differenze tra var e let/const
Come detto in precedenza la differenza di maggior rilievo lasciando stare il fatto che `const` dichiara delle variabili **immutabili**, tra `var` e `let/const` risiede nella visibilità:

Con la parola chiave `var` lo scope è a livello di *funzione*

```javascript
function foo() {
if (true) {
 var val = 1111;
}
console.log(val); // 111
}
```
Con la parola chiave `const` lo scope è a livello di *blocco*

```javascript
function foo() {
const tmp = 4;
if (···) {
 const tmp = 3; // shadows outer `tmp`
 console.log(tmp); // 3
}
console.log(tmp); // 4
}
```

Grazie all'esempio penso sia molto chiaro il concetto di **shadowing** e la visibilità delle variabili.

#### Immutabilità di const
Il concetto di immutabilità di `const`, significa che una variabile ha sempre il medesimo valore, ma non vuol dire che il *valore stesso* è o diventa immutabile.

Se infatti creiamo un oggetto ed aggiungiamo una proprietà a quell'oggetto, quest'ultima è mutabile.
Vediamo un piccolo esempio che spiega il concetto:

```javascript
const obj = {};
obj.prop = 12222;
console.log(obj.prop); // 12222
```
L'oggetto dichiarato `const` invece rimane immutabile:

```javascript
obj = {}; //TypeError
```
#### Conclusione
Ci sono molti altri concetti che necessitano di chiarimento per i due nuovi modi di dichiarare variabili in ES6, ma prima di poterlo fare, dobbiamo passare in rassegna altre novità del linguaggio Javascript.

Arrivederci alla prossima puntata.
Stay Tuned!