---
keywords: implementa, implementa, configura, configura, parametro pagina
description: Ottieni dati in [!DNL Target] utilizzando gli attributi del profilo nella pagina.
title: Come posso inserire dati in [!DNL Target] utilizzando attributi di profilo nella pagina?
feature: Implementation
exl-id: c19fd746-21a2-4eb5-8c2a-c24806e09324
TQID: https://experienceleague.adobe.com/jXWNNl7HfrR03tEoMz7KApX3onc1Zc44IAuXy4QF2tU
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: adee20bd-51f4-461d-b9db-d215f8756eeb
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 311
ht-degree: 40%

---

# Attributi di profilo nella pagina

Gli attributi del profilo nella pagina in [!DNL Adobe Target] (detti anche &quot;attributi del profilo nella mbox&quot;) sono coppie nome/valore passate direttamente attraverso il codice della pagina e memorizzate nel profilo del visitatore per utilizzi futuri.

Gli attributi di profilo nella pagina consentono l&#39;archiviazione dei dati specifici dell&#39;utente nel profilo di Target per targeting e segmentazione in un secondo momento.

## Formato

Gli attributi del profilo nella pagina vengono passati in [!DNL Target] tramite una chiamata al server come coppia nome/valore stringa con il prefisso &quot;profile&quot;. prima del nome dell&#39;attributo.

I nomi e i valori degli attributi sono personalizzabili (sebbene esistano alcuni “nomi riservati” per usi specifici).

Di seguito sono riportati alcuni esempi di attributi di profilo nella pagina:

* `profile.membershipLevel=silver`
* `profile.visitCount=3`

## Casi d’uso di esempio

* **Informazioni di accesso**: condividere dati non PII (personalmente identificabili) con [!DNL Target] in base al login dell&#39;utente. Questi dati possono essere lo stato di appartenenza, la cronologia degli ordini o altro ancora.
* **Informazioni sul negozio**: tracciare quale negozio è la posizione preferita dell’utente.
* **Interazioni precedenti**: tenere traccia di ciò che l’utente ha fatto precedentemente sul sito per informare le future personalizzazioni.

## Vantaggi del metodo

I dati vengono inviati a [!DNL Target] in tempo reale e possono essere utilizzati nella stessa chiamata al server in cui vengono inseriti i dati.

## Avvertenze

Richiede aggiornamenti del codice della pagina (direttamente o tramite un sistema di gestione dei tag).

Gli attributi e i valori sono visibili nelle chiamate al server, pertanto un visitatore può visualizzare i valori. Se si condividono informazioni quali fasce di credito o altre informazioni potenzialmente private, questo metodo potrebbe non essere l’approccio migliore.

## Esempi di codice

targetPageParamsAll (aggiunge gli attributi a tutte le chiamate mbox nella pagina):

`function targetPageParamsAll() { return "profile.param1=value1&profile.param2=value2&profile.p3=hello%20world"; }`

targetPageParams (aggiunge gli attributi alla mbox globale nella pagina):

`function targetPageParams() { return profile.param1=value1&profile.param2=value2&profile.p3=hello%20world"; }`

Attributi nel codice mboxCreate:

`<div class="mboxDefault"> default content to replace by offer </div> <script> mboxCreate('mboxName','profile.param1=value1','profile.param2=value2'); </script>`

## Collegamenti alle informazioni rilevanti

[Attributi del profilo](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/profile-parameters.html?lang=it)

[Profilo visitatore](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html?lang=it)
