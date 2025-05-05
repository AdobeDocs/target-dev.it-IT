---
keywords: diagramma di sistema, visualizzazione momentanea di altri contenuti, at.js, implementazione, libreria javascript, js, atjs, $ 8
description: Scopri come funziona la libreria JavaScript at.js di  [!DNL Target]  e i diagrammi di sistema per comprendere il flusso di lavoro durante il caricamento delle pagine.
title: Come funziona la libreria JavaScript at.js?
feature: at.js
exl-id: 9183797c-857b-4b7f-a573-6bb1d583f7b1
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '1127'
ht-degree: 66%

---

# Funzionamento di at.js

Per implementare [!DNL Adobe Target] sul lato client, devi utilizzare la libreria JavaScript at.js.

In un’implementazione lato client di [!DNL Adobe Target], [!DNL Target] distribuisce le esperienze associate a un’attività direttamente al browser client. Il browser determina quale esperienza visualizzare e la visualizza. Con un’implementazione lato client, puoi utilizzare un editor WYSIWYG (il [Compositore esperienza visivo](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html?lang=it)) o un’interfaccia non visiva (il [Compositore esperienza basato su moduli](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?lang=it)) per creare esperienze di test e personalizzazione.

## Che cos’è at.js?

La libreria at.js è la libreria di implementazione per l&#39;implementazione lato client di [!DNL Adobe Target]. La libreria at.js migliora i tempi di caricamento delle pagine per le implementazioni Web e fornisce migliori opzioni di implementazione per le applicazioni a pagina singola. at.js è la libreria di implementazione consigliata e viene aggiornata frequentemente con nuove funzionalità. È consigliabile che tutti i clienti implementino o eseguano la migrazione alla [versione più recente di at.js](/help/dev/implement/client-side/atjs/target-atjs-versions.md).

