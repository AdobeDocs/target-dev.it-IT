---
keywords: mbox globale, target classic, usare mbox globale da target classic
description: Scopri come utilizzare una mbox globale legacy per le attività di [!DNL Adobe Target]  se hai già creato una mbox globale sulle pagine per le implementazioni legacy.
title: Posso utilizzare una mbox globale da un’implementazione legacy?
feature: at.js
exl-id: fe608b5e-ff66-4ba2-a622-d4f7307a9ca9
TQID: https://experienceleague.adobe.com/BCubNDwB8gxZ9bpuCNhxcnFnjB1xQK8ZRkLveinPj4w
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2:
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 304
ht-degree: 19%

---

# Utilizzare una mbox globale da un’implementazione legacy

Per impostazione predefinita, [!DNL Target] crea una mbox globale denominata target-global-mbox, utilizzata per eseguire le attività create in [!DNL Target]. Tuttavia, se hai già creato una mbox globale sulle tue pagine per le implementazioni legacy, puoi utilizzarla per le attività [!DNL Target].

>[!NOTE]
>
>Puoi avere una sola mbox globale per ogni account.

Per utilizzare la mbox globale esistente sia per [!DNL Target] che per l&#39;implementazione precedente, è necessario impostare alcuni parametri.

1. Vai a [!DNL Target], quindi fai clic su **[!UICONTROL Amministrazione]** > **[!UICONTROL Implementazione]**.

   Per impostazione predefinita, è abilitato il caricamento di **[!UICONTROL pagine (la creazione automatica della mbox globale]** è abilitata e la mbox globale personalizzata è denominata `target-global-mbox`.

1. Se desideri utilizzare una mbox esistente, disabilita **[!UICONTROL Caricamento pagina abilitato (creazione automatica mbox globale]**) e specifica il nome di una mbox globale creata in precedenza nel campo **[!UICONTROL Mbox globale]**.

   Il menu a discesa Mbox globale elenca tutte le mbox nel tuo account. Se desideri utilizzare una mbox che non esiste ancora, creala.

1. Fai clic su **[!UICONTROL Salva]**.

   Le impostazioni del tuo account vengono aggiornate.

1. Scarica il nuovo file at.js e fai riferimento a esso sul tuo sito.

   Tutte le attività esistenti vengono aggiornate in modo da utilizzare la mbox globale specificata, comprese quelle create e implementate in precedenza.

## Risoluzione dei problemi di implementazione della mbox globale

Le seguenti domande frequenti possono essere utilizzate per risolvere i problemi relativi all’implementazione della mbox globale:

### Perché la mbox globale non viene caricata o perché c&#39;è latenza nel caricamento della mbox globale quando la pagina viene caricata?

Assicurati che il riferimento at.js sia la prima chiamata JavaScript sulla pagina. Per altre soluzioni a questo problema, vedi [Domande frequenti sulla mbox globale](/help/dev/implement/client-side/atjs/global-mbox/global-mbox-faq.md).
