---
keywords: implementazione, libreria javascript, js, atjs, decisioning sul dispositivo, decisioning sul dispositivo, funzionalità supportate, $ 8
description: Scopri le funzioni supportate per [!UICONTROL decisioning sul dispositivo].
title: Quali funzioni sono supportate in Decisioning sul dispositivo
feature: at.js
exl-id: bdd65658-6c4a-41ae-a222-59c00a11bdac
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '651'
ht-degree: 9%

---

# Funzioni supportate per [!UICONTROL decisioning sul dispositivo]

Il [!DNL Adobe Target] L’SDK JS offre ai clienti la flessibilità di scegliere tra prestazioni e aggiornamento dei dati per le decisioni. In altre parole, se la distribuzione dei contenuti personalizzati più rilevanti e coinvolgenti tramite l’apprendimento automatico è la cosa più importante per te, è necessario effettuare una chiamata al server live. Tuttavia, quando le prestazioni sono più importanti, è necessario prendere una decisione su dispositivo e in memoria. Per [!UICONTROL decisioning sul dispositivo] per ulteriori informazioni, consulta le sezioni seguenti che elencano le funzioni supportate.

## Tipi di attività supportati

La tabella seguente indica quali [tipi di attività](https://experienceleague.adobe.com/docs/target/using/activities/target-activities-guide.html) creato da [Compositore esperienza basato su moduli](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html) o [Compositore esperienza visivo](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html) (VEC) sono supportati o non supportati per [!UICONTROL decisioning sul dispositivo].

| Tipo di attività | Supportate? |
| --- | --- |
| [Test A/B](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html) | Sì |
| [Allocazione automatica](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/automated-traffic-allocation.html) | No |
| [Targeting automatico](https://experienceleague.adobe.com/docs/target/using/activities/auto-target/auto-target-to-optimize.html) ![Premium](../../../assets/premium.png) | No |
| [Test multivariato](https://experienceleague.adobe.com/docs/target/using/activities/multivariate-test/multivariate-testing.html) (MVT) | No |
| [Targeting dell’esperienza](https://experienceleague.adobe.com/docs/target/using/activities/experience-targeting/experience-target.html) (XT) | Sì |
| [Automated Personalization](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html) ![Premium](../../../assets/premium.png) | No |
| [Consigli](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations.html) ![Premium](../../../assets/premium.png) | No |
| [Attività con Analytics for Target](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?) (A4T) | Sì |

## Targeting del pubblico

La tabella seguente indica per quali regole di pubblico sono supportate o meno [!UICONTROL decisioning sul dispositivo].

| Regola pubblico | Supportate? |
| --- | --- |
| [Geo](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html) | Sì |
| [Rete](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/network.html) | No |
| [Mobile](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html) | No |
| [Parametri personalizzati](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/custom-parameters.html) | Sì |
| [Sistema operativo](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/operating-system.html) | Sì |
| [Pagine del sito](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/site-pages.html) | Sì |
| [Browser](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/browser.html) | Sì |
| [Profilo visitatore](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html) | No |
| [Origini del traffico](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/traffic-sources.html) | No |
| [Arco temporale](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/time-frame.html) | Sì |
| Pubblico Adobe Experience Cloud<P>([!DNL Audiences from Adobe Analytics], [!DNL Adobe Audience Manager], e [!DNL Adobe Experience Manager]) | No |

### Geotargeting per [!UICONTROL decisioning sul dispositivo]

Per mantenere una latenza minima per [!UICONTROL decisioning sul dispositivo] con tipi di pubblico basati sulla geotargeting, l’Adobe consiglia di fornire tu stesso i valori geografici nella chiamata a [getOffers](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md). Imposta l’oggetto Geo nel contesto della richiesta. Questo significa dal browser, un modo per determinare la posizione di ogni visitatore. Ad esempio, puoi eseguire una ricerca IP-Geo utilizzando un servizio configurato. Alcuni provider di hosting, come Google Cloud, forniscono questa funzionalità tramite intestazioni personalizzate in ogni `HttpServletRequest`.

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

Tuttavia, se non è possibile eseguire ricerche IP-to-Geo sul server, ma si desidera comunque eseguire [!UICONTROL decisioning sul dispositivo] per [getOffers](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md) sono supportate anche le richieste che contengono tipi di pubblico basati su geotargeting. Il lato negativo di questo approccio è che utilizza una ricerca IP-to-Geo remota, che aggiunge latenza a ogni `getOffers` chiamare. Questa latenza deve essere inferiore a `getOffers` effettua una chiamata con decisioning lato server, perché hit una rete CDN situata vicino al server. Specifica solo il campo &quot;ipAddress&quot; nell’oggetto Geo nel contesto della richiesta per l’SDK di recuperare la geolocalizzazione dell’indirizzo IP del visitatore. Se viene fornito un altro campo oltre a &quot;ipAddress&quot;, il [!DNL Target] L’SDK non recupererà i metadati di geolocalizzazione per la risoluzione.

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

Nella tabella seguente vengono indicati i metodi di allocazione supportati o non supportati per [!UICONTROL decisioning sul dispositivo].

| Metodo di allocazione | Supportate? |
| --- | --- |
| Manuale | Sì |
| [Allocazione automatica all’esperienza migliore](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/automated-traffic-allocation.html) | No |
| [Targeting automatico per esperienze personalizzate](https://experienceleague.adobe.com/docs/target/using/activities/auto-target/auto-target-to-optimize.html) | No |
