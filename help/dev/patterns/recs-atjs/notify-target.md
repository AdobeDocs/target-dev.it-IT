---
title: Notifica Target
description: Assicurarsi che tutti gli eventi che devono essere tracciati da [!DNL Target]  vengano inviati utilizzando il metodo trackEvent.
feature: APIs/SDKs
level: Experienced
role: Developer
exl-id: efccadab-d139-4423-8613-c2743d87b3a0
source-git-commit: 50ee7e66e30c0f8367763a63b6fde5977d30cfe7
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 1%

---

# Notifica [!DNL Target]

Il completamento di questo passaggio garantisce che tutti gli eventi che devono essere inviati a [!DNL Adobe Target] vengano inviati utilizzando il metodo `trackEvent`.

Qualsiasi evento che deve essere tracciato in [!DNL Target] può essere un evento di conversione primario o una metrica di successo.

>[!TIP]
>
>Fare clic sulle immagini in questo argomento per espandere a schermo intero.

## Notifica diagramma [!DNL Target] {#diagram}

Il numero del passaggio nell&#39;illustrazione seguente corrisponde alla sezione seguente.

![Diagramma notifica destinazione](/help/dev/patterns/recs-atjs/assets/diagram-notify-target.png){width="600" zoomable="yes"}

## 4.1: Attivare l&#39;API di tracciamento [!DNL Adobe Target]

Questo passaggio ti aiuta a garantire che tutti gli eventi che devono essere inviati a [!DNL Target] siano inviati utilizzando il metodo `trackEvent`.

+++Consulta i dettagli

![Attiva diagramma API di Adobe Target Track](/help/dev/patterns/recs-atjs/assets/fire-adobe-target-track-api-diagram-combined.png){width="400" zoomable="yes"}

Invii gli attributi di conversione dell&#39;ordine come indicato nella sezione *Prerequisiti* seguente. Il nome della mbox non ha importanza, ma la conversione deve utilizzare `orderConfirmPage`.

Non è necessario includere gli attributi di conversione dell&#39;ordine in questa chiamata. Idealmente, queste chiamate registrano metriche di successo che possono essere considerate come eventi di mini-conversione prima degli eventi di conversione principali. `CardIds` deve essere incluso nei consigli basati su carrello in base all&#39;evento `Add to Cart`.

**Prerequisiti**

* Incontra il tuo team aziendale per identificare tutti gli eventi che possono essere considerati come metriche di conversione o di successo. È inoltre necessario identificare l&#39;evento di conversione che genera ricavi in modo che tali dettagli possano essere inviati a [!DNL Target] insieme ai dati dell&#39;evento.
* Assicurati che i seguenti attributi siano disponibili nel livello dati in modo da poterli inviare con l’evento di conversione. L’evento di conversione genera ricavi, ad esempio un acquisto di prodotto o un evento Aggiungi al carrello.

   * `productPurchaseId`: ID prodotto acquistati come parte dell&#39;ordine. Separa più prodotti utilizzando le virgole.
   * `orderTotal`: totale ordine per l&#39;acquisto.
   * `orderId`: ID ordine dell&#39;acquisto.

  La figura seguente mostra una regola [per [!DNL tags] in [!DNL Experience Platform]](https://experienceleague.adobe.com/docs/tags.html){target=_blank} che deve essere attivata solo sulla pagina [!UICONTROL Confirmation].

  ![Pagina Configurazione azione](/help/dev/patterns/recs-atjs/assets/action-configuration.png){width="400" zoomable="yes"}

* Se tieni traccia di un evento per l&#39;aggiunta al carrello, invia `cartIds` come parametro. È possibile passare un elenco separato da virgole di ID prodotto per `cardIds`.

**Letture**

* [metodo adobe.target.trackEvent()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md)
* [cartIds per i criteri basati sul carrello](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/base-the-recommendation-on-a-recommendation-key.html?lang=en#cart-based){target=_blank}

**Azioni**

* Utilizzare il metodo `adobe.target-trackEvent()` per inviare tutti i dati da inviare a [!DNL Target].
