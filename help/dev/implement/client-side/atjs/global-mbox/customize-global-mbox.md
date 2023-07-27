---
keywords: mbox globale, personalizza mbox globale, modifica at.js, at.js, implementa at.js
description: Scopri come personalizzare una mbox globale per at.js sulla [!UICONTROL Amministrazione]-[!UICONTROL Implementazione] pagina in [!DNL Adobe Target].
title: Come posso personalizzare una mbox globale?
feature: at.js
exl-id: f7809c3d-6e77-4bbe-8da3-4ab0a448c801
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 17%

---

# Personalizzare una mbox globale

Informazioni su come personalizzare un [!DNL Adobe Target] mbox globale per at.js.

1. Fai clic su **[!UICONTROL Amministrazione]** > **[!UICONTROL Implementazione]**.

1. Disattiva **[!UICONTROL Caricamento pagina abilitato (creazione automatica di una mbox globale)]**, quindi aggiungi il nome della mbox globale personalizzata che desideri utilizzare per inviare attività da [!DNL Target].

>[!WARNING]
>
>La modifica viene salvata automaticamente quando selezioni una mbox globale diversa.

Questa mbox globale personalizzata è utilizzata anche per il monitoraggio dei clic.

![custom-global-mbox](../../assets/custom-global-mbox.png)

1. Implementa la libreria at.js sul tuo sito.

   Consulta [Come distribuire at.js](/help/dev/implement/client-side/atjs/how-to-deployatjs/how-to-deployatjs.md) per ulteriori informazioni.

1. Sincronizza il passaggio con la pubblicazione.

   Quando sei pronto per [!DNL Target] per iniziare a utilizzare la mbox globale per tutte le attività future, puoi procedere con questo passaggio.

   Aggiorna il nome della mbox globale personalizzata in corrispondenza del nome utilizzato nel passaggio 2, sopra.


>[!WARNING]
>
>Tutte le attività nel tuo account si sincronizzano con questa mbox. Assicurati che la mbox globale sia presente sul tuo sito in modo che le attività continuino a funzionare. Assicurati di modificare e salvare nuovamente le attività interessate create con [!UICONTROL Compositore esperienza visivo] (Compositore esperienza visivo) che si sincronizzano con questa mbox. Non è necessario salvare nuovamente le attività create in [!UICONTROL Compositore esperienza basato su moduli] o tramite API.
