---
title: Accesso ai token di risposta tramite Adobe Experience Platform Web SDK
description: Scopri come accedere ai token di risposta con  [!DNL Adobe Experience Platform Web SDK].
keywords: personalizzazione;target;adobe target;renderDecisions;sendEvent;decisionScopes;result.Decisions,response tokens;
feature: AEP Web SDK
source-git-commit: f010ca54aac3c2a644a77fb2f88aff1996f6ddfe
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 0%

---

# Accesso ai token di risposta

Il contenuto Personalization restituito da [!DNL Adobe Target] include [token di risposta](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html?lang=it), ovvero dettagli su attività, offerta, esperienza, profilo utente, informazioni geografiche e altro ancora. Questi dettagli possono essere condivisi con strumenti di terze parti o utilizzati per il debug. I token di risposta possono essere configurati nell&#39;interfaccia utente [!DNL Target].

Per accedere a qualsiasi contenuto di personalizzazione, fornisci una funzione di callback durante l’invio di un evento. Questo callback viene richiamato dopo che SDK ha ricevuto una risposta corretta dal server. Al callback viene fornito un oggetto `result`, che potrebbe contenere una proprietà `propositions` contenente eventuali contenuti di personalizzazione restituiti. Di seguito è riportato un esempio di fornitura di una funzione di callback.

```javascript
alloy("sendEvent", {
    renderDecisions: true,
    xdm: {}
  }).then(function(result) {
    if (result.propositions) {
      // Manually render propositions
    }
  });
```

In questo esempio, `result.propositions`, se esiste, è un array contenente proposte di personalizzazione relative all&#39;evento. Consulta [Rendering del contenuto di personalizzazione](https://experienceleague.adobe.com/it/docs/experience-platform/web-sdk/personalization/rendering-personalization-content) per ulteriori informazioni sul contenuto di `result.propositions.`

Supponiamo di voler raccogliere tutti i nomi di attività da tutte le proposte di cui è stato eseguito il rendering automatico dal SDK web e inviarli in un singolo array. È quindi possibile inviare il singolo array a una terza parte. In questo caso:

1. Estrarre le proposte dall&#39;oggetto `result`.
1. Eseguire un ciclo tra le proposte.
1. Determinare se la proposta è stata sottoposta a rendering da SDK.
1. In tal caso, scorri ciclicamente ogni elemento della proposta.
1. Recuperare il nome dell&#39;attività dalla proprietà `meta`, che è un oggetto contenente token di risposta.
1. Invia il nome dell’attività a un array.
1. Invia i nomi delle attività a una terza parte.

Il codice si presenterà come segue:

```javascript
alloy("sendEvent", {
    renderDecisions: true,
    xdm: {}
  }).then(function(result) {
    var activityNames = [];
    propositions.forEach(function(proposition) {
      if (proposition.renderAttempted) {
        proposition.items.forEach(function(item) {
          if (item.meta) {
            // item.meta contains the response tokens.
            var activityName = item.meta["activity.name"];
            // Ignore duplicates
            if (activityNames.indexOf(activityName) === -1) {
              activityNames.push(activityName);
            }
          }
        });
      }
    });
    // Now that activity names are in an array,
    // you can send them to a third party or use
    // them in some other way.
  });
```
