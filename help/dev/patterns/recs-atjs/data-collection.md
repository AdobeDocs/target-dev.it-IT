---
title: Configurare la raccolta dati
description: Assicurati che tutte le attività necessarie per la raccolta dei dati vengano eseguite nella sequenza corretta.
feature: APIs/SDKs
level: Experienced
role: Developer
hide: true
hidefromtoc: true
source-git-commit: 6cd78f8e3cbdd97a09b0cb6ca3af55994e85f819
workflow-type: tm+mt
source-wordcount: '386'
ht-degree: 3%

---

# Configurare la raccolta dati

Segui i passaggi descritti in *Raccolta dati* per garantire che tutte le attività necessarie per la raccolta dei dati vengano eseguite nella sequenza corretta.

>[!TIP]
>
>Fare clic sulle immagini in questo argomento per espandere a schermo intero.

Il livello dati è pronto durante il caricamento della pagina o quando cambia dopo il caricamento della pagina.

Se hai già mappato i dati durante il [fase di inizializzazione SDK](/help/dev/patterns/recs-atjs/initialize-sdk.md), è necessario eseguire i passaggi in questo diagramma se:

* Il livello dati viene potenziato in qualsiasi modo sulla stessa pagina e desideri inviare tali dati aggiuntivi a [!DNL Target]
* Desideri inviare i dati del catalogo prodotti a [!DNL Target Recommendations]

## Raccogli diagramma dati {#diagram}

I numeri dei passi nella figura seguente corrispondono alle sezioni riportate di seguito.

![Diagramma di Data Collection](/help/dev/patterns/recs-atjs/assets/data-collection-diagram.png){width="600" zoomable="yes"}

Fai clic sui seguenti collegamenti per passare alle sezioni desiderate:

* [2.1: Configurare la mappatura dei dati](#configure)
* [2.2 Collegamento agli attributi di entità](#entity-attributes)
* [2.3 Attivare l’API di Adobe Target Track](#fire-api)

## 2.1: Configurare la mappatura dei dati {#configure}

Questo passaggio garantisce che tutti i dati che devono essere inviati a [!DNL Adobe Target] è impostato.

+++Consulta i dettagli

![Configurare il diagramma di mappatura dei dati](/help/dev/patterns/recs-atjs/assets/cofigure-data-mapping.png){width="400" zoomable="yes"}

**Prerequisiti**

* Data Layer deve essere pronto con tutti i dati che devono essere inviati a [!DNL Target].

**Letture**

[funzione targetPageParams](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md)

**Azioni**

Utilizza il `targetPageParams()` per impostare tutti i dati richiesti da inviare a [!DNL Target].

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 2.2: Collegamento ad attributi di entità {#entity-attributes}

Collega ad attributi di entità per aggiornare il catalogo prodotti per [!DNL Target Recommendations].

+++Consulta i dettagli

**Letture**

* [Attributi di entità](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html){target=_blank}

**Considerazioni**

* Un modo alternativo per trasmettere gli attributi di entità consiste nell’aggiornare il catalogo prodotti in [!DNL Target] Interfaccia utente da utilizzare [Feed di prodotto di Recommendations](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/feeds.html){target=_blank}.
* Il passaggio degli attributi di entità è applicabile solo alle pagine in cui i dati del catalogo prodotti sono disponibili nel livello dati.
* Superamento del `entity.event.detailsOnly=true` Il parametro in qualsiasi chiamata ha la priorità.

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 2.3 Attivare l’API di Adobe Target Track {#fire-api}

Questo passaggio garantisce che tutti i dati che devono essere inviati a [!DNL Target] viene inviato.

+++Consulta i dettagli

![Attiva diagramma API del tracciamento di Adobe Target](/help/dev/patterns/recs-atjs/assets/fire-track-api.png){width="400" zoomable="yes"}

**Prerequisiti**

* Tutta la mappatura dei dati deve essere stata eseguita utilizzando [funzione targetPageParams](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md).

**Letture**

* [metodo adobe.target.trackEvent()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md)

**Azioni**

Utilizzare [metodo adobe.target.trackEvent()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md) per inviare tutti i dati da inviare a [!DNL Target].

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

