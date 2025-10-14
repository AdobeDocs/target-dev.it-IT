---
title: Comprendere l’artefatto della regola di decisioning sul dispositivo
description: Scopri come utilizzare l'artefatto della regola, una rappresentazione JSON delle attività di [!DNL Adobe Target] [!UICONTROL on-device decisioning].
feature: APIs/SDKs
exl-id: 3dfb08df-eaa9-43d4-b009-e5f64c3a96d7
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 0%

---

# Panoramica dell’artefatto della regola

L&#39;artefatto della regola è una rappresentazione JSON delle attività [!DNL Adobe Target] [!UICONTROL on-device decisioning]. Viene generato da [!DNL Adobe Target] e propagato alla rete CDN Akamai per verificare che sia disponibile un artefatto di regola il più vicino possibile agli utenti finali. Contiene metadati che garantiscono l’esecuzione e la consegna precise delle attività, consentendo al contempo l’analisi in tempo reale tramite il tracciamento degli eventi. Gli SDK [!DNL Adobe Target] possono essere configurati in modo da consentire la gestione automatica dell&#39;artefatto della regola, tramite la quale possono essere scaricati o aggiornati in base a un intervallo di tempo specificato dall&#39;utente. Inoltre, puoi anche gestire la tua copia locale dell&#39;artefatto della regola utilizzando un sistema di caching della memoria distribuita come [Memcached](https://memcached.org/) per inizializzare l&#39;SDK [!DNL Adobe Target], in modo che i server senza stato possano servire le richieste immediatamente. Per ulteriori informazioni su queste opzioni, consulta le seguenti guide:

* [Download, archiviazione e aggiornamento automatico dell&#39;artifact della regola tramite l&#39;SDK  [!DNL Adobe Target] &#x200B;](rule-artifact-sdk.md)
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
