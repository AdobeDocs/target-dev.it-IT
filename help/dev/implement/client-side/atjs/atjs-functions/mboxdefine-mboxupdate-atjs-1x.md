---
keywords: mboxDefine, mboxdefine, mbox define, mboxUpdate, mboxupdate, mbox update, at.js, funzioni, funzione, mboxDefine0
description: Utilizza le funzioni [!UICONTROL mboxDefine()] e [!UICONTROL mboxUpdate()] per la libreria JavaScript di  [!DNL Adobe Target] at.js per definire o aggiornare una mbox. (at.js 1.x)
title: Come si utilizzano le funzioni [!UICONTROL mboxDefine()] e [!UICONTROL mboxUpdate()]?
feature: at.js
exl-id: 0a7dbea2-1cbd-4a5b-ba68-4c76a88d65c4
TQID: https://experienceleague.adobe.com/Fn-Ej8jk2AMEn79tOtRoP9GQc36Ugy6FtXyn6x7jkmA
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2: id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: a1af9d2c909d9b3d506dd4875d1bd75149dbf636
workflow-type: tm+mt
source-wordcount: 208
ht-degree: 46%

---

# [!UICONTROL mboxDefine()] e [!UICONTROL mboxUpdate()] - at.js 1.x

Definire e aggiornare una mbox in [!DNL Adobe Target].

>[!NOTE]
>
>Queste funzioni sono disponibili solo per at.js versione 1.*x*. Queste funzioni sono state rimosse con il rilascio di at.js 2.*x*. Queste funzioni restituiscono il contenuto predefinito se utilizzate con at.js 2.*x*.

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



