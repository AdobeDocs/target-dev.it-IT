---
title: Quali funzioni sono supportate nel decisioning sul dispositivo?
description: Scopri come distribuire i contenuti personalizzati più rilevanti e coinvolgenti tramite l’apprendimento automatico utilizzando una chiamata al server live.
feature: APIs/SDKs
exl-id: 15d9870f-6c58-4da0-bfe5-ef23daf7d273
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '474'
ht-degree: 13%

---

# Panoramica delle funzioni supportate

Gli SDK lato server di [!DNL Adobe Target] offrono agli sviluppatori la flessibilità di scegliere tra prestazioni e aggiornamento dei dati per le decisioni. In altre parole, se la distribuzione dei contenuti personalizzati più rilevanti e coinvolgenti tramite l’apprendimento automatico è la cosa più importante per te, è necessario effettuare una chiamata al server live. Tuttavia, quando le prestazioni sono più importanti, è necessario prendere una decisione sul dispositivo. Affinché [!UICONTROL on-device decisioning] funzioni, consulta il seguente elenco di funzioni supportate:

* Tipi di attività
* Targeting del pubblico
* Metodo di allocazione

## Tipi di attività

La tabella seguente indica quali [tipi di attività](https://experienceleague.adobe.com/docs/target/using/activities/target-activities-guide.html?lang=it) creati utilizzando il [Compositore esperienza basato su moduli](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?lang=it&) sono supportati o non supportati per [!UICONTROL on-device decisioning].

| Tipo di attività | Supportato |
| --- | --- |
| [Test A/B](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html?lang=it) | Sì |
| [Allocazione automatica](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/automated-traffic-allocation.html?lang=it) | No |
| [Targeting automatico](https://experienceleague.adobe.com/docs/target/using/activities/auto-target/auto-target-to-optimize.html?lang=it) | No |
| [Analytics for Target](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?lang=it) (A4T) | Sì |
| [Test multivariato](https://experienceleague.adobe.com/docs/target/using/activities/multivariate-test/multivariate-testing.html?lang=it) (MVT) | No |
| [Targeting dell’esperienza](https://experienceleague.adobe.com/docs/target/using/activities/experience-targeting/experience-target.html?lang=it) (XT) | Sì |
| [Automated Personalization](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html?lang=it) (AP) | No |
| [Consigli](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations.html?lang=it) | No |


## Targeting del pubblico

La tabella seguente indica quali regole di pubblico sono supportate o meno per [!UICONTROL on-device decisioning].

| Regola pubblico | Decisioning sul dispositivo |
| --- | --- |
| [Geo](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html?lang=it) | Sì |
| [Rete](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/network.html?lang=it) | No |
| [Mobile](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html?lang=it) | No |
| [Parametri personalizzati](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/custom-parameters.html?lang=it) | Sì |
| [Sistema operativo](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/operating-system.html?lang=it) | Sì |
| [Pagine del sito](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/site-pages.html?lang=it) | Sì |
| [Browser](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/browser.html?lang=it) | Sì |
| [Profilo visitatore](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html?lang=it) | No |
| [Origini del traffico](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/traffic-sources.html?lang=it) | No |
| [Arco temporale](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/time-frame.html?lang=it) | Sì |
| [Tipi di pubblico di Experience Cloud](https://experienceleague.adobe.com/docs/target/using/integrate/mmp.html?lang=it) (tipi di pubblico da Adobe Audience Manager, Adobe Analytics e Adobe Experience Manager) | No |

### Geotargeting per [!UICONTROL on-device decisioning]

Per mantenere una latenza prossima allo zero per le attività [!UICONTROL on-device decisioning] con pubblico basato su geotargeting, l&#39;Adobe consiglia di fornire personalmente i valori geografici nella chiamata a `getOffers`. A tale scopo, impostare l&#39;oggetto `Geo` in `Context` della richiesta. Ciò significa che il server dovrà essere in grado di determinare la posizione di ciascun utente finale. Ad esempio, il server potrebbe eseguire una ricerca IP-to-Geo utilizzando un servizio configurato. Alcuni provider di hosting, come Google Cloud, forniscono questa funzionalità tramite intestazioni personalizzate in ogni `HttpServletRequest`.

>[!BEGINTABS]

>[!TAB Node.js]

```csharp {line-numbers="true"}
const CONFIG = {
    client: "acmeclient",
    organizationId: "1234567890@AdobeOrg",
    decisioningMethod: "on-device"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
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

>[!TAB Java]

```javascript {line-numbers="true"}
public class TargetRequestUtils {

    public static Context getContext(HttpServletRequest request) {
        Context context = new Context()
            .geo(ipToGeoLookup(request.getRemoteAddr()))
            .channel(ChannelType.WEB)
            .timeOffsetInMinutes(330.0)
            .address(getAddress(request));
        return context;
    }

    public static Geo ipToGeoLookup(String ip) {
        GeoResult geoResult = geoLookupService.lookup(ip);
        return new Geo()
            .city(geoResult.getCity())
            .stateCode(geoResult.getStateCode())
            .countryCode(geoResult.getCountryCode());
    }

}
```

>[!ENDTABS]

Tuttavia, se non hai la possibilità di eseguire ricerche IP-to-Geo sul server, ma desideri comunque eseguire [!UICONTROL on-device decisioning] per `getOffers` richieste che contengono tipi di pubblico basati su geotargeting, anche questo è supportato. Il lato negativo di questo approccio è che utilizzerà una ricerca remota IP-Geo, che aggiungerà latenza a ogni chiamata `getOffers`. Questa latenza deve essere inferiore a una chiamata `getOffers` remota, poiché raggiunge una rete CDN vicina al server. È necessario fornire solo il campo `ipAddress` nell&#39;oggetto `Geo` nella `Context` della richiesta, affinché l&#39;SDK possa recuperare la geolocalizzazione dell&#39;indirizzo IP dell&#39;utente. Se viene fornito un altro campo oltre a `ipAddress`, l&#39;SDK [!DNL Target] non recupererà i metadati di geolocalizzazione per la risoluzione.


>[!BEGINTABS]

>[!TAB Node.js]

```csharp {line-numbers="true"}
const CONFIG = {
    client: "acmeclient",
    organizationId: "1234567890@AdobeOrg",
    decisioningMethod: "on-device"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
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

>[!TAB Java]

```javascript {line-numbers="true"}
public class TargetRequestUtils {

    public static Context getContext(HttpServletRequest request) {
        Context context = new Context()
            .geo(new Geo().ipAddress(request.getRemoteAddr()))
            .channel(ChannelType.WEB)
            .timeOffsetInMinutes(330.0)
            .address(getAddress(request));
        return context;
    }

}
```

>[!ENDTABS]

## Metodo di allocazione

La tabella seguente indica i metodi di allocazione supportati o non supportati per [!UICONTROL on-device decisioning].

| Metodo di allocazione | Supportato |
| --- | --- |
| Manuale | Sì |
| [Allocazione automatica all&#39;esperienza migliore](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/automated-traffic-allocation.html?lang=it) | No |
| [Targeting automatico per esperienze personalizzate](https://experienceleague.adobe.com/docs/target/using/activities/auto-target-to-optimize.html?lang=it) | No |
