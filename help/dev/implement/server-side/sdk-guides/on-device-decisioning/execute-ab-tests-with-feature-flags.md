---
title: Eseguire test A/B con flag di funzionalità e decisioning sul dispositivo
description: Esegui test A/B con flag di funzione utilizzando decisioning sul dispositivo.
feature: APIs/SDKs
exl-id: abf66e00-742d-4d40-9b6e-9bd71638c31a
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '772'
ht-degree: 0%

---

# Eseguire test A/B con flag di funzione

## Riepilogo dei passaggi

1. Abilita [!UICONTROL decisioning sul dispositivo] per la tua organizzazione
1. Creare un [!UICONTROL Test A/B] attività
1. Definire A e B
1. Aggiungere un pubblico
1. Imposta allocazione traffico
1. Impostare la distribuzione del traffico sulle varianti
1. Configurare la generazione di rapporti
1. Aggiungere metriche per il tracciamento dei KPI
1. Implementare il codice per eseguire test A/B con flag di funzione
1. Attivare il test A/B con i flag delle funzioni

>[!NOTE]
>
>Supponiamo di voler determinare se la riprogettazione a tema autunno della tua pagina home verrà ricevuta bene dagli utenti. Decidi di testarlo eseguendo un esperimento A/B in [!DNL Adobe Target]. Inoltre, è importante assicurarsi che l’esperimento venga fornito con prestazioni eccellenti, in modo che un’esperienza utente negativa o lenta non alteri i risultati.

## 1. Abilita [!UICONTROL decisioning sul dispositivo] per la tua organizzazione

L’abilitazione del decisioning sul dispositivo garantisce che un’attività A/B venga eseguita con latenza vicina allo zero. Per abilitare questa funzione, vai a **[!UICONTROL Amministrazione]** > **[!UICONTROL Implementazione]** > **[!UICONTROL Dettagli account]** in [!DNL Adobe Target], e abilita **[!UICONTROL Decisioning sul dispositivo]** attivare/disattivare.

&lt;!— Inserisci immagine-dispari4.png —>
![immagine alt](assets/asset-odd-toggle.png)

>[!NOTE]
>
>È necessario disporre dell&#39;amministratore o dell&#39;approvatore [ruolo utente](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/user-management.html) per abilitare o disabilitare l’interruttore Decisioning sul dispositivo.

Dopo aver abilitato **[!UICONTROL Decisioning sul dispositivo]** attivare/disattivare, [!DNL Adobe Target] inizia a generare gli artefatti della regola per il client.

## 2. Creare un [!UICONTROL Test A/B] attività

In entrata [!DNL Adobe Target], passare alla **[!UICONTROL Attività]** , quindi seleziona **[!UICONTROL Crea attività]** > **[!UICONTROL Test A/B]**.

![immagine alt](assets/asset-ab.png)

In **[!UICONTROL Crea attività test A/B]** , lascia il valore predefinito **[!UICONTROL Web]** opzione selezionata (1), seleziona **[!UICONTROL Modulo]** come compositore esperienza (2), seleziona **[!UICONTROL Area di lavoro predefinita]** con No **[!UICONTROL Restrizioni proprietà]** (3) e fai clic su **[!UICONTROL Successivo]** 4).

![immagine alt](assets/asset-form.png)

## 3. Definire A e B

1. In **[!UICONTROL Esperienze]** fase di creazione dell’attività, fornisci un nome per l’attività (1) e aggiungi una seconda esperienza, Esperienza B, facendo clic sul pulsante **[!UICONTROL Aggiungi esperienza]** (2) Immetti il nome della posizione (3) all’interno dell’applicazione in cui desideri eseguire il test A/B. Nell’esempio riportato di seguito, homepage è la posizione definita per l’Esperienza A. (È anche la posizione definita per l’Esperienza B.)

   L’Esperienza A definisce il controllo, che è la progettazione della pagina home corrente.

   ![immagine alt](assets/asset-exp-a.png)

   L&#39;esperienza B definisce lo sfidante, che rappresenterà una homepage riprogettata. Fai clic su per modificare il contenuto predefinito (1).

   ![immagine alt](assets/asset-exp-b.png)

1. Nell’Esperienza B, fai clic su per modificare il contenuto da **[!UICONTROL Contenuto predefinito]** al contenuto riprogettato selezionando **[!UICONTROL Crea offerta JSON]** come illustrato di seguito (1).

   ![immagine alt](assets/asset-offer.png)

