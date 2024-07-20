---
title: Identificazione utente e bucket
description: Identificazione utente e bucket
exl-id: 4fcf235b-6a58-442c-ae13-9d05ec1033fc
feature: Implement Server-side
source-git-commit: 09a50aa67ccd5c687244a85caad24df56c0d78f5
workflow-type: tm+mt
source-wordcount: '1130'
ht-degree: 3%

---

# Identificazione utente e bucket

## Identificazione utente

Un utente può essere identificato in più modi in [!DNL Adobe Target]. [!UICONTROL Target] utilizza i seguenti identificatori:

| Nome campo | Descrizione |
| --- | --- |
| `tntID` | `tntId` è l&#39;identificatore primario in [!DNL Target] per un utente. È possibile specificare questo ID oppure [!DNL Target] lo genererà automaticamente se la richiesta non ne contiene uno. |
| `thirdPartyId` | `thirdPartyId` è l&#39;identificatore dell&#39;utente della tua società, che puoi inviare con ogni chiamata. Quando un utente accede al sito di un’azienda, l’azienda in genere crea un ID associato all’account del visitatore, alla carta fedeltà, al numero di iscrizione o ad altri identificatori applicabili per l’azienda. |
| `marketingCloudVisitorId` | `marketingCloudVisitorId` viene utilizzato per unire e condividere dati tra diverse soluzioni Adobe. MarketingCloudVisitorId è richiesto per le integrazioni con Adobe Analytics e Adobe Audience Manager. |
| `customerIds` | Oltre all&#39;ID visitatore Experience Cloud, è possibile utilizzare [ID cliente](https://experienceleague.adobe.com/docs/id-service/using/reference/authenticated-state.html) aggiuntivi e uno stato autenticato per ogni visitatore. |

## ID [!DNL Target] (tntID)

L&#39;ID [!DNL Target], o `tntId`, può essere considerato un ID dispositivo. `tntId` viene generato automaticamente da [!DNL Adobe Target] se non viene fornito nella richiesta. Le richieste successive devono includere `tntId` per consentire la distribuzione del contenuto corretto a un dispositivo utilizzato dallo stesso utente.

La seguente chiamata di esempio dimostra una situazione in cui `tntId` non viene passato a [!DNL Target].

>[!BEGINTABS]

>[!TAB SDK di Node.js]

```javascript {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");

const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
  request: {
    execute: {
      mboxes: [{
        name: "some-mbox"
      }]
    }
  }
})
.then(console.log)
.catch(console.error);
```

>[!TAB SDK Java]

```java {line-numbers="true"}
ClientConfig config = ClientConfig.builder()
  .client("acmeclient")
  .organizationId("1234567890@AdobeOrg")
  .build();
TargetClient targetClient = TargetClient.create(config);

Context context = new Context().channel(ChannelType.WEB);
MboxRequest mbox = new MboxRequest()
  .name("some-mbox")
  .index(0);
ExecuteRequest executeRequest = new ExecuteRequest()
  .mboxes(Arrays.asList(mbox));

TargetDeliveryRequest request = TargetDeliveryRequest.builder()
  .context(context)
  .execute(executeRequest)
  .build();

TargetDeliveryResponse offers = targetClient.getOffers(request);
```

>[!ENDTABS]

In assenza di un `tntId`, [!DNL Adobe Target] genera un `tntId` e lo fornisce nella risposta, come segue.

```json {line-numbers="true"}
{
  "status": 200,
  "requestId": "5b586f83-890c-46ae-93a2-610b1caa43ef",
  "client": "acmeclient",
  "id": {
      "tntId": "10abf6304b2714215b1fd39a870f01afc.35_0"
  },
  "edgeHost": "mboxedge35.tt.omtrdc.net",
  ...
}
```

In questo esempio `tntId` generato è `10abf6304b2714215b1fd39a870f01afc.35_0`. Nota: `tntId` deve essere utilizzato per lo stesso utente in più sessioni.

## ID di terze parti (thirdPartyId)

Se l&#39;organizzazione utilizza un ID per identificare il visitatore, è possibile utilizzare `thirdPartyID` per distribuire il contenuto. Un `thirdPartyID` è un ID persistente utilizzato dalla tua azienda per identificare un utente finale, indipendentemente dal fatto che interagisca con la tua azienda dai canali web, mobili o IoT. In altre parole, `thirdPartyId` fa riferimento ai dati del profilo utente che possono essere utilizzati nei diversi canali. È tuttavia necessario fornire `thirdPartyID` per ogni chiamata API di consegna [!DNL Adobe Target] effettuata.

