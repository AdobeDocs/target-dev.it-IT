---
title: Inizializzare gli SDK
description: Assicurati che tutti i passaggi necessari per il caricamento della libreria JavaScript at.js di  [!DNL Adobe Target]  siano eseguiti nella sequenza corretta.
feature: APIs/SDKs
level: Experienced
role: Developer
exl-id: 250a8382-1fdd-4a70-b712-a25af5adad71
source-git-commit: 50ee7e66e30c0f8367763a63b6fde5977d30cfe7
workflow-type: tm+mt
source-wordcount: '1558'
ht-degree: 5%

---

# Inizializzare gli SDK

Segui i passaggi descritti nel diagramma *Initialize SDK* per assicurarti che tutte le attività necessarie per caricare la libreria at.js JavaScript di [!DNL Adobe Target] vengano eseguite nella sequenza corretta.

>[!TIP]
>
>Fare clic sulle immagini in questo argomento per espandere a schermo intero.

## Inizializza diagramma SDK {#diagram}

Per le applicazioni multipagina, questo flusso si verifica ogni volta che la pagina si ricarica o il visitatore passa a una nuova pagina del sito web.

>[!NOTE]
>
>I numeri dei passi nella figura seguente corrispondono alle sezioni riportate di seguito. I numeri dei passaggi non sono in un ordine particolare e non riflettono l&#39;ordine dei passaggi eseguiti nell&#39;interfaccia utente [!DNL Target] durante la creazione dell&#39;attività.

![Inizializza diagramma SDK](/help/dev/patterns/recs-atjs/assets/diagram-initiaze-sdk.png){width="600" zoomable="yes"}

Fai clic sui seguenti collegamenti per passare alle sezioni desiderate:

* [1.1: Caricare l’SDK dell’API visitatore](#load)
* [1.2: Imposta ID cliente](#set)
* [1.3: Configurare la richiesta di caricamento pagina automatico](#automatic)
* [1.4: Configurare la gestione dello sfarfallio](#flicker)
* [1.5: Configurare la mappatura dei dati](#data-mapping)
* [1.6: Promozione](#promotion)
* [1.7: Criteri basati sul carrello](#cart)
* [1.8: Criteri basati sulla popolarità](#popularity)
* [1.9: Criteri basati sugli articoli](#item)
* [1.10: Criteri basati sugli utenti](#user)
* [1.11: Criteri personalizzati](#custom)
* [1.12: Fornire gli attributi utilizzati nelle regole di inclusione](#inclusion)
* [1.13: Fornire excludedIds](#exclude)
* [1.14: passa il parametro entity.event.detailsOnly=true](#true)
* [1.15: Configurare la mappatura dei dati remoti](#remote)
* [1.16: Caricare at.js](#web)

## 1.1: Caricare l’SDK dell’API visitatore {#load}

Questo passaggio garantisce che la libreria `VisitorAPI.js` sia caricata, configurata e inizializzata correttamente.

+++Consulta i dettagli

![Carica diagramma SDK API visitatore](/help/dev/patterns/recs-atjs/assets/load-visitor-combined.png){width="400" zoomable="yes"}

**Prerequisiti**

* Per utilizzare il servizio ID visitatore/API, la società deve essere abilitata per [!DNL Adobe Experience Cloud] e disporre di [!UICONTROL Organization ID]. Per ulteriori informazioni, consulta [Requisiti di Experience Cloud: ID organizzazione](https://experienceleague.adobe.com/docs/id-service/using/reference/requirements.html?lang=it&){target=_blank} nella *Guida del servizio Identity*.
* È necessario il file `VisitorAPI.js`. Questo file dovrebbe essere già presente se sono stati implementati [!DNL Adobe Analytics]. Questo file può anche essere aggiunto tramite l&#39;estensione [[!DNL Adobe Experience Platform] tags](https://experienceleague.adobe.com/docs/tags.html?lang=it){target=_blank} o scaricato da [Adobe Analytics Code Manager](https://experienceleague.adobe.com/docs/id-service/using/implementation/setup-analytics.html?lang=it){target=_blank}.

**Configura e fai riferimento a VisitorAPI.js**

Per ulteriori informazioni, vedere [Implementare il servizio Experience Cloud per Target](https://experienceleague.adobe.com/docs/id-service/using/implementation/setup-target.html?lang=it){target=_blank}.

**Letture**

* [Panoramica del servizio Experience Cloud Identity](https://experienceleague.adobe.com/docs/id-service/using/intro/overview.html?lang=it){target=_blank}
* [Informazioni sul servizio ID](https://experienceleague.adobe.com/docs/id-service/using/intro/about-id-service.html?lang=it){target=_blank}
* [Cookie e il servizio Experience Cloud Identity](https://experienceleague.adobe.com/docs/id-service/using/intro/cookies.html?lang=it){target=_blank}
* [Richiesta e impostazione degli ID da parte del servizio Experience Cloud Identity](https://experienceleague.adobe.com/docs/id-service/using/intro/id-request.html?lang=it){target=_blank}
* [Informazioni sulla sincronizzazione degli ID e sulle percentuali di corrispondenza](https://experienceleague.adobe.com/docs/id-service/using/intro/match-rates.html?lang=it){target=_blank}

**Azioni**

* Incorporare il file `VisitorAPI.js` nelle pagine Web.
* Scopri le [configurazioni disponibili per il servizio ID visitatore/API](https://experienceleague.adobe.com/docs/id-service/using/reference/requirements.html?lang=it){target=_blank}.
* Dopo il caricamento del file `VisitorAPI.js`, utilizzare il metodo `Visitor.getInstance` per inizializzare utilizzando le configurazioni necessarie.
* Acquisisci familiarità con i [metodi disponibili](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/get-set.html?lang=it){target=_blank}.

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 1.2: Imposta ID cliente {#set}

Questo passaggio garantisce che gli ID noti dei visitatori (ID CRM, ID utente e così via) siano associati all&#39;ID anonimo di [!DNL Adobe] per la personalizzazione tra dispositivi diversi.

+++Consulta i dettagli

![Imposta ID cliente](/help/dev/patterns/recs-atjs/assets/set-customer-id-combined.png){width="400" zoomable="yes"}

**Prerequisiti**

* L’ID noto dei visitatori deve essere disponibile nel livello dati.

**Imposta ID cliente**
Per ulteriori informazioni, vedere [setCustomerIDs](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html?lang=it){target=_blank}.

**Letture**

* [Sincronizzazione dei profili in tempo reale per mbox3rdPartyId](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/3rd-party-id.html?lang=it){target=_blank}

**Azioni**

* Utilizza `visitor.setCustomerIDs` per impostare l&#39;ID noto del visitatore.

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 1.3: Configurare la richiesta di caricamento pagina automatico {#automatic}

Questo passaggio consente a at.js di recuperare tutte le esperienze di cui è necessario eseguire il rendering sulla pagina durante il caricamento del file della libreria JavaScript at.js.

+++Consulta i dettagli

![Configura richiesta di caricamento pagina automatico](/help/dev/patterns/recs-atjs/assets/configure-automatic-page-request-combined.png){width="400" zoomable="yes"}

**Prerequisiti**

* Non tutti i dati nel livello dati devono essere inviati a [!DNL Target]. Rivolgiti al team aziendale (team di marketing digitale) per determinare quali dati sono utili per la sperimentazione, l’ottimizzazione e la personalizzazione. Solo questi dati devono essere inviati a [!DNL Target].
* Assicurarsi di non inviare dati PII (Personally Identifiable Information) a [!DNL Target].

**Configura richiesta di caricamento pagina automatico**

Per ulteriori informazioni, consulta [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md).

**Letture**

Informazioni sull&#39;impostazione `pageLoadEnabled` in [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md).

**Azioni**

* Modificare l&#39;oggetto `window.targetGlobalSettings` per abilitare le richieste di caricamento pagina automatico.

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 1.4: Configurare la gestione dello sfarfallio {#flicker}

Questo passaggio consente di evitare la visualizzazione momentanea di altri contenuti della pagina durante la distribuzione delle esperienze.

+++Consulta i dettagli

![Configura diagramma gestione sfarfallio](/help/dev/patterns/recs-atjs/assets/flicker-handling-combined.png){width="400" zoomable="yes"}

**Prerequisiti**

* Discuti con il team responsabile delle prestazioni delle pagine web dei pro e dei contro del controllo della visualizzazione momentanea di altri contenuti utilizzando il metodo predefinito utilizzato da at.js. È possibile cercare modelli di progettazione che consentano di utilizzare soluzioni personalizzate per la gestione dello sfarfallio, ad esempio l&#39;animazione del caricatore. Se non trovate un pattern, potete richiederne uno nuovo.

**Configura gestione sfarfallio**

Per ulteriori informazioni, consulta [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md).

Se si imposta `bodyHidingEnabled` su `true`, verrà nascosto l&#39;intero corpo della pagina mentre è in corso la richiesta di caricamento della pagina. Se non hai attivato la richiesta di caricamento pagina automatico per nessun motivo (ad esempio, se i dati non sono pronti in un secondo momento), conviene impostare questa impostazione su `false`.

Se `bodyHidingEnabled` è stato disabilitato perché non si desidera attivare l&#39;APLR e si desidera attivare la richiesta di pagina in un secondo momento oppure non è necessario gestire la visualizzazione momentanea di altri contenuti, è necessario implementare la gestione della visualizzazione momentanea di altri contenuti. È possibile gestire la visualizzazione momentanea di altri contenuti in due modi: nascondendo le sezioni in fase di test o visualizzando un pulsante sulle sezioni in fase di test.

**Letture**

* [Gestione at.js della visualizzazione momentanea di altri contenuti](/help/dev/implement/client-side/atjs/how-atjs-works/manage-flicker-with-atjs.md)
* Scopri gli oggetti bodyHiddenStyle e bodyHidingEnabled in [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md).

**Azioni**

* Modificare l&#39;oggetto `window.targetGlobalSettings` per impostare `bodyHiddenStyle` e `bodyHidingEnabled`.

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 1.5: Configurare la mappatura dei dati {#data-mapping}

Questo passaggio garantisce che tutti i dati che devono essere inviati a [!DNL Target] siano impostati.

+++Consulta i dettagli

![Diagramma mappatura dati](/help/dev/patterns/recs-atjs/assets/data-mapping-combined.png){width="400" zoomable="yes"}

**Prerequisiti**

* Il livello dati deve essere pronto con tutti i dati che devono essere inviati a [!DNL Target].
* Recommendations: arricchisci profilo.
   * Passa `entity.id` per acquisire i dati per gli elementi e i criteri visualizzati di recente in base ai criteri dell&#39;ultimo prodotto visualizzato.
   * Passa `entity.id` per acquisire i dati per i criteri di popolarità in base alla categoria preferita.
   * Passa l’attributo di profilo se i criteri personalizzati sono basati su di esso o utilizzati nel filtro della regola di inclusione in qualsiasi criterio.
* Recommendations: acquisire i dati di prodotto.
   * Altri parametri di entità (riservati e personalizzati) possono essere passati per acquisire o aggiornare il catalogo prodotti in [!DNL Recommendations].
   * È inoltre possibile aggiornare il catalogo prodotti utilizzando feed di entità tramite l&#39;interfaccia utente o l&#39;API [!DNL Target].

**Mappa i dati su[!DNL Target]**

Per ulteriori informazioni, vedere [targetPageParams()](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md).

**Letture**

* [targetPageParams()](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md)
* [Pianificare e implementare la funzione Consigli](/help/dev/implement/recommendations/recommendations.md)
* [Configurare il catalogo Recommendations](/help/dev/implement/recommendations/recommendations.md)

**Azioni**

* Utilizzare la funzione `targetPageParams()` per impostare tutti i dati richiesti da inviare a [!DNL Target].

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 1.6: Promozione {#promotion}

Aggiungi elementi promossi e controllane il posizionamento nelle [!DNL Target Recommendations] [progettazioni](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-design/create-design.html?lang=it){target=_blank}.

+++Consulta i dettagli

**Opzioni disponibili**

* Promuovi per ID
* [Promuovi per raccolta](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/collections.html?lang=it){target=_blank}
* [Promuovi per attributo](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html?lang=it){target=_blank}

**Parametri di entità richiesti**

* L&#39;attributo dell&#39;articolo nella promozione deve essere passato quando si utilizza l&#39;opzione &quot;Promuovi per attributo&quot;.

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 1.7: Criteri basati sul carrello {#cart}

Creare consigli in base al contenuto del carrello dell’utente.

+++Consulta i dettagli

**Criteri disponibili**

* [!UICONTROL People Who Viewed These, Viewed Those]
* [!UICONTROL People Who Viewed These, Bought Those]
* [!UICONTROL People Who Bought These, Bought Those]

**Parametri di entità richiesti**

* cartIds

**Letture**

* [Basato su carrello](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=it#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 1.8: Criteri basati sulla popolarità {#popularity}

Puoi formulare raccomandazioni in base alla popolarità complessiva di un elemento nel tuo sito o in base alla popolarità degli elementi nella categoria, nel brand, nel genere e così via preferiti o più visualizzati di un utente.

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

* `entity.categoryId` o l&#39;attributo dell&#39;elemento per la popolarità se il criterio è basato sull&#39;elemento corrente o sull&#39;attributo dell&#39;elemento.
* Non è necessario trasmettere nulla per i Più visualizzati/Più venduti nel sito.

**Letture**

* [Basato sulla popolarità](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=it#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 1.9: Criteri basati sugli articoli {#item}

Puoi formulare raccomandazioni in base alla ricerca di elementi simili a quelli di un elemento che l’utente sta visualizzando o che ha recentemente visualizzato.

+++Consulta i dettagli

**Criteri disponibili**

* [!UICONTROL People Who Viewed This, Viewed That]
* [!UICONTROL People Who Viewed This, Bought That]
* [!UICONTROL People Who Bought This, Bought That]
* [!UICONTROL Items with Similar Attributes]

**Parametri di entità richiesti**

* `entity.id` o qualsiasi attributo di profilo utilizzato come chiave

**Letture**

* [Basato su elemento](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=it#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 1.10: Criteri basati sugli utenti {#user}

Creare consigli in base al comportamento dell’utente.

+++Consulta i dettagli

**Criteri disponibili**

* [!UICONTROL Recently Viewed Items]
* [!UICONTROL Recommended for You]

**Parametri di entità richiesti**

* `entity.id`

**Letture**

* [Basato su utente](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=it#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 1.11: Criteri personalizzati {#custom}

Formulare raccomandazioni in base a un file personalizzato caricato.

+++Consulta i dettagli

**Criteri disponibili**

* [!UICONTROL Custom algorithm]

**Parametri di entità richiesti**

`entity.id` o l&#39;attributo utilizzato come chiave per l&#39;algoritmo personalizzato

**Letture**

* [Criteri personalizzati](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=it#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 1.12: Fornire gli attributi utilizzati nelle regole di inclusione {#inclusion}

+++Consulta i dettagli

**Letture**

* [Utilizzare regole di inclusione dinamiche e statiche](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/dynamic-static/use-dynamic-and-static-inclusion-rules.html?lang=it){target=_blank}

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 1.13: Fornire excludedIds {#exclude}

Passa gli ID entità per le entità da escludere dai consigli. Ad esempio, puoi escludere gli articoli già presenti nel carrello.

+++Consulta i dettagli

**Letture**

* [È possibile escludere un&#39;entità in modo dinamico?](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-faq/recommendations-faq.html?lang=it#exclude){target=_blank}

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 1.14: passa il parametro `entity.event.detailsOnly=true` {#true}

Utilizzare gli attributi di entità per passare informazioni su prodotti o contenuti a [!DNL Target Recommendations].

+++Consulta i dettagli

**Letture**

* [Attributi di entità](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html?lang=it){target=_blank}

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 1.15: Configurare la mappatura dei dati remoti (remota)

Questo passaggio assicura che tutti i dati che devono essere inviati a [!DNL Target] siano impostati.

+++Consulta i dettagli

![Diagramma di mappatura dati remoti](/help/dev/patterns/recs-atjs/assets/remote-data-mapping-combined.png){width="400" zoomable="yes"}

**Prerequisiti**

* Data Layer deve essere pronto con tutti i dati che devono essere inviati a [!DNL Target].

**Configurare i provider di dati**

Per ulteriori informazioni, vedere [Provider di dati](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#data-providers).

**Letture**

[funzione targetPageParams](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md)

**Azioni**

Utilizzare la funzione `targetPageParams()` per impostare tutti i dati richiesti da inviare a [!DNL Target].

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 1.16: Caricare at.js {#web}

Questo passaggio assicura che la libreria JavaScript at.js sia caricata e inizializzata.

+++Consulta i dettagli

![Carica diagramma at.js di Adobe Target](/help/dev/patterns/recs-atjs/assets/load-atjs-combined.png){width="400" zoomable="yes"}

**Prerequisiti**

* Scarica o chiedi al tuo team di marketing digitale il file della libreria JavaScript `at.js 2.*x*`.

*Letture*

* [Funzionamento di Target](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html?lang=it){target=_blank}
* [Funzionamento di at.js](/help/dev/implement/client-side/atjs/how-atjs-works/how-atjs-works.md)
* [Implementare Target senza un sistema per la gestione dei tag](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md)

**Azioni**

Incorpora il file at.js in tutte le tue pagine web in cui si devono effettuare sperimentazione, ottimizzazione, personalizzazione e raccolta dati.

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

Procedere con il passaggio 2: [Configurare la raccolta dati](/help/dev/patterns/recs-atjs/data-collection.md).
