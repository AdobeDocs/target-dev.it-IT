---
keywords: adobe.target.getOffers, getOffers, getoffers, ottieni offerte, at.js, funzioni, funzione, $ 8
description: Utilizza la funzione [!UICONTROL adobe.target.getOffers()] e le relative opzioni per la libreria  [!DNL Adobe Target] at.js per attivare le richieste per ottenere più [!DNL Target] offerte. (at.js 2.x)
title: Come si utilizza la funzione [!UICONTROL adobe.target.getOffers()]?
feature: at.js
exl-id: b96a3018-93eb-49e7-9aed-b27bd9ae073a
source-git-commit: 67cc93cf697f8d5bca6fedb3ae974e4012347a0b
workflow-type: tm+mt
source-wordcount: '1317'
ht-degree: 62%

---

# [!UICONTROL adobe.target.getOffers()] - at.js 2.x

Questa funzione ti consente di recuperare più offerte passando più mbox. Inoltre, è possibile recuperare più offerte per tutte le visualizzazioni nelle attività attive.

>[!NOTE]
>
>Questa funzione è stata introdotta con at.js 2.x e non è disponibile per at.js versione 1.*x*.

| Chiave | Tipo | Obbligatorio | Descrizione |
| --- | --- | --- | --- |
| `consumerId` | Stringa | No | Il valore predefinito è la mbox globale del client, se non specificato. Questa chiave viene utilizzata per generare l’ID dati supplementare (SDID) utilizzato per l’integrazione di A4T.<P>Quando si utilizza `getOffers()`, ogni chiamata genera un nuovo SDID. Se sulla stessa pagina sono presenti più richieste mbox e desideri mantenere l&#39;SDID (in modo che corrisponda all&#39;SDID di target-global-mbox e all&#39;SDID [!DNL Adobe Analytics]), utilizza il parametro `consumerId`.<P>Se `getOffers()` include tre mbox (denominate &quot;mbox1&quot;, &quot;mbox2&quot; e &quot;mbox3&quot;), includere: `consumerId: "mbox1, mbox2, mbox3"` nella chiamata `getOffers()`. |
| `decisioningMethod` | Stringa | No | &quot;lato server&quot;, &quot;su dispositivo&quot;, &quot;ibrido&quot; |
| `request` | Oggetto | Sì | Vedi la tabella Richieste di seguito. |
| `timeout` | Numero | No | Timeout richiesta. Se non specificato, si utilizza il timeout predefinito di at.js. |

## Richiesta

>[!NOTE]
>
>Consulta la [documentazione sulle API di consegna](/help/dev/implement/delivery-api/overview.md) per informazioni sui tipi accettabili per tutti i campi elencati di seguito.

