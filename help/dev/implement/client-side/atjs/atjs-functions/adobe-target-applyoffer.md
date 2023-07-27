---
keywords: adobe.target.applyOffer, applyOffer, applyoffer, applica offerta, at.js, funzioni, funzione, $ 8
description: Utilizza il [!UICONTROL adobe.target.applyOffer()] funzione per [!DNL Adobe Target] Libreria JavaScript at.js per applicare il contenuto della risposta.
title: Come si utilizza [!UICONTROL adobe.target.applyOffer()] Funzione?
feature: at.js
exl-id: 957bbe92-8012-4bd5-95d6-1ae38b72bb16
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '173'
ht-degree: 69%

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
