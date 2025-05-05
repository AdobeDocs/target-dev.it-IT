---
title: Configurare la raccolta dati
description: Assicurati che tutte le attività necessarie per la raccolta dei dati vengano eseguite nella sequenza corretta.
feature: APIs/SDKs
level: Experienced
role: Developer
exl-id: 66e0f18d-c78c-463b-8c47-132ef6332927
source-git-commit: 50ee7e66e30c0f8367763a63b6fde5977d30cfe7
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 2%

---

# Configurare la raccolta dati

Segui i passaggi nel diagramma *Raccolta dati* per assicurarti che tutte le attività necessarie per la raccolta dati vengano eseguite nella sequenza corretta.

>[!TIP]
>
>Fare clic sulle immagini in questo argomento per espandere a schermo intero.

Il livello dati è pronto durante il caricamento della pagina o quando cambia dopo il caricamento della pagina.

Se hai già mappato i dati durante la [fase di inizializzazione dell&#39;SDK](/help/dev/patterns/recs-atjs/initialize-sdk.md), devi eseguire i passaggi in questo diagramma se:

* Il livello dati viene potenziato in qualsiasi modo sulla stessa pagina e desideri inviare tali dati aggiuntivi a [!DNL Target]
* Si desidera inviare i dati del catalogo prodotti a [!DNL Target Recommendations]

## Raccogli diagramma dati {#diagram}

I numeri dei passi nella figura seguente corrispondono alle sezioni riportate di seguito.

![Diagramma raccolta dati](/help/dev/patterns/recs-atjs/assets/data-collection-diagram.png){width="600" zoomable="yes"}

Fai clic sui seguenti collegamenti per passare alle sezioni desiderate:

* [2.1: Configurare la mappatura dei dati](#configure)
* [2.2 Collegamento agli attributi di entità](#entity-attributes)
* [2.3 Attivare l’API di Adobe Target Track](#fire-api)

## 2.1: Configurare la mappatura dei dati {#configure}

Questo passaggio garantisce che tutti i dati che devono essere inviati a [!DNL Adobe Target] siano impostati.

+++Consulta i dettagli

![Configura diagramma di mappatura dati](/help/dev/patterns/recs-atjs/assets/configure-data-mapping-combined.png){width="400" zoomable="yes"}

**Prerequisiti**

* Data Layer deve essere pronto con tutti i dati che devono essere inviati a [!DNL Target].

**Letture**

[funzione targetPageParams](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md)

**Azioni**

Utilizzare la funzione `targetPageParams()` per impostare tutti i dati richiesti da inviare a [!DNL Target].

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 2.2: Collegamento ad attributi di entità {#entity-attributes}

Collegamento agli attributi di entità per aggiornare il catalogo prodotti per [!DNL Target Recommendations].

+++Consulta i dettagli

**Letture**

* [Attributi di entità](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html?lang=it){target=_blank}

**Considerazioni**

* Un modo alternativo per passare gli attributi di entità consiste nell&#39;aggiornare il catalogo prodotti nell&#39;interfaccia utente [!DNL Target] per utilizzare [feed prodotti Recommendations](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/feeds.html?lang=it){target=_blank}.
* Il passaggio degli attributi di entità è applicabile solo alle pagine in cui i dati del catalogo prodotti sono disponibili nel livello dati.
* Il passaggio del parametro `entity.event.detailsOnly=true` in qualsiasi chiamata ha la priorità.

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 2.3 Attivare l’API di Adobe Target Track {#fire-api}

Questo passaggio garantisce l&#39;invio di tutti i dati che devono essere inviati a [!DNL Target].

+++Consulta i dettagli

![Attiva diagramma API di Adobe Target Track](/help/dev/patterns/recs-atjs/assets/fire-track-api-combined.png){width="400" zoomable="yes"}

**Prerequisiti**

* Tutte le mappature dei dati devono essere state eseguite utilizzando la funzione [targetPageParams](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md).

**Letture**

* [metodo adobe.target.trackEvent()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md)

**Azioni**

Utilizzare il metodo [adobe.target.trackEvent()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md) per inviare tutti i dati che devono essere inviati a [!DNL Target].

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

Procedi al passaggio 3: [Esperienze di rendering](/help/dev/patterns/recs-atjs/render-experiences.md)
