---
keywords: mbox globale, personalizza mbox globale, modifica at.js, at.js, implementa at.js
description: Scopri come personalizzare una mbox globale per at.js nella pagina [!UICONTROL Administration]-[!UICONTROL Implementation] in [!DNL Adobe Target].
title: Come posso personalizzare una mbox globale?
feature: at.js
exl-id: f7809c3d-6e77-4bbe-8da3-4ab0a448c801
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 18%

---

# Personalizzare una mbox globale

Informazioni utili per personalizzare una mbox globale di [!DNL Adobe Target] per at.js.

1. Fare clic su **[!UICONTROL Administration]** > **[!UICONTROL Implementation]**.

1. Disabilita **[!UICONTROL Page load enabled (Auto create global mbox)]**, quindi aggiungi il nome della mbox globale personalizzata che desideri utilizzare per distribuire le attività da [!DNL Target].

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
>Tutte le attività nel tuo account si sincronizzano con questa mbox. Assicurati che la mbox globale sia presente sul tuo sito in modo che le attività continuino a funzionare. Assicurarsi di modificare e salvare nuovamente le attività interessate create con [!UICONTROL Visual Experience Composer] (VEC) sincronizzate con questa mbox. Non è necessario salvare nuovamente le attività create in [!UICONTROL Form-Based Experience Composer] o tramite API.