| Nome campo | Obbligatorio | Limitazioni | Descrizione |
| --- | --- | --- | --- |
| request > id | No |  | Uno tra `tntId`, `thirdPartyId` e `marketingCloudVisitorId` è obbligatorio. |
| Request > id > thirdPartyId | No | Dimensione massima = 128. |  |
| Request > experienceCloud | No |  |  |
| Request > experienceCloud > analytics | No |  | Integrazione di Adobe Analytics |
| Request > experienceCloud > analytics > logging | No | I seguenti elementi devono essere implementati sulla pagina:<ul><li>Servizio ID visitatori</li><li>Appmeasurement.js</li></ul> | Sono supportati i seguenti valori:<P>**client_side**: se specificato, verrà restituito un payload di Analytics al chiamante che deve essere utilizzato per inviare a [!UICONTROL Adobe Analytics] tramite [!UICONTROL Data Insertion API].<P>**lato server**: questo è il valore predefinito, in cui il backend [!DNL Target] e [!DNL Analytics] utilizzeranno l&#39;identificatore SDID per unire le chiamate a scopo di reporting. |
| Request > prefetch | No |  |  |
| Request > prefetch > views | No | Numero massimo: 50.<P>Nome non vuoto.<P>Lunghezza nome `<=` 128.<P>Lunghezza valore `<=` 5000.<P>Il nome non deve iniziare con &quot;profile&quot;.<P>Nomi non consentiti: &quot;orderId&quot;, &quot;orderTotal&quot;, &quot;productPurchasedId&quot;. | Passa i parametri da utilizzare per recuperare le visualizzazioni rilevanti nelle attività attive. |
| Request > prefetch > views > profileParameters | No | Numero massimo: 50.<P>Nome non vuoto.<P>Lunghezza nome `<=` 128.<P>Lunghezza valore `<=` 5000.<P>Accetta solo valori String.<P>Il nome non deve iniziare con &quot;profile&quot;. | Passa i parametri di profilo da utilizzare per recuperare le visualizzazioni rilevanti nelle attività attive. |
| Request > prefetch > views > product | No |  |  |
| Request > prefetch > views > product -> id | No | Non vuoto.<P>dimensione massima = 128. | Passa gli ID prodotto da utilizzare per recuperare le visualizzazioni rilevanti nelle attività attive. |
| Request > prefetch > views > product > categoryId | No | Non vuoto.<P>dimensione massima = 128. | Passa gli ID categoria prodotto da utilizzare per recuperare le visualizzazioni rilevanti nelle attività attive. |
| Request > prefetch > views > order | No |  |  |
| Request > prefetch > views > order > id | No | Lunghezza massima = 250. | Passa gli ID ordine da utilizzare per recuperare le visualizzazioni rilevanti nelle attività attive. |
| Request > prefetch > views > order > total | No | Totale `>=` 0. | Passa i totali dell’ordine da utilizzare per recuperare le visualizzazioni rilevanti nelle attività attive. |
| Request > prefetch > views > order > purchasedProductIds | No | Nessun valore vuoto.<P>Lunghezza massima di ogni valore: 50.<P>Concatenato e separato da virgole.<P>Lunghezza totale ID prodotto `<=` 250. | Passa gli ID prodotti acquistati da utilizzare per recuperare le visualizzazioni rilevanti nelle attività attive. |
| Request > execute | No |  |  |
| Request > execute > pageLoad | No |  |  |
| Request > execute > pageLoad > parameters | No | Numero massimo: 50.<P>Nome non vuoto.<P>Lunghezza nome `<=` 128.<P>Lunghezza valore `<=` 5000.<P>Accetta solo valori String.<P>Il nome non deve iniziare con &quot;profile&quot;.<P>Nomi non consentiti: &quot;orderId&quot;, &quot;orderTotal&quot;, &quot;productPurchasedId&quot;. | Al caricamento della pagina, recupera le offerte con i parametri specificati. |
| Request > execute > pageLoad > profileParameters | No | Numero massimo: 50.<P>Nome non vuoto.<P>Lunghezza nome `<=` 128.<P>Lunghezza del valore `<=`256.<P>Il nome non deve iniziare con &quot;profile&quot;.<P>Accetta solo valori String. | Al caricamento della pagina, recupera le offerte con i parametri del profilo specificati. |
| Request > execute > pageLoad > product | No |  |  |
| Request > execute > pageLoad > product -> id | No | Non vuoto.<P>Dimensione massima = 128. | Al caricamento della pagina, recupera le offerte con gli ID prodotto specificati. |
| Request > execute > pageLoad > product > categoryId | No | Non vuoto.<P>Dimensione massima = 128. | Al caricamento della pagina, recupera le offerte con gli ID categoria prodotto specificati. |
| Request > execute > pageLoad > order | No |  |  |
| Request > execute > pageLoad > order > id | No | Lunghezza massima = 250. | Al caricamento della pagina, recupera le offerte con gli ID ordine specificati. |
| Request > execute > pageLoad > order > total | No | `>=` 0 | Al caricamento della pagina, recupera le offerte con i totali dell’ordine specificati. |
| Request > execute > pageLoad > order > purchasedProductIds | No | Nessun valore vuoto.<P>Lunghezza massima di ogni valore: 50.<P>Concatenato e separato da virgole.<P>Lunghezza totale ID prodotto `<=` 250. | Al caricamento della pagina, recupera le offerte con gli ID prodotti acquistati specificati. |
| Request > execute > mboxes | No | Dimensione massima = 50.<P>Nessun elemento null. |  |
| Request > execute > mboxes>mbox | Sì | Non vuoto.<P>Nessun suffisso &quot;-clicked&quot;.<P>Dimensione massima = 250.<P>Caratteri consentiti: `'-, ._\/=:;&!@#$%^&*()_+|?~[]{}'`\|Nome della mbox. |
| Request > execute > mboxes>mbox>index | Sì | Non nulle.<P>Univoco.<P>`>=` 0 | Nota che l’indice non rappresenta l’ordine di elaborazione delle mbox. Come in una pagina web con diverse mbox regionali, non è possibile specificarne l’ordine di elaborazione. |
| Request > execute > mboxes > mbox > parameters | No | Numero massimo = 50.<P>Nome non vuoto.<P>Lunghezza nome `<=` 128.<P>Accetta solo valori String.<P>Lunghezza valore `<=` 5000.<P>Il nome non deve iniziare con &quot;profile&quot;.<P>Nomi non consentiti: &quot;orderId&quot;, &quot;orderTotal&quot;, &quot;productPurchasedId&quot;. | Recupera le offerte per una data mbox con i parametri specificati. |
| Request > execute > mboxes>mbox>profileParameters | No | Numero massimo = 50.<P>Nome non vuoto.<P>Lunghezza nome `<=` 128.<P>Accetta solo valori String.<P>Lunghezza del valore `<=`256.<P>Il nome non deve iniziare con &quot;profile&quot;. | Recupera le offerte per una data mbox con i parametri del profilo specificati. |
| Request > execute > mboxes>mbox > product | No |  |  |
| Request > execute > mboxes > mbox > product > id | No | Non vuoto.<P>Dimensione massima = 128. | Recupera le offerte per una data mbox con gli ID prodotto specificati. |
| Request > execute > mboxes > mbox > product > categoryId | No | Non vuoto.<P>Dimensione massima = 128. | Recupera le offerte per una data mbox con gli ID categoria prodotto specificati. |
| Request > execute > mboxes > mbox > order | No |  |  |
| Request > execute > mboxes>mbox > order > id | No | Lunghezza massima = 250. | Recupera le offerte per una data mbox con gli ID dell’ordine specificati. |
| Request > execute > mboxes > mbox > order > total | No | `>=` 0 | Recupera le offerte per una data mbox con i totali dell’ordine specificati. |
| Request > execute > mboxes > mbox > order > purchasedProductIds | No | Nessun valore vuoto.<P>Lunghezza massima di ogni valore = 50.<P>Concatenato e separato da virgole.<P>Lunghezza totale ID prodotto `<=` 250. | Recupera le offerte per una data mbox con gli ID dei prodotti acquistati dell’ordine specificati. |

