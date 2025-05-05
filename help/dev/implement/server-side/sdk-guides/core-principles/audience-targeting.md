---
title: Targeting del pubblico
description: I tipi di pubblico possono essere utilizzati per eseguire il targeting delle attività di sperimentazione e personalizzazione. [!DNL Adobe Target] supporta una miriade di potenti funzionalità di targeting del pubblico.
exl-id: df1bd856-e848-452c-90a0-abf29e7a2313
feature: Implement Server-side
source-git-commit: 09a50aa67ccd5c687244a85caad24df56c0d78f5
workflow-type: tm+mt
source-wordcount: '702'
ht-degree: 25%

---

# Targeting del pubblico

## Panoramica

I tipi di pubblico possono essere utilizzati per eseguire il targeting delle attività di sperimentazione e personalizzazione. [!DNL Adobe Target] supporta una miriade di potenti funzionalità di targeting del pubblico preconfigurate. I seguenti attributi sono disponibili per il [targeting di pubblico](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/create-audience.html?lang=it):

### Libreria [!DNL Target]

Per ulteriori informazioni, vedere [[!DNL Target] Libreria](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/target-library.html?lang=it).
&#x200B;
* Referrer da Bing
* Browser Chrome
* Browser Firefox
* Referrer da Google
* Internet Explorer
* Sistema operativo Linux
* Sistema operativo Mac OS
* Visitatori nuovi
* Visitatori di ritorno
* Browser Safari
* Dispositivo tablet
* Sistema operativo Windows
* Referrer da Yahoo

### Geo

Per ulteriori informazioni, vedere [Geo](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html?lang=it).
&#x200B;&#x200B;
* Paese/Area geografica
* Stato
* Città
* Codice postale
* Latitudine
* Longitudine
* DMA
* Gestore di telefonia mobile

### Rete

Per ulteriori informazioni, vedere [Rete](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/network.html?lang=it).

* ISP
* Nome di dominio
* Velocità di connessione

### Dispositivi mobili

Per ulteriori informazioni, consulta [Mobile](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html?lang=it).

* Nome marketing del dispositivo
* Modello dispositivo
* Produttore dispositivo
* È un dispositivo mobile
* È un telefono cellulare
* È un tablet
* Sistema operativo
* Altezza schermo (px)
* Larghezza schermo (px)

### Personalizzato

Per ulteriori informazioni, vedere [Parametri personalizzati](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/custom-parameters.html?lang=it).

* qualsiasi coppia chiave/valore

### Sistema operativo

Per ulteriori informazioni, vedere [Sistema operativo](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/operating-system.html?lang=it).

* Linux
* Macintosh
* Windows

### Pagine del sito

Per ulteriori informazioni, vedere [Pagine del sito](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/site-pages.html?lang=it).

* Pagina corrente
* Pagina precedente
* Pagina di destinazione
* Intestazione HTTP

### Browser

Per ulteriori informazioni, vedere [Browser](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/browser.html?lang=it).

* Tipo
* Lingua
* Versione

### Profilo visitatore

Per ulteriori informazioni, consulta [Profilo visitatore](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html?lang=it).

* qualsiasi coppia chiave/valore, che viene mantenuta

### Origini del traffico

Per ulteriori informazioni, vedere [Origini traffico](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/traffic-sources.html?lang=it).

* Da Baidu
* Da Bing
* Da Google
* Da Yahoo
* Pagina di destinazione di riferimento: URL
* Pagina di destinazione di riferimento: dominio
* Pagina di destinazione di riferimento: query

### Arco temporale

Per ulteriori informazioni, consulta [Intervallo di tempo](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/time-frame.html?lang=it).

* Data inizio / Data fine

## Client Hints

[!DNL Adobe Target] richiede Client Hints per la corretta segmentazione degli attributi di browser, sistema operativo e pubblico mobile, nonché di alcune istanze degli script di profilo. Per ulteriori informazioni in background, vedere [User Agent e Client Hints](../../../client-side/atjs/user-agent-and-client-hints.md).

### Come trasmettere gli Client Hints a [!DNL Adobe Target]

A partire da Node.js SDK v2.4.0 e Java SDK v2.3.0, gli Client Hints possono essere inviati a [!DNL Target] tramite chiamate `getOffers()`. Gli Client Hints devono essere inclusi nell&#39;oggetto `request.context` insieme all&#39;agente utente.

>[!BEGINTABS]

>[!TAB SDK di Node.js]