1. Definisci il JSON con gli attributi che verranno utilizzati come flag per consentire alla logica di business di eseguire il rendering della home page appena riprogettata, anziché della home page corrente in produzione.


   >[!NOTE]
   >
   >Quando [!DNL Adobe Target] Quando un utente riceve un bucket per visualizzare l’Esperienza B (la pagina home riprogettata), viene restituito il JSON con gli attributi definiti nell’esempio. Nel codice, dovrai controllare i valori degli attributi per decidere se eseguire la logica di business per eseguire il rendering della home page riprogettata. Puoi definire i nomi, i valori e il numero di attributi in questa risposta JSON.

   ![immagine alt](assets/asset-homepage.png)

## 4. Aggiungere un pubblico

Supponiamo di voler testare innanzitutto la riprogettazione per i clienti fedeli, che puoi identificare in base al fatto che abbiano effettuato o meno l’accesso.

1. In **[!UICONTROL Targeting]** , fare clic per sostituire il **[!UICONTROL Tutti i visitatori]** pubblico, come illustrato.

   ![immagine alt](assets/asset-all-audiences.png)

1. In **[!UICONTROL Crea pubblico]** , definisci una regola personalizzata in cui `logged-in = true`. Definisce il gruppo di utenti che hanno effettuato l’accesso. Utilizza questo pubblico nella tua attività.

   ![immagine alt](assets/asset-audience.png)

## 5. Impostare l’allocazione del traffico

Definisci la percentuale di utenti connessi rispetto alla quale desideri testare la nuova progettazione della pagina home. In altre parole, a quale percentuale degli utenti desideri eseguire il test? In questo esempio, per distribuire il test a tutti gli utenti connessi, mantieni l’allocazione del traffico al 100%.

![immagine alt](assets/asset-allocation.png)

## 6. Impostare la distribuzione del traffico sulle varianti

Definisci la percentuale di utenti connessi che vedranno la progettazione corrente della pagina home o la riprogettazione completamente nuova. In questo esempio, mantieni la distribuzione del traffico come suddivisione 50/50 tra le esperienze A e B.

![immagine alt](assets/asset-traffic-distribution.png)

## 7. Impostare la generazione rapporti

In **[!UICONTROL Obiettivi e impostazioni]** passo, scegli **[!UICONTROL Adobe Target]** come **[!UICONTROL Origine per la generazione di rapporti]** per visualizzare i risultati dell’attività in [!DNL Adobe Target] UI o scegli **[!UICONTROL Adobe Analytics]** per visualizzarli nell’interfaccia utente di Adobe Analytics.

![immagine alt](assets/asset-reporting.png)

## 8. Aggiungere metriche per il tracciamento dei KPI

Scegli un **[!UICONTROL Metrica per obiettivo]** per misurare il test A/B. In questo esempio, una conversione corretta si basa sul fatto che l’utente raggiunga la parte inferiore della pagina, indicando il coinvolgimento. Pertanto, **[!UICONTROL Conversione]** viene determinato in base al fatto che l’utente abbia visualizzato la posizione denominata in fondo alla pagina.

## 9. Implementa il codice per eseguire test A/B con i flag di funzione nell’applicazione

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");
const options = {
  client: "testClient",
  organizationId: "ABCDEF012345677890ABCDEF0@AdobeOrg",
  decisioningMethod: "on-device",
  events: {
    clientReady: targetClientReady
  }
};
const targetClient = TargetClient.create(options);

function targetClientReady() {
  return targetClient.getAttributes(["homepage"]).then(function(attributes) {
    const flag = attributes.getValue("homepage", "feature-flag");
    // ...
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

ClientConfig config = ClientConfig.builder()
    .client("testClient")
    .organizationId("ABCDEF012345677890ABCDEF0@AdobeOrg")
    .build();
TargetClient targetClient = TargetClient.create(config);
MboxRequest mbox = new MboxRequest().name("homepage").index(0);
TargetDeliveryRequest request = TargetDeliveryRequest.builder()
    .context(new Context().channel(ChannelType.WEB))
    .execute(new ExecuteRequest().mboxes(Arrays.asList(mbox)))
    .build();
Attributes attributes = targetClient.getAttributes(request, "homepage");
String flag = attributes.getString("homepage", "feature-flag");
```

>[!ENDTABS]

## 10. Attiva il test A/B con il flag di funzione

![immagine alt](assets/asset-activate.png)
