---
title: Confronto di at.js con Experience Platform Web SDK
description: Scopri le funzionalità di at.js rispetto a [!DNL Experience Platform Web SDK].
keywords: target;adobe target;activity.id;experience.id;renderDecisions;decisionScopes;prehiding snippet;vec;Form-Based Experience Composer;xdm;audiences;Decisions;scope;schema;diagramma di sistema;diagramma
feature: AEP Web SDK
exl-id: 31c9722b-5d92-4653-aa20-4183d166c097
source-git-commit: 158c45b824df8d3bd565ac7c654b65f1fd631e2c
workflow-type: tm+mt
source-wordcount: '2006'
ht-degree: 5%

---

# Confronta la libreria at.js con [!DNL Adobe Experience Platform Web SDK]

## Panoramica

Questo articolo fornisce una panoramica delle differenze tra la libreria `at.js` e Experience Platform Web SDK.

## Installazione delle librerie

### Installazione di at.js

[!DNL Adobe] consente ai clienti di scaricare la libreria direttamente dalla scheda [!DNL Adobe Experience Cloud], [!UICONTROL Implementation]. La libreria at.js è personalizzata con impostazioni simili a quelle del cliente: clientCode, imsOrgId, ecc.

### Installazione del Web SDK

La versione predefinita è disponibile su una rete CDN. Puoi fare riferimento alla libreria sulla rete CDN direttamente sulla tua pagina, oppure scaricarla e ospitarla sulla tua infrastruttura. È disponibile in formati minimizzati e non minimizzati. La versione non minimizzata è utile a scopo di debug.

