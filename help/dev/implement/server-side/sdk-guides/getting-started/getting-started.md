---
title: Guida introduttiva agli SDK di Target
description: Come si utilizzano gli SDK di Adobe Target?
feature: APIs/SDKs
exl-id: a5ae9826-7bb5-41de-8796-76edc4f5b281
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '664'
ht-degree: 0%

---

# Guida introduttiva a [!DNL Target] SDK

Per iniziare, ti invitiamo a creare il tuo primo [decisioning sul dispositivo](../on-device-decisioning/overview.md) attività flag di funzione nella lingua desiderata:

* Node.js
* Java
* .NET
* Python

## Riepilogo dei passaggi

1. Abilitare il decisioning sul dispositivo per la tua organizzazione
1. Installare l’SDK
1. Inizializzare l’SDK
1. Impostare i flag di funzione in un [!DNL Adobe Target] [!UICONTROL Test A/B] attività
1. Implementare ed eseguire il rendering della funzione nell’applicazione
1. Implementa il tracciamento degli eventi nell’applicazione
1. Attiva [!UICONTROL Test A/B] attività

## 1. Abilitare il decisioning sul dispositivo per l’organizzazione

L’abilitazione del decisioning sul dispositivo garantisce che [!UICONTROL Test A/B] l&#39;attività viene eseguita con latenza quasi pari a zero. Per abilitare questa funzione, vai a **[!UICONTROL Amministrazione]** > **[!UICONTROL Implementazione]** > **[!UICONTROL Dettagli account]** e abilita **[!UICONTROL Decisioning sul dispositivo]** attivare/disattivare.

![immagine alt](assets/asset-odd-toggle.png)

>[!NOTE]
>
>È necessario disporre di **[!UICONTROL Amministratore]** o **[!UICONTROL Approvatore]** [ruolo utente](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/user-management.html) per abilitare o disabilitare **[!UICONTROL Decisioning sul dispositivo]** attivare/disattivare.

Dopo aver abilitato **[!UICONTROL Decisioning sul dispositivo]** attivare/disattivare, [!DNL Adobe Target] inizia a generare [artefatti regola](../on-device-decisioning/rule-artifact-overview.md) per il tuo cliente.

## 2. Installare l’SDK

