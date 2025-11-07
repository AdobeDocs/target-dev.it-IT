---
keywords: at.js, agente utente browser, agente utente, hint client, agente utente
description: Scopri come Adobe Target utilizza user-agent e Client Hints per qualificare i visitatori per la segmentazione e la personalizzazione.
title: User Agent e Client Hints
feature: at.js
exl-id: e0d87d95-ee95-4ca9-8632-222ae1fb9a91
source-git-commit: 67cc93cf697f8d5bca6fedb3ae974e4012347a0b
workflow-type: tm+mt
source-wordcount: '1162'
ht-degree: 72%

---

# User-agent e Client Hints

Adobe Target utilizza user-agent per qualificare i visitatori per la segmentazione e la personalizzazione.

>[!NOTE]
>
>Le informazioni contenute in questo articolo si applicano alla versione [2.9.0](target-atjs-versions.md) di at.js (o successiva).

Ogni volta che un browser web effettua una richiesta a un server, nell’intestazione della richiesta sono incluse informazioni sul browser e sull’ambiente in cui viene eseguito il browser. Fin dai primi giorni di Internet, questi dati sono stati aggregati in una singola stringa denominata user-agent.

Il testo seguente è un esempio di user-agent di un computer basato su Mac OS X che utilizza un browser Safari:

```
Mozilla/5.0 (Linux; Android 12; SM-S908E) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/101.0.4951.41 Mobile Safari/537.36 
```

Da questo user-agent, il server che riceve la richiesta può distinguere le seguenti informazioni sul browser e sul sistema operativo:

| Informazioni | Dettagli |
| --- | --- |
| Nome software | Chrome |
| Versione software | 101 |
| Versione software completa | 101.0.4951.41 |
| Nome del motore di layout | AppleWebKit |
| Versione del motore di layout | 537.36 |
| Sistema operativo | Android |
| Versione del sistema operativo | Android 12 (Snow Cone) |
| Dispositivo | SM-S908E (Samsung Galaxy S22 Ultra) |

Nel corso degli anni, la quantità di informazioni sul browser e sul dispositivo incluse nella stringa user-agent è aumentata.

## Casi d’uso dell’User-agent

Gli User-agents sono stati a lungo utilizzati per fornire ai team di marketing e sviluppatori informazioni importanti sul modo in cui browser, sistemi operativi e dispositivi visualizzano il contenuto del sito e sul modo in cui gli utenti interagiscono con i siti web. Gli User-agents vengono inoltre utilizzati per bloccare spam e filtrare bot che esaminano i siti per diversi scopi aggiuntivi.

Tuttavia, negli ultimi anni alcuni proprietari di siti e fornitori di marketing hanno utilizzato user-agent, insieme ad altre informazioni incluse nelle intestazioni della richiesta, per creare impronte digitali che possono essere utilizzate come mezzo per identificare gli utenti senza esserne a conoscenza. Nonostante lo scopo importante che user-agent svolge per i proprietari del sito, gli sviluppatori del browser hanno deciso di apportare modifiche al modo in cui gli user-agents operano per limitare potenziali problemi di privacy per i visitatori del sito.

Gli sviluppatori di browser hanno creato User-Agent Client Hints come soluzione a questa sfida. I Client Hints consentono ancora ai siti di raccogliere le informazioni necessarie su browser, sistema operativo e dispositivo, fornendo al tempo stesso una maggiore protezione contro i metodi di tracciamento nascosti, come la impronta digitale.

## Client Hints

User-Agent Client Hints forniscono ai proprietari del sito web la possibilità di accedere a gran parte delle stesse informazioni disponibili nell’user-agent, ma in un modo più rispettoso della privacy. Quando i browser moderni inviano un user-agent a un server Web, l’intero user-agent viene inviato su ogni richiesta, indipendentemente dal fatto che sia necessario. I Client Hints, invece, impongono un modello in cui il server deve chiedere al browser le informazioni aggiuntive che desidera conoscere sul client. Dopo aver ricevuto questa richiesta, il browser può applicare i propri criteri o la propria configurazione utente per determinare quali dati vengono restituiti. Invece di esporre l’intero user-agent per impostazione predefinita su tutte le richieste, l’accesso viene ora gestito in modo esplicito e verificabile.

