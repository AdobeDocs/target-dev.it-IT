---
title: Esperienze di rendering
description: Assicurati che tutti i passaggi necessari per il rendering delle esperienze vengano eseguiti nella sequenza corretta.
feature: APIs/SDKs
level: Experienced
role: Developer
hide: true
hidefromtoc: true
source-git-commit: 1349004b7da45b775b27a6ba119a26f77a9607fb
workflow-type: tm+mt
source-wordcount: '1080'
ht-degree: 7%

---

# Esperienze di rendering

Segui i passaggi descritti in *Esperienze di rendering* per garantire che tutte le attività necessarie per il rendering delle esperienze vengano eseguite nella sequenza corretta.

>[!NOTE]
>
>Se è stata abilitata la funzione Automatic Page Load Request durante il [Passaggio Configura richiesta di caricamento pagina automatico](/help/dev/patterns/recs-atjs/initialize-sdk.md#automatic) in *Inizializzare gli SDK* , puoi saltare questa attività a meno che non desideri chiamare l&#39;SDK di Adobe Target per eseguire il rendering di altre esperienze utilizzando una richiesta di posizione regionale.

>[!TIP]
>
>Fare clic sulle immagini in questo argomento per espandere a schermo intero.

## Diagramma esperienze di rendering {#diagram}

La gestione automatica della visualizzazione momentanea di altri contenuti pronta all’uso disponibile con at.js ha senso solo quando si dispone di [!UICONTROL Richiesta caricamento pagina automatico] abilitato. Questa opzione nasconde l’intero corpo del HTML durante il recupero delle esperienze da [!DNL Target]. In questo caso, è tua responsabilità gestire la visualizzazione momentanea di altri contenuti. Per assistenza, cerca i modelli di implementazione disponibili per la gestione della visualizzazione momentanea di altri contenuti.

I numeri dei passi nella figura seguente corrispondono alle sezioni riportate di seguito.

![Diagramma esperienze di rendering](/help/dev/patterns/recs-atjs/assets/diagram-render-experiences-new.png){width="600" zoomable="yes"}

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

Aggiungi articoli in promozione e controllane il posizionamento nel Recommendations di Target [progetti](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-design/create-design.html){target=_blank}.

+++Consulta i dettagli

**Opzioni disponibili**

* Promuovi per ID
* [Promuovi per raccolta](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/collections.html){target=_blank}
* [Promuovi per attributo](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html){target=_blank}

**Parametri di entità richiesti**

* Gli attributi degli articoli nelle promozioni devono essere passati quando si utilizza l&#39;opzione &quot;Promuovi per attributo&quot;.

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 3.2: Criteri basati sul carrello {#cart}

Creare consigli in base al contenuto del carrello dell’utente.

+++Consulta i dettagli

**Criteri disponibili**

* [!UICONTROL Chi ha visualizzato questi ha visualizzato anche quelli]
* [!UICONTROL Chi ha visualizzato questi ha acquistato anche quelli]
* [!UICONTROL Chi ha comprato questi ha acquistato anche quelli]

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

* [!UICONTROL Articoli più visualizzati nel sito]
* [!UICONTROL Più visualizzati per categoria]
* [!UICONTROL Più visualizzati per attributo articolo]
* [!UICONTROL Articoli più venduti in tutto il sito]
* [!UICONTROL Articoli più venduti per categoria]
* [!UICONTROL Attributo Articoli più venduti]
* [!UICONTROL Primi per metrica di Analytics]

**Parametri di entità richiesti**

* `entity.categoryId` o l&#39;attributo dell&#39;articolo in base alla popolarità, se il criterio è basato sull&#39;attributo corrente o sull&#39;attributo dell&#39;articolo.
* Non devi passare nulla per Più visualizzato/Più venduto in tutto il sito.

**Letture**

* [Basato sulla popolarità](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 3.4: Criteri basati sugli articoli {#item}

Puoi formulare raccomandazioni in base alla ricerca di elementi simili a quelli di un elemento che l’utente sta visualizzando o che ha recentemente visualizzato.

+++Consulta i dettagli

**Criteri disponibili**

* [!UICONTROL Chi ha visualizzato questo ha visualizzato anche quello]
* [!UICONTROL Chi ha visualizzato questo ha acquistato anche quello]
* [!UICONTROL Chi ha comprato questo ha acquistato anche quello]
* [!UICONTROL Articoli con attributi simili]

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

* [!UICONTROL Articoli visualizzati di recente]
* [!UICONTROL Consigliato per te]

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

* [!UICONTROL Algoritmo personalizzato]

**Parametri di entità richiesti**

`entity.id` o l’attributo utilizzato come chiave per l’algoritmo personalizzato

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

* [È possibile escludere un’entità in modo dinamico?](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-faq/recommendations-faq.html?lang=en#exclude){target=_blank}

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 3.9: Fornisci attributi di entità per aggiornare il catalogo prodotti per [!DNL Recommendations] {#entity-attributes}

+++Consulta i dettagli

**Letture**

* [Attributi di entità](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html){target=_blank}

Puoi anche eseguire questo passaggio creando [feed di prodotto](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/feeds.html){target=_blank} utilizzando [!DNL Target] Interfaccia utente per aggiornare il catalogo prodotti per [!DNL Recommendations].

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 3.10: Fornire attributi di profilo utilizzati come chiavi per le regole di inclusione {#keys}

Fornisci gli attributi del profilo utilizzati come chiavi per le regole di inclusione in qualsiasi criterio di Recommendations menzionato sopra.

+++ Consulta i dettagli

**Letture**

* [Attributi del profilo](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/profile-parameters.html){target=_blank}

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 3.11: Attivare la richiesta di caricamento pagina {#fire}

Questo passaggio attiva un [!DNL Delivery API] chiama con `execute` > `pageLoad` payload nella richiesta. Il `getOffers()` il metodo recupera l&#39;esperienza e `applyOffers()` esegue il rendering dell’esperienza sulla pagina. Il `pageLoad` richiesta necessaria per il rendering delle esperienze create in [Compositore esperienza visivo](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html){target=_blank} (VEC).

+++Consulta i dettagli

![Attiva il diagramma di richiesta di caricamento pagina](/help/dev/patterns/recs-atjs/assets/fire-page-load-request.png){width="300" zoomable="yes"}

**Prerequisiti**

* Tutta la mappatura dei dati deve essere eseguita utilizzando `targetPageParams` funzione.

**Letture**

* [adobe.target.getOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md)
* [adobe.target.applyOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-applyoffers-atjs-2.md)

**Azioni**

* Utilizza il `getOffers` e `applyOffers` metodi per recuperare l’esperienza utilizzando una chiamata API Page Load Request.

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 3.12: richiesta di localizzazione regionale di un incendio (#location)

Questo passaggio attiva un [!DNL Delivery API] chiama con `execute` > `mboxes` payload nella richiesta. Il `getOffers` il metodo recupera l&#39;esperienza e `applyOffers` esegue il rendering dell’esperienza nella pagina. Puoi inviare più mbox sotto `execute` > `mboxes` payload.

+++Consulta i dettagli

![Attiva diagramma di richiesta posizione regionale](/help/dev/patterns/recs-atjs/assets/fire-regional-location-request.png){width="300" zoomable="yes"}

**Prerequisiti**

* Tutta la mappatura dei dati deve essere eseguita utilizzando `targetPageParams` funzione.

**Letture**

* [adobe.target.getOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md)
* [adobe.target.applyOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-applyoffers-atjs-2.md)

**Azioni**

* Utilizza il `getOffers` e `applyOffers` metodi per recuperare l’esperienza utilizzando una chiamata API Page Load Request.

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)