---
title: Integrazione con il reporting di Experience Cloud A4T
description: Integrazione con Experience Cloud, reporting A4T, integrazione con Analytics for Target
keywords: api di consegna, lato server, lato server, integrazione, a4t
exl-id: 0d09d7a1-528d-4e6a-bc6c-f7ccd61f5b75
feature: Implement Server-side
source-git-commit: cbae0f1758fb0dee4837e8c237f8617ecb46eb25
workflow-type: tm+mt
source-wordcount: '372'
ht-degree: 5%

---

# Generazione rapporti per [!UICONTROL Analytics for Target] (A4T)

[!DNL Adobe Target] supporta il reporting A4T sia per le attività di decisioning sul dispositivo che per le attività [!DNL Target] lato server. Esistono due opzioni di configurazione per abilitare il reporting A4T:

* [!DNL Adobe Target] inoltra automaticamente il payload di analytics a [!DNL Adobe Analytics], oppure
* L&#39;utente richiede il payload di Analytics da [!DNL Adobe Target]. ([!DNL Adobe Target] restituisce il payload [!DNL Adobe Analytics] al chiamante.)

>[!NOTE]
>
>Il decisioning sul dispositivo supporta solo il reporting A4T, di cui [!DNL Adobe Target] inoltra automaticamente il payload di Analytics a [!DNL Adobe Analytics]. Il recupero del payload di Analytics da [!DNL Adobe Target] non è supportato.

## Prerequisiti

1. Configurare l&#39;attività nell&#39;interfaccia utente di [!DNL Adobe Target] con [!DNL Adobe Analytics] come origine per la generazione di rapporti e assicurarsi che gli account siano abilitati per A4T.
1. L&#39;utente API genera l&#39;Adobe [!UICONTROL Marketing Cloud Visitor ID] e garantisce che questo ID sia disponibile quando viene eseguita la richiesta [!DNL Target].

## [!DNL Adobe Target] inoltra automaticamente il payload [!DNL Analytics]

[!DNL Adobe Target] può inoltrare automaticamente il payload di Analytics a [!DNL Adobe Analytics] se vengono forniti i seguenti identificatori:

1. `supplementalDataId`: ID utilizzato per unire tra [!DNL Adobe Analytics] e [!DNL Adobe Target]. Affinché [!DNL Adobe Target] e [!DNL Adobe Analytics] possano unire correttamente i dati, è necessario passare lo stesso `supplementalDataId` a [!DNL Adobe Target] e [!DNL Adobe Analytics].
1. `trackingServer`: Il Server [!DNL Adobe Analytics].

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

## L&#39;utente recupera il payload di Analytics da [!DNL Adobe Target]

Un utente può recuperare il payload [!DNL Adobe Analytics] per una data mbox, quindi inviarlo a [!DNL Adobe Analytics] tramite l&#39;[API di inserimento dati](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md). Quando viene attivata una richiesta [!DNL Adobe Target], passare `client_side` al campo `logging` nella richiesta. Questa richiesta restituisce un payload se la mbox specificata è presente in un&#39;attività che utilizza [!DNL Analytics] come origine per la generazione di rapporti.

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

Se la risposta di [!DNL Target] contiene elementi nella proprietà `analytics -> payload`, inoltrarla così com&#39;è a [!DNL Adobe Analytics]. [!DNL Adobe Analytics] sa come elaborare questo payload. Questa operazione può essere eseguita in una richiesta GET utilizzando il seguente formato:

```
https://{datacollectionhost.sc.omtrdc.net}/b/ss/{rsid}/{content_type_num}/{code_ver}/{session}?pe=tnt&tnta={payload}&c.&a.&target.&sessionId={sessionId}&.target&.a&.c&mid={mid}
```

### Parametri e variabili della stringa di query

| Nome campo | Obbligatorio | Descrizione |
| --- | --- | --- |
| `rsid` | Sì | ID suite di rapporti |
| `content_type_num` | Sì | Sempre impostato su &quot;0&quot; |
| `code_ver` | Sì | Sempre impostato su &quot;MOBILE-1.0&quot; |
| `session` | Sì | Sempre impostato su &quot;0&quot; |
| `pe` | Sì | Evento pagina. Sempre impostato su `tnt` |
| `tnta` | Sì | Payload [!DNL Analytics] restituito dal server [!DNL Target] in `analytics -> payload -> tnta` |
| `sessionId` | Sì | ID sessione [!DNL Target] della sessione in corso |
| `mid` | Sì | ID visitatore di Marketing Cloud |

### Valori di intestazione richiesti

| Nome intestazione | Valore intestazione |
| --- | --- |
| Host | Server di raccolta dati di Analytics (esempio: `adobeags421.sc.omtrdc.net`) |

### Esempio di chiamata HTTP Get per inserimento dati A4T

Versione non codificata URL per la leggibilità (formato non da utilizzare per le chiamate API):

```
https://demo.sc.omtrdc.net/b/ss/myCustomRsid/0/MOBILE-1.0/0?tnta=253236:0:0|0,253236:0:0|2,253236:0:0|1,253613:0:0|0,253613:0:0|2,253613:0:0|1&c.&a.&target.&sessionId=45c08980-f4b9-4e11-96db-067d58e49f74&.target&.a&.c&pe=tnt&mid=69170113867710665996968872592584719577
```

Versione con codifica URL (formato da utilizzare per le chiamate API):

```
https://demo.sc.omtrdc.net/b/ss/myCustomRsid/0/MOBILE-1.0/0?tnta=253236%3A0%3A0%7C0%2C253236%3A0%3A0%7C2%2C253236%3A0%3A0%7C1%2C253613%3A0%3A0%7C0%2C253613%3A0%3A0%7C2%2C253613%3A0%3A0%7C1&c.%26a.%26target.%26sessionId=45c08980-f4b9-4e11-96db-067d58e49f74%26.target%26.a%26.c&pe=tnt&mid=69170113867710665996968872592584719577 
```
