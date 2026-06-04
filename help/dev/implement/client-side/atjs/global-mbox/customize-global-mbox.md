---
keywords: mbox globale, personalizza mbox globale, modifica at.js, at.js, implementa at.js
description: Scopri come personalizzare una mbox globale per at.js nella pagina [!UICONTROL Amministrazione]-[!UICONTROL Implementazione] in [!DNL Adobe Target].
title: Come posso personalizzare una mbox globale?
feature: at.js
exl-id: f7809c3d-6e77-4bbe-8da3-4ab0a448c801
TQID: https://experienceleague.adobe.com/MtbjwpKrZ-WmBnE5tBY74oJgQVB-zPLrCuFDrFshkGo
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2: id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 227
ht-degree: 16%

---

# Personalizzare una mbox globale

Informazioni utili per personalizzare una mbox globale di [!DNL Adobe Target] per at.js.

1. Fare clic su **[!UICONTROL Amministrazione]** > **[!UICONTROL Implementazione]**.

1. Disabilita **[!UICONTROL Caricamento pagina abilitato (creazione automatica mbox globale)]**, quindi aggiungi il nome della mbox globale personalizzata che desideri utilizzare per distribuire le attività da [!DNL Target].

>[!WARNING]
>
>La modifica viene salvata automaticamente quando selezioni una mbox globale diversa.

Questa mbox globale personalizzata è utilizzata anche per il monitoraggio dei clic.

![custom-global-mbox](../../assets/custom-global-mbox.png)

1. Implementa la libreria at.js sul tuo sito.

   Per ulteriori informazioni, consulta [Come distribuire at.js](/help/dev/implement/client-side/atjs/how-to-deployatjs/how-to-deployatjs.md).

1. Sincronizza il passaggio con la pubblicazione.

   Quando sei pronto affinché [!DNL Target] inizi a utilizzare la tua mbox globale per tutte le attività future, puoi procedere con questo passaggio.

   Aggiorna il nome della mbox globale personalizzata in corrispondenza del nome utilizzato nel passaggio 2, sopra.


>[!WARNING]
>
>Tutte le attività nel tuo account si sincronizzano con questa mbox. Assicurati che la mbox globale sia presente sul tuo sito in modo che le attività continuino a funzionare. Assicurati di modificare e salvare nuovamente le attività interessate create con il [!UICONTROL Compositore esperienza visivo] (VEC) sincronizzato con questa mbox. Non è necessario salvare nuovamente le attività create in [!UICONTROL Compositore esperienza basato su moduli] o tramite API.
