---
keywords: mbox globale, target classic, usare mbox globale da target classic
description: Scopri come utilizzare una mbox globale legacy per le attività di [!DNL Adobe Target]  se hai già creato una mbox globale sulle pagine per le implementazioni legacy.
title: Posso utilizzare una mbox globale da un’implementazione legacy?
feature: at.js
exl-id: fe608b5e-ff66-4ba2-a622-d4f7307a9ca9
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 19%

---

# Utilizzare una mbox globale da un’implementazione legacy

Per impostazione predefinita, [!DNL Target] crea una mbox globale denominata target-global-mbox, utilizzata per eseguire le attività create in [!DNL Target]. Tuttavia, se hai già creato una mbox globale sulle tue pagine per le implementazioni legacy, puoi utilizzarla per le attività [!DNL Target].

>[!NOTE]
>
>Puoi avere una sola mbox globale per ogni account.

Per utilizzare la mbox globale esistente sia per [!DNL Target] che per l&#39;implementazione precedente, è necessario impostare alcuni parametri.

1. Vai a [!DNL Target], quindi fai clic su **[!UICONTROL Administration]** > **[!UICONTROL Implementation]**.

   Per impostazione predefinita, **[!UICONTROL Page load enabled (Auto-create global mbox]** è abilitato e la mbox globale personalizzata è denominata `target-global-mbox`.

1. Se desideri utilizzare una mbox esistente, disabilita **[!UICONTROL Page load enabled (Auto-create global mbox]** e specifica il nome di una mbox globale creata in precedenza nel campo **[!UICONTROL Global Mbox]**.

   Il menu a discesa Mbox globale elenca tutte le mbox nel tuo account. Se desideri utilizzare una mbox che non esiste ancora, creala.

1. Fare clic su **[!UICONTROL Save]**.

   Le impostazioni del tuo account vengono aggiornate.

1. Scarica il nuovo file at.js e fai riferimento a esso sul tuo sito.

   Tutte le attività esistenti vengono aggiornate in modo da utilizzare la mbox globale specificata, comprese quelle create e implementate in precedenza.

## Risoluzione dei problemi di implementazione della mbox globale

Le seguenti domande frequenti possono essere utilizzate per risolvere i problemi relativi all’implementazione della mbox globale:

### Perché la mbox globale non viene caricata o perché c&#39;è latenza nel caricamento della mbox globale quando la pagina viene caricata?

Assicurati che il riferimento at.js sia la prima chiamata JavaScript sulla pagina. Per altre soluzioni a questo problema, vedi [Domande frequenti sulla mbox globale](/help/dev/implement/client-side/atjs/global-mbox/global-mbox-faq.md).
