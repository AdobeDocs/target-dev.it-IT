---
keywords: targetPageParamsAll, targetpageparamsall, PageParamsAll, pageparamsall, parametri di pagina, parametri di pagina, at.js, funzioni, funzione, targetPageParamsAll0
description: Utilizza la funzione [!UICONTROL targetPageParamsAll()] per la libreria JavaScript at.js  [!DNL Adobe Target]  per allegare i parametri a tutte le mbox dall'esterno del codice di richiesta.
title: Come si utilizza la funzione [!UICONTROL targetPageParamsAll()]?
feature: at.js
exl-id: 32045e60-6904-42a1-bf71-fd7e167a829f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 68%

---

# [!UICONTROL targetPageParamsAll()]

Questo metodo consente di allegare i parametri a tutte le mbox dall’esterno del codice di richiesta.

Questa funzione è molto utile per includere lo stesso insieme di parametri su più chiamate alla mbox. La funzione deve essere definita dal cliente. Dovrebbe restituire un array di parametri che verranno passati a tutte le richieste mbox nella pagina. Questa funzione può essere definita prima che at.js sia caricato o in **[!UICONTROL Administration]** > **[!UICONTROL Implementation]** > **[!UICONTROL Edit]** > **[!UICONTROL Code Settings]** > **[!UICONTROL Library Header]**.

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