Per ulteriori informazioni, vedere [Installare il Web SDK utilizzando la libreria JavaScript](https://experienceleague.adobe.com/it/docs/experience-platform/web-sdk/install/library).

## Configurazione delle librerie

### Configurazione di at.js

Alla fine di ogni file at.js, viene visualizzata una sezione in cui [!DNL Adobe] crea un&#39;istanza e passa un oggetto impostazione. È personalizzabile; al momento del download, Adobe popola tale sezione con le impostazioni correnti del cliente.

```javascript
window.adobe.target.init(window, document, {
  "clientCode": "demo",
  "imsOrgId": "",
  "serverDomain": "localhost:5000",
  "timeout": 2000,
  "globalMboxName": "target-global-mbox",
  "version": "2.0.0",
  "defaultContentHiddenStyle": "visibility: hidden;",
  "defaultContentVisibleStyle": "visibility: visible;",
  "bodyHiddenStyle": "body {opacity: 0 !important}",
  "bodyHidingEnabled": true,
  "deviceIdLifetime": 63244800000,
  "sessionIdLifetime": 1860000,
  "selectorsPollingTimeout": 5000,
  "visitorApiTimeout": 2000,
  "overrideMboxEdgeServer": false,
  "overrideMboxEdgeServerTimeout": 1860000,
  "optoutEnabled": false,
  "optinEnabled": false,
  "secureOnly": false,
  "supplementalDataIdParamTimeout": 30,
  "authoringScriptUrl": "//cdn.tt.omtrdc.net/cdn/target-vec.js",
  "urlSizeLimit": 2048,
  "endpoint": "/rest/v1/delivery",
  "pageLoadEnabled": true,
  "viewsEnabled": true,
  "analyticsLogging": "server_side",
  "serverState": {},
  "decisioningMethod": "server-side",
  "legacyBrowserSupport":  false
});
```

[Ulteriori informazioni](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md)

### Configurazione di Platform Web SDK

La configurazione per SDK viene eseguita con il comando [`configure`](https://experienceleague.adobe.com/it/docs/experience-platform/web-sdk/commands/configure/overview). Il comando `configure` è *always* chiamato per primo.

## Come richiedere ed eseguire automaticamente il rendering delle offerte di caricamento pagina [!DNL Target]

### Utilizzo di at.js

Utilizzando at.js 2.x, se abiliti l&#39;impostazione `pageLoadEnabled,` la libreria attiva una chiamata a [!DNL Target] Edge con `execute -> pageLoad`. Se tutte le impostazioni sono impostate sui valori predefiniti, non è necessaria alcuna codifica personalizzata. Una volta aggiunto at.js alla pagina e caricato dal browser, viene eseguita una chiamata Edge [!DNL Target].

### Utilizzo di [!DNL PLatform Web SDK]

Il contenuto creato in [!DNL Target] [Compositore esperienza visivo](https://experienceleague.adobe.com/it/docs/target/using/experiences/vec/visual-experience-composer) può essere recuperato e renderizzato automaticamente da SDK.

Per richiedere ed eseguire automaticamente il rendering di [!DNL Target] offerte, utilizza il comando `sendEvent` e imposta l&#39;opzione `renderDecisions` su `true.` In questo modo SDK esegue automaticamente il rendering di qualsiasi contenuto personalizzato idoneo per il rendering automatico.

Esempio:

```javascript
alloy("sendEvent", {
  "renderDecisions": true,
  "xdm": {
    "commerce": {
      "order": {
        "purchaseID": "a8g784hjq1mnp3",
        "purchaseOrderNumber": "VAU3123",
        "currencyCode": "USD",
        "priceTotal": 999.98
      }
    }
  }
});
```

[!DNL Experience Platform Web SDK] invia automaticamente una notifica con le offerte eseguite da [!DNL Platform WEB SDK]. Questo è un esempio di come si presenta un payload di richiesta di notifica:

```json
{
  "events": [{
      "xdm": {
        "_experience": {
          "decisioning": {
            "propositions": [
              {
                "id": "AT:eyJhY3Rpdml0eUlkIjoiMTI3MDE5IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
                "scope": "cart",
                "scopeDetails": {
                  "decisionProvider": "TGT",
                  "activity": {
                    "id": "127019"
                  },
                  "experience": {
                    "id": "0"
                  },
                  "strategies": [
                    {
                      "step": "entry",
                      "algorithmID": "0",
                      "trafficType": "0"
                    },
                    {
                      "step": "display",
                      "algorithmID": "0",
                      "trafficType": "0"
                    }
                  ],
                  "characteristics": {
                    "eventToken": "bKMxJ8dCR1XlPfDCx+2vSGqipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="
                  }
                }
              }
            ]
          }
        },
        "eventType": "display",
        "web": {
          "webPageDetails": {
            "viewName": "cart",
            "URL": "https://alloyio.com/personalizationSpa/cart"
          },
          "webReferrer": {
            "URL": ""
          }
        },
        "device": {
          "screenHeight": 800,
          "screenWidth": 1280,
          "screenOrientation": "landscape"
        },
        "environment": {
          "type": "browser",
          "browserDetails": {
            "viewportWidth": 1280,
            "viewportHeight": 284
          }
        },
        "placeContext": {
          "localTime": "2021-12-10T15:50:34.467+02:00",
          "localTimezoneOffset": -120
        },
        "timestamp": "2021-12-10T13:50:34.467Z",
        "implementationDetails": {
          "name": "https://ns.adobe.com/experience/alloy",
          "version": "2.6.2",
          "environment": "browser"
        }
      }
    }
  ]
}
```

[Ulteriori informazioni](https://experienceleague.adobe.com/it/docs/experience-platform/web-sdk/personalization/rendering-personalization-content)

## Come richiedere ed eseguire il rendering automatico di *NOT* delle offerte di Page Load Target

### Utilizzo di at.js

Esistono due modi per attivare una chiamata ad Edge [!DNL Target] che recupera le offerte per il caricamento della pagina.

Esempio 1:

```javascript
adobe.target.getOffer({
   mbox: "target-global-mbox", 
   success: console.log,
   error: console.error
});
```

Esempio 2:

```javascript
adobe.target.getOffers({
    request: {
      execute: {
        pageLoad: {}
    }
  }
})
.then(console.log)
.catch(console.error);
```

[Ulteriori informazioni](https://experienceleague.adobe.com/it/docs/target-dev/developer/client-side/at-js-implementation/functions-overview/atjs-functions)

### Utilizzo di [!DNL Platform Web SDK]

Eseguire un comando `sendEvent` con un ambito speciale in `decisionScopes`: `__view__`. [!DNL Adobe] utilizza questo ambito come segnale per recuperare tutte le attività di caricamento pagina da [!DNL Target] e preacquisire tutte le visualizzazioni. [!DNL Platform Web SDK] tenta inoltre di valutare tutte le attività basate sulla visualizzazione del Compositore esperienza visivo. La disabilitazione del recupero preventivo delle visualizzazioni non è attualmente supportata in [!DNL Platform Web SDK].

Per accedere a qualsiasi contenuto di personalizzazione, puoi fornire una funzione di callback, che verrà chiamata dopo che SDK avrà ricevuto una risposta corretta dal server. Al callback viene fornito un oggetto risultato, che potrebbe contenere una proprietà proposition contenente eventuali contenuti di personalizzazione restituiti.

Esempio:

```javascript
alloy("sendEvent", {
    xdm: {...},
    decisionScopes: ["__view__"]
  }).then(function(result) {
    if (result.propositions) {
      result.propositions.forEach(proposition => {
        proposition.items.forEach(item => {
          if (item.schema === HTML_SCHEMA) {
            // manually apply offer
            document.getElementById("form-based-offer-container").innerHTML =
              item.data.content;
            const executedPropositions = [
              {
                id: proposition.id,
                scope: proposition.scope,
                scopeDetails: proposition.scopeDetails
              }
            ];
          // manually send the display notification event, so that Target/Analytics impressions aare increased
            alloy("sendEvent",{
              "xdm": {
                "eventType": "decisioning.propositionDisplay",
                "_experience": {
                  "decisioning": {
                    "propositions": executedPropositions
                  }
                }
              }
            });
          }
        });
      });
    }
  });
```

[Ulteriori informazioni](https://experienceleague.adobe.com/it/docs/experience-platform/web-sdk/personalization/rendering-personalization-content)

## Richiedere mbox di Target basate su moduli specifiche

### Utilizzo di at.js

È possibile recuperare le attività utilizzando la funzione `getOffer`:

Esempio 1:

```javascript
adobe.target.getOffer({
   mbox: "hero-banner", 
   success: console.log,
   error: console.error
});
```

Esempio 2:

```javascript
adobe.target.getOffers({
    request: {
      execute: {
        mboxes: [
        {
          index: 0,
          name: "hero-banner"
        }]
    }
  }
})
.then(console.log)
.catch(console.error);
```

[Ulteriori informazioni](https://experienceleague.adobe.com/docs/target-dev/developer/client-side/at-js-implementation/functions-overview/atjs-functions.html?lang=it)

### Utilizzo di [!DNL Platform Web SDK]

È possibile recuperare [!UICONTROL Form-Based Composer] attività basate sul comando `sendEvent` e passare i nomi mbox nell&#39;opzione `decisionScopes`. Il comando `sendEvent` restituisce una promessa che viene risolta con un oggetto contenente le attività/proposte richieste:

Questo frammento di codice è l&#39;aspetto dell&#39;array `propositions`:

```javascript
[
  {
    "id": "AT:eyJhY3Rpdml0eUlkIjoiNDM0Njg5IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
    "scope": "hero-banner",
    "scopeDetails": {
      "decisionProvider": "TGT",
      "activity": {
        "id": "434689"
      },
      "experience": {
        "id": "0"
      },
      "strategies": [
        {
          "algorithmID": "0",
          "trafficType": "0"
        }
      ],
      "characteristics": {
        "eventToken": "2lTS5KA6gj4JuSjOdhqUhGqipfsIHvVzTQxHolz2IpTMromRrB5ztP5VMxjHbs7c6qPG9UF4rvQTJZniWgqbOw=="
      }
    },
    "items": [
      {
        "id": "1184844",
        "schema": "https://ns.adobe.com/personalization/html-content-item",
        "meta": {
          "geo.state": "bucuresti",
          "activity.id": "434689",
          "experience.id": "0",
          "activity.name": "a4t test form based activity",
          "offer.id": "1184844",
          "profile.tntId": "04608610399599289452943468926942466370-pybgfJ"
        },
        "data": {
          "id": "1184844",
          "format": "text/html",
          "content": "<div> analytics impressions </div>"
        }
      }
    ]
  },
  {
    "id": "AT:eyJhY3Rpdml0eUlkIjoiNDM0Njg5IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
    "scope": "hero-banner",
    "scopeDetails": {
      "decisionProvider": "TGT",
      "activity": {
        "id": "434689"
      },
      "characteristics": {
        "eventToken": "E0gb6q1+WyFW3FMbbQJmrg=="
      }
    },
    "items": [
      {
        "id": "434689",
        "schema": "https://ns.adobe.com/personalization/measurement",
        "data": {
          "type": "click",
          "format": "application/vnd.adobe.target.metric"
        }
      }
    ]
  }
]
```

Esempio:

```javascript
alloy("sendEvent", {
  xdm: { ...},
  decisionScopes: ["hero-banner"]
}).then(function (result) {
  var propositions = result.propositions;

  if (propositions) {
    // Find the discount proposition, if it exists.
    for (var i = 0; i < propositions.length; i++) {
      var proposition = propositions[i];
      for (var j = 0; j < proposition.items; j++) {
        var item = proposition.items[j];
        if (item.schema === HTML_SCHEMA) {
          // apply offer
          document.getElementById("form-based-offer-container").innerHTML =
            item.data.content;
          const executedPropositions = [
            {
              id: proposition.id,
              scope: proposition.scope,
              scopeDetails: proposition.scopeDetails
            }
          ];

          alloy("sendEvent", {
            "xdm": {
              "eventType": "decisioning.propositionDisplay",
              "_experience": {
                "decisioning": {
                  "propositions": executedPropositions
                }
              }
            }
          });
        }
      }
    }
  }
});
```

[Ulteriori informazioni](https://experienceleague.adobe.com/it/docs/experience-platform/web-sdk/personalization/rendering-personalization-content)

## Come applicare le attività [!DNL Target]

### Utilizzo di at.js

È possibile applicare le attività [!DNL Target] utilizzando la funzione `applyOffers`: `adobe.target.applyOffer(options).`

Esempio:

```javascript
adobe.target.getOffers({...})
  .then(response => adobe.target.applyOffers({ response: response }))
  .then(() => console.log("Success"))
  .catch(error => console.log("Error", error));
```

Ulteriori informazioni sul comando `applyOffers` sono disponibili nella [documentazione dedicata](https://experienceleague.adobe.com/it/docs/target-dev/developer/client-side/at-js-implementation/functions-overview/adobe-target-applyoffers-atjs-2).

### Utilizzo di [!DNL Platform Web SDK]

È possibile applicare le attività [!DNL Target] utilizzando il comando `applyPropositions`.

Esempio:

```javascript
alloy("applyPropositions", {
    propositions: [...]
});
```

Ulteriori informazioni sul comando `applyPropositions` sono disponibili nella [documentazione dedicata](https://experienceleague.adobe.com/it/docs/experience-platform/web-sdk/personalization/rendering-personalization-content).

## Come tenere traccia degli eventi

### Utilizzo di at.js

È possibile tenere traccia degli eventi utilizzando la funzione `trackEvent` o utilizzando `sendNotifications.`

Questa funzione genera una richiesta per segnalare le azioni dell&#39;utente, ad esempio clic e conversioni. Questa funzione non fornisce attività nella risposta.

**Esempio 1**

```javascript
adobe.target.trackEvent({ 
    "type": "click",
    "mbox": "some-mbox"
});
```

**Esempio 2**

```javascript
adobe.target.sendNotifications({ 
    request: {
       notifications: [{
          ...,
          mbox: {
            name: "some-mbox"
          },
          type: "click",
          ...
       }]
    }
});
```

[Ulteriori informazioni](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/adobe-target-trackevent.html?lang=it)

### Utilizzo di [!DNL Platform Web SDK]

È possibile tenere traccia degli eventi e delle azioni dell&#39;utente chiamando il comando `sendEvent`, popolando `_experience.decisioning.propositions` XDM `fieldgroup` e impostando `eventType` su uno dei due valori seguenti:

* `decisioning.propositionDisplay`: segnala il rendering dell&#39;attività [!DNL Target].
* `decisioning.propositionInteract`: segnala un&#39;interazione dell&#39;utente con l&#39;attività, come un clic del mouse.

L&#39;XDM `_experience.decisioning.propositions` di `fieldgroup` è un array di oggetti. Le proprietà di ciascun oggetto sono derivate da `result.propositions` restituito nel comando `sendEvent`: `{ id, scope, scopeDetails }.`

**Esempio 1 - Traccia un evento `decisioning.propositionDisplay` dopo il rendering di un&#39;attività**

```javascript
alloy("sendEvent", {
  xdm: {},
  decisionScopes: ['discount']
}).then(function(result) {
  var propositions = result.propositions;

  var discountProposition;
  if (propositions) {
    // Find the discount proposition, if it exists.
    for (var i = 0; i < propositions.length; i++) {
      var proposition = propositions[i];
      if (proposition.scope === "discount") {
        discountProposition = proposition;
        break;
      }
    }
  }

  if (discountProposition) {
    // Find the item from proposition that should be rendered.
    // Rather than assuming there a single item that has HTML
    // content, find the first item whose schema indicates
    // it contains HTML content.
    for (var j = 0; j < discountProposition.items.length; j++) {
      var discountPropositionItem = discountProposition.items[i];
      if (discountPropositionItem.schema === "https://ns.adobe.com/personalization/html-content-item") {
        var discountHtml = discountPropositionItem.data.content;
        // Render the content
        var dailySpecialElement = document.getElementById("daily-special");
        dailySpecialElement.innerHTML = discountHtml;
        
        // For this example, we assume there is only a single place to update in the HTML.
        break;  
      }
    }
    // Send a "decisioning.propositionDisplay" event signaling that the proposition has been rendered.
    alloy("sendEvent", {
      "xdm": {
        "eventType": "decisioning.propositionDisplay",
        "_experience": {
          "decisioning": {
            "propositions": [{
              "id": id,
              "scope": scope,
              "scopeDetails": scopeDetails
            }],
            "propositionEventType": {
              "display": 1
            }
          }
        }
      }
    });
  }
});
```

**Esempio 2 - Traccia un evento `decisioning.propositionInteract` dopo che si è verificata una metrica di clic**

```javascript
alloy("sendEvent", {
  xdm: { ...},
  decisionScopes: ["hero-banner"]
}).then(function (result) {
  var propositions = result.propositions;

  if (propositions) {
    // Find the discount proposition, if it exists.
    for (var i = 0; i < propositions.length; i++) {
      var proposition = propositions[i];
      for (var j = 0; j < proposition.items.length; j++) {
        var item = proposition.items[j];

        if (item.schema === "https://ns.adobe.com/personalization/measurement") {
          // add metric to the DOM element
          const button = document.getElementById("form-based-click-metric");

          button.addEventListener("click", event => {
            const executedPropositions = [
              {
                id: proposition.id,
                scope: proposition.scope,
                scopeDetails: proposition.scopeDetails
              }
            ];
            // send the click track event
            alloy("sendEvent", {
              "xdm": {
                "eventType": "decisioning.propositionInteract",
                "_experience": {
                  "decisioning": {
                    "propositions": executedPropositions
                  }
                }
              }
            });
          });
        }
      }
    }
  }
});
```

[Ulteriori informazioni](https://experienceleague.adobe.com/it/docs/experience-platform/web-sdk/personalization/rendering-personalization-content#manual)

**Esempio 3 - Tracciare un evento generato dopo l&#39;esecuzione di un&#39;azione**

Questo esempio tiene traccia di un evento che è stato attivato dopo l’esecuzione di un’azione specifica, ad esempio il clic su un pulsante.
È possibile aggiungere altri parametri personalizzati tramite l&#39;oggetto dati `__adobe.target`.

È inoltre possibile aggiungere l&#39;oggetto XDM `commerce`.

```js
alloy("sendEvent", {
    "xdm": {
        "_experience": {
            "decisioning": {
                "propositions": [
                    {
                        "scope": "orderConfirm" //example scope name
                    }
                ],
                "propositionEventType": {
                    "display": 1
                }
            }
        },
        "eventType": "decisioning.propositionDisplay"
    },
    "commerce": {
        "order": {
            "purchaseID": "a8g784hjq1mnp3",
            "purchaseOrderNumber": "VAU3123",
            "currencyCode": "USD",
            "priceTotal": 999.98
        }
    },
    "data": {
        "__adobe": {
            "target": {
                "pageType": "Order Confirmation",
                "user.categoryId": "Insurance"
            }
        }
    }
})
```

## Come attivare una modifica della visualizzazione in un&#39;applicazione a pagina singola

### Utilizzo di at.js

Utilizzare la funzione `adobe.target.triggerView`. È possibile chiamare questa funzione a ogni caricamento di una nuova pagina o quando si esegue di nuovo il rendering di un componente di una pagina. La funzione `adobe.target.triggerView()` deve essere implementata affinché le applicazioni a pagina singola utilizzino il Compositore esperienza visiva [!UICONTROL Visual Experience Composer] per creare attività [!UICONTROL A/B Test] e [!UICONTROL Experience Targeting] (XT). Se `adobe.target.triggerView()` non è implementato sul sito, non è possibile utilizzare il Compositore esperienza visivo per le applicazioni a pagina singola.

**Esempio**

```javascript
adobe.target.triggerView("homeView")
```

[Ulteriori informazioni](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-triggerview-atjs-2.md)

### Utilizzo di [!DNL Platform Web SDK]

Per attivare o segnalare un&#39;applicazione a pagina singola [!UICONTROL View Change], impostare la proprietà `web.webPageDetails.viewName` nell&#39;opzione `xdm` del comando `sendEvent`. [!DNL Platform Web SDK] controlla la cache di visualizzazione. Se sono presenti offerte per `viewName` specificate in `sendEvent`, le esegue e invia un evento di notifica di visualizzazione.

**Esempio**

```javascript
alloy("sendEvent", {
  renderDecisions: true,
  xdm:{
    web:{
      webPageDetails:{
        viewName: "homeView"
      }
    }
  }
});
```

[Ulteriori informazioni](/help/dev/implement/client-side/aep-web-sdk/spa-implementation.md)

## Come sfruttare [!UICONTROL Response Tokens]

Il contenuto Personalization restituito da [!DNL Target] include [token di risposta](https://experienceleague.adobe.com/it/docs/target/using/administer/response-tokens). I token di risposta sono dettagli su attività, offerta, esperienza, profilo utente, informazioni geografiche e altro ancora. Questi dettagli possono essere condivisi con strumenti di terze parti o utilizzati per il debug. I token di risposta possono essere configurati nell&#39;interfaccia utente [!DNL Target].

### Utilizzo di at.js

Utilizza gli eventi personalizzati di at.js per ascoltare la risposta [!DNL Target] e leggere i token di risposta.

**Esempio**

```javascript
document.addEventListener(adobe.target.event.REQUEST_SUCCEEDED, function(e) { 
  console.log("Request succeeded", e.detail); 
}); 
```

[Ulteriori informazioni](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html?lang=it)

### Utilizzo di [!DNL Platform Web SDK]

>[!IMPORTANT]
>
>Assicurarsi di utilizzare [!DNL Experience Platform Web SDK] versione 2.6.0 o successiva.

I token di risposta vengono restituiti come parte di `propositions` esposti nel risultato del comando `sendEvent`. Ogni proposta contiene un array di `items,` e ogni elemento ha un oggetto `meta` popolato con token di risposta, se abilitati nell&#39;interfaccia utente di amministrazione di [!DNL Target]. [Ulteriori informazioni](https://experienceleague.adobe.com/it/docs/target/using/administer/response-tokens)

**Esempio**

```javascript
alloy("sendEvent", {
    renderDecisions: true,
    xdm: {}
  }).then(function(result) {
    if (result.propositions) {
      // Format of result.propositions:
      /*
        [
            {
                "id": "",
                "scope": "",
                "items": [
                    {
                        "id": "",
                        "schema": "",
                        "data": {},
                        "meta": { // RESPONSE TOKENS
                            "activity.name": ...,
                            "offer.id": ...,
                            "profile.activeActivities": ...
                        }
                    }
                ],
                "scopeDetails": {}
                "renderAttempted": false
            }
        ]
      */
    }
  });
```

[Ulteriori informazioni](/help/dev/implement/client-side/aep-web-sdk/accessing-response-tokens.md)

## Come gestire la visualizzazione momentanea di altri contenuti

### Utilizzo di at.js

Utilizzando at.js puoi gestire la visualizzazione momentanea di altri contenuti impostando `bodyHidingEnabled: true` in modo che at.js si occupi di
nascondere anticipatamente i contenitori personalizzati prima che vengano recuperate e applicate le modifiche DOM.

Le sezioni di pagina che contengono contenuto personalizzato possono essere nascoste anticipatamente eseguendo l&#39;override di at.js `bodyHiddenStyle.`

Per impostazione predefinita `bodyHiddenStyle` nasconde l&#39;intero HTML `body.`

Entrambe le impostazioni possono essere ignorate utilizzando `window.targetGlobalSettings.` `window.targetGlobalSettings` deve essere inserito prima di caricare at.js.

### Utilizzo di [!DNL Platform Web SDK]

Utilizzando [!DNL Platform Web SDK] il cliente può impostare il proprio stile di pre-hiding nel comando di configurazione, come nell&#39;esempio seguente:

```javascript
alloy("configure", {
  datastreamId: "configurationId",
  orgId: "orgId@AdobeOrg",
  debugEnabled: true,
  prehidingStyle: "body { opacity: 0 !important }"
});
```

Durante il caricamento dell&#39;asincronia [!DNL Platform Web SDK], [!DNL Adobe] consiglia di inserire il seguente frammento nella pagina prima di inserire [!DNL Platform Web SDK]:

```html
<script>
  !function(e,a,n,t){
  if (a) return;
  var i=e.head;if(i){
  var o=e.createElement("style");
  o.id="alloy-prehiding",o.innerText=n,i.appendChild(o),
  setTimeout(function(){o.parentNode&&o.parentNode.removeChild(o)},t)}}
  (document, document.location.href.indexOf("adobe_authoring_enabled") !== -1, "body { opacity: 0 !important }", 3000);
</script>
```

## Come viene gestito A4T

### Utilizzo di at.js

Esistono due tipi di registrazione A4T supportati utilizzando at.js:

* Registrazione lato client di Analytics
* Registrazione lato server di Analytics

#### Registrazione lato client di Analytics

**Esempio 1: utilizzo di [!DNL Target] impostazione globale**

La registrazione lato client di Analytics può essere abilitata impostando `analyticsLogging: client_side` nelle impostazioni at.js o ignorando l&#39;oggetto `window.targetglobalSettings`.

Quando questa opzione è impostata, il formato del payload restituito è simile al seguente:

```json
{
  "analytics": {
    "payload": {
      "pe": "tnt",
      "tnta": "167169:0:0|0|100,167169:0:0|2|100,167169:0:0|1|100"
    }
  }
}
```

Il payload può quindi essere inoltrato a [!DNL Analytics] tramite [!DNL &#x200B; Data Insertion API].

Esempio 2: configurazione in ogni funzione `getOffers`:

```javascript
adobe.target.getOffers({
      request: {
        experienceCloud: {
          analytics: {
            logging: "client_side"
          }
        },
        prefetch: {
          mboxes: [{
            index: 0,
            name: "a1-serverside-xt"
          }]
        }
      }
    })
    .then(console.log)
```

Questo frammento di codice mostra l’aspetto del payload di risposta:

```json
{
  "prefetch": {
    "mboxes": [{
      "index": 0,
      "name": "a1-serverside-xt",
      "options": [{
        "content": "<img src=\"http://s7d2.scene7.com/is/image/TargetAdobeTargetMobile/L4242-xt-usa?tm=1490025518668&fit=constrain&hei=491&wid=980&fmt=png-alpha\"/>",
        "type": "html",
        "eventToken": "n/K05qdH0MxsiyH4gX05/2qipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==",
        "responseTokens": {
          "profile.memberlevel": "0",
          "geo.city": "bucharest",
          "activity.id": "167169",
          "experience.name": "USA Experience",
          "geo.country": "romania"
        }
      }],
      "analytics": {
        "payload": {
          "pe": "tnt",
          "tnta": "167169:0:0|0|100,167169:0:0|2|100,167169:0:0|1|100"
        }
      }
    }]
  }
}
```

Il payload [!DNL Analytics] (`tnta` token) deve essere incluso nell&#39;hit [!DNL Analytics] utilizzando [API di inserimento dati](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md).

#### Registrazione lato server di [!DNL Analytics]

La registrazione lato server di [!DNL Analytics] può essere abilitata impostando `analyticsLogging: server_side` nelle impostazioni at.js o ignorando l&#39;oggetto `window.targetglobalSettings`.

I dati scorrono quindi come segue:

![Diagramma che mostra il flusso di lavoro di registrazione lato server di Analytics](/help/dev/implement/client-side/aep-web-sdk/assets/a4t-server-side-atjs.png)

[Ulteriori informazioni](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4timplementation.html?lang=it)

### Utilizzo di [!DNL Platform Web SDK]

Il Web SDK supporta inoltre:

* Registrazione lato client di Analytics
* Registrazione lato server di Analytics

#### Registrazione lato client [!DNL Analytics]

La registrazione lato client di [!DNL Analytics] è abilitata quando [!DNL Adobe Analytics] è disabilitato per la configurazione DataStream.

![Diagramma che mostra il flusso di lavoro di registrazione lato client di Analytics](/help/dev/implement/client-side/aep-web-sdk/assets/analytics-disabled-datastream-config.png)

Il cliente ha accesso al token [!DNL Analytics] (`tnta`) che deve essere condiviso con [!DNL Analytics] utilizzando [Data Insertion API](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md) concatenando il comando `sendEvent` e ripetendo l&#39;iterazione nell&#39;array di proposte risultante.

**Esempio**

```javascript
alloy("sendEvent", {
    "renderDecisions": true,
    "xdm": {
      "web": {
        "webPageDetails": {
          "name": "Home Page"
        }
      }
    }
  }
).then(function (results) {
  var analyticsPayloads = new Set();
  for (var i = 0; i < results.propositions.length; i++) {
    var proposition = results.propositions[i];
    var renderAttempted = proposition.renderAttempted;

    if (renderAttempted === true) {
      var analyticsPayload = getAnalyticsPayload(proposition);
      if (analyticsPayload !== undefined) {
        analyticsPayloads.add(analyticsPayload);
      }
    }
  }
  var analyticsPayloadsToken = concatenateAnalyticsPayloads(analyticsPayloads);
  // send the page view Analytics hit with collected Analytics payload using Data Insertion API
});
```

Ecco un diagramma per mostrare il flusso dei dati quando [!DNL Analytics] Client Side è abilitato:

![Diagramma del flusso dei dati nella registrazione lato client di Analytics](/help/dev/implement/client-side/aep-web-sdk/assets/analytics-client-side-logging.png)

#### Registrazione lato server di [!DNL Analytics]

La registrazione lato server di [!DNL Analytics] è abilitata quando [!DNL Analytics] è abilitato per tale configurazione DataStream.

![Interfaccia utente per gli stream di dati con le impostazioni di Analytics.](/help/dev/implement/client-side/aep-web-sdk/assets/analytics-enabled-datastream-config.png)

Quando la registrazione lato server [!DNL Analytics] è abilitata, il payload A4T deve essere condiviso con [!DNL Analytics] in modo che il reporting di [!DNL Analytics] mostri le impression corrette e le conversioni condivise a livello di Edge Network, in modo che il cliente non debba eseguire alcuna elaborazione aggiuntiva.

Ecco come fluiscono i dati nei sistemi quando è abilitata la registrazione di Analytics lato server:

![Diagramma che mostra il flusso di dati nella registrazione di Analytics lato server](/help/dev/implement/client-side/aep-web-sdk/assets/analytics-server-side-logging.png)

## Come impostare le impostazioni globali di [!DNL Target]

### Utilizzo di at.js

È possibile modificare le impostazioni nella libreria at.js utilizzando `window.targetGlobalSettings,` anziché configurare le impostazioni nell&#39;interfaccia utente di [!DNL Target] o utilizzando le API REST.

La sostituzione deve essere definita prima che at.js sia caricato o in Amministrazione > Implementazione > Modifica impostazioni at.js > Impostazioni codice > Intestazione libreria.

Esempio:

```javascript
window.targetGlobalSettings = {  
   timeout: 200, // using custom timeout  
   visitorApiTimeout: 500, // using custom API timeout  
   enabled: document.location.href.indexOf('https://www.adobe.com') >= 0 // enabled ONLY on adobe.com  
};
```

[Ulteriori informazioni](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/targetgobalsettings.html?lang=it)

### Utilizzo di [!DNL Platform Web SDK]

Questa funzione non è supportata nel Web SDK.

## Come aggiornare gli attributi del profilo [!DNL Target]

### Utilizzo di at.js

**Esempio 1**

```javascript
adobe.target.getOffer({
   mbox: "target-global-mbox",
   params: {
     "profile.name": "test",
     "profile.gender": "female"
   },
   success: console.log,
   error: console.error
});
```

**Esempio 2**

```javascript
adobe.target.getOffers({
    request: {
      execute: {
        pageLoad: {
          profileParameters: {
            name: "test",
            gender: "female"
          }
        }
    }
  }
})
.then(console.log)
.catch(console.error);
```

### Utilizzo di [!DNL Platform Web SDK]

Per aggiornare un profilo [!DNL Target], utilizzare il comando `sendEvent` e impostare la proprietà `data.__adobe.target`, con prefisso per i nomi delle chiavi `profile.`

**Esempio**

```javascript
alloy("sendEvent", {
  renderDecisions: true,
  data: {
    __adobe: {
      target: {
        "profile.gender": "female",
        "profile.age": 30
      }
    }
  }
});
```

## Come si utilizza [!DNL Target Recommendations]

### Utilizzo di at.js

**Esempio 1**

```javascript
adobe.target.getOffer({
   mbox: "target-global-mbox",
   params: {
     "entity.name": "T-shirt",
     "entity.id": "1234"
   },
   success: console.log,
   error: console.error
});
```

**Esempio 2**

```javascript
adobe.target.getOffers({
    request: {
      execute: {
        pageLoad: {
          parameters: {
            "entity.name": "T-shirt",
            "entity.id": "1234"
          }
        }
    }
  }
})
.then(console.log)
.catch(console.error);
```

[Ulteriori informazioni](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/adobe-target-getoffers-atjs-2.html?lang=it)

### Utilizzo di [!DNL Platform Web SDK]

Per inviare i dati di [!DNL Recommendations], utilizzare il comando `sendEvent` e impostare la proprietà `data.__adobe.target`, con prefisso per i nomi delle chiavi `entity.`

**Esempio**

```javascript
alloy("sendEvent", {
  renderDecisions: true,
  data: {
    __adobe: {
      target: {
        "entity.name": "T-shirt",
        "entity.id": "1234"
      }
    }
  }
});
```

## Come si utilizzano gli ID di terze parti

### Utilizzo di at.js

Utilizzando at.js è possibile inviare `mbox3rdPartyId` in diversi modi, utilizzando `getOffer,` o `getOffers`:

**Esempio 1**

```javascript
adobe.target.getOffer({
  mbox:"test",
  params:{
    "mbox3rdPartyId": "1234"
  },
  success: console.log,
  error: console.error
});
```

**Esempio 2**

```javascript
adobe.target.getOffers({
    request: {
      id:{
        thirdPartyId: "1234"
      },
      execute: {
        pageLoad: {}
    }
  }
})
.then(console.log)
.catch(console.error);
```

Oppure è possibile impostare `mbox3rdPartyId` in `targetPageParams` o `targetPageParamsAll.`

Durante l&#39;impostazione di `targetPageParams`, invia le richieste per `target-global-mbox` note anche come `pag-lLoad`.

Il consiglio deve essere impostato utilizzando `targetPageParamsAll` in quanto verrà inviato in ogni richiesta di [!DNL Target]. Il vantaggio dell&#39;utilizzo di `targetPageParamsAll` è che è possibile definire `mbox3rdPartyId` sulla pagina una volta per assicurarsi che tutte le [!DNL Target] richieste abbiano il diritto `mbox3rdPartyId.`

```javascript
window.targetPageParamsAll = function() {
      return {
        "mbox3rdPartyId": "1234"
      };
    };
```

```javascript
window.targetPageParams = function() {
  return {
    "mbox3rdPartyId": "1234"
  };
};
```

[Ulteriori informazioni](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/targetpageparams.html?lang=it)

### Utilizzo di [!DNL Platform Web SDK]

[!DNL Platform Web SDK] supporta [!DNL Target] ID di terze parti. Tuttavia, richiede alcuni passaggi aggiuntivi.

Identity Map consente ai clienti di inviare più identità. Tutte le identità sono namespace. Ogni spazio dei nomi può avere una o più identità. Una particolare identità può essere contrassegnata come primaria. Tenendo conto di queste informazioni, puoi visualizzare i passaggi necessari per impostare [!DNL Platform Web SDK] per l&#39;utilizzo dell&#39;ID di terze parti [!DNL Target].

1. Imposta lo spazio dei nomi che contiene l&#39;ID di terze parti [!DNL Target] nella pagina di configurazione dello stream di dati:

![Interfaccia utente Datastreams con il campo spazio dei nomi dell&#39;ID di terze parti di Target](/help/dev/implement/client-side/aep-web-sdk/assets/mbox3rdpartyid.png)

1. Invia lo spazio dei nomi dell&#39;identità in ogni comando `sendEvent` come segue:

```javascript
alloy("sendEvent", {
  "renderDecisions": true,
  "xdm": {
    "identityMap": {
      "TGT3PID": [
        {
          "id": "1234",
          "primary": true
        }
      ]
    }
  }
});
```

## Come si impostano i token di proprietà

### Utilizzo di at.js

Utilizzando at.js è possibile configurare i token di proprietà in due modi, utilizzando `targetPageParams` o `targetPageParamsAll.` Utilizzando `targetPageParams` il token di proprietà viene aggiunto alla chiamata `target-global-mbox`, ma utilizzando `targetPageParamsAll` il token viene aggiunto a tutte le chiamate [!DNL Target]:

**Esempio 1**

```javascript
   window.targetPageParamsAll = function() {
      return {
        "at_property": "1234"
      };
    };
```

**Esempio 2**

```javascript
window.targetPageParams = function() {
      return {
        "at_property": "1234"
      };
    };
```

### Utilizzo di [!DNL Platform Web SDK]

Utilizzando [!DNL Platform Web SDK] i clienti possono impostare la proprietà a un livello più alto, durante la configurazione dello stream di dati, nello spazio dei nomi [!DNL Adobe Target]:

![Interfaccia utente Datastreams con le impostazioni di Adobe Target.](/help/dev/implement/client-side/aep-web-sdk/assets/at-property-setup.png)

Ciò significa che ogni chiamata [!DNL Target] per quella specifica configurazione del flusso di dati contiene quel token di proprietà.

## Come posso preacquisire gli elementi mbox

### Utilizzo di at.js

Questa funzionalità è disponibile solo in at.js 2.x. In at.js 2.x è presente una nuova funzione denominata `getOffers`. La funzione `getOffers` consente ai clienti di preacquisire il contenuto per una o più mbox. Ecco un esempio:

```javascript
adobe.target.getOffers({
    request: {
      prefetch: {
        mboxes: [{
          index: 0,
          name: "test-mbox",
          parameters: {
            ...
          },
          profileParameters: {
            ...
          }
        }]
    }
  }
})
.then(console.log)
.catch(console.error);
```

>[!NOTE]
>
>Adobe consiglia di verificare che ogni `mbox` nell&#39;array `mboxes` abbia il proprio indice. Di solito la prima mbox ha `index=0` la successiva `index=1,` e così via.

### Utilizzo di [!DNL Platform Web SDK]

Questa funzionalità non è attualmente supportata in [!DNL Platform Web SDK].

## Come si esegue il debug dell&#39;implementazione di [!DNL Target]

### Utilizzo di at.js

La libreria at.js espone le seguenti funzioni di debug:

* Disabilita mbox: disabilita il recupero e il rendering di [!DNL Target] per verificare se la pagina è interrotta senza [!DNL Target] interazioni
* Debug mbox: at.js registra ogni azione
* Traccia di destinazione: con un token di traccia mbox generato in un oggetto di traccia con dettagli che hanno partecipato al processo decisionale, è disponibile nell&#39;oggetto `window.___target_trace`.

>[!NOTE]
>
>Tutte queste funzionalità di debug sono disponibili con funzionalità avanzate in [Adobe Experience Platform Debugger](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob).

### Utilizzo di [!DNL Platform Web SDK]

Sono disponibili più funzionalità di debug quando si utilizza [!DNL Platform Web SDK]:

* Utilizzo di [Assurance](https://experienceleague.adobe.com/it/docs/experience-platform/assurance/home)
* [Debug di Web SDK abilitato](https://experienceleague.adobe.com/it/docs/experience-platform/assurance/home)
* Utilizza [hook di monitoraggio di Web SDK](https://github.com/adobe/alloy/wiki/Monitoring-Hooks)
* Usa [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/it/docs/experience-platform/debugger/home)
* Traccia di destinazione