La chiamata di esempio seguente illustra l&#39;utilizzo di `thirdPartyId`.

>[!BEGINTABS]

>[!TAB SDK di Node.js]

```javascript {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");

const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
  request: {
    id: {
      thirdPartyId: "B234A029348"
    },
    execute: {
      mboxes: [{
        name: "some-mbox"
      }]
    }
  }
})
.then(console.log)
.catch(console.error);
```

>[!TAB SDK Java]

```java {line-numbers="true"}
ClientConfig config = ClientConfig.builder()
  .client("acmeclient")
  .organizationId("1234567890@AdobeOrg")
  .build();
TargetClient targetClient = TargetClient.create(config);

VisitorId id = new VisitorId()
  .thirdPartyId("B234A029348");
Context context = new Context().channel(ChannelType.WEB);
MboxRequest mbox = new MboxRequest()
  .name("some-mbox")
  .index(0);
ExecuteRequest executeRequest = new ExecuteRequest()
  .mboxes(Arrays.asList(mbox));

TargetDeliveryRequest request = TargetDeliveryRequest.builder()
  .context(context)
  .execute(executeRequest)
  .build();

TargetDeliveryResponse offers = targetClient.getOffers(request);
```

>[!ENDTABS]

In questo scenario, [!DNL Adobe Target] genererà un `tntId` poiché non è stato passato nella chiamata originale, che verrà mappato al `thirdPartyId` fornito.

## ID visitatore Marketing Cloud (marketingCloudVisitorId)

`marketingCloudVisitorId` è un ID universale e costante che identifica i visitatori in tutte le soluzioni di Adobe Experience Cloud. Quando l&#39;organizzazione implementa il servizio ID, questo ID consente di identificare lo stesso visitatore del sito e i relativi dati in diverse soluzioni di Experience Cloud, tra cui [!DNL Adobe Target], Adobe Analytics e Adobe Audience Manager. Si noti che `marketingCloudVisitorId` è obbligatorio per l&#39;integrazione di [!DNL Target] con [!DNL Adobe Analytics] e [!DNL Adobe Audience Manager].

La seguente chiamata di esempio dimostra come un `marketingCloudVisitorId` recuperato dal servizio ID Experience Cloud viene passato a [!DNL Target].

>[!BEGINTABS]

>[!TAB SDK di Node.js]

```javascript {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");

const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
  request: {
    id: {
      marketingCloudVisitorId: "10527837386392355901041112038610706884"
    },
    execute: {
      mboxes: [{
        name: "some-mbox"
      }]
    }
  }
})
.then(console.log)
.catch(console.error);
```

>[!TAB SDK Java]

```java {line-numbers="true"}
ClientConfig config = ClientConfig.builder()
  .client("acmeclient")
  .organizationId("1234567890@AdobeOrg")
  .build();
TargetClient targetClient = TargetClient.create(config);

VisitorId id = new VisitorId()
  .marketingCloudVisitorId("10527837386392355901041112038610706884");
Context context = new Context().channel(ChannelType.WEB);
MboxRequest mbox = new MboxRequest()
  .name("some-mbox")
  .index(0);
ExecuteRequest executeRequest = new ExecuteRequest()
  .mboxes(Arrays.asList(mbox));

TargetDeliveryRequest request = TargetDeliveryRequest.builder()
  .context(context)
  .execute(executeRequest)
  .build();

TargetDeliveryResponse offers = targetClient.getOffers(request);
```

>[!ENDTABS]

In questo scenario, [!DNL Target] genererà un `tntId` poiché non è stato passato nella chiamata originale, che verrà mappato al `marketingCloudVisitorId` fornito.

## ID cliente (customerIds)

