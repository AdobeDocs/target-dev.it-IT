---
keywords: implementazione, libreria javascript, js, atjs, decisioning sul dispositivo, decisioning sul dispositivo, at.js, su dispositivo, sul dispositivo, implementazione0
description: Scopri come eseguire [!UICONTROL on-device decisioning] con la libreria at.js
title: Come funziona il decisioning sul dispositivo con la libreria JavaScript at.js?
feature: at.js
exl-id: bd0e062f-c259-46f3-adba-e380af058ac8
source-git-commit: 67cc93cf697f8d5bca6fedb3ae974e4012347a0b
workflow-type: tm+mt
source-wordcount: '3478'
ht-degree: 4%

---

# [!UICONTROL On-device decisioning] per at.js

A partire dalla versione 2.5.0, at.js offre [!UICONTROL on-device decisioning]. [!UICONTROL On-device decisioning] consente di memorizzare nella cache le attività [Test A/B](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html) e [Targeting esperienza](https://experienceleague.adobe.com/docs/target/using/activities/experience-targeting/experience-target.html) (XT) nel browser per eseguire le decisioni in memoria senza una richiesta di blocco della rete all&#39;Edge Network [!DNL Adobe Target].

>[!NOTE]
>
>[!UICONTROL On-device decisioning] è disponibile per le implementazioni lato client e lato server. Questo articolo descrive [!UICONTROL on-device decisioning] per lato client. Per informazioni su [!UICONTROL on-device decisioning] per lato server, consulta la documentazione sull&#39;implementazione lato server [qui](../../../server-side/sdk-guides/on-device-decisioning/overview.md).

[!DNL Target] offre inoltre la flessibilità di fornire l&#39;esperienza più rilevante e aggiornata dalle attività di personalizzazione basate sulla sperimentazione e sull&#39;apprendimento automatico (basate su apprendimento automatico) tramite una chiamata al server live. In altre parole, quando le prestazioni sono più importanti, è possibile scegliere di utilizzare [!UICONTROL on-device decisioning]. Tuttavia, quando è necessaria l’esperienza più rilevante, aggiornata e basata su apprendimento automatico, è possibile effettuare una chiamata al server.

## Quali vantaggi offre [!UICONTROL on-device decisioning]?

I vantaggi di [!UICONTROL on-device decisioning] includono:

* **Offri esperienze e decisioni rapide e sorprendenti.** Il bucket e le decisioni vengono eseguiti in memoria e nel browser per evitare il blocco delle richieste di rete.
* **Miglioramento delle prestazioni dell&#39;applicazione.** Esegui esperimenti e fornisci personalizzazione ai tuoi clienti e utenti senza compromettere le esperienze degli utenti finali.
* **Miglioramento del punteggio di qualità del sito Google.** Con le decisioni prese in memoria, migliora il punteggio di qualità del sito Google del tuo business online per renderlo più individuabile dai consumatori.
* **Scopri le funzionalità di analisi in tempo reale.** Ottieni informazioni dalle prestazioni dell&#39;attività in tempo reale tramite il reporting di [Analytics for Target](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html) (A4T). A4T consente di ruotare la strategia nei momenti critici.

## Funzioni supportate

Il SDK JS [!DNL Adobe Target] offre ai clienti la flessibilità di scegliere tra prestazioni e aggiornamento dei dati per le decisioni. In altre parole, se la distribuzione dei contenuti personalizzati più rilevanti e coinvolgenti tramite l’apprendimento automatico è la cosa più importante per te, è necessario effettuare una chiamata al server live. Tuttavia, quando le prestazioni sono più importanti, è necessario prendere una decisione su dispositivo e in memoria. Affinché [!UICONTROL on-device decisioning] funzioni, consulta l&#39;elenco delle funzionalità supportate:

* Tipi di attività
* Targeting del pubblico
* Metodo di allocazione

Per ulteriori informazioni, vedere [Funzionalità supportate per [!UICONTROL on-device decisioning]](/help/dev/implement/client-side/atjs/on-device-decisioning/supported-features.md).

## Come funziona [!UICONTROL on-device decisioning]?

Quando distribuisci e inizializzi at.js con [!UICONTROL on-device decisioning] abilitato, un [artefatto regola](/help/dev/implement/client-side/atjs/on-device-decisioning/rule-artifact.md) che include [!UICONTROL on-device decisioning] per attività A/B e XT, tipi di pubblico e risorse, viene scaricato dalla rete CDN Akamai più vicina al visitatore e memorizzato nella cache locale nel browser del visitatore. Quando si effettua una richiesta da at.js per recuperare un’esperienza, la decisione relativa all’esperienza da restituire viene presa in memoria, in base ai metadati codificati nell’artefatto della regola memorizzata nella cache.

## Metodo di decisione

Con [!UICONTROL on-device decisioning], [!DNL Target] introduce una nuova impostazione denominata Metodo di decisione. L’impostazione del metodo Decisioning determina il modo in cui at.js distribuisce le esperienze. Il metodo Decisioning ha tre valori:

* Solo lato server
* Solo su dispositivo
* Ibrido

### Solo lato server

Solo lato server è il metodo decisionale predefinito impostato automaticamente quando at.js 2.5.0+ viene implementato e distribuito sulle proprietà web.

Se si utilizza solo lato server come configurazione predefinita, tutte le decisioni vengono prese sulla rete Edge [!DNL Target], il che comporta una chiamata di blocco al server. Questo approccio può introdurre una latenza incrementale, ma offre anche vantaggi significativi, come la possibilità di applicare le funzionalità di machine learning di [!DNL Target], che includono [attività Consigli](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations.html), [Automated Personalization](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html) (AP) e [Targeting automatico](https://experienceleague.adobe.com/docs/target/using/activities/auto-target/auto-target-to-optimize.html).

Inoltre, migliorare le tue esperienze personalizzate utilizzando il profilo utente di [!DNL Target], che viene mantenuto tra sessioni e canali diversi, può fornire risultati potenti per la tua azienda.

Infine, solo lato server consente di utilizzare Adobe Experience Cloud e di perfezionare i tipi di pubblico a cui rivolgersi tramite i segmenti di Audience Manager e Adobe Analytics.

Il diagramma seguente illustra l&#39;interazione tra il visitatore, il browser, at.js 2.5.0+ e la rete Edge [!DNL Adobe Target]. Questo diagramma di flusso acquisisce nuovi visitatori e visitatori di ritorno.

(Fare clic sull&#39;immagine per espanderla a larghezza intera.)

![Diagramma di flusso solo lato server](/help/dev/implement/client-side/atjs/on-device-decisioning/assets/server-side-only.png "Diagramma di flusso solo lato server"){zoomable="yes"}

L&#39;elenco seguente corrisponde ai numeri del diagramma:

| Passaggio | Descrizione |
| --- | --- |
| 1 | L&#39;ID visitatore di Experience Cloud viene recuperato dal [servizio Adobe Experience Cloud Identity](https://experienceleague.adobe.com/docs/id-service/using/home.html?). |
| 2 | La libreria at.js viene caricata in modo sincrono e nasconde il corpo del documento.<br />   La libreria at.js può anche essere caricata in modo asincrono con un eventuale snippet prenascosto implementato sulla pagina. |
| 3 | La libreria at.js nasconde il corpo per evitare sfarfallii. |
| 4 | Viene effettuata una richiesta di caricamento della pagina che include tutti i parametri configurati, ad esempio (ECID, ID cliente, parametri personalizzati, profilo utente e così via). |
| 5 | Gli script di profilo vengono eseguiti e quindi inseriti nell’archivio profili.<br />L&#39;archivio profili richiede un pubblico idoneo dalla libreria Pubblico (ad esempio, un pubblico condiviso da Adobe Analytics, Adobe Audience Manager e così via).<br />Gli attributi del cliente vengono inviati all’archivio profili in un processo batch. |
| 6 | L’archivio profili viene utilizzato per la qualifica del pubblico e il bucket per filtrare le attività. |
| 7 | Il contenuto risultante viene selezionato dopo che l&#39;esperienza è determinata dalle attività live di [!DNL Target]. |
| 8 | La libreria at.js nasconde gli elementi corrispondenti sulla pagina che sono associati all’esperienza di cui è necessario eseguire il rendering. |
| 9 | La libreria at.js visualizza il corpo in modo che il resto della pagina possa essere caricato per essere visualizzato dal visitatore. |
| 10 | La libreria at.js modifica il DOM per eseguire il rendering dell&#39;esperienza dall&#39;Edge Network [!DNL Target]. |
| 11 | L’esperienza viene riprodotta per il visitatore. |
| 12 | Viene caricata l’intera pagina web. |
| 13 | I dati Analytics vengono inviati ai server di raccolta dati. |
| 14 | I dati di destinazione vengono confrontati con i dati di Analytics tramite SDID ed elaborati nell’archivio dei rapporti di Analytics. I dati di Analytics possono quindi essere visualizzati sia in Analytics che in [!DNL Target] tramite i rapporti [!UICONTROL Analytics for Target] (A4T). |

### Solo su dispositivo

Solo su dispositivo è il metodo decisionale che deve essere impostato in at.js 2.5.0+ quando [!UICONTROL on-device decisioning] deve essere utilizzato solo in tutte le pagine web.

[!UICONTROL On-device decisioning] può fornire le tue esperienze e attività di personalizzazione a una velocità sorprendente perché le decisioni sono prese da un artefatto di regole memorizzate nella cache che contiene tutte le tue attività che si qualificano per [!UICONTROL on-device decisioning].

Per ulteriori informazioni sulle attività idonee per [!UICONTROL on-device decisioning], vedere [Funzioni supportate in [!UICONTROL on-device decisioning]](/help/dev/implement/client-side/atjs/on-device-decisioning/supported-features.md).

Questo metodo decisionale deve essere utilizzato solo se le prestazioni sono altamente critiche in tutte le pagine che richiedono decisioni da Target. Inoltre, ricorda che quando viene selezionato questo metodo decisionale, le attività [!DNL Target] che non sono idonee per [!UICONTROL on-device decisioning] non verranno consegnate o eseguite. La libreria at.js 2.5.0+ è configurata per cercare solo l’artefatto delle regole memorizzate nella cache per prendere decisioni.

Il diagramma seguente illustra l’interazione tra il visitatore, il browser, at.js 2.5.0+ e la rete CDN di Akamai. Il CDN di Akamai memorizza in cache l’artefatto delle regole per la prima visita del visitatore. Per la prima visita di pagina di un nuovo visitatore, l’artefatto delle regole JSON deve essere scaricato dalla rete CDN Akamai per essere memorizzato nella cache locale nel browser del visitatore. Una volta scaricato l’artefatto delle regole JSON, la decisione viene presa immediatamente senza una chiamata di rete di blocco. Il seguente diagramma di flusso acquisisce nuovi visitatori.

(Fare clic sull&#39;immagine per espanderla a larghezza intera.)

![Diagramma di flusso solo su dispositivo](/help/dev/implement/client-side/atjs/on-device-decisioning/assets/on-device-only.png "Diagramma di flusso solo su dispositivo"){zoomable="yes"}

L&#39;elenco seguente corrisponde ai numeri del diagramma:

>[!NOTE]
>
>I server di amministrazione [!DNL Adobe Target] qualificano tutte le attività idonee per [!UICONTROL on-device decisioning], generano l&#39;artefatto delle regole JSON e lo propagano alla rete CDN Akamai. Le attività vengono costantemente monitorate per rilevare aggiornamenti che generano un nuovo artefatto di regole JSON da propagare alla rete CDN di Akamai.

| Passaggio | Descrizione |
| --- | --- |
| 1 | L&#39;ID visitatore di Experience Cloud viene recuperato dal [servizio Adobe Experience Cloud Identity](https://experienceleague.adobe.com/docs/id-service/using/home.html). |
| 2 | La libreria at.js viene caricata in modo sincrono e nasconde il corpo del documento.<br />La libreria at.js può anche essere caricata in modo asincrono con un eventuale snippet prenascosto implementato sulla pagina. |
| 3 | La libreria at.js nasconde il corpo per evitare sfarfallii. |
| 4 | La libreria at.js richiede di recuperare l’artefatto della regola JSON dal CDN Akamai più vicino al visitatore. |
| 5 | Il CDN di Akamai risponde con l’artefatto della regola JSON. |
| 6 | L’artefatto della regola JSON viene memorizzato nella cache locale nel browser del visitatore. |
| 7 | La libreria at.js interpreta l’artefatto della regola JSON ed esegue la decisione di recuperare l’esperienza e nasconde gli elementi testati. |
| 8 | La libreria at.js visualizza il corpo in modo che il resto della pagina possa essere caricato per essere visualizzato dal visitatore. |
| 9 | La libreria at.js manipola il DOM per eseguire il rendering dell’esperienza dall’artefatto della regola JSON memorizzato nella cache. |
| 10 | L’esperienza viene riprodotta per il visitatore. |
| 11 | Viene caricata l’intera pagina web. |
| 12 | I dati di Analytics vengono inviati ai server di raccolta dati. I dati di destinazione vengono confrontati con i dati di Analytics tramite SDID ed elaborati nell’archivio dei rapporti di Analytics. I dati di Analytics possono quindi essere visualizzati sia in Analytics che in [!DNL Target] tramite i rapporti [!UICONTROL Analytics for Target] (A4T). |

Il diagramma seguente illustra l’interazione tra il visitatore, il browser, at.js 2.5.0+, e l’artefatto della regola JSON memorizzato nella cache per l’hit pagina o la visita di ritorno del visitatore. Poiché l’artefatto delle regole JSON è già memorizzato nella cache e disponibile sul browser, la decisione viene presa immediatamente senza una chiamata di rete di blocco. Questo diagramma di flusso acquisisce la navigazione successiva della pagina o i visitatori di ritorno.

(Fare clic sull&#39;immagine per espanderla a larghezza intera.)

![Diagramma di flusso solo sul dispositivo per la navigazione nelle pagine successive e le visite ripetute](/help/dev/implement/client-side/atjs/on-device-decisioning/assets/on-device-only-subsequent.png "Diagramma di flusso solo sul dispositivo per la navigazione nelle pagine successive e le visite ripetute"){zoomable="yes"}

L&#39;elenco seguente corrisponde ai numeri del diagramma:

>[!NOTE]
>
>I server di amministrazione [!DNL Adobe Target] qualificano tutte le attività idonee per [!UICONTROL on-device decisioning], generano l&#39;artefatto delle regole JSON e lo propagano alla rete CDN Akamai. Le attività vengono costantemente monitorate per rilevare aggiornamenti che generano un nuovo artefatto di regole JSON da propagare alla rete CDN di Akamai.

| Passaggio | Descrizione |
| --- | --- |
| 1 | L&#39;ID visitatore di Experience Cloud viene recuperato dal [servizio Adobe Experience Cloud Identity](https://experienceleague.adobe.com/docs/id-service/using/home.html). |
| 2 | La libreria at.js viene caricata in modo sincrono e nasconde il corpo del documento.<br />La libreria at.js può anche essere caricata in modo asincrono con un eventuale snippet prenascosto implementato sulla pagina. |
| 3 | La libreria at.js nasconde il corpo per evitare sfarfallii. |
| 4 | La libreria at.js interpreta l’artefatto della regola JSON ed esegue la decisione in memoria per recuperare l’esperienza. |
| 5 | Gli elementi testati sono nascosti. |
| 6 | La libreria at.js visualizza il corpo in modo che il resto della pagina possa essere caricato per essere visualizzato dal visitatore. |
| 7 | La libreria at.js manipola il DOM per eseguire il rendering dell’esperienza dall’artefatto della regola JSON memorizzato nella cache. |
| 8 | L’esperienza viene riprodotta per il visitatore. |
| 9 | Viene caricata l’intera pagina web. |
| 10 | I dati di Analytics vengono inviati ai server di raccolta dati. I dati di destinazione vengono confrontati con i dati di Analytics tramite SDID ed elaborati nell’archivio dei rapporti di Analytics. I dati di Analytics possono quindi essere visualizzati sia in Analytics che in [!DNL Target] tramite i rapporti [!UICONTROL Analytics for Target] (A4T). |

### Ibrido

Ibrido è il metodo decisionale che deve essere impostato in at.js 2.5.0+ quando è necessario eseguire sia [!UICONTROL on-device decisioning] che le attività che richiedono una chiamata alla rete Edge [!DNL Adobe Target].

Quando gestisci sia le attività [!UICONTROL on-device decisioning] che quelle lato server, può essere un po&#39; complicato e noioso pensare a come distribuire ed eseguire il provisioning di [!DNL Target] nelle pagine. Con il metodo decisionale ibrido, [!DNL Target] sa quando deve effettuare una chiamata al server alla rete Edge [!DNL Adobe Target] per le attività che richiedono l&#39;esecuzione lato server e anche quando eseguire solo le decisioni su dispositivo.

L&#39;artefatto delle regole JSON include metadati per informare at.js se una mbox ha un&#39;attività lato server in esecuzione o un&#39;attività [!UICONTROL on-device decisioning]. Questo metodo decisionale assicura che le attività che intendi consegnare rapidamente vengano eseguite tramite [!UICONTROL on-device decisioning] e che quelle che richiedono una personalizzazione basata su ML più potente vengano eseguite tramite la rete Edge [!DNL Adobe Target].

Il diagramma seguente illustra l&#39;interazione tra il visitatore, il browser, at.js 2.5.0+, la rete CDN Akamai e l&#39;Edge Network [!DNL Adobe Target] per un nuovo visitatore che visita la pagina per la prima volta. La rimozione da questo diagramma consiste nel fatto che l&#39;artefatto delle regole JSON viene scaricato in modo asincrono mentre le decisioni vengono prese tramite la rete Edge [!DNL Adobe Target].

Questo approccio assicura che la dimensione dell’artefatto, che può includere molte attività, non influisca negativamente sulla latenza della decisione. Il download dell’artefatto delle regole JSON in modo sincrono e la decisione successiva può avere anche effetti negativi sulla latenza e può essere incoerente. Pertanto, il metodo di decisione ibrido è un consiglio sulle best practice per effettuare sempre una chiamata lato server per la decisione relativa a un nuovo visitatore e poiché l’artefatto delle regole JSON viene memorizzato nella cache in parallelo. Per tutte le visite di pagina e le visite di ritorno successive, le decisioni vengono prese dalla cache e nella memoria tramite l’artefatto delle regole JSON.

(Fare clic sull&#39;immagine per espanderla a larghezza intera.)

![Diagramma di flusso ibrido per un nuovo visitatore](/help/dev/implement/client-side/atjs/on-device-decisioning/assets/hybrid-first-time-visitor.png "Diagramma di flusso ibrido per un nuovo visitatore"){zoomable="yes"}

L&#39;elenco seguente corrisponde ai numeri del diagramma:

>[!NOTE]
>
>I server di amministrazione [!DNL Adobe Target] qualificano tutte le attività idonee per [!UICONTROL on-device decisioning], generano l&#39;artefatto delle regole JSON e lo propagano alla rete CDN Akamai. Le attività vengono costantemente monitorate per rilevare aggiornamenti che generano un nuovo artefatto di regole JSON da propagare alla rete CDN di Akamai.

| Passaggio | Descrizione |
| --- | --- |
| 1 | L&#39;ID visitatore di Experience Cloud viene recuperato dal [servizio Adobe Experience Cloud Identity](https://experienceleague.adobe.com/docs/id-service/using/home.html). |
| 2 | La libreria at.js viene caricata in modo sincrono e nasconde il corpo del documento.<br />La libreria at.js può anche essere caricata in modo asincrono con un eventuale snippet prenascosto implementato sulla pagina. |
| 3 | La libreria at.js nasconde il corpo per evitare sfarfallii. |
| 4 | All&#39;Edge Network [!DNL Adobe Target] viene effettuata una richiesta di caricamento pagina, inclusi tutti i parametri configurati come (ECID, ID cliente, parametri personalizzati, profilo utente e così via). |
| 5 | In parallelo, at.js effettua una richiesta per recuperare l’artefatto della regola JSON dal CDN di Akamai più vicino al visitatore. |
| 6 | ([!DNL Adobe Target] Edge Network) Gli script di profilo vengono eseguiti e quindi inseriti nell&#39;archivio profili. L’archivio profili richiede un pubblico idoneo dalla libreria Pubblico (ad esempio, pubblico condiviso da Adobe Analytics, Adobe Audience Manager e così via). |
| 7 | Il CDN di Akamai risponde con l’artefatto della regola JSON. |
| 8 | L’archivio profili viene utilizzato per la qualifica del pubblico e il bucket per filtrare le attività. |
| 9 | Il contenuto risultante viene selezionato dopo che l&#39;esperienza è determinata dalle attività live di [!DNL Target]. |
| 10 | La libreria at.js nasconde gli elementi corrispondenti sulla pagina che sono associati all’esperienza di cui è necessario eseguire il rendering. |
| 11 | La libreria at.js visualizza il corpo in modo che il resto della pagina possa essere caricato per essere visualizzato dal visitatore. |
| 12 | La libreria at.js modifica il DOM per eseguire il rendering dell&#39;esperienza dall&#39;Edge Network [!DNL Target]. |
| 13 | L’esperienza viene riprodotta per il visitatore. |
| 14 | Viene caricata l’intera pagina web. |
| 15 | I dati di Analytics vengono inviati ai server di raccolta dati. I dati di destinazione vengono confrontati con i dati di Analytics tramite SDID ed elaborati nell’archivio dei rapporti di Analytics. I dati di Analytics possono quindi essere visualizzati sia in Analytics che in [!DNL Target] tramite i rapporti [!UICONTROL Analytics for Target] (A4T). |

Il diagramma seguente illustra l’interazione tra il visitatore, il browser, at.js 2.5.0+, e l’artefatto delle regole JSON memorizzate nella cache per una navigazione di pagina successiva o per una visita di ritorno. In questo diagramma, concentra l’attenzione solo sul caso d’uso in cui viene presa una decisione sul dispositivo per la successiva navigazione della pagina o visita di ritorno. Tieni presente che, a seconda delle attività live per determinate pagine, è possibile effettuare una chiamata lato server per eseguire decisioni lato server.

(Fare clic sull&#39;immagine per espanderla a larghezza intera.)

![Diagramma di flusso ibrido per la navigazione delle pagine successive e le visite ripetute](/help/dev/implement/client-side/atjs/on-device-decisioning/assets/hybrid-subsequent.png "Diagramma di flusso ibrido per la navigazione delle pagine successive e le visite ripetute"){zoomable="yes"}

L&#39;elenco seguente corrisponde ai numeri del diagramma:

>[!NOTE]
>
>I server di amministrazione [!DNL Adobe Target] qualificano tutte le attività idonee per [!UICONTROL on-device decisioning], generano l&#39;artefatto delle regole JSON e lo propagano alla rete CDN Akamai. Le attività vengono costantemente monitorate per rilevare aggiornamenti che generano un nuovo artefatto di regole JSON da propagare alla rete CDN di Akamai.

| Passaggio | Descrizione |
| --- | --- |
| 1 | L&#39;ID visitatore di Experience Cloud viene recuperato dal [servizio Adobe Experience Cloud Identity](https://experienceleague.adobe.com/docs/id-service/using/home.html). |
| 2 | La libreria at.js viene caricata in modo sincrono e nasconde il corpo del documento.<br />La libreria at.js può anche essere caricata in modo asincrono con un eventuale snippet prenascosto implementato sulla pagina. |
| 3 | La libreria at.js nasconde il corpo per evitare sfarfallii. |
| 4 | Viene effettuata una richiesta per recuperare un’esperienza. |
| 5 | La libreria at.js conferma che l’artefatto della regola JSON è già stato memorizzato nella cache ed esegue la decisione in memoria per recuperare l’esperienza. |
| 6 | Gli elementi testati sono nascosti. |
| 7 | La libreria at.js visualizza il corpo in modo che il resto della pagina possa essere caricato per essere visualizzato dal visitatore. |
| 8 | La libreria at.js manipola il DOM per eseguire il rendering dell’esperienza dall’artefatto della regola JSON memorizzato nella cache. |
| 9 | L’esperienza viene riprodotta per il visitatore. |
| 10 | Viene caricata l’intera pagina web. |
| 11 | I dati di Analytics vengono inviati ai server di raccolta dati. I dati di destinazione vengono confrontati con i dati di Analytics tramite SDID ed elaborati nell’archivio dei rapporti di Analytics. I dati di Analytics possono quindi essere visualizzati sia in Analytics che in [!DNL Target] tramite i rapporti [!UICONTROL Analytics for Target] (A4T). |

## Come si abilita [!UICONTROL on-device decisioning]?

[!UICONTROL On-device decisioning] è disponibile per tutti i [!DNL Target] clienti che utilizzano At.js 2.5.0+.

Per abilitare [!UICONTROL on-device decisioning]:

>[!NOTE]
>
>Per abilitare o disabilitare l&#39;attivazione/disattivazione di Decisioning sul dispositivo, è necessario disporre del ruolo utente [amministratore o approvatore](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/user-management.html).

1. Fare clic su **[!UICONTROL Administration]** > **[!UICONTROL Implementation]** > **[!UICONTROL Account details]**.
1. In **[!UICONTROL Account details]**, far scorrere **[!UICONTROL On-Device Decisioning]** in posizione &quot;on&quot;.

   Attivazione/disattivazione ![[!UICONTROL On-device decisioning]](assets/on-device-decisioning-toggle.png)

   Se abiliti [!UICONTROL on-device decisioning], viene visualizzata l&#39;opzione &quot;Includi tutte le attività qualificate [!UICONTROL on-device decisioning] esistenti nell&#39;artefatto&quot;.
1. (Facoltativo) Se desideri includere automaticamente nell&#39;artefatto tutte le attività di [!DNL Target] live idonee per [!UICONTROL on-device decisioning], imposta l&#39;opzione su &quot;attivato&quot;, scegli.

   Se si lascia questa opzione disattivata, sarà necessario ricreare e attivare tutte le attività [!UICONTROL on-device decisioning] affinché vengano incluse nell&#39;artefatto delle regole generato. In altre parole, qualsiasi attività in stato live prima di attivare l’interruttore On-Device Decisioning non è inclusa nell’artefatto delle regole.

Dopo aver attivato l&#39;interruttore Decisioning sul dispositivo, [!DNL Target] inizia a generare e propagare [artefatti regola](/help/dev/implement/client-side/atjs/on-device-decisioning/rule-artifact.md) per il client.

>[!WARNING]
>
>Assicurarsi di attivare l&#39;interruttore prima di inizializzare il SDK [!DNL Adobe Target] per utilizzare [!UICONTROL on-device decisioning]. Gli artefatti della regola devono prima essere generati e propagati ai CDN Akamai affinché [!UICONTROL on-device decisioning] funzioni. La propagazione può richiedere da cinque a dieci minuti affinché il primo artefatto della regola venga generato e propagato alla rete CDN di Akamai.

## Come si configura at.js 2.5.0+ per utilizzare [!UICONTROL on-device decisioning]?

1. Fare clic su **[!UICONTROL Administration]** > **[!UICONTROL Implementation]** > **[!UICONTROL Account details]**.
1. In **[!UICONTROL Implementation Methods]** > **[!UICONTROL Main Implementation Method]**, fai clic su **[!UICONTROL Edit]** accanto alla versione di at.js (deve essere at.js 2.5.0 o successiva).

   ![Modifica impostazioni metodo di implementazione principale](assets/main-implementation-method.png)

   >[!WARNING]
   >
   >Prima di modificare queste impostazioni predefinite, consulta l’Assistenza clienti per evitare di influenzare l’implementazione corrente.

1. Seleziona il metodo di decisione desiderato:

   * Solo lato server
   * Solo su dispositivo
   * Ibrido

   ![Modifica pannello impostazioni at.js](assets/global-settings.png)

### Impostazioni globali

È possibile configurare un metodo di decisione predefinito per tutte le [!DNL Target] decisioni. I vari metodi decisionali sono Solo lato server, Solo su dispositivo e Ibrido. Il metodo decisionale selezionato nell&#39;interfaccia utente [!DNL Target] è configurato in `window.targetGlobalSettings` nel campo `decisioningMethod`. Ulteriori informazioni su `decisioningMethod` in [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#decisioningmethod).

```javascript {line-numbers="true"}
<head> 
    <script type="text/javascript">

        window.targetGlobalSettings = { 
            clientCode: "yourClientCodeHere", 
            imsOrgId: "imsOrgId@AdobeOrg", 
            decisioningMethod: "on-device"

        }; 
    </script>

    <script type="text/javascript" src="at.js"></script> 
</head>
```

### Impostazione personalizzata

Se si imposta `decisioningMethod` in `window.targetGlobalSettings`, ma si desidera ignorare `decisioningMethod` per ogni decisione [!DNL Adobe Target] in base al caso d&#39;uso, è possibile eseguire questa procedura specificando `decisioningMethod` nella chiamata [getOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md) di at.js2.5.0+.

```javascript {line-numbers="true"}
adobe.target.getOffers({ 

  decisioningMethod:"on-device", 
  request: { 
    execute: { 
      mboxes: [ 
        { 
          index: 0, 
          name: "homepage" 
        } 
      ] 
    } 
 } 
});
```

>[!NOTE]
>
>Per utilizzare &quot;su dispositivo&quot; o &quot;ibrido&quot; come metodo decisionale nella chiamata getOffers(), accertati che l&#39;impostazione globale abbia `decisioningMethod` come &quot;su dispositivo&quot; o &quot;ibrido&quot;. La libreria at.js 2.5.0+ deve sapere se scaricare e memorizzare in cache l’artefatto delle regole JSON immediatamente dopo il caricamento sulla pagina. Se il metodo decisionale per l’impostazione globale è impostato su &quot;lato server&quot; e il metodo decisionale &quot;su dispositivo&quot; o &quot;ibrido&quot; viene passato alla chiamata getOffers(), at.js 2.5.0+ non avrà l’artefatto della regola JSON memorizzato nella cache per eseguire le decisioni su dispositivo.

### TTL cache artefatto

Target rappresenta le attività che si qualificano per [!UICONTROL on-device decisioning] come artefatto costituito da metadati, regole e condizioni. Questo artefatto viene memorizzato nella cache sulla rete CDN Akamai. Durante la prima visita dell&#39;utente, il browser dell&#39;utente scarica e memorizza in cache l&#39;artefatto che rappresenta le attività di [!UICONTROL on-device decisioning].

Nelle visite successive al sito, il browser controlla automaticamente se deve scaricare una versione più recente dell’artefatto. Questo controllo aggiunge latenza. Il valore TTL della cache degli artefatti definisce il numero di minuti necessari affinché il browser non controlli la presenza di un artefatto aggiornato dall’ultimo download riuscito. Più è lungo l&#39;intervallo di tempo, migliori saranno le prestazioni. Più breve è l’intervallo di tempo, migliore sarà l’aggiornamento dei dati, ma a costo di una latenza aggiuntiva.

## Come posso sapere se un&#39;attività è idonea [!UICONTROL on-device decisioning]?

Dopo aver creato un&#39;attività idonea per [!UICONTROL on-device decisioning], un&#39;etichetta che indica Idonea a Decisioning sul dispositivo è visibile nella pagina Panoramica dell&#39;attività.

![Etichetta idonea per Decisioning sul dispositivo nella pagina Panoramica dell&#39;attività.](assets/on-device-decisioning-eligible-label.png)

Questa etichetta non significa che l&#39;attività verrà sempre recapitata tramite [!UICONTROL on-device decisioning]. Questa attività verrà eseguita sul dispositivo solo quando at.js 2.5.0+ è configurato per l&#39;utilizzo di [!UICONTROL on-device decisioning]. Se at.js 2.5.0+ non è configurato per l’utilizzo su dispositivo, questa attività verrà comunque consegnata tramite una chiamata al server effettuata da at.js.

È possibile filtrare tutte le attività idonee per [!UICONTROL on-device decisioning] nella pagina Attività tramite il filtro Idoneo Decisioning sul dispositivo.

![Filtro idoneo per Decisioning sul dispositivo nella pagina Attività.](assets/on-device-decisioning-filter.png)

>[!NOTE]
>
>Dopo aver creato e attivato un&#39;attività idonea [!UICONTROL on-device decisioning], possono essere necessari da cinque a dieci minuti prima che venga inclusa nell&#39;artefatto delle regole generato e propagato al punto di presenza CDN di Akamai.

## Riepilogo dei passaggi per garantire che le mie attività [!UICONTROL on-device decisioning] vengano consegnate tramite At.js 2.5.0+?

1. Accedi all&#39;interfaccia utente di [!DNL Adobe Target] e passa a **[!UICONTROL Administration]** > **[!UICONTROL Implementation]** > **[!UICONTROL Account Details]** per abilitare l&#39;opzione **[!UICONTROL On-Device Decisioning]**.
1. Attiva/disattiva **[!UICONTROL "Include all existing [!UICONTROL on-device decisioning] qualified activities in the artifact"]**.

   La prima generazione di artefatti con regole JSON può richiedere fino a 10 minuti.

1. Creare e attivare un tipo di attività [ supportato da [!UICONTROL on-device decisioning]](/help/dev/implement/client-side/atjs/on-device-decisioning/supported-features.md) e verificare che sia idoneo [!UICONTROL on-device decisioning].
1. Imposta **[!UICONTROL Decisioning Method]** su **[!UICONTROL "Hybrid"]** o **[!UICONTROL "On-device only"]** tramite l&#39;interfaccia utente delle impostazioni di at.js.
1. Scarica e distribuisci at.js 2.5.0+ nelle tue pagine.
