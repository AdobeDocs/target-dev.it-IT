---
keywords: implementa, implementa, configura, configura, parametro pagina
description: Inserire dati in [!DNL Target] utilizzo degli attributi di profilo nella pagina.
title: Come posso inserire dati in [!DNL Target] Usare Attributi Di Profilo Nella Pagina?
feature: Implementation
exl-id: c19fd746-21a2-4eb5-8c2a-c24806e09324
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '298'
ht-degree: 45%

---

# Attributi di profilo nella pagina

Attributi di profilo nella pagina in [!DNL Adobe Target] (detti anche &quot;attributi di profilo in-mbox&quot;) sono coppie nome/valore passate direttamente attraverso il codice della pagina e memorizzate nel profilo del visitatore per utilizzi futuri.

Gli attributi di profilo nella pagina consentono l&#39;archiviazione dei dati specifici dell&#39;utente nel profilo di Target per targeting e segmentazione in un secondo momento.

## Formato

Gli attributi del profilo nella pagina vengono trasmessi in [!DNL Target] tramite una chiamata al server come coppia nome/valore stringa con il prefisso &quot;profile&quot;. prima del nome dell&#39;attributo.

I nomi e i valori degli attributi sono personalizzabili (sebbene esistano alcuni “nomi riservati” per usi specifici).

Di seguito sono riportati alcuni esempi di attributi di profilo nella pagina:

* `profile.membershipLevel=silver`
* `profile.visitCount=3`

## Casi d’uso di esempio

* **Informazioni di login**[!DNL Target]: condividi dati non-PII (personalmente identificabili) con in base al login dell’utente. Questi dati possono essere lo stato di appartenenza, la cronologia degli ordini o altro ancora.
* **Informazioni sul negozio**: tracciare quale negozio è la posizione preferita dell’utente.
* **Interazioni precedenti**: tenere traccia di ciò che l’utente ha fatto precedentemente sul sito per informare le future personalizzazioni.

## Vantaggi del metodo

I dati vengono inviati a [!DNL Target] in tempo reale, e possono essere utilizzati nella stessa chiamata al server in cui arrivano i dati.

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

[Attributi del profilo](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/profile-parameters.html)

[Profilo visitatore](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html)