I User-Agent Client Hints. sono disponibili in Chrome dalla versione 89. Le versioni recenti dei browser basati su Chromium, come Microsoft Edge, Opera, Brave, Chrome Android, Opera Android e Samsung Internet, supportano anche l’API dei Client Hints.

I Client Hints contenuti nelle intestazioni della prima richiesta effettuata dal browser a un server Web contengono il marchio del browser, la versione principale del browser e un indicatore che suggerisce se il client è un dispositivo mobile. Ogni elemento di dati ha un proprio valore di intestazione, anziché essere raggruppato in una singola stringa user-agent.

Ad esempio, ecco alcuni Client Hints:

```
Sec-CH-UA: "Chromium";v="101", "Google Chrome";v="101", " Not;A Brand";v="99" 
Sec-CH-UA-Mobile: ?0 
Sec-CH-UA-Platform: "macOS"
```

...considerando che si tratta dell’agente utente per lo stesso browser:

```
Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/101.0.4951.54 Safari/537.36 
```

Sebbene le informazioni siano simili, la prima richiesta al server contiene Client Hints che contengono solo un sottoinsieme di ciò che è disponibile nella stringa user-agent. L’architettura del sistema operativo, la versione completa del sistema operativo, il nome del motore di layout, la versione del motore di layout e la versione completa del browser mancano nella richiesta. Tuttavia, nelle richieste successive, l’API dei Client Hints consente ai server Web di richiedere ulteriori dettagli di entropia elevata sul dispositivo. Quando si richiedono questi valori entropici elevati, a seconda della policy del browser o delle impostazioni utente, la risposta del browser può includere tali informazioni.

L’esempio seguente è un oggetto JSON restituito dall’API Client Hints quando vengono richiesti valori di entropia elevati:

```
{ 

    "architecture":"x86", 
    "bitness":"64", 
    "brands":[ 
        { 
            "brand":" Not A;Brand", 
            "version":"99" 
        }, 
        { 
            "brand":"Chromium", 
            "version":"100" 
        }, 
        { 
            "brand":"Google Chrome", 
            "version":"100" 
        } 
    ], 
    "fullVersionList":[ 
        { 
            "brand":" Not A;Brand", 
            "version":"99.0.0.0" 
        }, 
        { 
            "brand":"Chromium", 
            "version":"100.0.4896.127" 
        }, 
        { 
            "brand":"Google Chrome", 
            "version":"100.0.4896.127" 
        } 
    ], 
    "mobile":false, 
    "model":"", 
    "platformVersion":"12.2.1" 
} 
```

I valori di entropia elevati includono diverse informazioni aggiuntive che non sono disponibili nelle informazioni predefinite dei Client Hints. La tabella seguente contiene i dettagli dei dati disponibili nella richiesta predefinita rispetto alle informazioni sui User-Agent Client Hints entropici elevati.

| Intestazione HTTP | JavaScript | User-agent | Client hint | Client hint entropico elevato |
| --- | --- | --- | --- | --- |
| Sec-CH-UA | marchi | Sì | Sì | No |
| Sec-CH-UA-Platform | piattaforma | Sì | Sì | No |
| Sec-CH-UA-Mobile | dispositivi mobili | Sì | Sì | No |
| Sec-CH-UA-Platform-Version | platformVersion | Sì | No | Sì |
| Sec-CH-UA-Arch | architettura | Sì | No | Sì |
| Sec-CH-UA-Model | modello | Sì | No | Sì |
| Sec-CH-UA-Bitness | numero di bit | Sì | No | Sì |
| Sec-CH-UA-Full-Version-List | fullVersionList | Sì | No | Sì |

## Migrazione agli Client Hints

Attualmente, i browser basati su Chromium continuano a inviare user-agent insieme agli Client Hints nelle intestazioni delle richieste effettuate ai server Web. Tuttavia, a partire da aprile 2022 e fino a febbraio 2023, la quantità di dati contenuti nella stringa user-agent viene ridotta. Altri browser, come Safari e Firefox, continuano a sfruttare la stringa user-agent; tuttavia, anche loro riducono la quantità di informazioni in essa contenute.

