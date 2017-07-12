---
layout: post
title: Javascript ES6 Funzioni a freccia
published: true
categories: Sviluppo software
tags:
- Javascript
---
![Jekyll]({{site.baseurl}}/assets/arrow-500.jpeg)
### Introduzione

Continuiamo con la panoramica sulle nuove funzionalità che porta con sè Javascript Es6.<br/>
E' la volta delle **funzioni freccia** (Arrow functions) un nuovo modo, di creare funzioni nei nostri programmi JS.<!--more-->



### ES6 funzioni a freccia
Senza entrare nella definizione di cosa sia una [funzione](https://it.wikipedia.org/wiki/Programmazione_funzionale) in un linguaggio di programmazione, andiamo a ricordare invece la sintassi ES5 che rimane utilizzabile anche in questa nuova versione del linguaggio (preferibile in taluni casi):

```javascript
function square(x) {
  return x * x;
}
```
Come possiamo vedere dall'esempio tutto comincia con la parola chiave `function` poi abbiamo le parentesi graffe che raccolgono il corpo della funzione.<br/>
In JS possiamo assegnare la nostra funzione ad una variabile con una sintassi di questo tipo:

```javascript
var square = function(x) {
  return x * x;
};
```
Vediamo ora come cambia la sintassi con le funzioni freccia:

```javascript
x => { return x * x }  // block
```
possiamo scrivere il costrutto anche in altro modo 

```javascript
x => x * x  // expression, equivalent to previous line return
			  // return value is IMPLICIT	
```
#### Parametri
L'utilizzo dei parametri nella sintassi con `function` è a parer mio molto intuitivo, i parametri vengono dichiarati tramite le parentesi tonde seguendo l'ordine inserito.<br/>
Se fornisco meno parametri di quelli che ho dichiarato tramite `function(params)` i parametri saranno tutti `undefined`, se invece ne passo troppi, quelli in eccesso saranno ignorati.<br/>
Questo comportamento è mantenuto anche nelle funzioni a freccia, quello che cambia è la sintassi di come si dichiarano i parametri.<br/>
Andiamo a vedere:

```javascript
() => { ... } // no parameter
x => { ... } // one parameter, an identifier
(x, y) => { ... } // several parameters
```
Si capisce subito che la parola chiave `function` in questa tipologia di funzioni non serve, sostituita da `=>` appunto la freccia, le parentesi tonde continuano a racchiudere la dichiarazione dei parametri, ma con un solo parametro possiamo ometterle.<br/>
Da l'ultimo esempio sopra poi, si vede come il blocco di codice con il ritorno di un valore `{return x * x}`, può essere omesso rendendolo **implicito** se omettiamo le parentesi tonde `{}`.<br/>
Questa sintassi rende molto coinciso il codice, anche se potrebbe essere in certi casi molto meno leggibile, ma questo è un mio pensiero.

```javascript
> [1,2,3].map(x => 2 * x)  //[ 2, 4, 6 ]
```
L'esempio mostra come le funzioni a freccia sono ideali come **callback** di altre funzioni, proprio per la loro sintassi breve.<br/>

#### Differenze delle funzioni a freccia con le tradizionali
Come abbiamo visto, questa nuova sintassi è preferibile alla vecchia in moltissime situazioni, ma dobbiamo stare attenti a particolari comportamento che presentano.

##### Mancato binding di `this`
Nelle funzioni dichiarate con `function` ogni funzione, definiva un **proprio** `this`, ossia il contesto al quale faceva riferimento, questo comportamento non è così nella funzioni a freccia che invece mantengono il `this` ereditato dal genitore. <br/>Vediamo un esempio.


Problema:

```javascript
function Person() {
  // The Person() constructor defines `this` as an instance of itself.
  this.age = 0;

  setInterval(function growUp() {
    // In non-strict mode, the growUp() function defines `this` 
    // as the global object, which is different from the `this`
    // defined by the Person() constructor.
    this.age++;
  }, 1000);
}

var p = new Person();
```
Soluzione ES5 assegno il `this` ad una variabile `that`:

```javascript
function Person() {
  var that = this;
  that.age = 0;

  setInterval(function growUp() {
    // The callback refers to the `that` variable of which
    // the value is the expected object.
    that.age++;
  }, 1000);
}
```
Nelle funzioni a freccia **eredito** il `this`, quindi il codice iniziale funziona senza problemi.

```javascript
function Person(){
  this.age = 0;

  setInterval(() => {
    this.age++; // |this| properly refers to the person object
  }, 1000);
}

var p = new Person();
```
Questa differenza fondamentale sul `this` ereditato è sempre da tener presente nell'uso delle funzioni a freccia.

##### Mancato binding di `arguments`
Un'altra differenza da segnalare è la **mancanza** di [arguments](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/arguments), nelle funzioni tradizionali questo è un particolare Array che corrisponde ai parametri passati alla funzione.

```javascript
arguments[0]
arguments[1]
arguments[2]
```
Questo Array è molto utile per capire il numero dei parametri passati alla funzione o per altre operazioni sui parametri stessi.<br/>

##### Altre differenze
Altre differenze che segnalo semplicemente sono:<br/>
1. la non possibilità di usare le funzioni a freccia come `metodi`
2. Le funzioni a freccia non possono essere usate come costruttori, ed emetteranno un errore se usate con `new`.

### Conclusioni
Le funzioni a freccia come abbiamo visto, sono un ottimo modo coinciso e potente che ci permetterà di scrivere meno codice e meglio.<br/>

Alla prossima puntata &#x1f44b;&#x1f3fb;.
<br/>
Stay tuned!



**PS**: Come sempre per approfondire meglio, consiglio il libro *Exploring ES6*, e l'apposito capitolo per le [Arrow Functions](http://exploringjs.com/es6/ch_arrow-functions.html).