Per Node.js, Java e Python, esegui il seguente comando nella directory del progetto nel terminale. Per .NET, aggiungerlo come dipendenza tramite [installazione da NuGet](https://www.nuget.org/packages/Adobe.Target.Client).

>[!BEGINTABS]

>[!TAB Node.js (NPM)]

```js {line-numbers="true"}
npm i @adobe/target-nodejs-sdk -P
```

>[!TAB Java (Maven)]

```javascript {line-numbers="true"}
<dependency>
   <groupId>com.adobe.target</groupId>
   <artifactId>java-sdk</artifactId>
   <version>2.0</version>
</dependency>
```

>[!TAB .NET (Bash)]

```bash {line-numbers="true"}
dotnet add package Adobe.Target.Client
```

>[!TAB Python (pip)]

```python {line-numbers="true"}
pip install target-python-sdk
```

>[!ENDTABS]

## 3. Inizializzare l’SDK

L’artefatto della regola viene scaricato durante il passaggio di inizializzazione dell’SDK. Puoi personalizzare il passaggio di inizializzazione per determinare come viene scaricato e utilizzato l’artefatto.

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");

const CONFIG = {
   client: "<your target client code>",
   organizationId: "your EC org id",
   decisioningMethod: "on-device",
   events: {
      clientReady: targetClientReady
      }
};

const tClient = TargetClient.create(CONFIG);

function targetClientReady() {
   //Adobe Target SDK has now downloaded the JSON artifact locally, which contains the activity details.
   //We will see how to use the artifact here very soon.
}
```

>[!TAB Java (Maven)]

```javascript {line-numbers="true"}
ClientConfig config = ClientConfig.builder()
   .client("testClient")
   .organizationId("ABCDEF012345677890ABCDEF0@AdobeOrg")
   .build();
TargetClient targetClient = TargetClient.create(config);
```

>[!TAB .NET (C#)]

```csharp {line-numbers="true"}
var targetClientConfig = new TargetClientConfig.Builder("testClient", "ABCDEF012345677890ABCDEF0@AdobeOrg")
   .Build();
this.targetClient.Initialize(targetClientConfig);
```

>[!TAB Python]

```python {line-numbers="true"}
from target_python_sdk import TargetClient

def target_client_ready():
   # Adobe Target SDK has now downloaded the JSON artifact locally, which contains the activity details.
   # We will see how to use the artifact here very soon.

CONFIG = {
   "client": "<your target client code>",
   "organization_id": "your EC org id",
   "decisioning_method": "on-device",
   "events": {
      "client_ready": target_client_ready
   }
}

target_client = TargetClient.create(CONFIG)
```

>[!ENDTABS]

## 4. Impostare i flag di funzione in un [!DNL Adobe Target] [!UICONTROL Test A/B] attività

1. In entrata [!DNL Target], passare alla **[!UICONTROL Attività]** , quindi seleziona **[!UICONTROL Crea attività]** > **[!UICONTROL Test A/B]**.

   ![immagine alt](assets/asset-ab.png)

1. In **[!UICONTROL Crea attività test A/B]** , lascia selezionata l&#39;opzione Web predefinita (1), seleziona **[!UICONTROL Modulo]** come compositore esperienza (2), seleziona **[!UICONTROL Area di lavoro predefinita]** con **[!UICONTROL Nessuna restrizione di proprietà]**(3), quindi fai clic su **[!UICONTROL Successivo]** 4).

   ![immagine alt](assets/asset-form.png)

1. In **[!UICONTROL Esperienze]** per creare un’attività, specifica un nome per l’attività (1) e aggiungi una seconda esperienza, Esperienza B, facendo clic su **[!UICONTROL Aggiungi esperienza]** (2). Immettere il nome della posizione desiderata (3). Ad esempio: `ondevice-featureflag` o `homepage-addtocart-featureflag` sono nomi di posizione che indicano le destinazioni per il test dei flag di funzione.  Nell’esempio riportato di seguito, `ondevice-featureflag` è la posizione definita per l’Esperienza B. Facoltativamente, puoi aggiungere Perfezionamenti del pubblico (4) per limitare la qualifica all’attività.

   ![immagine alt](assets/asset-location.png)

1. In **[!UICONTROL CONTENUTO]** nella stessa pagina, seleziona **[!UICONTROL Crea offerta JSON]** nell’elenco a discesa (1), come illustrato.

   ![immagine alt](assets/asset-offer.png)

1. In **[!UICONTROL Dati JSON]** casella di testo visualizzata, digita le variabili del flag di funzione per ogni esperienza (1), utilizzando un oggetto JSON valido (2).

   Immetti le variabili dei flag di funzione per l’Esperienza A.

   ![immagine alt](assets/asset-json_a.png)

   **(Esempio di JSON per l’esperienza A, vedi sopra)**

   ```json {line-numbers="true"}
   {
      "enabled" : true,
      "flag" : "expA"
   }
   ```

   Immetti le variabili dei flag di funzione per l’Esperienza B.

   ![immagine alt](assets/asset-json_b.png)

   **(Esempio di JSON per l’esperienza B, vedi sopra)**

   ```json {line-numbers="true"}
   {
      "enabled" : true,
      "flag" : "expB"
   }
   ```

1. Clic **[!UICONTROL Successivo]** (1) anticipare al **[!UICONTROL Targeting]** passaggio di creazione dell’attività.

   ![immagine alt](assets/asset-next_2_t.png)

1. In **[!UICONTROL Targeting]** Esempio del passaggio mostrato di seguito, Targeting del pubblico (2) rimane sul set predefinito di Tutti i visitatori, per semplicità. Ciò significa che l’attività non è targetizzata. Tuttavia, un Adobe di nota consiglia di indirizzare sempre i tipi di pubblico alle attività di produzione. Clic **[!UICONTROL Successivo]** (3) anticipare al **[!UICONTROL Obiettivi e impostazioni]** passaggio di creazione dell’attività.

   ![immagine alt](assets/asset-next_2_g.png)

1. In **[!UICONTROL Obiettivi e impostazioni]** step, set **[!UICONTROL Origine per la generazione di rapporti]** a **[!UICONTROL Adobe Target]** (1) Definisci il **[!UICONTROL Metrica per obiettivo]** as **[!UICONTROL Conversione]**, specificando i dettagli in base alle metriche di conversione del sito (2). Clic **[!UICONTROL Salva e chiudi]** (3) per salvare l’attività.

   ![immagine alt](assets/asset-conv.png)

## 5. Implementare ed eseguire il rendering della funzione nell’applicazione

Dopo aver impostato le variabili dei flag di funzione in [!DNL Target], modifica il codice dell’applicazione per utilizzarli. Ad esempio, dopo aver ottenuto il flag di funzione nell’applicazione, puoi utilizzarlo per abilitare le funzioni ed eseguire il rendering dell’esperienza per la quale il visitatore si è qualificato.

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
//... Code removed for brevity
​
let featureFlags = {};
​
function targetClientReady() {
   tClient.getAttributes(["ondevice-featureflag"]).then(function(response) {
      const featureFlags = response.asObject("ondevice-featureflag");
      if(featureFlags.enabled && featureFlags.flag !== "expA") { //Assuming "expA" is control
         console.log("Render alternate experience" + featureFlags.flag);
      }
      else {
         console.log("Render default experience");
      }
   });
}
```

>[!TAB Java (Maven)]

```javascript {line-numbers="true"}
MboxRequest mbox = new MboxRequest().name("ondevice-featureflag").index(0);
TargetDeliveryRequest request = TargetDeliveryRequest.builder()
   .context(new Context().channel(ChannelType.WEB))
   .execute(new ExecuteRequest().mboxes(Arrays.asList(mbox)))
   .build();
Attributes attributes = targetClient.getAttributes(request, "ondevice-featureflag");
String flag = attributes.getString("ondevice-featureflag", "flag");
```

>[!TAB .NET (C#)]

```csharp {line-numbers="true"}
var mbox = new MboxRequest(index: 0, name: "ondevice-featureflag");
var deliveryRequest = new TargetDeliveryRequest.Builder()
   .SetContext(new Context(ChannelType.Web))
   .SetExecute(new ExecuteRequest(mboxes: new List<MboxRequest> { mbox }))
   .Build();
var attributes = targetClient.GetAttributes(request, "ondevice-featureflag");
var flag = attributes.GetString("ondevice-featureflag", "flag");
```

>[!TAB Python]

```python {line-numbers="true"}
# ... Code removed for brevity

feature_flags = {}

def target_client_ready():
   attribute_provider = target_client.get_attributes(["ondevice-featureflag"])
   feature_flags = attribute_provider.as_object(mbox_name="ondevice-featureflag")
   if feature_flags.get("enabled") and feature_flags.get("flag") != "expA": # Assuming "expA" is control
      print("Render alternate experience {}".format(feature_flags.get("flag")))
   else:
      print("Render default experience")
```

>[!ENDTABS]

## 6. Implementa il tracciamento aggiuntivo per gli eventi nell’applicazione

Facoltativamente, puoi inviare eventi aggiuntivi per il tracciamento delle conversioni utilizzando la funzione sendNotification().

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
//... Code removed for brevity
​
//When a conversion happens
TargetClient.sendNotifications({
   targetCookie,
   "request" : {
      "notifications" : [
      {
         type: "display",
         timestamp : Date.now(),
         id: "conversion",
         mbox : {
            name : "orderConfirm"
         },
         order : {
            id: "BR9389",
            total : 98.93,
            purchasedProductIds : ["J9393", "3DJJ3"]
         }
      }
      ]
   }
})
```

>[!TAB Java (Maven)]

```javascript {line-numbers="true"}
Notification notification = new Notification();
notification.setId("conversion");
notification.setImpressionId(UUID.randomUUID().toString());
notification.setType(MetricType.DISPLAY);
notification.setTimestamp(System.currentTimeMillis());
Order order = new Order("BR9389");
order.total(98.93);
order.purchasedProductIds(["J9393", "3DJJ3"]);
notification.setOrder(order);

TargetDeliveryRequest notificationRequest =
   TargetDeliveryRequest.builder()
      .context(new Context().channel(ChannelType.WEB))
      .notifications(Collections.singletonList(notification))
      .build();

NotificationDeliveryService notificationDeliveryService = new NotificationDeliveryService();
notificationDeliveryService.sendNotification(notificationRequest);
```

>[!TAB .NET (C#)]

```csharp {line-numbers="true"}
var order = new Order
{
   Id = "BR9389",
   Total = 98.93M,
   PurchasedProductIds = new List<string> { "J9393", "3DJJ3" },
};
​
var notification = new Notification
{
   Id = "conversion",
   ImpressionId = Guid.NewGuid().ToString(),
   Type = MetricType.Display,
   Timestamp = DateTimeOffset.UtcNow.ToUnixTimeMilliseconds(),
   Order = order,
};
​
var notificationRequest = new TargetDeliveryRequest.Builder()
   .SetContext(new Context(ChannelType.Web))
   .SetNotifications(new List<Notification> {notification})
   .Build();
​
targetClient.SendNotifications(notificationRequest);
```

>[!TAB Python]

```python {line-numbers="true"}
# ... Code removed for brevity

# When a conversion happens
notification_mbox = NotificationMbox(name="orderConfirm")
order = Order(id="BR9389, total=98.93, purchased_product_ids=["J9393", "3DJJ3"])
notification = Notification(
   id="conversion",
   type=MetricType.DISPLAY,
   timestamp=1621530726000,  # Epoch time in milliseconds
   mbox=notification_mbox,
   order=order
)
notification_request = DeliveryRequest(notifications=[notification])


target_client.send_notifications({
   "target_cookie": target_cookie,
   "request" : notification_request
})
```

>[!ENDTABS]

## 7. Attiva il [!UICONTROL Test A/B] attività

1. Clic **[!UICONTROL Attiva]** (1) per attivare [!UICONTROL Test A/B] attività.

   >[!NOTE]
   >
   >È necessario disporre di **[!UICONTROL Approvatore]** o **[!UICONTROL Editore]** [ruolo utente](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/user-management.html) per eseguire questo passaggio.

   ![immagine alt](assets/asset-activate.png)
