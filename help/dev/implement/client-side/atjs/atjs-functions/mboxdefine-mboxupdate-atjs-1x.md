---
keywords: mboxDefine, mboxdefine, mbox define, mboxUpdate, mboxupdate, mbox update, at.js, funzioni, funzione, mboxDefine0
description: Utilizza le funzioni [!UICONTROL mboxDefine()] e [!UICONTROL mboxUpdate()] per la libreria JavaScript at.js  [!DNL Adobe Target]  per definire o aggiornare una mbox. (at.js 1.x)
title: Come si utilizzano le funzioni [!UICONTROL mboxDefine()] e [!UICONTROL mboxUpdate()]?
feature: at.js
exl-id: 0a7dbea2-1cbd-4a5b-ba68-4c76a88d65c4
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 53%

---

# [!UICONTROL mboxDefine()] e [!UICONTROL mboxUpdate()] - at.js 1.x

Definire e aggiornare una mbox in [!DNL Adobe Target].

>[!NOTE]
>
>Queste funzioni sono disponibili solo per at.js versione 1.Solo *x*. Queste funzioni sono state rimosse con il rilascio di at.js 2.*x*. Queste funzioni restituiscono il contenuto predefinito se utilizzate con at.js 2.*x*.

`[!UICONTROL mboxDefine()]` e `[!UICONTROL mboxCreate()]` sono legate agli elementi DIV HTML in cui l’offerta deve essere visualizzata. Questi elementi DIV HTML devono avere la classe `mboxDefault`. Se gli elementi HTML non hanno questa classe collegata, è possibile che venga visualizzato momentaneamente altro contenuto.

## mboxDefine

Crea una mappatura interna tra un nodeId e un nome mbox, ma non esegue la richiesta. Utilizzato in combinazione con `[!UICONTROL mboxUpdate()]`. Integrato in at.js per lo più per facilitare la transizione da mbox.js (ora obsoleto) a at.js.

## mboxUpdate

Esegue la richiesta e applica l&#39;offerta all&#39;elemento identificato da `nodeId` in `mboxDefine()`. Può essere utilizzato anche per aggiornare una mbox iniziata da `[!UICONTROL mboxCreate]`. Integrato in at.js per lo più per facilitare la transizione da mbox.js (ora obsoleto) a at.js. È possibile sostituire `mboxDefine()`/`mboxUpdate()` con [adobe.target.getOffer()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffer.md) e [adobe.target.applyOffer()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-applyoffer.md) utilizzando l’opzione del selettore.

## Esempio

```javascript {line-numbers="true"}
<div id="someId" class="mboxDefault"></div> 
<script> 
 mboxDefine('someId','mboxName','param1=value1','param2=value2'); 
 mboxUpdate('mboxName','param3=value3','param4=value4'); 
</script>
```
