---
keywords: implementare, implementare, configurare, configurare, parametri di pagina
description: Inserire dati in [!DNL Target] utilizzo dei parametri di pagina.
title: Come posso inserire dati in [!DNL Target] Utilizzare i parametri di pagina?
feature: Implementation
exl-id: 9bb7157e-a938-4150-8a15-c9bf0a0e2296
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 38%

---

# Parametri di pagina

I parametri di pagina (detti anche &quot;parametri mbox&quot;) sono coppie nome/valore passate direttamente attraverso il codice della pagina e non memorizzate nel profilo del visitatore per utilizzi futuri.

I parametri di pagina sono utili per inviare i dati di pagina a [!DNL Adobe Target] che non deve essere memorizzato con il profilo del visitatore per utilizzi futuri di targeting. Questi valori vengono invece utilizzati per descrivere la pagina o l&#39;azione che l&#39;utente ha assunto nella pagina specifica.

## Formato

I parametri di pagina vengono trasmessi in [!DNL Target] tramite una chiamata al server come coppia nome stringa/valore. I nomi dei parametri e i valori sono personalizzabili (anche se ci sono alcuni “nomi riservati” per usi specifici).

Ecco alcuni esempi di parametri di pagina

* `page=productPage`

* `categoryId=homeLoans`

## Casi d’uso di esempio

* **Pagine prodotto**: invia informazioni sul prodotto specifico visualizzato (questo metodo corrisponde al funzionamento di Recommendations)
* **Dettagli ordine**: invia ID ordine, orderTotal e così via, per la raccolta dell’ordine
* **Affinità tra categorie**[!DNL Target]: invia informazioni visualizzate per categoria a per conoscere l’affinità dell’utente a particolari categorie di siti
* **Dati di terze parti**: invia informazioni da fonti di dati di terze parti, ad esempio i provider di targeting del meteo, i dati dell’account (DemandBase), i dati demografici (ad esempio Experian) e altro ancora.

## Vantaggi del metodo

I dati vengono inviati a [!DNL Target] in tempo reale, e possono essere utilizzati nella stessa chiamata al server dei dati in cui vengono inseriti.

## Avvertenze

* Richiede l&#39;aggiornamento del codice della pagina (direttamente o tramite un sistema di gestione dei tag).
* Se i dati devono essere utilizzati per il targeting in una chiamata successiva alla pagina o al server, devono essere tradotti in uno script di profilo.
* Le stringhe di query possono contenere solo caratteri che rispettano lo [standard Internet Engineering Task Force (IETF)](https://www.ietf.org/rfc/rfc3986.txt).

  Oltre ai caratteri menzionati sul sito dell&#39;IETF, [!DNL Target] consente i seguenti caratteri nelle stringhe di query:

  ```< > # % " { } | \ ^ [ ] ` ``` {line-numbers=&quot;true&quot;}

  Tutto il resto deve avere la codifica URL. Lo standard specifica il seguente formato ( [https://www.ietf.org/rfc/rfc1738.txt](https://www.ietf.org/rfc/rfc1738.txt) ), come illustrato di seguito:

  ![immagine alt](assets/ietf1.png)

  Oppure, l&#39;elenco completo per semplicità:

  ![immagine alt](assets/ietf2.png)

## Esempi di codice

targetPageParamsAll (aggiunge i parametri a tutte le chiamate mbox nella pagina):

`function targetPageParamsAll() { return "param1=value1&param2=value2&p3=hello%20world";`

targetPageParams (aggiunge i parametri alla MBOX globale nella pagina):

`function targetPageParams() { return "param1=value1&param2=value2&p3=hello%20world";`

## Collegamenti alle informazioni rilevanti

Consigli: [Implementazione in base al tipo di pagina](https://experienceleague.adobe.com/docs/target/using/recommendations/plan-implement.html)

Conferma ordine: [Tracciare le conversioni](../../implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md#track-conversions)

Affinità tra categorie: [Affinità tra categorie](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/category-affinity.html)
