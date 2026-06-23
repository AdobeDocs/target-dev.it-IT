---
keywords: adobe.target.applyOffers, applyOffers, applyoffers, applica offerte, at.js, funzioni, funzione,
description: Utilizza la funzione [!UICONTROL adobe.target.applyOffers()] per la libreria JavaScript at.js [!DNL Adobe Target] per applicare più offerte nella risposta. (at.js 2.x)
title: Come si utilizza la funzione [!UICONTROL adobe.target.applyOffers()]?
feature: at.js
exl-id: c391e3f4-fdf1-4e33-8dcb-6bf46e390538
TQID: https://experienceleague.adobe.com/9WIJvPZIlrtLkv-vv-HRkctgwHn3nX-jrE4-4usXW0Y
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2: id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: aa2f3246-cb95-4b30-8899-fdf7d73550ccid: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 4d0e7f9f2887db71229061fa64b2633a84c6d054
workflow-type: tm+mt
source-wordcount: 825
ht-degree: 78%

---

# [!UICONTROL adobe.target.applyOffers(options)] - at.js 2.x

Questa funzione ti consente di applicare più di un’offerta recuperata da `adobe.target.getOffers()`.

>[!NOTE]
>
>Funzione introdotta con at.js 2.*x*. Funzione non disponibile per at.js versione 1.*x*.

| Chiave | Tipo | Obbligatorio | Descrizione |
| --- | --- | --- | --- |
| selector | Stringa | No | Elemento HTML o selettore CSS utilizzato per identificare l’elemento HTML in cui [!DNL Target] deve inserire il contenuto dell’offerta. Se non viene fornito un selettore, [!DNL Target] presuppone che l&#39;elemento HTML da utilizzare sia HTML HEAD. |
| Risposta | Oggetto | Sì | Oggetto di risposta da `getOffers()`.<br />Vedere la tabella Richieste di seguito. |

## Risposta

>[!NOTE]
>
>Consulta la [documentazione sulle API di consegna](/help/dev/implement/delivery-api/overview.md) per informazioni sui tipi accettabili per tutti i campi elencati di seguito.

