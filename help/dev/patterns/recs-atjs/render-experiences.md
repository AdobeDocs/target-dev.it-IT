---
title: Esperienze di rendering
description: Assicurati che tutti i passaggi necessari per il rendering delle esperienze vengano eseguiti nella sequenza corretta.
feature: APIs/SDKs
level: Experienced
role: Developer
exl-id: 7cf0c70b-a4bc-46f4-9b33-099bdb7dd9a9
source-git-commit: 50ee7e66e30c0f8367763a63b6fde5977d30cfe7
workflow-type: tm+mt
source-wordcount: '908'
ht-degree: 4%

---

# Esperienze di rendering

Segui i passaggi nel diagramma *Esperienze di rendering* per assicurarti che tutte le attività necessarie per il rendering delle esperienze vengano eseguite nella sequenza corretta.

>[!NOTE]
>
>Se hai abilitato la richiesta automatica di caricamento pagina durante il passaggio [Configura richiesta automatica di caricamento pagina](/help/dev/patterns/recs-atjs/initialize-sdk.md#automatic) in *Inizializza SDKS* , puoi saltare questa attività a meno che non desideri chiamare l&#39;SDK di Adobe Target per eseguire il rendering di altre esperienze utilizzando una richiesta di posizione regionale.

>[!TIP]
>
>Fare clic sulle immagini in questo argomento per espandere a schermo intero.

## Diagramma esperienze di rendering {#diagram}

La gestione automatica della visualizzazione momentanea di altri contenuti pronta all&#39;uso con at.js ha senso solo se è stato abilitato [!UICONTROL Automatic Page Load Request]. Questa opzione nasconde l&#39;intero corpo del HTML durante il recupero delle esperienze da [!DNL Target]. In questo caso, è tua responsabilità gestire la visualizzazione momentanea di altri contenuti. Per assistenza, cerca i modelli di implementazione disponibili per la gestione della visualizzazione momentanea di altri contenuti.

>[!NOTE]
>
>I numeri dei passi nella figura seguente corrispondono alle sezioni riportate di seguito. I numeri dei passaggi non sono in un ordine particolare e non riflettono l&#39;ordine dei passaggi eseguiti nell&#39;interfaccia utente [!DNL Target] durante la creazione dell&#39;attività.

![Diagramma esperienze rendering](/help/dev/patterns/recs-atjs/assets/diagram-render-experiences-new.png){width="600" zoomable="yes"}

Fai clic sui seguenti collegamenti per passare alle sezioni desiderate:

* [3.1. Promozione](#promotion)
* [3.2: Criteri basati sul carrello](#cart)
* [3.3: Criteri basati sulla popolarità](#popularity)
* [3.4: Criteri basati sugli articoli](#item)
* [3.5: Criteri basati sugli utenti](#user)
* [3.6: Criteri personalizzati](#custom)
* [3.7: Fornire gli attributi utilizzati nelle regole di inclusione](#inclusion)
* [3.8: Fornire excludedIds](#exclude)
* [3.9: fornire attributi di entità per aggiornare il catalogo prodotti per Recommendations](#entity-attributes)
* [3.10: Fornire attributi di profilo utilizzati come chiavi per le regole di inclusione](#keys)
* [3.11: Attivare la richiesta di caricamento pagina](#fire)
* [3.12: richiesta di localizzazione regionale di un incendio](#location)

## 3.1. Promozione {#promotion}

Aggiungi gli elementi in promozione e controllane il posizionamento nella progettazione dei consigli scegliendo Promozioni prima o dopo nell&#39;interfaccia utente [!DNL Target] durante la creazione dell&#39;attività.

+++Consulta i dettagli

**Opzioni disponibili**

* Promuovi per ID
* [Promuovi per raccolta](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/collections.html){target=_blank}
* [Promuovi per attributo](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html){target=_blank}

**Parametri di entità richiesti**

* Gli attributi degli articoli nelle promozioni devono essere passati quando si utilizza l&#39;opzione &quot;Promuovi per attributo&quot;.

**Letture**

* [Aggiungi promozioni](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-activity/adding-promotions.html){target=_blank}

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 3.2: Criteri basati sul carrello {#cart}

Creare consigli in base al contenuto del carrello dell’utente.

+++Consulta i dettagli

**Criteri disponibili**

* [!UICONTROL People Who Viewed These, Viewed Those]
* [!UICONTROL People Who Viewed These, Bought Those]
* [!UICONTROL People Who Bought These, Bought Those]

**Parametri di entità richiesti**

* cartIds

**Letture**

* [Basato su carrello](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 3.3: Criteri basati sulla popolarità {#popularity}

Puoi formulare raccomandazioni in base alla popolarità complessiva di un elemento nel tuo sito o in base alla popolarità degli elementi nella categoria, nel brand, nel genere e così via preferiti o più visualizzati di un visitatore.

+++Consulta i dettagli

**Criteri disponibili**

* [!UICONTROL Most Viewed Across the Site]
* [!UICONTROL Most Viewed by Category]
* [!UICONTROL Most Viewed by Item Attribute]
* [!UICONTROL Top Sellers Across the Site]
* [!UICONTROL Top Sellers by Category]
* [!UICONTROL Top Sellers by Item Attribute]
* [!UICONTROL Top by Analytics Metric]

**Parametri di entità richiesti**

* `entity.categoryId` o l&#39;attributo dell&#39;elemento per la popolarità se il criterio è basato sull&#39;attributo corrente o sull&#39;attributo dell&#39;elemento.
* Non devi passare nulla per Più visualizzato/Più venduto in tutto il sito.

**Letture**

* [Basato sulla popolarità](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 3.4: Criteri basati sugli articoli {#item}

Puoi formulare raccomandazioni in base alla ricerca di elementi simili a quelli di un elemento che l’utente sta visualizzando o che ha recentemente visualizzato.

+++Consulta i dettagli

**Criteri disponibili**

* [!UICONTROL People Who Viewed This, Viewed That]
* [!UICONTROL People Who Viewed This, Bought That]
* [!UICONTROL People Who Bought This, Bought That]
* [!UICONTROL Items with Similar Attributes]

**Parametri di entità richiesti**

* `entity.id`
* Se come chiave viene utilizzato un attributo di profilo

**Letture**

* [Basato su elemento](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 3.5: Criteri basati sugli utenti {#user}

Creare consigli in base al comportamento dell’utente.

+++Consulta i dettagli

**Criteri disponibili**

* [!UICONTROL Recently Viewed Items]
* [!UICONTROL Recommended for You]

**Parametri di entità richiesti**

* `entity.id`

**Letture**

* [Basato su utente](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 3.6: Criteri personalizzati {#custom}

Formulare raccomandazioni in base a un file personalizzato caricato.

+++Consulta i dettagli

**Criteri disponibili**

* [!UICONTROL Custom algorithm]

**Parametri di entità richiesti**

`entity.id` o l&#39;attributo utilizzato come chiave per l&#39;algoritmo personalizzato

**Letture**

* [Criteri personalizzati](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 3.7: Fornire gli attributi utilizzati nelle regole di inclusione {#inclusion}

+++Consulta i dettagli

**Letture**

* [Utilizzare regole di inclusione dinamiche e statiche](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/dynamic-static/use-dynamic-and-static-inclusion-rules.html){target=_blank}

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 3.8: Fornire excludedIds {#exclude}

Passa gli ID entità per le entità da escludere dai consigli. Ad esempio, puoi escludere gli articoli già presenti nel carrello.

+++Consulta i dettagli

**Letture**

* [È possibile escludere un&#39;entità in modo dinamico?](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-faq/recommendations-faq.html?lang=en#exclude){target=_blank}

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 3.9: fornisci attributi di entità per aggiornare il catalogo prodotti per [!DNL Recommendations] {#entity-attributes}

+++Consulta i dettagli

**Letture**

* [Attributi di entità](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html){target=_blank}

Puoi eseguire questo passaggio anche creando [feed di prodotto](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/feeds.html){target=_blank} utilizzando l&#39;interfaccia utente [!DNL Target] per aggiornare il catalogo prodotti per [!DNL Recommendations].

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 3.10: Fornire attributi di profilo utilizzati come chiavi per le regole di inclusione {#keys}

Fornisci gli attributi del profilo utilizzati come chiavi per le regole di inclusione in qualsiasi criterio di Recommendations menzionato sopra.

+++ Vedi i dettagli

**Letture**

* [Attributi del profilo](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/profile-parameters.html){target=_blank}

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 3.11: Attivare la richiesta di caricamento pagina {#fire}

Questo passaggio attiva una chiamata [!DNL Delivery API] con payload `execute` > `pageLoad` nella richiesta. Il metodo `getOffers()` recupera l&#39;esperienza ed esegue il rendering di `applyOffers()` sulla pagina. La richiesta `pageLoad` è necessaria per il rendering delle esperienze create nel [Compositore esperienza visivo](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html){target=_blank} (VEC).

+++Consulta i dettagli

![Attiva diagramma richieste caricamento pagina](/help/dev/patterns/recs-atjs/assets/fire-page-load-request-combined.png){width="400" zoomable="yes"}

**Prerequisiti**

* Tutte le mappature dei dati devono essere eseguite utilizzando la funzione `targetPageParams`.

**Letture**

* [adobe.target.getOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md)
* [adobe.target.applyOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-applyoffers-atjs-2.md)

**Azioni**

* Utilizza i metodi `getOffers` e `applyOffers` per recuperare l&#39;esperienza utilizzando una chiamata API Page Load Request.

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 3.12: richiesta di localizzazione regionale di un incendio (#location)

Questo passaggio attiva una chiamata [!DNL Delivery API] con `execute` > `mboxes` payload nella richiesta. Il metodo `getOffers` recupera l&#39;esperienza e `applyOffers` esegue il rendering dell&#39;esperienza nella pagina. Puoi inviare più di una mbox sotto il payload `execute` > `mboxes`.

+++Consulta i dettagli

![Attiva diagramma di richiesta località regionale](/help/dev/patterns/recs-atjs/assets/fire-regional-location-request-combined.png){width="400" zoomable="yes"}

**Prerequisiti**

* Tutte le mappature dei dati devono essere eseguite utilizzando la funzione `targetPageParams`.

**Letture**

* [adobe.target.getOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md)
* [adobe.target.applyOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-applyoffers-atjs-2.md)

**Azioni**

* Utilizza i metodi `getOffers` e `applyOffers` per recuperare l&#39;esperienza utilizzando una chiamata API Page Load Request.

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

Procedi al passaggio 4: [Notifica a Target](/help/dev/patterns/recs-atjs/notify-target.md).
