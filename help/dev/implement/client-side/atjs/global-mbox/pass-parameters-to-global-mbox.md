---
keywords: parametri mbox globali, targetPageParams, stringa query, array, json, dtm
description: Scopri come utilizzare il [!UICONTROL targetPageParams] funzione per trasmettere ulteriori informazioni di targeting o contesto nel [!DNL Adobe Target] mbox globale.
title: Come si trasmettono i parametri a una mbox globale?
feature: at.js
exl-id: 2a6be3e4-a618-4812-9e87-b01789705c40
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '372'
ht-degree: 63%

---

# Trasmettere i parametri a una mbox globale

JavaScript `targetPageParams` viene utilizzata per trasmettere parametri alla mbox globale in [!DNL Adobe Target]. Ciò è necessario in qualsiasi scenario in cui debbano essere trasmesse informazioni di targeting/contesto aggiuntive [!DNL Target].

Ad esempio, in un&#39;attività di Consigli, rappresenta il prodotto o la categoria corrente che stai osservando con i parametri.

Il codice per chiamare la funzione JavaScript deve precedere la mbox globale sulla pagina, che sia attivata come parte di at.js o inclusa manualmente nel codice della pagina.

>[!NOTE]
>
>Se desideri aggiungere parametri a tutte le mbox sulla pagina e non solo alla mbox globale, utilizza [targetPageParamsAll()](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparamsall.md) funzione.

Puoi trasmettere parametri a `target-global-mbox` utilizzando la funzione `targetPageParams()` in uno dei seguenti modi:

* Un array
* Un oggetto JSON
* Un elenco delimitato dal simbolo &amp;

Utilizza questi tre metodi per verificare che i parametri vengano trasferiti correttamente. Potresti anche essere in grado di verificare il trasferimento di parametri utilizzando il [debugger Adobe Experience Cloud](https://experienceleague.adobe.com/docs/debugger/using/experience-cloud-debugger.html).

È necessario definire la funzione JavaScript prima di aggiungere la mbox globale alla pagina. Il nome deve essere `targetPageParams`.

## Stringa di query

```
p1=v1&p2=v2&p3=hello%20world
```

* Nome: `targetPageParams`
* Valore restituito: parametri delimitati da “&amp;”, con valori di parametro codificati tramite URL.

  Esempio:

  In questo esempio, p3 ha il valore `hello world`, il cui URL è codificato.

Prendi in considerazione il seguente codice di pagina di esempio:

```html {line-numbers="true"}
<html> 
  <head> 
    <title>Title here..</title> 
    <script type="text/javascript"> 
        function targetPageParams() { 
          return "p1=v1&p2=v2&p3=hello%20world";
        } 
    </script> 
    <script src="mbox.js" type="text/javascript"></script> 
  </head> 
  <body>Body here... 
  </body> 
</html>
```

In questo esempio vengono inviati i seguenti dati all&#39;edge della mbox:

* p1=v1
* p2=v2
* p3=hello world

## Array

```javascript {line-numbers="true"}
<!--window.-->targetPageParams = function() { 
  return ["a=1", "b=2", "c=hello world"]; 
}; 
```

I valori non devono avere la codifica URL. Ad esempio, se un valore contiene uno spazio, non è necessario codificare lo spazio.

In questo esempio vengono inviati i seguenti dati all&#39;edge della mbox:

* a=1
* b=2
* c=hello world

## JSON

JSON è una potente modalità di trasferimento dei parametri. [!DNL Target] utilizza le chiavi oggetto di JSON per appiattire le strutture complesse in parametri semplici.

```json {line-numbers="true"}
<!--window.-->targetPageParams = function() { 
  return { 
    "a": 1, 
    "b": 2, 
    "profile": { 
                  "memberStatus": Gold, 
                  "country": { 
                                "city": "San Francisco" 
                            } 
              } 
  }; 
}; 
```

I valori non devono avere la codifica URL. Ad esempio, “San Francisco” non richiede la codifica dello spazio. Lo spazio è sufficiente.

In questo esempio vengono inviati i seguenti dati all&#39;edge della mbox:

* a=1
* b=2
* `profile.memberStatus`=Oro
* `profile.country.city`=San Francisco