| Nome campo | Descrizione |
| --- | --- |
| response > prefetch > views > options > content | Nota che il contenuto di “option” non è definito e dipende direttamente dalla struttura tipo/modello dell’opzione. |
| response > prefetch > views > options > type | Tipo di opzione. Riflette il tipo del campo “content”. Il tipo supportato è “actions”. |
| response > prefetch > views > state | Un token opaco da inoltrare con la notifica sul display per la visualizzazione |
| response > prefetch  > views > options > responseTokens | Contiene la mappa dei valori `responseTokens` raccolti quando l’opzione corrente era in fase di elaborazione. |
| response > prefetch  > views > analytics > payload | Payload [!DNL Analytics] per l&#39;integrazione sul lato client da inviare a [!DNL Analytics] dopo l&#39;applicazione della visualizzazione. |
| response > prefetch  > views > trace | L’oggetto contenente tutti i dati di trace per la chiamata di preacquisizione per la visualizzazione.<br />L’oggetto trace includerà anche una versione per la traccia.<br />L’oggetto trace includerà anche i dettagli della visualizzazione corrente. |
| response > prefetch  > views > options > eventToken | La registrazione degli eventi è eseguita per opzione. Per ogni opzione applicata, il rispettivo token evento deve essere aggiunto all’elenco dei token di notifica. Una visualizzazione è composta da più opzioni. Se sono state applicate e visualizzate tutte le opzioni, è necessario includere tutti i valori `eventTokens` nella notifica. |
| response > prefetch  > views > name | Il nome della visualizzazione leggibile dall’utente. |
| response > prefetch  > views > metrics | Metriche di reporting da controllare e segnalare a [!DNL Target]. Al momento, è disponibile il supporto alla metrica di solo clic. Se si verifica un clic sull’elemento, è necessario raccogliere il valore `eventTokens` adeguato e inviare una notifica. |
| response > prefetch  > views > key | Chiave o impronta digitale che identifica la visualizzazione. |
| response > prefetch  > views > id | ID della visualizzazione. |
| response > notifications > id | ID notifica. |
| response > notifications > events > type | Tipo di notifica, clic o visualizzazione. |
| response > notifications > events > trace | La traccia per l’evento di notifica. |
| response > notifications > events > token | Token inviato con l’evento di notifica. |
| response > notifications > events > timestamp | La marca temporale inviata con l’evento di notifica. |
| response > notifications > events > errorCode | Se la notifica non riesce, il codice indica il motivo dell’errore. |
| response > notifications > events | Gli eventi registrati o non registrati per la notifica corrente. |
| response > notifications | Indica le notifiche registrate o non riuscite. |
| response > execute > mboxes > mbox > trace | L’oggetto contenente tutti i dati di trace per la singola richiesta mbox. |
| response > execute > mboxes > mbox > responseTokens | Contiene la mappa di `responseTokens` per un’esecuzione specifica della richiesta mbox. |
| response > execute > mboxes > mbox > option > content | Nota che il contenuto di “option” non è definito e dipende direttamente dalla struttura tipo/modello dell’opzione. |
| response > execute > mboxes > mbox > option > type | Tipo di opzione. Riflette il tipo del campo “content”. I tipi supportati sono: html, redirect, JSON e dinamico. |
| response > execute > mboxes > mbox > options | Opzione Risposta. |
| response > execute > mboxes > mbox > metrics > eventToken | Token dell’evento di clic. |
| response > execute > mboxes > mbox > metrics > type | &quot;click&quot; |
| response > execute > mboxes > mbox > metrics | Contiene l’elenco `clickThrough` delle metriche. |
| response > execute > mboxes > mbox > mbox | Nome della mbox. |
| response > execute > mboxes > mbox >index | Indica che la risposta è per la mbox con questo indice della richiesta. |
| response > execute > mboxes > mbox > analytics > payload | Payload [!DNL Analytics] per l&#39;integrazione sul lato client da inviare a [!DNL Analytics] dopo l&#39;applicazione della mbox. (Vedi la sezione Campagne abilitate per A4T.) |
| response > execute > mboxes | Elenco delle mbox eseguite. |
| response > execute > pageLoad > options > content | Nota che il contenuto di “option” non è definito e dipende direttamente dalla struttura tipo/modello dell’opzione. |
| response > execute > pageLoad > options > type | Tipo di opzione. Riflette il tipo del campo “content”. I tipi supportati sono: html, redirect, JSON, dinamico e “action”. |
| response > execute > pageLoad > options | Opzioni non raggruppate per visualizzazioni (target-global-mbox + opzioni di attività con viste non raggruppate per viste). |
| response > execute > pageLoad > metrics | Metriche di clic che non erano impostate per appartenere a una visualizzazione specifica. |
| response > execute > pageLoad > trace | L’oggetto contenente tutti i dati trace per la richiesta pageload. |
| response > execute > pageLoad > analytics > payload | Payload [!DNL Analytics] per l&#39;integrazione sul lato client da inviare a [!DNL Analytics] dopo l&#39;applicazione del contenuto di caricamento pagina. (Vedi la sezione Campagne abilitate per A4T.) |

## Esempio di chiamata [!UICONTROL applyOffers()]

```javascript {line-numbers="true"}
adobe.target.applyOffers({response:{
  "execute": {
    "pageLoad": {
      "options": [{
        "type": "html",
        "content": "page-load"
      },
      {
        "type": "actions",
        "content": [{
          "type": "setHtml",
          "content": "<h1>Container 1</h1>",
          "selector": "#container1",
          "cssSelector": "#container1"
        },
        {
          "type": "setHtml",
          "content": "<h3>Container 3</h3>",
          "selector": "#container3",
          "cssSelector": "#container3"
        }]
      }],
 
 
      "metrics": [{
        "type": "click",
        "selector": "#container1",
        "eventToken": "page-load-click-metric" 
      }]
    }
  }
}});
```

## Esempio di chiamate della catena di promesse con `[!UICONTROL getOffers()]` e `[!UICONTROL applyOffers()]`, perché queste funzioni sono basate su promesse

```javascript {line-numbers="true"}
adobe.target.getOffers({...})
.then(response => adobe.target.applyOffers({ response: response }))
.then(() => console.log("Success"))
.catch(error => console.log("Error", error));
```

Per ulteriori esempi su come utilizzare getOffers(), consulta la [documentazione](adobe-target-getoffers-atjs-2.md) di getOffers

### Esempio di richiesta di caricamento pagina


```javascript {line-numbers="true"}
adobe.target.getOffers({
    request: {
        execute: {
            pageLoad: {}
        }
    }
}).
then(response => adobe.target.applyOffers({ response: response }))
.then(() => console.log("Success"))
.catch(error => console.log("Error", error));
```

