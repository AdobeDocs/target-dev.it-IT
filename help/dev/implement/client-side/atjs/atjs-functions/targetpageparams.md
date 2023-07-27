---
keywords: targetPageParams, targetpageparams, pageParams, pageparams, page params, page parameters, at.js, funzioni, funzione, targetPageParams0
description: Utilizza il [!UICONTROL targetPageParams()] funzione per [!DNL Adobe Target] Libreria JavaScript at.js per allegare i parametri alla mbox globale dall'esterno del codice di richiesta.
title: Come si utilizza [!UICONTROL targetPageParams()] Funzione?
feature: at.js
exl-id: 274e4d1f-843a-443b-ad98-7139dc4a13f8
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 65%

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
