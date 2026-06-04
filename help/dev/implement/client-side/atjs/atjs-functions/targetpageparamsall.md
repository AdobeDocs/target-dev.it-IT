---
keywords: targetPageParamsAll, targetpageparamsall, PageParamsAll, pageparamsall, parametri di pagina, parametri di pagina, at.js, funzioni, funzione, targetPageParamsAll0
description: Utilizza la funzione [!UICONTROL targetPageParamsAll()] per la libreria JavaScript di  [!DNL Adobe Target] at.js per allegare i parametri a tutte le mbox dall'esterno del codice di richiesta.
title: Come si utilizza la funzione [!UICONTROL targetPageParamsAll()]?
feature: at.js
exl-id: 32045e60-6904-42a1-bf71-fd7e167a829f
TQID: https://experienceleague.adobe.com/A2sZYp7CeE3-zGcqfbvgo32auAtXBKN0dYNa84grs1Q
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
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 171
ht-degree: 54%

---

# [!UICONTROL targetPageParamsAll()]

Questo metodo consente di allegare i parametri a tutte le mbox dall’esterno del codice di richiesta.

Questa funzione è molto utile per includere lo stesso insieme di parametri su più chiamate alla mbox. La funzione deve essere definita dal cliente. Dovrebbe restituire un array di parametri che verranno passati a tutte le richieste mbox nella pagina. Questa funzione può essere definita prima che at.js sia caricato o in **[!UICONTROL Amministrazione]** > **[!UICONTROL Implementazione]** > **[!UICONTROL Modifica]** > **[!UICONTROL Impostazioni codice]** > **[!UICONTROL Intestazione libreria]**.

Puoi trasmettere parametri a target-global-mbox utilizzando la funzione [!UICONTROL targetPageParamsAll()] in uno dei seguenti modi:

* Un elenco delimitato dal simbolo &amp;
* Un array
* Un oggetto JSON

## Esempi

Elenco delimitato dal simbolo &amp; (i valori devono avere la codifica URL):

```javascript {line-numbers="true"}
function targetPageParamsAll() { 
    return "param1=value1&param2=value2&p3=hello%20world"; 
}
```

Array (i valori non devono avere la codifica URL):

```javascript {line-numbers="true"}
targetPageParamsAll = function() { 
     return ["a=1", "b=2", "c=hello world"]; 
};
```

JSON (i valori non devono avere la codifica URL):

```javascript {line-numbers="true"}
targetPageParamsAll = function() { 
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
