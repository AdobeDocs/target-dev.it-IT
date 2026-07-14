---
keywords: targetPageParams, targetpageparams, pageParams, pageparams, page params, page parameters, at.js, funzioni, funzione, targetPageParams0
description: Utilizza la funzione [!UICONTROL targetPageParams()] per la libreria JavaScript at.js [!DNL Adobe Target] per allegare i parametri alla mbox globale dall'esterno del codice di richiesta.
title: Come si utilizza la funzione [!UICONTROL targetPageParams()]?
feature: at.js
exl-id: 274e4d1f-843a-443b-ad98-7139dc4a13f8
TQID: https://experienceleague.adobe.com/xaSxd1biZ8G-LmgYN4YW9BZ2lDyjZyPHOOlfnbXC2jU
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2:
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: ca4254966a337a0215d66bd28506128b9751d0e0
workflow-type: tm+mt
source-wordcount: 170
ht-degree: 66%

---

# [!UICONTROL targetPageParams()]

Questo metodo consente di allegare i parametri alla mbox globale dall’esterno del codice di richiesta.

Questa funzione è molto utile per includere lo stesso insieme di parametri su più chiamate alla mbox. La funzione deve essere definita dal cliente. Dovrebbe restituire un array di parametri che verranno passati solo alla richiesta mbox globale. Questa funzione può essere definita prima che at.js sia caricato o in **[!UICONTROL Amministrazione]** > **[!UICONTROL Implementazione]** > **[!UICONTROL Modifica]** > **[!UICONTROL Intestazione libreria]**.

Puoi trasmettere parametri a target-global-mbox utilizzando la funzione `[!UICONTROL targetPageParams()]` in uno dei seguenti modi:

* Un elenco delimitato dal simbolo &amp;
* Un array
* Un oggetto JSON

## Esempi

Elenco delimitato dal simbolo &amp; (i valori devono avere la codifica URL):

```javascript {line-numbers="true"}
function targetPageParams() { 
    return "param1=value1&param2=value2&p3=hello%20world"; 
}
```

Array (i valori non devono avere la codifica URL):

```javascript {line-numbers="true"}
targetPageParams = function() { 
     return ["a=1", "b=2", "c=hello world"]; 
};
```

JSON (i valori non devono avere la codifica URL):

```javascript {line-numbers="true"}
targetPageParams = function() { 
  return { 
    "a": 1, 
    "b": 2, 
    "profile": { 
        "age": 26, 
        "country": { 
          "city": "San Francisco" 
        } 
      } 
  }; 
};
```