Per ulteriori informazioni, consulta [Librerie JavaScript di Target](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html?lang=it#libraries).

Nell&#39;implementazione di [!DNL Target] illustrata di seguito sono implementate le seguenti soluzioni Adobe Experience Cloud: [!DNL Analytics], Target e [!DNL Audience Manager]. Inoltre, sono implementati i seguenti servizi di base [!DNL Experience Cloud]: [!DNL Adobe Experience Platform], [!UICONTROL Audiences] e [!UICONTROL Visitor ID Service].

## Qual è la differenza tra i diagrammi dei flussi di lavoro di at.js 1.*x* e di at.js 2.x?

Consulta [Aggiornamento da at.js 1.x a at.js 2.x](/help/dev/implement/client-side/atjs/upgrading-from-atjs-1x-to-atjs-20.md) per informazioni sulle differenze tra 2.0 e 1.*x*.

Da un punto di vista avanzato, esistono alcune differenze tra le due versioni:

* at.js 2.x non ha un concetto di richiesta mbox globale, ma una richiesta al caricamento della pagina. Una richiesta di caricamento della pagina si può intendere come una richiesta per recuperare il contenuto da applicare al caricamento iniziale della pagina del sito web.
* at.js 2.x gestisce un concetto denominato [!UICONTROL Views], utilizzato per le applicazioni a pagina singola (SPA). at.js 1.*x* questo concetto non è disponibile.

## Diagrammi at.js 2.x

I seguenti diagrammi ti aiutano a comprendere il flusso di lavoro di at.js 2.x con [!UICONTROL Views] e come questo migliori l&#39;integrazione con l&#39;SPA. Per una migliore introduzione dei concetti utilizzati in at.js 2.x, consulta [Implementazione di un’applicazione a pagina singola](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md).

(Fare clic sull&#39;immagine per espanderla a larghezza intera.)

![Flusso di Target con at.js 2.x](/help/dev/implement/client-side/assets/system-diagram-atjs-20.png "Flusso di Target con at.js 2.x"){zoomable="yes"}

| Passaggio | Dettagli |
| --- | --- |
| 1 | La chiamata restituisce l&#39;[!UICONTROL Experience Cloud ID] se l’utente è autenticato; un’altra chiamata sincronizza l’ID cliente. |
| 2 | La libreria at.js viene caricata in modo sincrono e nasconde il corpo del documento.<br />at.js si carica anche in modo asincrono con un eventuale snippet prenascosto implementato sulla pagina. |
| 3 | Si effettua una richiesta di caricamento della pagina, con tutti i parametri configurati (MCID, SDID e ID cliente). |
| 4 | Gli script di profilo vengono eseguiti e quindi inseriti in [!UICONTROL Profile Store]. Store richiede tipi di pubblico idonei da [!UICONTROL Audience Library] (ad esempio, tipi di pubblico condivisi da [!DNL Adobe Analytics], [!DNL Audience Manager], ecc.).<br />Gli attributi del cliente vengono inviati al [!UICONTROL Profile Store] in un processo batch. |
| 5 | In base ai parametri di richiesta dell’URL e ai dati di profilo, [!DNL Target] determina le attività ed esperienze da restituire al visitatore per la pagina corrente e le visualizzazioni future. |
| 6 | Il contenuto di destinazione viene rinviato alla pagina, includendo facoltativamente i valori di profilo per ulteriore personalizzazione.<br />Il contenuto mirato sulla pagina corrente viene mostrato il più rapidamente possibile senza che venga visualizzato momentaneamente il contenuto predefinito.<br />Il contenuto mirato per le viste mostrate come risultato delle azioni dell’utente effettuate in un’applicazione a pagina singola viene memorizzato nella cache del browser, in modo da applicarlo immediatamente senza una chiamata al server aggiuntiva quando si attivano le viste tramite `triggerView()`. |
| 7 | I dati di Analytics vengono inviati ai server [!UICONTROL Data Collection]. |
| 8 | I dati di destinazione vengono confrontati con i dati di Analytics tramite SDID ed elaborati nell&#39;archivio dei rapporti di [!DNL Analytics].I dati di <br />[!DNL Analytics] possono quindi essere visualizzati sia in [!DNL Analytics] che in [!DNL Target] tramite i report (A4T). |

Ora, ovunque `triggerView()` sia implementato nel tuo SPA, le azioni e [!UICONTROL Views] vengono recuperati dalla cache e mostrati all&#39;utente senza una chiamata al server. `triggerView()` invia anche una richiesta di notifica al backend [!DNL Target] per incrementare e registrare i conteggi delle impression. Per ulteriori informazioni su at.js per applicazioni a pagina singola con viste, consulta [Implementazione di un’applicazione a pagina singola](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md).

(Fare clic sull&#39;immagine per espanderla a larghezza intera.)

![triggerView at.js 2.x flusso di Target](/help/dev/implement/client-side/assets/atjs-20-triggerview.png "triggerView at.js 2.x flusso di Target"){zoomable="yes"}

| Passaggio | Dettagli |
| --- | --- |
| 1 | Chiamata di `triggerView()` nell&#39;SPA per eseguire il rendering di [!UICONTROL View] e applicare azioni per modificare gli elementi visivi. |
| 2 | Il contenuto mirato per la visualizzazione viene letto dalla cache. |
| 3 | Il contenuto mirato viene mostrato il più rapidamente possibile senza che venga visualizzato momentaneamente il contenuto predefinito. |
| 4 | Richiesta di notifica inviata a [!DNL Target] [!UICONTROL Profile Store] per conteggiare il visitatore nell&#39;attività e nelle metriche incrementali. |
| 5 | [!DNL Analytics] dati inviati a [!UICONTROL Data Collection Servers]. |
| 6 | I dati di [!DNL Target] corrispondono ai dati di [!DNL Analytics] tramite SDID ed vengono elaborati nell&#39;archivio dei report di [!DNL Analytics]. I dati di [!DNL Analytics] possono quindi essere visualizzati sia in [!DNL Analytics] che in [!DNL Target] tramite i rapporti A4T. |

### Video: diagramma architetturale di at.js 2.x

at.js 2.x migliora il supporto di Adobe Target per le applicazioni a pagina singola e consente l’integrazione con altre soluzioni Experience Cloud. Questo video spiega come tutti questo elementi funzionano insieme.

>[!VIDEO](https://video.tv.adobe.com/v/26250/?quality=12)

Consulta la pagina relativa al [funzionamento di at.js 2.x](https://experienceleague.adobe.com/docs/target-learn/tutorials/implementation/understanding-how-atjs-20-works.html?lang=it) per ulteriori informazioni.

## Diagramma di at.js 1.x

I seguenti diagrammi ti aiutano a comprendere il flusso di lavoro di at.js 1.x.

(Fare clic sull&#39;immagine per espanderla a larghezza intera.)

![Flusso di Target at.js 1.x](/help/dev/implement/client-side/assets/target-flow.png "Flusso di Target at.js 1.x"){zoomable="yes"}

| Passaggio | Descrizione | Chiamata | Descrizione |
|--- |--- |--- |--- |
| 1 | La chiamata restituisce l’ID Experience Cloud (MCID) se l’utente è autenticato; un’altra chiamata sincronizza l’ID cliente. | 2 | La libreria at.js viene caricata in modo sincrono e nasconde il corpo del documento. |
| 3 | Viene effettuata una richiesta mbox globale, con tutti i parametri configurati, MCID, SDID e ID cliente (facoltativo). | 4 | Gli script di profilo vengono eseguiti e quindi inseriti nell’archivio profili. L’archivio richiede un pubblico idoneo dalla libreria Pubblico (ad esempio, pubblico condiviso da Adobe Analytics, Audience Manager, ecc.).<br />Gli attributi del cliente vengono inviati all’archivio profili in un processo batch. |
| 5 | In base all’URL, ai parametri mbox e ai dati di profilo, [!DNL Target] decide quali attività ed esperienze restituire al visitatore. | 6 | Il contenuto di destinazione viene rinviato alla pagina, includendo facoltativamente i valori di profilo per ulteriore personalizzazione.<br />L’esperienza viene mostrata il più rapidamente possibile senza che venga visualizzato momentaneamente il contenuto predefinito. |
| 7 | I dati Analytics vengono inviati ai server di raccolta dati. | 8 | I dati di Target vengono confrontati con i dati di Analytics tramite SDID ed elaborati nell’archivio dei rapporti di Analytics.I dati di <br />Analytics possono quindi essere visualizzati sia in Analytics che in [!DNL Target] tramite i report [!UICONTROL Analytics for Target] (A4T). |

### Video: Office Hours, suggerimenti e panoramica su at.js (26 giugno 2019)

Questo video è una registrazione di &quot;Office Hours&quot;, un&#39;iniziativa condotta dal team [!UICONTROL Adobe Customer Care].

* Vantaggi dell’utilizzo di at.js
* Impostazioni di at.js
* Gestione dello sfarfallio
* Debug di at.js
* Problemi noti
* Domande frequenti

>[!VIDEO](https://video.tv.adobe.com/v/27959/?quality=12)

## Come avviene il rendering delle offerte con contenuti HTML in at.js

Nel rendering delle offerte con contenuti HTML, at.js applica il seguente algoritmo:

1. Le immagini vengono precaricate (se nel contenuto HTML sono presenti tag `<img>`).

1. Il contenuto HTML viene associato al nodo DOM.

1. Vengono eseguiti gli script in linea (codice racchiuso tra tag `<script>`).

1. Gli script remoti (tag `<script>` con attributi `src`) vengono caricati in modo asincrono ed eseguiti.

Note importanti:

* at.js non garantisce in alcun modo l’ordine di esecuzione di script remoti, poiché questi vengono caricati in modo asincrono.
* Gli script in linea non devono avere dipendenze da script remoti, poiché questi ultimi vengono caricati ed eseguiti successivamente.