[Gli ID cliente](https://experienceleague.adobe.com/docs/id-service/using/reference/authenticated-state.html) possono essere aggiunti o associati a un ID visitatore di Experience Cloud. Ogni volta che si invia `customerIds`, è necessario specificare anche `marketingCloudVisitorId`. Inoltre, è possibile fornire uno stato di autenticazione insieme a ogni `customerId` per ogni visitatore. È possibile utilizzare i seguenti stati di autenticazione:

| Stato di autenticazione | Stato dell&#39;utente |
| --- | --- |
| `unknown` | Utente sconosciuto o mai autenticato. Questo stato può essere utilizzato per scenari come quello in cui un visitatore arriva sul sito facendo clic su un annuncio pubblicitario. |
| `authenticated` | Al momento l&#39;utente è autenticato con una sessione attiva sul sito Web o sull&#39;app. |
| `logged_out` | L&#39;utente si è autenticato ma si è disconnesso in modo attivo. L&#39;utente intendeva disconnettersi dallo stato autenticato. L&#39;utente non desidera più essere trattato come utente autenticato. |

Tieni presente che solo quando `customerId` è in uno stato autenticato [!DNL Target] farà riferimento ai dati del profilo utente memorizzati e collegati al customerId. Se `customerId` è in uno stato sconosciuto o `logged_out`, verrà ignorato e tutti i dati del profilo utente eventualmente associati a tale `customerId` non verranno utilizzati per il targeting del pubblico.

>[!BEGINTABS]

>[!TAB SDK di Node.js]

```javascript {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");

const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
  request: {
    id: {
      marketingCloudVisitorId : "10527837386392355901041112038610706884",
      customerIds: [{
        id: "134325423",
        integrationCode : "crm_data",
        authenticatedState : "authenticated"
      }]
    },
    execute: {
      mboxes: [{
        name: "some-mbox"
      }]
    }
  }
})
.then(console.log)
.catch(console.error);
```

>[!TAB SDK Java]

```java {line-numbers="true"}
ClientConfig config = ClientConfig.builder()
  .client("acmeclient")
  .organizationId("1234567890@AdobeOrg")
  .build();
TargetClient targetClient = TargetClient.create(config);

CustomerId customerId = new CustomerId()
  .id("134325423")
  .integrationCode("crm_data")
  .authenticatedState(AuthenticatedState.AUTHENTICATED);
VisitorId id = new VisitorId()
  .marketingCloudVisitorId("10527837386392355901041112038610706884")
  .customerIds(Arrays.asList(customerId));
Context context = new Context().channel(ChannelType.WEB);
MboxRequest mbox = new MboxRequest()
  .name("some-mbox")
  .index(0);
ExecuteRequest executeRequest = new ExecuteRequest()
  .mboxes(Arrays.asList(mbox));

TargetDeliveryRequest request = TargetDeliveryRequest.builder()
  .context(context)
  .execute(executeRequest)
  .build();

TargetDeliveryResponse offers = targetClient.getOffers(request);
```

>[!ENDTABS]

L&#39;esempio precedente illustra come inviare un `customerId` con un `authenticatedState`. Quando si invia un `customerId`, sono necessari `integrationCode`, `id` e `authenticatedState` e `marketingCloudVisitorId`. `integrationCode` è l&#39;alias del [file degli attributi del cliente](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/working-with-customer-attributes.html?lang=it) fornito tramite CRS.

## Profilo unito

È possibile combinare `tntId`, `thirdPartyID` e `marketingCloudVisitorId` nella stessa richiesta. In questo scenario, [!DNL Adobe Target] manterrà la mappatura di tutti questi ID e lo fisserà a un visitatore. Scopri come unire e sincronizzare in tempo reale [i profili](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/3rd-party-id.html) utilizzando i diversi identificatori.

>[!BEGINTABS]

>[!TAB SDK di Node.js]

```javascript {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");

const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
  request: {
    id: {
      tntId: "d359234570e044f14e1faeeba02d6ab23439914e.35_0",
      thirdPartyId: "B234A029348",
      marketingCloudVisitorId : "10527837386392355901041112038610706884"
    },
    execute: {
      mboxes: [{
        name: "some-mbox"
      }]
    }
  }
})
.then(console.log)
.catch(console.error);
```

>[!TAB SDK Java]

```java {line-numbers="true"}
ClientConfig config = ClientConfig.builder()
  .client("acmeclient")
  .organizationId("1234567890@AdobeOrg")
  .build();
TargetClient targetClient = TargetClient.create(config);

VisitorId id = new VisitorId()
  .tntId("d359234570e044f14e1faeeba02d6ab23439914e.35_0")
  .thirdPartyId("B234A029348")
  .marketingCloudVisitorId("10527837386392355901041112038610706884");
Context context = new Context().channel(ChannelType.WEB);
MboxRequest mbox = new MboxRequest()
  .name("some-mbox")
  .index(0);
ExecuteRequest executeRequest = new ExecuteRequest()
  .mboxes(Arrays.asList(mbox));

TargetDeliveryRequest request = TargetDeliveryRequest.builder()
  .context(context)
  .execute(executeRequest)
  .build();

TargetDeliveryResponse offers = targetClient.getOffers(request);
```

>[!ENDTABS]

L&#39;esempio precedente illustra come combinare `tntId`, `thirdPartyID` e `marketingCloudVisitorId` nella stessa richiesta.

## Bucket

Gli utenti sono inseriti nel gruppo di visualizzazione di un&#39;esperienza in base alla configurazione delle attività di [!DNL Adobe Target]. In [!DNL Adobe Target], bucket è:

* **Deterministico**: MurmurHash3 viene utilizzato per garantire che l&#39;utente sia inserito nel bucket e veda la giusta variante ogni volta, purché l&#39;ID utente sia coerente.
* **Sticky**: [!DNL Adobe Target] memorizza la variante visualizzata dall&#39;utente nel profilo utente per garantire che la variante venga mostrata in modo coerente all&#39;utente in tutte le sessioni e i canali. Le varianti e la fedeltà sono garantite quando si utilizzano decisioni lato server. Quando si utilizzano decisioni su dispositivo, la fedeltà non è garantita.

## Flusso di lavoro di bucket end-to-end

Prima di immergerti nell’algoritmo di bucket effettivo, è importante sottolineare che passaggi simili vengono utilizzati sia per selezionare le attività in base alla loro percentuale di allocazione del traffico, sia per selezionare un’esperienza all’interno di un’attività.

### Passaggi per la selezione di attività

1. Generare un ID dispositivo, in genere un UUID
1. Ottieni il codice client
1. Ottieni l’ID attività
1. Prendi il sale, che di solito è una stringa come &quot;attività&quot;
1. Calcola l’hash utilizzando MurmurHash3
1. Ottieni il valore assoluto dell’hash
1. Dividi il valore assoluto dell’hash per 10000
1. Dividere il resto per 10000, che dovrebbe produrre un valore compreso tra 0 e 1
1. Moltiplica il risultato per 100%
1. Confronta la percentuale di allocazione del traffico dell’attività con la percentuale ottenuta. Se la percentuale di allocazione del traffico è inferiore, l’attività viene selezionata. In caso contrario, l’attività viene saltata.

### Passaggi per la selezione dell’esperienza

1. Generare un ID dispositivo, in genere un UUID
1. Ottieni il codice client
1. Ottieni l’ID attività
1. Prendi il sale, che di solito è una stringa come &quot;esperienza&quot;
1. Calcola l’hash utilizzando MurmurHash3
1. Ottieni il valore assoluto dell’hash
1. Dividi il valore assoluto dell’hash per 10000
1. Dividere il resto per 10000, che dovrebbe produrre un valore compreso tra 0 e 1
1. Moltiplica il risultato per il numero totale di esperienze all’interno dell’attività
1. Arrotondare il risultato. Questo dovrebbe produrre l’indice dell’esperienza.

### Esempio

Si supponga quanto segue:

* Client C con codice client `acmeclient`
* Attività A con ID `1111` e tre esperienze `E1`, `E2`, `E3`
* Le esperienze hanno la seguente distribuzione: `E1` - 33%, `E2` - 33%, `E3` - 34%

Il flusso di selezione si presenta così:

1. ID dispositivo `702ff4d0-83b1-4e2e-a0a6-22cbe460eb15`
1. Codice client `acmeclient`
1. ID attività `1111`
1. Sale `experience`
1. Valore da hash `acmeclient.1111.702ff4d0-83b1-4e2e-a0a6-22cbe460eb15.experience`, valore hash `-919077116`
1. Valore assoluto dell&#39;hash `919077116`
1. Rimanente dopo la divisione per 10000, `7116`
1. Il valore dopo il resto è diviso per 10000, `0.7116`
1. Risultato dopo la moltiplicazione del valore per il numero totale di esperienze `3 * 0.7116 = 2.1348`
1. L&#39;indice di esperienza è `2`, ovvero la terza esperienza, poiché viene utilizzata l&#39;indicizzazione basata su `0`.
