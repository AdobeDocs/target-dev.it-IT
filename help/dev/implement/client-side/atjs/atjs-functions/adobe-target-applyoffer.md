---
keywords: adobe.target.applyOffer, applyOffer, applyoffer, applica offerta, at.js, funzioni, funzione, $ 8
description: Utilizza la funzione [!UICONTROL adobe.target.applyOffer()] per la libreria JavaScript at.js [!DNL Adobe Target]  per applicare il contenuto della risposta.
title: Come si utilizza la funzione [!UICONTROL adobe.target.applyOffer()]?
feature: at.js
exl-id: 957bbe92-8012-4bd5-95d6-1ae38b72bb16
TQID: https://experienceleague.adobe.com/lrjsIl-gKu1SnrZapxYcoDObvUCGG2ht58QtWbQkYts
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2: id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 4d0e7f9f2887db71229061fa64b2633a84c6d054
workflow-type: tm+mt
source-wordcount: 173
ht-degree: 68%

---

# [!UICONTROL adobe.target.applyOffer(options)]

Questa funzione consente di applicare il contenuto della risposta con [!DNL Adobe Target].

>[!NOTE]
>
>`applyOffer` richiede il parametro `mbox`. Se non viene specificato alcun nome mbox, si verificherà un errore.

Il parametro delle opzioni è obbligatorio e ha la seguente struttura:

| Chiave | Tipo | Obbligatorio | Descrizione |
|--- |--- |--- |--- |
| mbox | Stringa | Sì | Nome mbox<br />Con at.js 1.3.0 (e versioni successive), Target impone l’utilizzo della chiave mbox. Questa chiave era già richiesta in passato, ma Target ora ne impone l’utilizzo per garantire la corretta convalida di Target e il corretto utilizzo di questa funzione da parte dei clienti. |
| selector | Elemento Stringa o DOM | No | Elemento HTML o selettore CSS utilizzato per identificare l&#39;elemento HTML in cui Target deve inserire il contenuto dell&#39;offerta. Se il selettore non viene fornito, Target presuppone che l’elemento HTML debba utilizzare HTML HEAD. |
| Offerta | Array | Sì | Azioni di array che devono essere applicate all&#39;elemento. |

## Esempio

Nell&#39;esempio seguente viene illustrato come utilizzare insieme `[!UICONTROL getOffer]` e `[!UICONTROL applyOffer]`:

```javascript {line-numbers="true"}
adobe.target.getOffer({   
  "mbox": "mbox",   
  "success": function(offers) {           
        adobe.target.applyOffer( {  
           "mbox": "mbox", 
           "offer": offers  
        } ); 
  },   
  "error": function(status, error) {           
      if (console && console.log) { 
        console.log(status); 
        console.log(error); 
      } 
  }, 
 "timeout": 5000 
}); 
```

