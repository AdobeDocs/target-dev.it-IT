---
title: Integrazione con la generazione di rapporti Experience Cloud A4T
description: Integrazione con Experience Cloud, Reporting A4T, Integrazione con Analytics for Target
keywords: api di consegna, lato server, lato server, integrazione, a4t
exl-id: 0d09d7a1-528d-4e6a-bc6c-f7ccd61f5b75
feature: Implement Server-side
source-git-commit: 09a50aa67ccd5c687244a85caad24df56c0d78f5
workflow-type: tm+mt
source-wordcount: '360'
ht-degree: 6%

---

# Reporting di Analytics for Target (A4T)

[!DNL Adobe Target] supporta il reporting A4T sia per le attività di decisioning sul dispositivo che per le attività Target lato server. Esistono due opzioni di configurazione per abilitare il reporting A4T:

* [!DNL Adobe Target] inoltra automaticamente il payload di analytics a [!DNL Adobe Analytics], o
* L’utente richiede il payload di Analytics da [!DNL Adobe Target]. ([!DNL Adobe Target] restituisce il [!DNL Adobe Analytics] payload al chiamante).

>[!NOTE]
>
>Il decisioning sul dispositivo supporta solo il reporting A4T di cui [!DNL Adobe Target] inoltra automaticamente il payload di analytics a [!DNL Adobe Analytics]. Recupero del payload di Analytics da [!DNL Adobe Target] non è supportato.

## Prerequisiti

1. Configurare l’attività in [!DNL Adobe Target] Interfaccia utente con [!DNL Adobe Analytics] come origine per la generazione di rapporti e assicurati che i conti siano abilitati per A4T.
1. L’utente API genera l’ID visitatore di Adobe Marketing Cloud e si assicura che sia disponibile quando viene eseguita la richiesta di Target.

## [!DNL Adobe Target] inoltra automaticamente il payload di Analytics

[!DNL Adobe Target] può inoltrare automaticamente il payload di analytics a [!DNL Adobe Analytics] se sono forniti i seguenti identificatori:

1. `supplementalDataId`: ID utilizzato per unire tra [!DNL Adobe Analytics] e [!DNL Adobe Target]. Per ottenere [!DNL Adobe Target] e [!DNL Adobe Analytics] per unire correttamente i dati, lo stesso `supplementalDataId` deve essere passato a entrambi [!DNL Adobe Target] e [!DNL Adobe Analytics].
1. `trackingServer`: Il [!DNL Adobe Analytics] Server.

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");

const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
  request: {     
    id: {
      marketingCloudVisitorId : "2304820394812039",
      tntId: "d359234570e044f14e1faeeba02d6ab23439914e.35_0",
      thirdPartyId:"23423432"
    },
    experienceCloud: {
      analytics: {
        logging: "server_side",
        supplementalDataId: "7D3AA246CC99FD7F-1B3DD2E75595498E",
        trackingServer: "jimsbrims.sc.omtrds.net"
      }
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

>[!TAB Java]

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

AnalyticsRequest analyticsRequest =
    new AnalyticsRequest()
        .trackingServer("jimsbrims.sc.omtrds.net")
        .logging(LoggingType.SERVER_SIDE)
        .supplementalDataId("7D3AA246CC99FD7F-1B3DD2E75595498E");
ExperienceCloud expCloud =
    new ExperienceCloud()
        .setAnalytics(analyticsRequest);

TargetDeliveryRequest request = TargetDeliveryRequest.builder()
  .context(context)
  .execute(executeRequest)
  .experienceCloud(expCloud)
  .build();

TargetDeliveryResponse offers = targetClient.getOffers(request);
```

>[!ENDTABS]

## L’utente recupera il payload di Analytics da [!DNL Adobe Target]

Un utente può recuperare [!DNL Adobe Analytics] payload per una data mbox, quindi invialo a [!DNL Adobe Analytics] tramite [API di inserimento dati](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md). Quando un [!DNL Adobe Target] richiesta attivata, riuscita `client_side` al `logging` nella richiesta. Questo restituirà un payload se la mbox specificata è presente in un’attività che utilizza Analytics come origine per la generazione di rapporti.

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");
const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};
const targetClient = TargetClient.create(CONFIG);
targetClient.getOffers({
  request: {     
    id: {
      marketingCloudVisitorId : "2304820394812039",
      tntId: "d359234570e044f14e1faeeba02d6ab23439914e.35_0",
      thirdPartyId:"23423432"
    },
    experienceCloud: {
      analytics: {
        logging: "client_side"
      }
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

>[!TAB Java]

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

AnalyticsRequest analyticsRequest =
    new AnalyticsRequest()
        .logging(LoggingType.CLIENT_SIDE);
ExperienceCloud expCloud =
    new ExperienceCloud()
        .setAnalytics(analyticsRequest);

TargetDeliveryRequest request = TargetDeliveryRequest.builder()
  .context(context)
  .execute(executeRequest)
  .experienceCloud(expCloud)
  .build();

TargetDeliveryResponse offers = targetClient.getOffers(request);
```

>[!ENDTABS]

Dopo aver specificato `logging = client_side`, riceverai il payload nel campo mbox.

Se la risposta di Target contiene elementi nel `analytics -> payload` proprietà, inoltrarla così com&#39;è [!DNL Adobe Analytics]. [!DNL Adobe Analytics] sa come elaborare questo payload. Questa operazione può essere eseguita in una richiesta GET utilizzando il seguente formato:

```
https://{datacollectionhost.sc.omtrdc.net}/b/ss/{rsid}/0/CODEVERSION?pe=tnt&tnta={payload}&mid={mid}&vid={vid}&aid={aid}
```

### Parametri e variabili della stringa di query

| Nome campo | Obbligatorio | Descrizione |
| --- | --- | --- |
| `rsid` | Sì | ID suite di rapporti |
| `pe` | Sì | Evento pagina. Sempre impostato su `tnt` |
| `tnta` | Sì | Payload di Analytics restituito dal server Target in `analytics -> payload -> tnta` |
| `mid` | Sì | ID visitatore di Marketing Cloud |

### Valori intestazione richiesti

| Nome intestazione | Valore intestazione |
| --- | --- |
| Host | Server di raccolta dati di Analytics (esempio: `adobeags421.sc.omtrdc.net`) |

### Esempio di chiamata HTTP Get di inserimento dati A4T

```
https://demo.sc.omtrdc.net/b/ss/myCustomRsid/0/MOBILE-1.0?pe=tnt&tnta=285408:0:0|2&mid=2304820394812039
```
