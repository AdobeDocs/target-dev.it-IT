---
title: Best practice per l’utilizzo delle decisioni su dispositivo
description: Scopri le best practice per l'utilizzo di [!UICONTROL on-device decisioning] in [!DNL Adobe Target]
feature: Implement Server-side
exl-id: a0ca014d-ad9f-4ecc-961d-cb7ba236507f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 0%

---

# Best practice

[!DNL Adobe] consiglia le seguenti best practice per l&#39;utilizzo di [!UICONTROL on-device decisioning]:

## Best practice quando il metodo decisionale è &quot;su dispositivo&quot;

Quando si utilizza &quot;su dispositivo&quot; come metodo decisionale, l’artefatto viene scaricato quando il visitatore carica la pagina web per la prima volta. Qualsiasi qualifica di attività che deve essere eseguita al primo caricamento della pagina (nessuna cache) si verifica solo dopo che l’artefatto è stato completamente scaricato. È possibile seguire alcune best practice per garantire che le qualifiche dell’attività avvengano rapidamente per un nuovo visitatore anonimo.

* Disattiva le attività che supportano &quot;On-Device&quot; e che non devono essere incluse nell’artefatto.
* Se disponi di Target Premium, puoi utilizzare [proprietà/aree di lavoro](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/property-channel.html?lang=it) per creare file di artefatti diversi per aree di lavoro diverse.
* Se i file degli artefatti diventano molto grandi per motivi legittimi, puoi utilizzare il metodo decisionale &quot;ibrido&quot;. Questo metodo consente di scaricare l’artefatto in parallelo e tutte le chiamate API di Target passano sul cavo fino a quando l’artefatto non viene scaricato. Per ulteriori informazioni su questo approccio, consulta la sezione sulle best practice sulla modalità decisionale &quot;ibrida&quot; riportata di seguito.
* Se si dispone di un&#39;applicazione a pagina singola (SPA), [!DNL Adobe] consiglia di caricare e inizializzare at.js prima di caricare il file JavaScript principale dell&#39;applicazione durante il primo caricamento della pagina. Questo approccio avvia il download dell’artefatto molto prima, offrendo un rendering più rapido dell’esperienza.

## Best practice quando il metodo decisionale è &quot;ibrido&quot;

Quando si utilizza &quot;ibrido&quot; come metodo decisionale, l’artefatto viene scaricato in parallelo. Fino a quando l&#39;artefatto non viene scaricato, tutte le chiamate API [!DNL Target] passano sul cavo anche se le &quot;posizioni&quot; sono compatibili con il dispositivo. Questo comportamento è predefinito per tutte le chiamate `getOffers()` e fornisce le migliori prestazioni nella maggior parte delle situazioni. Se si modifica il comportamento predefinito di `getOffers()` impostando `decisioningMethod` su `on-device`, seguire queste best practice per evitare errori e garantire le prestazioni migliori.

* Se decidi di chiamare `getOffers()` con `decisioningMethod` come `on-device` quando la pagina viene caricata per la prima volta, devi farlo all&#39;interno del gestore eventi &quot;ARTIFACT_DOWNLOAD_SUCCESSEDED&quot; at.js per evitare errori. Se l’artefatto è molto grande, tutte le &quot;posizioni&quot; che utilizzano questo approccio vengono riprodotte solo dopo che l’artefatto è stato completamente scaricato, ritardando così il rendering dell’esperienza. [!DNL Adobe] consiglia di utilizzare raramente questo approccio. Segui le best practice per ridurre le dimensioni degli artefatti nella sezione sulle best practice per i dispositivi, quando utilizzi questo approccio.
