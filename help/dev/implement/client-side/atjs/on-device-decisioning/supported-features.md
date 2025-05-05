---
keywords: implementazione, libreria javascript, js, atjs, decisioning sul dispositivo, decisioning sul dispositivo, funzionalità supportate, $ 8
description: Scopri le funzionalità supportate per [!UICONTROL on-device decisioning].
title: Quali funzioni sono supportate in Decisioning sul dispositivo
feature: at.js
exl-id: bdd65658-6c4a-41ae-a222-59c00a11bdac
source-git-commit: 79ffa3f58d780f587fe1202b82d3860395504dfe
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 12%

---

# Funzioni supportate per [!UICONTROL on-device decisioning]

L&#39;SDK JS di [!DNL Adobe Target] offre ai clienti la flessibilità di scegliere tra prestazioni e aggiornamento dei dati per le decisioni. In altre parole, se la distribuzione dei contenuti personalizzati più rilevanti e coinvolgenti tramite l’apprendimento automatico è la cosa più importante per te, è necessario effettuare una chiamata al server live. Tuttavia, quando le prestazioni sono più importanti, è necessario prendere una decisione su dispositivo e in memoria. Affinché [!UICONTROL on-device decisioning] funzioni, fare riferimento alle sezioni seguenti in cui sono elencate le funzionalità supportate.

## Tipi di attività supportati

La tabella seguente indica quali [tipi di attività](https://experienceleague.adobe.com/docs/target/using/activities/target-activities-guide.html?lang=it) creati dal [Compositore esperienza basato su moduli](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?lang=it) o [Compositore esperienza visivo](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html?lang=it) sono supportati o non supportati per [!UICONTROL on-device decisioning].

| Tipo di attività | Supportate? |
| --- | --- |
| [Test A/B](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html?lang=it) | Sì |
| [Allocazione automatica](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/automated-traffic-allocation.html?lang=it) | No |
| [Targeting automatico](https://experienceleague.adobe.com/docs/target/using/activities/auto-target/auto-target-to-optimize.html?lang=it) ![Premium](../../../assets/premium.png) | No |
| [Test multivariato](https://experienceleague.adobe.com/docs/target/using/activities/multivariate-test/multivariate-testing.html?lang=it) (MVT) | No |
| [Targeting dell’esperienza](https://experienceleague.adobe.com/docs/target/using/activities/experience-targeting/experience-target.html?lang=it) (XT) | Sì |
| [Automated Personalization](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html?lang=it) ![Premium](../../../assets/premium.png) | No |
| [Recommendations](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations.html?lang=it) ![Premium](../../../assets/premium.png) | No |
| [Attività con Analytics for Target](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?lang=it&) (A4T) | Sì |

## Targeting del pubblico

La tabella seguente indica quali regole di pubblico sono supportate o meno per [!UICONTROL on-device decisioning].

| Regola pubblico | Supportate? |
| --- | --- |
| [Geo](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html?lang=it) | Sì<P>Quando si utilizzano le decisioni sul dispositivo, sono supportati i seguenti attributi geografici:<ul><li>Paese/Area geografica</li><li>Città</li><li>Latitudine</li><li>Longitudine</li></ul> |
| [Rete](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/network.html?lang=it) | No |
| [Mobile](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html?lang=it) | No |
| [Parametri personalizzati](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/custom-parameters.html?lang=it) | Sì |
| [Sistema operativo](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/operating-system.html?lang=it) | Sì |
| [Pagine del sito](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/site-pages.html?lang=it) | Sì |
| [Browser](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/browser.html?lang=it) | Sì |
| [Profilo visitatore](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html?lang=it) | No |
| [Origini del traffico](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/traffic-sources.html?lang=it) | No |
| [Arco temporale](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/time-frame.html?lang=it) | Sì |
| Pubblico Adobe Experience Cloud<P>([!DNL Audiences from Adobe Analytics], [!DNL Adobe Audience Manager] e [!DNL Adobe Experience Manager]) | No |

### Geotargeting per [!UICONTROL on-device decisioning]

Per mantenere una latenza minima per le attività [!UICONTROL on-device decisioning] con tipi di pubblico basati su geotargeting, l&#39;Adobe consiglia di fornire autonomamente i valori geografici nella chiamata a [getOffers](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md). Imposta l’oggetto Geo nel contesto della richiesta. Questo significa dal browser, un modo per determinare la posizione di ogni visitatore. Ad esempio, puoi eseguire una ricerca IP-Geo utilizzando un servizio configurato. Alcuni provider di hosting, come Google Cloud, forniscono questa funzionalità tramite intestazioni personalizzate in ogni `HttpServletRequest`.

```javascript {line-numbers="true"}
window.adobe.target.getOffers({ 
    decisioningMethod: "on-device", 
    request: { 
        context: { 
            geo: { 
                city: "SAN FRANCISCO", 
                countryCode: "US", 
                stateCode: "CA", 
                latitude: 37.75, 
                longitude: -122.4 
            } 
        }, 
        execute: { 
            pageLoad: {} 
        } 
    } 
})
```

Tuttavia, è supportato anche se non è possibile eseguire ricerche IP-to-Geo nel server, ma si desidera comunque eseguire [!UICONTROL on-device decisioning] per [richieste getOffers](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md) che contengono tipi di pubblico basati su geotargeting. Il lato negativo di questo approccio è che utilizza una ricerca remota IP-Geo, che aggiunge latenza a ogni chiamata `getOffers`. Questa latenza deve essere inferiore a una chiamata `getOffers` con decisioni lato server, perché raggiunge una rete CDN vicina al server. Specifica solo il campo &quot;ipAddress&quot; nell’oggetto Geo nel contesto della richiesta per l’SDK di recuperare la geolocalizzazione dell’indirizzo IP del visitatore. Se viene fornito un altro campo oltre a &quot;ipAddress&quot;, l&#39;SDK [!DNL Target] non recupererà i metadati di geolocalizzazione per la risoluzione.

```javascript {line-numbers="true"}
window.adobe.target.getOffers({ 
    decisioningMethod: "on-device", 
    request: { 
        context: { 
            geo: { 
                ipAddress: "127.0.0.1" 
            } 
        }, 
        execute: { 
            pageLoad: {} 
        } 
    } 
})
```

### Metodo di allocazione

La tabella seguente indica i metodi di allocazione supportati o non supportati per [!UICONTROL on-device decisioning].

| Metodo di allocazione | Supportate? |
| --- | --- |
| Manuale | Sì |
| [Allocazione automatica all&#39;esperienza migliore](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/automated-traffic-allocation.html?lang=it) | No |
| [Targeting automatico per esperienze personalizzate](https://experienceleague.adobe.com/docs/target/using/activities/auto-target/auto-target-to-optimize.html?lang=it) | No |
