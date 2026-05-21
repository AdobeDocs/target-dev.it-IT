---
title: Scarica, archivia e aggiorna automaticamente l’artefatto della regola di decisioning sul dispositivo
description: Scopri come utilizzare l'artefatto della regola di decisioning sul dispositivo durante l'inizializzazione di  [!DNL Adobe Target] SDK.
feature: APIs/SDKs
exl-id: be41a723-616f-4aa3-9a38-8143438bd18a
TQID: https://experienceleague.adobe.com/o4oNaCtd3PS1cDndSJHkI10pDke1DTaEnBn8u9pIQk8
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 7a5aae2510a014c6efaeee63080cde3e7746f91c
workflow-type: tm+mt
source-wordcount: 350
ht-degree: 0%

---

# Download, archiviazione e aggiornamento automatico dell&#39;artefatto della regola tramite il SDK [!DNL Adobe Target]

Questo approccio è ideale quando è possibile inizializzare il SDK [!DNL Adobe Target] contemporaneamente all&#39;inizializzazione e all&#39;avvio del server Web. L&#39;artefatto della regola verrà scaricato dal SDK [!DNL Adobe Target] e memorizzato nella cache prima che l&#39;applicazione server Web inizi a gestire le richieste. Quando l&#39;applicazione Web è in esecuzione, tutte le [!DNL Adobe Target] decisioni verranno eseguite utilizzando l&#39;artefatto della regola in memoria. L&#39;artefatto della regola memorizzata nella cache verrà aggiornato in base ai `pollingInterval` specificati durante il passaggio di inizializzazione di SDK.

## Riepilogo dei passaggi

1. Installare SDK
1. Inizializzare SDK
1. Memorizza e utilizza l’artefatto della regola

## &#x200B;1. Installare SDK

>[!BEGINTABS]

>[!TAB NPM]

```javascript {line-numbers="true"}
npm i @adobe/target-nodejs-sdk -P
```

>[!TAB MVN]

```javascript {line-numbers="true"}
<dependency>
    <groupId>com.adobe.target</groupId>
    <artifactId>java-sdk</artifactId>
    <version>1.0</version>
</dependency>
```

>[!ENDTABS]

## &#x200B;2. Inizializzare SDK

1. Importa innanzitutto il SDK. Importa nello stesso file da cui è possibile controllare l&#39;avvio del server.

   **Node.js**

   ```javascript {line-numbers="true"}
   const TargetClient = require("@adobe/target-nodejs-sdk");
   ```

   **Java**

   ```javascript {line-numbers="true"}
   import com.adobe.target.edge.client.ClientConfig;
   import com.adobe.target.edge.client.TargetClient;
   ```

1. Per configurare SDK, utilizzare il metodo create.

   **Node.js**

   ```javascript {line-numbers="true"}
   const CONFIG = {
       client: "<your target client code>",
       organizationId: "your EC org id",
       decisioningMethod: "on-device",
       pollingInterval : 300000,
       events: {
           clientReady: startWebServer
       }
   };
   
   const TargetClient = TargetClient.create(CONFIG);
   
   function startWebServer() {
       //Adobe Target SDK has now downloaded the JSON Artifacts and is available in the memory.
       //You can start your web server now to serve requests now.
   }
   ```

   **Java**

   ```javascript {line-numbers="true"}
   ClientConfig config = ClientConfig.builder()
       .client("<you target client code>")
       .organizationId("<your EC org id>")
       .build();
   TargetClient targetClient = TargetClient.create(config);
   ```

1. È possibile recuperare sia il client che l&#39;OrganizationId da [!DNL Adobe Target] passando a **[!UICONTROL Administration]** > **[!UICONTROL Implementation]**, come illustrato di seguito.

   <!-- Insert image-client-code.png -->
   ![Pagina di implementazione in Amministrazione in Target](assets/asset-rule-artifact-3.png)

## &#x200B;3. Memorizzare e utilizzare l’artefatto della regola

Non è necessario gestire autonomamente l’artefatto della regola; la chiamata ai metodi SDK deve essere semplice.

>[!BEGINTABS]

>[!TAB Node.js]

```javascript {line-numbers="true"}
//req is the request object from the server request listener method
const targetCookie = req.cookies[TargetClient.TargetCookieName];
const request = {
    context: {
        channel: "web"
    },
    execute: {
        mboxes: [{
            address: { url: req.headers.host + req.originalUrl },
            name: "on-device-homepage-banner"
        }],
    },
};

TargetClient.getOffers({
    request,
    targetCookie
}).then(function(response) {
    //This Target response is coming from the In-memory Target artifact.
    console.log("Target response", response);
}).catch(function(err) {
    console.error("Target:", err);
})
```

