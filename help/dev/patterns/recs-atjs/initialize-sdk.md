---
title: Inizializzare gli SDK
description: Verificare che siano stati eseguiti tutti i passaggi necessari per caricare [!DNL Adobe Target] La libreria JavaScript at.js viene eseguita nella sequenza corretta.
feature: APIs/SDKs
level: Experienced
role: Developer
hide: true
hidefromtoc: true
source-git-commit: 60b986b4d956972714cb485057484ee5d6eed2bb
workflow-type: tm+mt
source-wordcount: '1791'
ht-degree: 7%

---

# Inizializzare gli SDK

Segui i passaggi descritti in *Inizializza SDK* per garantire che tutte le attività necessarie per caricare [!DNL Adobe Target] La libreria JavaScript at.js viene eseguita nella sequenza corretta.

>[!TIP]
>
>Fare clic sulle immagini in questo argomento per espandere a schermo intero.

## Inizializza diagramma SDK {#diagram}

Per le applicazioni multipagina, questo flusso si verifica ogni volta che la pagina si ricarica o il visitatore passa a una nuova pagina del sito web.

I numeri dei passi nella figura seguente corrispondono alle sezioni riportate di seguito.

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

Questo passaggio garantisce che `VisitorAPI.js` la libreria è caricata, configurata e inizializzata correttamente.

+++Consulta i dettagli

![Caricare il diagramma SDK dell’API visitatore](/help/dev/patterns/recs-atjs/assets/load-visitor-combined.png){width="400" zoomable="yes"}

**Prerequisiti**

