---
title: Comprendere l’artefatto della regola di decisioning sul dispositivo
description: Scopri come utilizzare l'artefatto della regola, una rappresentazione JSON delle attività di [!DNL Adobe Target] [!UICONTROL on-device decisioning].
feature: APIs/SDKs
exl-id: 3dfb08df-eaa9-43d4-b009-e5f64c3a96d7
TQID: https://experienceleague.adobe.com/mPzCK-vBYFAQnslX-8FPsBaeSiYtyxjZv76anbpHWuE
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 278
ht-degree: 0%

---

# Panoramica dell’artefatto della regola

L&#39;artefatto della regola è una rappresentazione JSON delle attività [!DNL Adobe Target] [!UICONTROL on-device decisioning]. Viene generato da [!DNL Adobe Target] e propagato alla rete CDN Akamai per verificare che sia disponibile un artefatto di regola il più vicino possibile agli utenti finali. Contiene metadati che garantiscono l’esecuzione e la consegna precise delle attività, consentendo al contempo l’analisi in tempo reale tramite il tracciamento degli eventi. Gli SDK [!DNL Adobe Target] possono essere configurati in modo da consentire la gestione automatica dell&#39;artefatto della regola, tramite la quale possono essere scaricati o aggiornati in base a un intervallo di tempo specificato dall&#39;utente. Inoltre, puoi anche gestire la tua copia locale dell&#39;artefatto della regola utilizzando un sistema di caching della memoria distribuita come [Memcached](https://memcached.org/) per inizializzare il SDK [!DNL Adobe Target], in modo che i server senza stato possano servire le richieste immediatamente. Per ulteriori informazioni su queste opzioni, consulta le seguenti guide:

* [Download, archiviazione e aggiornamento automatico dell&#39;artifact della regola tramite  [!DNL Adobe Target] SDK](rule-artifact-sdk.md)
* [Download, archiviazione e aggiornamento dell’artefatto della regola tramite payload JSON](rule-artifact-json.md)

## Esempio di artefatto della regola

Fai clic qui per un esempio dell&#39;[artefatto regola](rule-artifact-example.md).

## Come visualizzare l’artefatto della regola per il client

L&#39;abilitazione delle tracce genererà ulteriori informazioni da [!DNL Adobe Target] per quanto riguarda l&#39;artefatto della regola, in particolare l&#39;URL.

1. Passa all’interfaccia utente di Target.

   &lt;!— Insert image-target-ui-1.png —>
   ![Alt immagine](assets/asset-rule-artifact-1.png)

1. Passare a **[!UICONTROL Administration]** > **[!UICONTROL Implementation]** e fare clic su **[!UICONTROL Generate New Authorization Token]**.

   &lt;!— Insert image-target-ui-2.png —>
   ![Alt immagine](assets/asset-rule-artifact-2.png)

1. Copia il token di autorizzazione appena generato negli Appunti e aggiungilo alla richiesta Target.

   ```javascript {line-numbers="true"}
   const request = {
     trace: {
       authorizationToken: '88f1a924-6bc5-4836-8560-2f9c86aeb36b'
     },
     execute: {
       mboxes: [{
         address: getAddress(req),
         name: "node-sdk-mbox"
       }]
   }};
   ```

1. Trasmetti la traccia di Target tramite il terminale per visualizzare i dettagli dell’artefatto. URL accessibile tramite la variabile `artifactLocation`.

   ```
   "trace": {
     "clientCode": "your-client-code",
     "artifact": {
       "artifactLocation": "https://assets.adobetarget.com/your-client-code/production/v1/rules.bin",
       "pollingInterval": 300000,
       "pollingHalted": false,
       "artifactVersion": "1.0.0",
       "artifactRetrievalCount": 10,
       "artifactLastRetrieved": "2020-09-20T00:09:42.707Z",
        "clientCode": "your-client-code",
      "environment": "production",
       "generatedAt": "2020-09-22T17:17:59.783Z"
     },
   ```
