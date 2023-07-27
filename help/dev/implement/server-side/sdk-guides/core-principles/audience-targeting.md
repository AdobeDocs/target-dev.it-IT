---
title: Targeting del pubblico
description: I tipi di pubblico possono essere utilizzati per eseguire il targeting delle attività di sperimentazione e personalizzazione. [!DNL Adobe Target] supporta una miriade di potenti funzionalità di targeting del pubblico pronte all’uso.
exl-id: df1bd856-e848-452c-90a0-abf29e7a2313
feature: Implement Server-side
source-git-commit: 09a50aa67ccd5c687244a85caad24df56c0d78f5
workflow-type: tm+mt
source-wordcount: '967'
ht-degree: 20%

---

# Targeting del pubblico

## Panoramica

I tipi di pubblico possono essere utilizzati per eseguire il targeting delle attività di sperimentazione e personalizzazione. [!DNL Adobe Target] supporta una miriade di potenti funzionalità di targeting del pubblico pronte all’uso. I seguenti attributi sono disponibili per [targeting di pubblico](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/create-audience.html):

### [!DNL Target] Libreria

Per ulteriori informazioni, consulta [[!DNL Target] Libreria](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/target-library.html). &#x200B;
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

Per ulteriori informazioni, consulta [Geo](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html).
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

Per ulteriori informazioni, consulta [Rete](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/network.html).

* ISP
* Nome di dominio
* Velocità di connessione

### Dispositivi mobili

Per ulteriori informazioni, consulta [Mobile](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html).

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

Per ulteriori informazioni, consulta [Parametri personalizzati](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/custom-parameters.html).

* qualsiasi coppia chiave/valore

### Sistema operativo

Per ulteriori informazioni, consulta [Sistema operativo](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/operating-system.html).

* Linux
* Macintosh
* Windows

### Pagine del sito

Per ulteriori informazioni, consulta [Pagine del sito](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/site-pages.html).

* Pagina corrente
* Pagina precedente
* Pagina di destinazione
* Intestazione HTTP

### Browser

Per ulteriori informazioni, consulta [Browser](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/browser.html).

* Tipo
* Lingua
* Versione

### Profilo visitatore

Per ulteriori informazioni, consulta [Profilo visitatore](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html).

* qualsiasi coppia chiave/valore, che viene mantenuta

### Origini del traffico

Per ulteriori informazioni, consulta [Sorgenti di traffico](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/traffic-sources.html).

* Da Baidu
* Da Bing
* Da Google
* Da Yahoo
* Pagina di destinazione di riferimento: URL
* Pagina di destinazione di riferimento: dominio
* Pagina di destinazione di riferimento: query

### Arco temporale

Per ulteriori informazioni, consulta [ Arco temporale](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/time-frame.html).

* Data inizio / Data fine

## Client Hints

[!DNL Adobe Target] richiede Client Hints per la segmentazione corretta degli attributi di browser, sistema operativo e pubblico mobile, nonché alcune istanze degli script di profilo. Per ulteriori informazioni, consulta [User Agent e Client Hints](../../../client-side/atjs/user-agent-and-client-hints.md).

### Come trasmettere gli Client Hints ad [!DNL Adobe Target]

A partire da Node.js SDK v2.4.0 e Java SDK v2.3.0, gli Client Hints possono essere inviati a [!DNL Target] tramite `getOffers()` chiamate. Gli Client Hints devono essere inclusi nel `request.context` insieme all&#39;agente utente.

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
| [Tipi di pubblico di Experience Cloud](https://experienceleague.adobe.com/docs/target/using/integrate/mmp.html) (Tipi di pubblico da Adobe Audience Manager, Adobe Analytics e Adobe Experience Manager | No |

### Geotargeting per le decisioni su dispositivo

Al fine di mantenere una latenza prossima allo zero per le attività di decisioning sul dispositivo con un pubblico basato su geotargeting, l’Adobe consiglia di fornire autonomamente i valori geografici nella chiamata a `getOffers`. A tale scopo, impostare `Geo` oggetto in `Context` della richiesta. Ciò significa che il server dovrà essere in grado di determinare la posizione di ciascun utente finale. Ad esempio, il server potrebbe eseguire una ricerca IP-to-Geo utilizzando un servizio configurato. Alcuni provider di hosting, come Google Cloud, forniscono questa funzionalità tramite intestazioni personalizzate in ogni `HttpServletRequest`.

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

Tuttavia, se non hai la possibilità di eseguire ricerche IP-to-Geo sul server, ma desideri comunque eseguire decisioni su dispositivo per `getOffers` sono supportate anche le richieste che contengono tipi di pubblico basati su geotargeting. Il lato negativo di questo approccio è che utilizzerà una ricerca remota IP-to-Geo, che aggiungerà latenza a ogni `getOffers` chiamare. Questa latenza deve essere inferiore a un `getOffers` , poiché raggiunge una rete CDN situata vicino al server. Devi **solo** fornire `ipAddress` campo in `Geo` oggetto in `Context` della richiesta, affinché l’SDK possa recuperare la geolocalizzazione dell’indirizzo IP dell’utente. Se sono presenti altri campi oltre a `ipAddress` viene fornito, il [!DNL Target] L’SDK non recupererà i metadati di geolocalizzazione per la risoluzione.

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
| [Geo](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html) | Sì |
| [Rete](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/network.html) | Sì |
| [Mobile](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html) | Sì |
| [Parametri personalizzati](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/custom-parameters.html) | Sì |
| [Sistema operativo](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/operating-system.html) | Sì |
| [Pagine del sito](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/site-pages.html) | Sì |
| [Browser](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/browser.html) | Sì |
| [Profilo visitatore](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html) | Sì |
| [Origini del traffico](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/traffic-sources.html) | Sì |
| [Arco temporale](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/time-frame.html) | Sì |
| [Tipi di pubblico di Experience Cloud](https://experienceleague.adobe.com/docs/target/using/integrate/mmp.html) (Tipi di pubblico da Adobe Audience Manager, Adobe Analytics e Adobe Experience Manager | Sì |
