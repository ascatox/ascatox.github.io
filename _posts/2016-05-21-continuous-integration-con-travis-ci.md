---
layout: post
title: Continuous Integration con Travis CI
published: true
categories: Sviluppo software
tags:
- Continuous Integration
---
![Jekyll]({{site.baseurl}}/assets/travis.png)
<h2>Introduzione</h2>
La [Continuous Integration](https://en.wikipedia.org/wiki/Continuous_integration) nel mondo dello sviluppo software moderno è una pratica sempre più diffusa e praticata, che permette di ridurre la possibilità di bug e favorisce la corretta integrazione dei componenti sviluppati da molti programmatori, specie in progetti Software molto grandi.
<!--more-->
<h3><a href="https://travis-ci.org/">Travis CI</a></h3>
Un'attore fondamentale della CI è proprio il server che esegue l'integrazione, ce ne sono molti, quelli più conosciuti sono sicuramente [Jenkins](https://jenkins.io/2.0/), [Hudson CI](http://hudson-ci.org/) ed appunto Travis CI.<br/>
Ma cosa fa effettivamente un server CI.<br/>
Un server CI esegue la [build](https://en.wikipedia.org/wiki/Software_build) di un progetto Software, quindi di solito, preleva l'ultima versione del progetto da un sistema di Version Control, come SVN o GIT ed esegue uno script che può spingersi fino al deploy del software in produzione oppure creare semplicemente dei pacchetti binari pronti per essere installati su una o più macchine di produzione.
<h2>Utilizzare Travis CI con Github</h2>
Una funzionalità particolare che ho scoperto in questi ultimi giorni utilizzando Github come repository GIT per una serie di progetti, è la possibilità di integrare Travis con Github ed eseguire quindi una CI anche per progetti ospitati dal celebre servizio.<br/><br/>
Ovviamente in questo caso non si parla di una fase di deploy ma vengono solo eseguiti una serie di test, in modo da verificare che la build del progetto sia corretta e non ci siano errori presenti, a seguito delle commit effettuate.
<h3>Come fare</h3>
<h4>Registrazione a Travis</h4>
La prima cosa da fare è andare sulla [Homepage](https://travis-ci.org/ "Homepage") del servizio e registrarsi con le proprie credenziali Github.<br/><br/>
Fatto questo è possibile tramite il sito stesso scegliere quali repository presenti nel proprio account, vogliamo collegare al servizio Travis, a questo punto andiamo ad inserire il file di configurazione di Travis nel nostro repository Github.<br/><br/>
![Jekyll]({{site.baseurl}}/assets/travis_ci_choose_repo.png)
<br/>
<h4>Integrazione con Github</h4>
Iniziare l'integrazione è davvero facile basta infatti inserire nella root del proprio progetto un file con nome:  
{%highlight bash%}.travis.yml {%endhighlight%}
All'interno di questo file va specificato il linguaggio con il quale stiamo sviluppando.<br/>
Mi spiego meglio con un'esempio:
{%highlight bash%}language:java {%endhighlight%}
questa è la **configurazione** **automatica**, che come ovvio che sia da moltissime cose per scontate in pieno stile *convention over configuration*.
Le convenzioni per quanto rigurda questo caso specifico con linguaggio Java e che il progetto utilizzi una di queste tecnologie:<br/>
*Oracle JDK 7 (default), Oracle JDK 8, OpenJDK 6, OpenJDK 7, Gradle 2.0, Maven 3.2 and Ant 1.8*

in automatico Travis riconoscerà la JDK usata ed eseguirà i test cercando i file di configurazione di uno dei principali package manager del mondo Java	.<br/>
Se trova infatti il file `build.gradle`, il progetto utilizza [Gradle](http://gradle.org/) quindi Travis lancerà in automatico il comando:
{%highlight bash%}gradle check{%endhighlight%}

se trova il file `pom.xml` ma non il `build.gradle` è un progetto che utilizza [Maven](https://maven.apache.org/) quindi il comando è:

{%highlight bash%}mvn test{%endhighlight%}
altrimenti se non trova nessuno dei due file utilizzerà [Apache Ant](http://ant.apache.org/) e lancerà il comando:


{%highlight bash%}ant test{%endhighlight%}
Tutto questo con una sola riga scritta nel file `.travis.yml`.
Comunque è sempre buona prassi essere più *espliciti* possibili specificando la JDK e lo script con il quale eseguire i test per determinare se la build è corretta o meno.<br/>
Nel mio caso infatti avendo un progetto sviluppato con JDK 1.8 e Maven ho creato un file fatto in questo modo:

{%highlight bash%}
    language: java
    jdk:
      - oraclejdk8
    script: cd ../service && mvn test
{%endhighlight%}
interessante vedere come tramite parametro `script` è possibile lanciare dei comandi per testare la build. Il processo di test partirà subito dopo aver effettuato una commit.
<br/><br/>
Un'ulteriore cosa opzionale che si può aggiungere al proprio progetto Github è il link nel file `README.md` all'immagine relativa allo stato, dell'ultima build testata da Travis, in modo da vedere a colpo d'occhio lo stato del repository.<br/>
[Qui](https://github.com/ascatox/BeInCpps/tree/master/CamService) un'esempio di quello che intendo. 


<h2>Conclusioni</h2>
Quello mostrato è un'esempio molto semplice e limitato per di più legato al mondo Java.<br/>
Travis permette di gestire e testare molte tecnologie, trovate comunque molti [esempi e documentazione](https://docs.travis-ci.com/) nel sito di Travis.<br/><br/>
**Stay Tuned!**
<br/>
<br/>
*Image Courtesy of [Travis CI](https://travis-ci.org/)*