## Casi di utilizzo di Target che richiedono Client Hints

I seguenti casi d’uso nella destinazione richiedono Client Hints:

### Attributi del pubblico

Se utilizzi i tipi di pubblico di Target e utilizzi uno dei seguenti attributi di pubblico, Target richiede Client Hints per eseguire la segmentazione corretta:

* Browser
* Sistema operativo
* Dispositivi mobili

### Script di profilo

Se utilizzi gli script di profilo e fai riferimento all’attributo `user.browser` (che fa riferimento user-agent), potrebbe essere necessario aggiornare lo script del profilo per controllare anche uno o più Client Hints. Puoi accedere a qualsiasi Client Hints utilizzando la funzione `user.clientHint('sec-ch-ua-xxxxx')`. Il nome dell’intestazione Client Hint deve essere tutto minuscolo.

L’esempio seguente mostra come rilevare correttamente un sistema operativo Windows in uno script di profilo:

```
"return (((user.browser != null) && (user.browser.indexOf(\"Windows\") > -1)) || " + 
      "((user.clientHint('sec-ch-ua-platform') != null) && 
(user.clientHint('sec-ch-ua-platform') === 'Windows')));" 
```

Di seguito è riportata una tabella degli Client Hints e della corrispondente semantica di utilizzo degli script di profilo.

| Intestazione Client Hint | Entropia | Attributo pubblico | Utilizzo degli script di profilo |
| --- | --- | --- | --- |
| [Sec-CH-UA](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Sec-CH-UA) | Bassa | Browser | `user.clientHint('sec-ch-ua')` |
| [Sec-CH-UA-Arch](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Sec-CH-UA-Arch) | Alta | Esposto agli utenti tramite script di profilo | `user.clientHint('sec-ch-ua-arch')` |
| [Sec-CH-UA-Bitness](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Sec-CH-UA-Bitness) | Alta | Esposto agli utenti tramite script di profilo | `user.clientHint('sec-ch-ua-bitness')` |
| [Sec-CH-UA-Full-Version-List](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Sec-CH-UA-Full-Version-List) | Alta | Browser | `user.clientHint('sec-ch-ua-full-version-list')` |
| [Sec-CH-UA-Mobile](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Sec-CH-UA-Mobile) | Bassa | Dispositivi mobili | `user.clientHint('sec-ch-ua-mobile')` |
| [Sec-CH-UA-Model](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Sec-CH-UA-Model) | Alta | Dispositivi mobili | `user.clientHint('sec-ch-ua-model')` |
| [Sec-CH-UA-Platform](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Sec-CH-UA-Platform) | Bassa | Sistema operativo | `user.clientHint('sec-ch-ua-platform')` |
| [Sec-CH-UA-Platform-Version](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Sec-CH-UA-Platform-Version) | Alta | Esposto agli utenti tramite script di profilo | `user.clientHint('sec-ch-ua-platform-version')` |

## Come trasmettere gli Client Hints ad Adobe Target

Le sezioni seguenti contengono ulteriori informazioni su come trasmettere gli Client Hints, a seconda dell’implementazione di Target.

### at.js versione 2.9.0 (o successiva)

A partire dalla versione at.js 2.9.0, gli User Agent Client Hints vengono raccolti automaticamente dal browser e inviati a Target quando viene chiamato `getOffer/getOffers()`. Per impostazione predefinita, at.js raccoglie solo i Client Hints a “Bassa entropia”. Se esegui la segmentazione del pubblico o utilizzi script di profilo basati su dati classificati ad “Alta entropia” dalle sezioni precedenti, devi configurare at.js per raccogliere Client Hints ad “Alta Entropia” dal browser tramite `targetGlobalSettings`.

```
window.targetGlobalSettings = { allowHighEntropyClientHints: true };
```

### SDK lato server

Per ulteriori informazioni su come trasmettere gli Client Hints tramite SDK lato server, vedi [Client Hints](../../server-side/sdk-guides/core-principles/audience-targeting.md#client-hints) nella documentazione dell&#39;implementazione lato server.