* Per utilizzare il servizio ID visitatore/API, la società deve essere abilitata per [!DNL Adobe Experience Cloud] e hanno un [!UICONTROL ID organizzazione]. Per ulteriori informazioni, consulta [Requisiti di Experience Cloud: ID organizzazione](https://experienceleague.adobe.com/docs/id-service/using/reference/requirements.html?){target=_blank} nel *Guida del servizio Identity* guida.
* Hai bisogno di `VisitorAPI.js` file. Dovresti avere già questo file, se [!DNL Adobe Analytics] implementato. Questo file può essere aggiunto anche tramite [[!DNL Adobe Experience Platform] estensione tag](https://experienceleague.adobe.com/docs/tags.html){target=_blank} or can be downloaded from the [Adobe Analytics Code Manager](https://experienceleague.adobe.com/docs/id-service/using/implementation/setup-analytics.html){target=_blank}.

**Configurare e consultare VisitorAPI.js**

Per ulteriori informazioni, consulta [Implementazione del servizio Experience Cloud per Target](https://experienceleague.adobe.com/docs/id-service/using/implementation/setup-target.html){target=_blank}.

**Letture**

* [Panoramica del servizio Experience Cloud Identity](https://experienceleague.adobe.com/docs/id-service/using/intro/overview.html){target=_blank}
* [Informazioni sul servizio ID](https://experienceleague.adobe.com/docs/id-service/using/intro/about-id-service.html){target=_blank}
* [Cookie e il servizio Experience Cloud Identity](https://experienceleague.adobe.com/docs/id-service/using/intro/cookies.html){target=_blank}
* [Richiesta e impostazione degli ID da parte del servizio Experience Cloud Identity](https://experienceleague.adobe.com/docs/id-service/using/intro/id-request.html){target=_blank}
* [Informazioni sulla sincronizzazione degli ID e sulle percentuali di corrispondenza](https://experienceleague.adobe.com/docs/id-service/using/intro/match-rates.html){target=_blank}

**Azioni**

* Incorpora il `VisitorAPI.js` nelle pagine Web.
* Leggi informazioni su [configurazioni disponibili per il servizio ID visitatore/API](https://experienceleague.adobe.com/docs/id-service/using/reference/requirements.html){target=_blank}.
* Dopo il `VisitorAPI.js` è caricato, utilizza `Visitor.getInstance` per inizializzare utilizzando le configurazioni necessarie.
* Acquisisci familiarità con [metodi disponibili](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/get-set.html){target=_blank}.

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 1.2: Imposta ID cliente {#set}

Questo passaggio ti aiuta a garantire che gli ID noti dei visitatori (ID CRM, ID utente e così via) siano associati all&#39;ID anonimo di [!DNL Adobe] per la personalizzazione tra dispositivi.

+++Consulta i dettagli

![Imposta ID cliente](/help/dev/patterns/recs-atjs/assets/set-customer-id-combined.png){width="400" zoomable="yes"}

**Prerequisiti**

* L’ID noto dei visitatori deve essere disponibile nel livello dati.

**Imposta ID cliente**
Per ulteriori informazioni, consulta [setCustomerIDs](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html){target=_blank}.

**Letture**

* [Sincronizzazione dei profili in tempo reale per mbox3rdPartyId](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/3rd-party-id.html){target=_blank}

**Azioni**

* Utilizzare `visitor.setCustomerIDs` per impostare l&#39;ID noto del visitatore.

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 1.3: Configurare la richiesta di caricamento pagina automatico {#automatic}

Questo passaggio consente a at.js di recuperare tutte le esperienze che devono essere sottoposte a rendering sulla pagina durante il caricamento del file di libreria JavaScript at.js.

+++Consulta i dettagli

![Configurare la richiesta di caricamento pagina automatico](/help/dev/patterns/recs-atjs/assets/configure-automatic-page-request-combined.png){width="400" zoomable="yes"}

**Prerequisiti**

* Non tutti i dati nel livello dati devono essere inviati a [!DNL Target]. Rivolgiti al team aziendale (team di marketing digitale) per determinare quali dati sono utili per la sperimentazione, l’ottimizzazione e la personalizzazione. Solo questi dati devono essere inviati a [!DNL Target].
* Assicurati di non inviare dati personali (PII, Personally Identifiable Information) a [!DNL Target].

**Configurare la richiesta di caricamento pagina automatico**

Per ulteriori informazioni, consulta [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md).

**Letture**

Scopri di più su `pageLoadEnabled` impostazione in [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md).

**Azioni**

* Modifica il `window.targetGlobalSettings` per abilitare le richieste di caricamento pagina automatico.

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 1.4: Configurare la gestione dello sfarfallio {#flicker}

Questo passaggio consente di evitare la visualizzazione momentanea di altri contenuti della pagina durante la distribuzione delle esperienze.

+++Consulta i dettagli

![Configurare il diagramma di gestione della visualizzazione momentanea di altri contenuti](/help/dev/patterns/recs-atjs/assets/flicker-handling-combined.png){width="400" zoomable="yes"}

**Prerequisiti**

* Discuti con il team responsabile delle prestazioni delle pagine web dei pro e dei contro del controllo della visualizzazione momentanea di altri contenuti utilizzando il metodo predefinito utilizzato da at.js. È possibile cercare modelli di progettazione che consentano di utilizzare soluzioni personalizzate per la gestione dello sfarfallio, ad esempio l&#39;animazione del caricatore. Se non trovate un pattern, potete richiederne uno nuovo.

**Configurare la gestione dello sfarfallio**

Per ulteriori informazioni, consulta [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md).

Impostazione `bodyHidingEnabled` a `true` nasconde l’intero corpo della pagina mentre è in corso la richiesta di caricamento della pagina. Se non hai attivato la richiesta di caricamento pagina automatico per qualsiasi motivo (ad esempio, se i dati non sono pronti in un secondo momento), è meglio impostare questa impostazione su `false`.

Se hai disabilitato `bodyHidingEnabled` poiché non si desidera attivare l&#39;APLR e si desidera attivare la richiesta di pagina in un secondo momento, oppure non è necessario gestire la visualizzazione momentanea di altri contenuti, è necessario implementare la gestione della visualizzazione momentanea di altri contenuti. È possibile gestire la visualizzazione momentanea di altri contenuti in due modi: nascondendo le sezioni in fase di test o visualizzando un pulsante sulle sezioni in fase di test.

**Letture**

* [Gestione at.js della visualizzazione momentanea di altri contenuti](/help/dev/implement/client-side/atjs/how-atjs-works/manage-flicker-with-atjs.md)
* Scopri gli oggetti bodyHiddenStyle e bodyHidingEnabled in [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md).

**Azioni**

* Modifica il `window.targetGlobalSettings` oggetto da impostare `bodyHiddenStyle` e `bodyHidingEnabled`.

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 1.5: Configurare la mappatura dei dati {#data-mapping}

Questo passaggio garantisce che tutti i dati che devono essere inviati a [!DNL Target] è impostato.

+++Consulta i dettagli

![Diagramma di mappatura dei dati](/help/dev/patterns/recs-atjs/assets/data-mapping.png){width="400" zoomable="yes"}

**Prerequisiti**

* Il livello dati deve essere pronto con tutti i dati che devono essere inviati a [!DNL Target].
* Recommendations: arricchisci profilo.
   * Superato `entity.id` per acquisire dati per articoli e criteri visualizzati di recente in base a criteri basati sull’ultimo prodotto visualizzato.
   * Superato `entity.id` per acquisire dati per i criteri di popolarità in base alla categoria preferita.
   * Passa l’attributo di profilo se i criteri personalizzati sono basati su di esso o utilizzati nel filtro della regola di inclusione in qualsiasi criterio.
* Recommendations: acquisire i dati di prodotto.
   * Altri parametri di entità (riservati e personalizzati) possono essere trasmessi per acquisire o aggiornare il catalogo dei prodotti in [!DNL Recommendations].
   * Il catalogo dei prodotti può essere aggiornato anche utilizzando i feed di entità utilizzando [!DNL Target] Interfaccia utente o API.

**Mappa dati su[!DNL Target]**

Per ulteriori informazioni, consulta [targetPageParams()](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md).

**Letture**

* [targetPageParams()](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md)
* [Pianificare e implementare la funzione Consigli](/help/dev/implement/recommendations/recommendations.md)
* [Configurare il catalogo Recommendations](/help/dev/implement/recommendations/recommendations.md)

**Azioni**

* Utilizza il `targetPageParams()` per impostare tutti i dati richiesti da inviare a [!DNL Target].

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 1.6: Promozione {#promotion}

Aggiungi articoli in promozione e controllane il posizionamento nel tuo [!DNL Target Recommendations] [progetti](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-design/create-design.html){target=_blank}.

+++Consulta i dettagli

**Opzioni disponibili**

* Promuovi per ID
* [Promuovi per raccolta](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/collections.html){target=_blank}
* [Promuovi per attributo](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html){target=_blank}

**Parametri di entità richiesti**

* L&#39;attributo dell&#39;articolo nella promozione deve essere passato quando si utilizza l&#39;opzione &quot;Promuovi per attributo&quot;.

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 1.7: Criteri basati sul carrello {#cart}

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

## 1.8: Criteri basati sulla popolarità {#popularity}

Puoi formulare raccomandazioni in base alla popolarità complessiva di un elemento nel tuo sito o in base alla popolarità degli elementi nella categoria, nel brand, nel genere e così via preferiti o più visualizzati di un utente.

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

* `entity.categoryId` o l&#39;attributo dell&#39;articolo in base alla popolarità, se il criterio è basato sull&#39;articolo corrente o sull&#39;attributo dell&#39;articolo.
* Non è necessario trasmettere nulla per i Più visualizzati/Più venduti nel sito.

**Letture**

* [Basato sulla popolarità](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 1.9: Criteri basati sugli articoli {#item}

Puoi formulare raccomandazioni in base alla ricerca di elementi simili a quelli di un elemento che l’utente sta visualizzando o che ha recentemente visualizzato.

+++Consulta i dettagli

**Criteri disponibili**

* [!UICONTROL Chi ha visualizzato questo ha visualizzato anche quello]
* [!UICONTROL Chi ha visualizzato questo ha acquistato anche quello]
* [!UICONTROL Chi ha comprato questo ha acquistato anche quello]
* [!UICONTROL Articoli con attributi simili]

**Parametri di entità richiesti**

* `entity.id` o qualsiasi attributo di profilo utilizzato come chiave

**Letture**

* [Basato su elemento](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 1.10: Criteri basati sugli utenti {#user}

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

## 1.11: Criteri personalizzati {#custom}

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

## 1.12: Fornire gli attributi utilizzati nelle regole di inclusione {#inclusion}

+++Consulta i dettagli

**Letture**

* [Utilizzare regole di inclusione dinamiche e statiche](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/dynamic-static/use-dynamic-and-static-inclusion-rules.html){target=_blank}

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 1.13: Fornire excludedIds {#exclude}

Passa gli ID entità per le entità da escludere dai consigli. Ad esempio, puoi escludere gli articoli già presenti nel carrello.

+++Consulta i dettagli

**Letture**

* [È possibile escludere un’entità in modo dinamico?](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-faq/recommendations-faq.html?lang=en#exclude){target=_blank}

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 1.14: Superare il `entity.event.detailsOnly=true` parametro {#true}

Utilizzare gli attributi di entità per trasmettere informazioni su prodotti o contenuti a [!DNL Target Recommendations].

+++Consulta i dettagli

**Letture**

* [Attributi di entità](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html?lang=en){target=_blank}

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 1.15: Configurare la mappatura dei dati remoti (remota)

Questo passaggio garantisce che tutti i dati che devono essere inviati a [!DNL Target] è impostato.

+++Consulta i dettagli

![Diagramma di mappatura dati remoti](/help/dev/patterns/recs-atjs/assets/remote-data-mapping.png){width="400" zoomable="yes"}

**Prerequisiti**

* Data Layer deve essere pronto con tutti i dati che devono essere inviati a [!DNL Target].

**Configurare i provider di dati**

Per ulteriori informazioni, consulta [Fornitori di dati](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#data-providers).

**Letture**

[funzione targetPageParams](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md)

**Azioni**

Utilizza il `targetPageParams()` per impostare tutti i dati richiesti da inviare a [!DNL Target].

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)

## 1.16: Caricare at.js {#web}

Questo passaggio assicura che la libreria JavaScript at.js sia caricata e inizializzata.

+++Consulta i dettagli

![Caricare un diagramma di Adobe Target Web SDK](/help/dev/patterns/recs-atjs/assets/load-web-sdk.png){width="400" zoomable="yes"}

**Prerequisiti**

* Scarica o chiedi al tuo team di marketing digitale `at.js 2.*x*` File libreria JavaScript.

*Letture*

* [Come funziona Target](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html){target=_blank}
* [Funzionamento di at.js](/help/dev/implement/client-side/atjs/how-atjs-works/how-atjs-works.md)
* [Implementare Target senza un sistema per la gestione dei tag](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md)

**Azioni**

Incorpora il file at.js in tutte le tue pagine web in cui si devono effettuare sperimentazione, ottimizzazione, personalizzazione e raccolta dati.

+++

[Torna al diagramma nella parte superiore di questa pagina.](#diagram)





