## Richiama [!UICONTROL getOffers()] per tutte le visualizzazioni

```javascript {line-numbers="true"}
adobe.target.getOffers({
    request: {
      prefetch: {
        views: [{}]
    }
  }
});
```

## Chiama [!UICONTROL getOffers()] per prendere decisioni su dispositivo

```javascript {line-numbers="true"}
adobe.target.getOffers({ 

  decisioningMethod:"on-device", 
  request: { 
    execute: { 
      mboxes: [ 
        { 
          index: 0, 
          name: "homepage" 
        } 
      ] 
    } 
 } 
}); 
```

## Richiama [!UICONTROL getOffers()] per recuperare le visualizzazioni più recenti con i parametri e i parametri del profilo passati

```javascript {line-numbers="true"}
adobe.target.getOffers({
  request: {
    "prefetch": {
      "views": [
        {
          "parameters": {
            "ad": "1"
          },
          "profileParameters": {
            "age": 23
          }
        }
      ]
    }
  }
});
```

## Richiama [!UICONTROL getOffers()] per recuperare mbox con parametri e parametri di profilo passati.

```javascript {line-numbers="true"}
adobe.target.getOffers({
  request: {
    execute: {
      mboxes: [
        {
          index: 0,
          name: "first-mbox"
        },
        {
          index: 1,
          name: "second-mbox",
          parameters: {
            a: 1
          },
          profileParameters: {
            b: 2
          }
        }
      ]
    }
  }
});
```