```js {line-numbers="true"}
targetClient.getOffers({ 
    request: { 
        context: { 
            channel: "mobile" 
            userAgent: "Mozilla/5.0 (Linux; Android 12; Pixel 4a) AppleWebKit/537.36 (KHTML, like Gecko) Mobile Safari/537.36", 
            clientHints: { 
                mobile: "true", 
                platform: "Linux", 
                platformVersion: "12.1", 
                model: "Pixel 4a", 
                browserUAWithMajorVersion: "\"Not A;Brand\";v=\"98\", \"Chromium\";v=\"98\", \"Google Chrome\";v=\"98\"", 
                browserUAWithFullVersion: "\" Not A;Brand\";v=\"98.0.0.0\", \"Chromium\";v=\"98.0.4844.83\", \"Google Chrome\";v=\"98.0.4758.101\"", 
                bitness: "64", 
                architecture: "x86" 
            } 
        }, 
        execute: { 
            mboxes: [{ 
                name: "home", 
                index: 1 
            }] 
        } 
    } 
});
```

>[!TAB SDK Java]

```javascript {line-numbers="true"}
import com.adobe.target.delivery.v1.model.ClientHints; 
import com.adobe.target.delivery.v1.model.Context; 
import com.adobe.target.delivery.v1.model.ExecuteRequest; 
import com.adobe.target.edge.client.model.TargetDeliveryRequest; 

 
ClientHints clientHints = new ClientHints(); 
clientHints.setMobile(true); 
clientHints.setPlatform("macOS"); 
clientHints.setArchitecture("x86"); 
clientHints.setPlatformVersion("11.3.1"); 
clientHints.setBrowserUAWithMajorVersion( 
  "\" Not A;Brand\";v=\"99\", \"Chromium\";v=\"99\", \"Google Chrome\";v=\"99\""); 
String userAgent = 
  "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Safari/537.36"; 

 
TargetDeliveryRequest request = TargetDeliveryRequest.builder() 
        .execute(new ExecuteRequest().pageLoad(pageLoad)) 
        .context(new Context().clientHints(clientHints).userAgent(userAgent)) 
        .build(); 
```

>[!ENDTABS]

## Decisioning sul dispositivo

La tabella seguente indica quali regole per il pubblico sono supportate o meno per le decisioni su dispositivo.

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

### Geotargeting per le decisioni su dispositivo

Per mantenere una latenza prossima allo zero per le attività di decisioning sul dispositivo con tipi di pubblico basati su geotargeting, l’Adobe consiglia di fornire autonomamente i valori geografici nella chiamata a `getOffers`. A tale scopo, impostare l&#39;oggetto `Geo` in `Context` della richiesta. Ciò significa che il server dovrà essere in grado di determinare la posizione di ciascun utente finale. Ad esempio, il server potrebbe eseguire una ricerca IP-to-Geo utilizzando un servizio configurato. Alcuni provider di hosting, come Google Cloud, forniscono questa funzionalità tramite intestazioni personalizzate in ogni `HttpServletRequest`.

>[!BEGINTABS]

>[!TAB SDK di Node.js]

```js {line-numbers="true"}
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

>[!TAB SDK Java]

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

Tuttavia, è supportato anche se non si dispone della capacità di eseguire ricerche IP-to-Geo sul server, ma si desidera comunque eseguire decisioni sul dispositivo per `getOffers` richieste che contengono tipi di pubblico basati su geotargeting. Il lato negativo di questo approccio è che utilizzerà una ricerca remota IP-Geo, che aggiungerà latenza a ogni chiamata `getOffers`. Questa latenza deve essere inferiore a una chiamata `getOffers` remota, poiché raggiunge una rete CDN vicina al server. È necessario **only** fornire il campo `ipAddress` nell&#39;oggetto `Geo` in `Context` della richiesta, affinché l&#39;SDK possa recuperare la geolocalizzazione dell&#39;indirizzo IP dell&#39;utente. Se viene fornito un altro campo oltre a `ipAddress`, l&#39;SDK [!DNL Target] non recupererà i metadati di geolocalizzazione per la risoluzione.

>[!BEGINTABS]

>[!TAB SDK di Node.js]

```js {line-numbers="true"}
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

>[!TAB SDK Java]

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

## Decisioning lato server

La tabella seguente indica quali regole per il pubblico sono supportate o meno per le decisioni lato server.

| Regola pubblico | Decisioning lato server |
| --- | --- |
| [Geo](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html?lang=it) | Sì |
| [Rete](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/network.html?lang=it) | Sì |
| [Mobile](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html?lang=it) | Sì |
| [Parametri personalizzati](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/custom-parameters.html?lang=it) | Sì |
| [Sistema operativo](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/operating-system.html?lang=it) | Sì |
| [Pagine del sito](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/site-pages.html?lang=it) | Sì |
| [Browser](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/browser.html?lang=it) | Sì |
| [Profilo visitatore](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html?lang=it) | Sì |
| [Origini del traffico](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/traffic-sources.html?lang=it) | Sì |
| [Arco temporale](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/time-frame.html?lang=it) | Sì |
| [Tipi di pubblico di Experience Cloud](https://experienceleague.adobe.com/docs/target/using/integrate/mmp.html?lang=it) (tipi di pubblico da Adobe Audience Manager, Adobe Analytics e Adobe Experience Manager) | Sì |
