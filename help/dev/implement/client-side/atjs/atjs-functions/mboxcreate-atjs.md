---
keywords: mboxCreate, mboxcreate, mbox create, at.js, funzioni, funzione
description: Utilizza la funzione [!UICONTROL mboxCreate()] per la libreria JavaScript at.js  [!DNL Adobe Target]  per applicare le offerte all'elemento DIV più vicino con il nome della classe mboxDefault. (at.js 1.x)
title: Come si utilizza la funzione [!UICONTROL mboxCreate()]?
feature: at.js
exl-id: 86eba1fc-4e1d-4793-94e7-898bf81f8945
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 56%

---

# [!UICONTROL mboxCreate(mbox,params)] - at.js 1.x

Esegue una richiesta e applica l’offerta all’elemento DIV più vicino con il nome della classe mboxDefault.

>[!NOTE]
>
>Questa funzione è disponibile per at.js versione 1.*x*. Questa funzione è stata rimossa con il rilascio di at.js 2.x e restituisce il contenuto predefinito se utilizzata con at.js 2.x.

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