>[!TAB Java]

```java {line-numbers="true"}
MboxRequest mbox = new MboxRequest().name("homepage").index(0);
TargetDeliveryRequest request = TargetDeliveryRequest.builder()
    .context(new Context().channel(ChannelType.WEB))
    .execute(new ExecuteRequest().mboxes(Arrays.asList(mbox)))
    .build();
TargetDeliveryResponse response = targetClient.getOffers(request);
```

>[!ENDTABS]

>[!NOTE]
>
>Nell&#39;esempio di codice precedente, l&#39;oggetto `TargetClient` contiene un riferimento all&#39;artefatto della regola in memoria. Quando si utilizza questo oggetto per richiamare i metodi SDK standard, viene utilizzato l&#39;artefatto della regola in memoria per le decisioni. Se l’applicazione è strutturata in modo tale da dover chiamare i metodi SDK in file diversi da quello che inizializza e ascolta le richieste dei client e se tali file non hanno accesso all’oggetto TargetClient, puoi scaricare il payload JSON e memorizzarlo in un file JSON locale da utilizzare su altri file, che devono inizializzare SDK. Questo è spiegato nella sezione successiva, relativa al [download dell&#39;artefatto della regola utilizzando un payload JSON](rule-artifact-json.md).

Nell&#39;esempio seguente viene avviata un&#39;applicazione Web dopo l&#39;inizializzazione del SDK [!DNL Adobe Target].

>[!BEGINTABS]

>[!TAB Node.js]

```javascript {line-numbers="true"}
const express = require("express");
const cookieParser = require("cookie-parser");
const TargetClient = require("@adobe/target-nodejs-sdk");
const CONFIG = {
    client: "<your target client code>",
    organizationId: "your EC org id",
    decisioningMethod: "on-device",
    pollingInterval : 300000,
    events: {
        clientReady: startWebServer
    }
};

const app = express();
const targetClient = TargetClient.create(CONFIG);

app.use(cookieParser());

function saveCookie(res, cookie) {
  if (!cookie) {
    return;
  }

  res.cookie(cookie.name, cookie.value, {maxAge: cookie.maxAge * 1000});
}

const getResponseHeaders = () => ({
  "Content-Type": "text/html",
  "Expires": new Date().toUTCString()
});

function sendSuccessResponse(res, response) {
  res.set(getResponseHeaders());
  saveCookie(res, response.targetCookie);
  res.status(200).send(response);
}

function sendErrorResponse(res, error) {
  res.set(getResponseHeaders());
  res.status(500).send(error);
}

function startWebServer() {
    app.get('/*', async (req, res) => {
    const targetCookie = req.cookies[TargetClient.TargetCookieName];
    const request = {
        execute: {
        mboxes: [{
            address: { url: req.headers.host + req.originalUrl },
            name: "on-device-homepage-banner" // Ensure that you have a LIVE Activity running on this location
        }]
        }};

    try {
        const response = await targetClient.getOffers({ request, targetCookie });
        sendSuccessResponse(res, response);
    } catch (error) {
        console.error("Target:", error);
        sendErrorResponse(res, error);
    }
    });

    app.listen(3000, function () {
    console.log("Listening on port 3000 and watching!");
    });
}
```

>[!TAB Java]

```java {line-numbers="true"}
import com.adobe.target.edge.client.ClientConfig;
import com.adobe.target.edge.client.TargetClient;
import com.adobe.target.delivery.v1.model.ChannelType;
import com.adobe.target.delivery.v1.model.Context;
import com.adobe.target.delivery.v1.model.ExecuteRequest;
import com.adobe.target.delivery.v1.model.MboxRequest;
import com.adobe.target.edge.client.entities.TargetDeliveryRequest;
import com.adobe.target.edge.client.model.TargetDeliveryResponse;

@Controller
public class TargetController {

  private TargetClient targetClient;

  TargetController() {
    // You should instantiate TargetClient in a Bean and inject the instance into this class 
    // but we show the code here for demonstration purpose.
    ClientConfig config = ClientConfig.builder()
        .client("<you target client code>")
        .organizationId("<your EC org id>")
        .build();
    targetClient = TargetClient.create(config);
  }

  @GetMapping("/")
  public String homePage() {
    MboxRequest mbox = new MboxRequest().name("homepage").index(0);
    TargetDeliveryRequest request = TargetDeliveryRequest.builder()
        .context(new Context().channel(ChannelType.WEB))
        .execute(new ExecuteRequest().mboxes(Arrays.asList(mbox)))
        .build();
    TargetDeliveryResponse response = targetClient.getOffers(request);
    // ...
  }
}
```

>[!ENDTABS]