## Chiama [!UICONTROL getOffers()] per recuperare il payload di Analytics dal lato client

```javascript {line-numbers="true"}
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

**Risposta**:

```javascript {line-numbers="true"}
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

Il payload può quindi essere inoltrato a [!DNL Adobe Analytics] tramite l&#39;[API di inserimento dati](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md).

## Recupera ed esegui il rendering di dati da più mbox tramite [!UICONTROL getOffers()] e [!UICONTROL applyOffers()]

at.js 2.x consente di recuperare più mbox tramite l’API `[!UICONTROL getOffers()]`. Puoi anche recuperare dati per più mbox e quindi utilizzare `[!UICONTROL applyOffers()]` per eseguire il rendering dei dati in posti diversi identificati da un selettore CSS.

L’esempio seguente mostra una semplice pagina HTML con at.js 2.x implementata:

```html {line-numbers="true"}
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>at.js 2.x, multiple selectors and multiple mboxes</title>
  <script src="at.js"></script>
</head>
<body>
  <div id="container1">Default content 1</div>
  
  <div id="container2">Default content 2</div>

  <div id="container3">Default content 3</div>
</body>
</html>
```

Supponi di disporre di tre contenitori che desideri modificare tramite il contenuto ricevuto da [!DNL Target]. Puoi creare una singola richiesta per tre mbox in cui ogni mbox ha del contenuto da riprodurre nel rispettivo contenitore.

La richiesta e il codice di rendering possono avere il seguente aspetto:

```javascript {line-numbers="true"}
adobe.target.getOffers({
  request: {
    prefetch: {
      mboxes: [
        {
          index: 0,
          name: "mbox1"
        },
        {
          index: 1,
          name: "mbox2"
        },
        {
          index: 2,
          name: "mbox3"
        }
      ]
    }
  }
})
.then(response => {
  // get all mboxes from response
  const mboxes = response.prefetch.mboxes;
  let count = 1;

  mboxes.forEach(el => {
    adobe.target.applyOffers({
      selector: "#container" + count,
      response: {
        prefetch: {
          mboxes: [el]
        }
      }
    });

    count += 1;
  });
});
```

Nella sezione `request > prefetch > mboxes` sono disponibili tre mbox diverse. Riceverai la risposta per ogni mbox da `response > prefetch > mboxes` se la richiesta è stata completata con successo. Una volta che possiedi le risposte e le posizioni da utilizzare per il rendering, puoi richiamare `applyOffers()` per riprodurre il contenuto recuperato da [!DNL Target]. In questo esempio abbiamo la seguente mappatura:

* mbox1 > Selettore CSS #container1
* mbox2 > Selettore CSS #container2
* mbox3 > Selettore CSS #container3

Questo esempio utilizza la variabile count per creare i selettori CSS. In uno scenario reale puoi utilizzare una mappatura diversa tra il selettore CSS e la mbox.

Questo esempio utilizza `prefetch > mboxes`, ma puoi anche utilizzare `execute > mboxes`. Se utilizzi la preacquisizione in `getOffers()`, devi anche usare la preacquisizione nella chiamata di `applyOffers()`.

## Chiamare [!UICONTROL getOffers()] per eseguire un pageLoad

L&#39;esempio seguente mostra come eseguire un pageLoad utilizzando [!UICONTROL getOffers()] con at.js 2.*x*

```javascript {line-numbers="true"}
adobe.target.getOffers({
    request: {
        execute: {
            pageLoad: {
                parameters: {}
            }
        }
    }
});
```
