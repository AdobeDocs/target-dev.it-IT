---
title: Notifica Target
description: Assicurati che tutti gli eventi che devono essere tracciati da [!DNL Target] vengono inviati utilizzando il metodo trackEvent.
feature: APIs/SDKs
level: Experienced
role: Developer
hide: true
hidefromtoc: true
source-git-commit: 8fae7e18f555e6b549e0b9c486be73e3483dac86
workflow-type: tm+mt
source-wordcount: '335'
ht-degree: 1%

---

# Notifica [!DNL Target]

Il completamento di questo passaggio garantisce che tutti gli eventi che devono essere inviati a [!DNL Adobe Target] vengono inviati utilizzando `trackEvent` metodo.

Qualsiasi evento che deve essere tracciato in [!DNL Target] può essere un evento di conversione principale o una metrica di successo.

>[!TIP]
>
>Fare clic sulle immagini in questo argomento per espandere a schermo intero.

## Notifica [!DNL Target] diagramma {#diagram}

Il numero del passaggio nell&#39;illustrazione seguente corrisponde alla sezione seguente.

![Diagramma Notify Target](/help/dev/patterns/recs-atjs/assets/diagram-notify-target.png){width="600" zoomable="yes"}

## 4.1: Incendio [!DNL Adobe Target] API di tracciamento

Questo passaggio ti aiuta a garantire che tutti gli eventi che devono essere inviati a [!DNL Target] vengono inviati utilizzando `trackEvent` metodo.

+++Consulta i dettagli

![Attiva diagramma API del tracciamento di Adobe Target](/help/dev/patterns/recs-atjs/assets/fire-adobe-target-track-api-diagram-combined.png){width="400" zoomable="yes"}

Invii gli attributi di conversione dell’ordine come indicato nella *Prerequisiti* sezione successiva. Il nome della mbox non ha importanza, ma la conversione deve utilizzare `orderConfirmPage`.

Non è necessario includere gli attributi di conversione dell&#39;ordine in questa chiamata. Idealmente, queste chiamate registrano metriche di successo che possono essere considerate come eventi di mini-conversione prima degli eventi di conversione principali. `CardIds` devono essere inclusi nei consigli basati sul carrello in base ai `Add to Cart` evento.

**Prerequisiti**

* Incontra il tuo team aziendale per identificare tutti gli eventi che possono essere considerati come metriche di conversione o di successo. Devi inoltre identificare l’evento di conversione che genera ricavi, in modo che i dettagli possano essere inviati a [!DNL Target] insieme ai dati dell’evento.
* Assicurati che i seguenti attributi siano disponibili nel livello dati in modo da poterli inviare con l’evento di conversione. L’evento di conversione genera ricavi, ad esempio un acquisto di prodotto o un evento Aggiungi al carrello.

   * `productPurchaseId`: ID prodotto acquistati come parte dell’ordine. Separa più prodotti utilizzando le virgole.
   * `orderTotal`: totale dell’ordine per l’acquisto.
   * `orderId`: ID ordine dell’acquisto.

* Se tieni traccia di un evento per l’aggiunta al carrello, invia `cartIds` come parametro. È possibile trasmettere un elenco separato da virgole di ID prodotto per `cardIds`.

**Letture**

* [metodo adobe.target.trackEvent()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md)
* [cartIds per criteri basati su carrello](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/base-the-recommendation-on-a-recommendation-key.html?lang=en#cart-based){target=_blank}

**Azioni**

* Utilizzare `adobe.target-trackEvent()` metodo per inviare tutti i dati da inviare a [!DNL Target].







