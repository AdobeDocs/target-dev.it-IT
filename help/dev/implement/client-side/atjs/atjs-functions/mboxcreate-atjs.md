---
keywords: mboxCreate, mboxcreate, mbox create, at.js, funzioni, funzione
description: Utilizza la funzione [!UICONTROL mboxCreate()] per la libreria JavaScript di  [!DNL Adobe Target] at.js per applicare le offerte all'elemento DIV più vicino con il nome della classe mboxDefault. (at.js 1.x)
title: Come si utilizza la funzione [!UICONTROL mboxCreate()]?
feature: at.js
exl-id: 86eba1fc-4e1d-4793-94e7-898bf81f8945
TQID: https://experienceleague.adobe.com/hCEKL9RPtqIbMVEouzObjU6dc7TKl1hBtKZ1jEdicRE
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2: id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: a1af9d2c909d9b3d506dd4875d1bd75149dbf636
workflow-type: tm+mt
source-wordcount: 216
ht-degree: 40%

---

# [!UICONTROL mboxCreate(mbox,params)] - at.js 1.x

Esegue una richiesta e applica l’offerta all’elemento DIV più vicino con il nome della classe mboxDefault.

>[!NOTE]
>
>Questa funzione è disponibile solo per at.js versione 1.*x*. Questa funzione è stata rimossa con il rilascio di at.js 2.x. Questa funzione restituisce il contenuto predefinito se utilizzata con at.js 2.x.

Questa funzione è incorporata in at.js per lo più per facilitare la transizione da mbox.js (ora obsoleto) a at.js. Un’alternativa più recente a `[!UICONTROL mboxCreate()]` è `[!UICONTROL adobe.target.getOffer()]`/`[!UICONTROL adobe.target.applyOffer()]` o la direttiva angolare.

## Esempio

```javascript {line-numbers="true"}
<div class="mboxDefault"> 
  default content to replace by offer 
</div> 
<script> 
  mboxCreate('mboxName','param1=value1','param2=value2'); 
</script>
```

## Note

`mboxCreate()` usa ora l&#39;endpoint “json” invece di quello “standard” e si attiva in modo asincrono. Per questo motivo:

* [Il debug](/help/dev/implement/client-side/target-debugging-atjs/target-debugging-atjs.md) è un po&#39; diverso.
* Evita di usare codice di offerte che richiede chiamate sincrone e bloccanti.

  Ad esempio, offerte che impostano variabili JavaScript che vengono utilizzate dal codice del sito o altre mbox che compaiono più avanti nella pagina.

* Assicurati di avere un `<div class="mboxDefault"></div>` prima di richiamare `[!UICONTROL mboxCreate()]`, perché at.js non ne aggiungerà uno per te.

* Le funzioni vuote di `[!UICONTROL mboxCreate()]` non sono consigliate come mbox globale.

  È preferibile utilizzare una mbox globale creata automaticamente in at.js, in quanto si attiva da `<head>` e può restituire prima il contenuto.



