---
keywords: implementazione, libreria javascript, js, atjs, decisioning sul dispositivo, decisioning sul dispositivo, at.js, sul dispositivo, sul dispositivo, risoluzione dei problemi, risoluzione dei problemi, implementazione2
description: Scopri come risolvere i problemi di [!UICONTROL decisioning sul dispositivo] con la libreria at.js.
title: Come posso risolvere i problemi relativi al decisioning sul dispositivo con la libreria JavaScript at.js?
feature: at.js
exl-id: b9530cc7-5e83-4fdf-bde9-b2492e0861ff
TQID: https://experienceleague.adobe.com/Ji3jAHC0Ek7FrVnabEEMm-KCtxJLJ5rSz4uyi6sWpiE
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2: id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: 235baadf4059d2c363368408012630d6619aef99
workflow-type: tm+mt
source-wordcount: 281
ht-degree: 0%

---

# Risoluzione dei problemi di [!UICONTROL decisioning sul dispositivo] per at.js

Completa i seguenti passaggi per risolvere i problemi di [!UICONTROL decisioning sul dispositivo] in [!UICONTROL Adobe Target] con la libreria JavaScript at.js:

## Passaggio 1: abilitare il registro della console per at.js

L&#39;aggiunta del parametro URL `mboxDebug=1` consente ad at.js di stampare messaggi nella console del browser.

Tutti i messaggi contengono il prefisso &quot;AT:&quot; per una comoda panoramica. Per garantire che un artefatto sia stato caricato correttamente, il registro della console deve contenere messaggi simili ai seguenti:

```
AT: LD.ArtifactProvider fetching artifact - https://assets.adobetarget.com/your-client-cide/production/v1/rules.json
AT: LD.ArtifactProvider artifact received - status=200
```

La figura seguente mostra questi messaggi nel registro della console:

(Fare clic sull&#39;immagine per espanderla a larghezza intera.)

![Registro console con messaggi artifact](/help/dev/implement/client-side/atjs/on-device-decisioning/assets/browser-console.png "Registro console con messaggi artifact"){zoomable="yes"}

## Passaggio 2: verifica il download dell’artefatto della regola nella scheda Rete del browser

Apri la scheda Rete del browser.

Ad esempio, per aprire DevTools in Google Chrome:

1. Premere Ctrl+Maiusc+J (Windows) o Comando+Opzione+J (Mac).
1. Passare alla scheda Rete.
1. Filtra le chiamate per parola chiave &quot;rules.json&quot; per assicurarti che venga visualizzato solo il file delle regole dell’artefatto.

   Inoltre, puoi filtrare per &quot;/delivery|rules.json/&quot; per visualizzare tutte le chiamate di Target e l’artefatto rules.json.

   ![Scheda Rete in Google Chrome](assets/rule-json.png)

## Passaggio 3: verifica il download dell’artefatto della regola utilizzando gli eventi personalizzati di at.js

La libreria at.js invia due nuovi eventi personalizzati per supportare [!UICONTROL le decisioni sul dispositivo].

* `adobe.target.event.ARTIFACT_DOWNLOAD_SUCCEEDED`
* `adobe.target.event.ARTIFACT_DOWNLOAD_FAILED`

Puoi abbonarti per ascoltare questi eventi personalizzati nell’applicazione e intervenire in caso di esito positivo o negativo del download del file delle regole dell’artefatto.

L’esempio seguente mostra un esempio di codice in ascolto degli eventi di esito positivo e negativo del download degli artefatti:

```javascript {line-numbers="true"}
document.addEventListener(adobe.target.event.ARTIFACT_DOWNLOAD_SUCCEEDED, function(e) { 
  console.log("Artifact successfully downloaded", e.detail);
}, false);

document.addEventListener(adobe.target.event.ARTIFACT_DOWNLOAD_FAILED, function(e) { 
  console.log("Artifact failed to download", e.detail);
}, false);
```



