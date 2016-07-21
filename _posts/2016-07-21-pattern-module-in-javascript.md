---
layout: post
title: Pattern Module in JavaScript
published: true
categories: Sviluppo software
tags:
- Design Pattern
---
![Jekyll]({{site.baseurl}}/assets/lego.png)
<h2>Introduzione</h2>
Definizione di [Wikipedia](https://it.wikipedia.org/wiki/Design_pattern "Wikipedia") per **Design Pattern**: <br/>
*"In informatica, nell'ambito dell'ingegneria del software, un design pattern, è un concetto che può essere definito una soluzione progettuale generale ad un problema ricorrente. Si tratta di una descrizione o modello logico da applicare per la risoluzione di un problema che può presentarsi in diverse situazioni durante le fasi di progettazione e sviluppo del software, ancor prima della definizione dell'algoritmo risolutivo della parte computazionale."*
<!--more-->
<br/><br/>
Il termine ha iniziato a prendere piede, tra i programmatori di tutto il mondo, dopo la pubblicazione del famoso libro Design Patterns: Elementi per il riuso di software a oggetti di Erich Gamma, Richard Helm, Ralph Johnson e John Vlissides nel 1995 (gli autori di questo libro vengono spesso chiamati la Gang of Four).<br/>
I design pattern sono uno strumento fondamentale nello sviluppo delle applicazioni software professionali. Essi vengono studiati in tutti corsi di ingegneria del software del mondo e in ambito professionale la loro conoscenza è diventata un prerequisito fondamentale (sono spesso argomento di discussione durante i colloqui tecnici nei processi di selezione del personale). I principali vantaggi dell’utilizzo dei design pattern nella progettazione di applicazioni software sono:<br/>

- ridurre i tempi di sviluppo, grazie al riutilizzo di soluzioni preesistenti;
- migliorare la condivisione del codice, grazie all’uso di tecniche standard note;
- migliorare la qualità del codice, grazie all’impiego di tecniche testate negli anni.

<br/>

I design pattern possono essere suddivisi **in tre categorie principali**:<br/>

- creazionali, utilizzati per risolvere problemi di programmazione che prevedono la creazione di oggetti a seconda di determinati parametri o condizioni;
- strutturali, per il riutilizzo degli oggetti esistenti fornendo, sostanzialmente, un’interfaccia più adatta alle loro esigenze;
- comportamentali, che forniscono soluzioni alle più comuni tipologie di interazione tra gli oggetti.
<br/>
Andiamo quindi a scoprire uno dei principali di natura creazionale il **pattern Module**.

<h2>Module</h2>
<h3>Problema da risolvere</h3>
Chiunque si sia cimentato nello scrivere codice Javascript (non considerando al momento ES6), conosce quanto sia difficile mantenere il codice organizzato e ben **isolato**. In linguaggi come il Java, tramite il concetto di classe e grazie ai modificatori di accesso si riesce ad isolare in maniera efficace delle determinate aree di codice, in JavaScript questo non è possibile e da qui nasce il Module.<br/>
*Il problema che si vuole risolvere con il pattern Module è creare porzioni isolate di codice con proprie variabili private e con la creazione di funzioni specifiche accessibili dall’esterno.*
<h2>Come funziona</h2>
Il pattern Module è basato sul concetto di funzione anonima auto eseguita ([anonymous self-executing function](http://markdalgleish.com/2011/03/self-executing-anonymous-functions/ "anonymous self-executing function")).
<br/>
La struttura di una funzione di questo tipo è definita nel modo seguente:
{%highlight javascript%}
(function() {
  //codice
})();
{%endhighlight%}
Come sappiamo, JavaScript ha un meccanismo molto semplice per la gestione delle variabili. Quando l’interprete utilizza per la prima volta una variabile, viene eseguito un controllo sul nome per stabilire la sua definizione, il suo ambiente (scope). Se la variabile non è stata definita esplicitamente, essa viene considerata globale.
<br/>
Con il pattern Module è possibile utilizzare variabili esterne alla funzione anonima passandole come parametro. Per esempio, è possibile utilizzare la variabile globale window all’interno di un Module in questo modo:
{%highlight javascript%}
(function(window) {
   //codice
})(window);
{%endhighlight%}
<br/>
Una struttura tipica del pattern Module, anche se esistono diverse varianti in circolazione, è quella riportata di seguito:
{%highlight javascript%}
var myModule = (function(){
    // private property
    var privateProperty = 'bar';

    // private function
    var privateFunction = function() {
        // ...
    }

    // constructor
    var myModule = function() {
        // ...
    }
    myModule.prototype = {
        constructor : myModule,
        publicMethod : function(){
           console.log('publicMethod');
        },
        getPrivateProperty : function(){
            return privateProperty;
        }
    }

    return myModule;
})();

var my = new myModule();
my.publicMethod();
console.log(my.getPrivateProperty());
{%endhighlight%}
Come si vede il concetto è proprio quello della classe, ben noto in altri linguaggi, all'interno dell'oggetto `prototype` è possibile decidere quali funzioni sono accessibili dall'esterno tramite l'oggetto `my` creato.
All’esterno della funzione anonima, è possibile istanziare uno o più oggetti myModule, utilizzando semplicemente l’istruzione `new`.

<h2>Conclusioni</h2>
L’utilizzo del pattern Module all’interno delle proprie applicazioni JavaScript ha numerosi vantaggi, tra i quali:

- scalabilità - la possibilità di definire porzioni isolate di codice agevola il processo di interazione tra gli oggetti all’aumentare del numero degli stessi;
- gestione della complessità del codice - l’utilizzo dei moduli consente di suddividere un progetto di grandi dimensioni in porzioni più piccole, che possono essere gestite in maniera più agevole. Inoltre, l’uso dei moduli consente una migliore interoperabilità all’interno di un team di sviluppo. Se ogni sviluppatore lavora su una porzione di codice isolata non ci sono problemi di sovrapposizione del codice e l’interazione tra i membri del team diventa più agevole;
- qualità del codice - l’uso di porzioni distinte e separate di codice migliora la qualità di un progetto software, garantendo anche migliori performance dal punto di vista dell’interprete JavaScript, per esempio nella gestione del garbage collector;
- estendibilità del codice - un progetto software suddiviso in moduli consente di essere esteso in maniera totalmente naturale. È sufficiente creare un nuovo modulo e aggiungerlo al progetto

<h2>Riferimenti</h2>
In rete ci sono moltissimi siti, blog o altro che spiegano questo famoso pattern. Secondo me comunque la scelta migliore se uno vuole conoscere bene il mondo JavaScript e l'utilizzo avanzato dei pattern è leggersi un libro, tra le altre cose italiano, il libro che consiglio è [Javascript best pratices](http://www.jsbestpractices.it/ "Javascript best pratices").
Buona Lettura.
<br/>
<br/>
Stay Tuned!
<br/>
<br/>
*Image Courtesy of [Visit Lego Liberty](http://visitlegoliberty.com/)*