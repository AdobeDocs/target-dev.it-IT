---
title: Come gestire i criteri personalizzati
description: Passaggi necessari per utilizzare le API di Adobe Target per gestire, creare, elencare, modificare, ottenere ed eliminare i criteri di Adobe Target Recommendations.
feature: APIs/SDKs, Recommendations, Administration & Configuration
kt: 3815
thumbnail: null
author: Judy Kim
exl-id: 51a67a49-a92d-4377-9a9f-27116e011ab1
source-git-commit: 2fba03b3882fd23a16342eaab9406ae4491c9044
workflow-type: tm+mt
source-wordcount: '869'
ht-degree: 0%

---

# Gestisci criteri personalizzati

A volte gli algoritmi forniti da Recommendations non sono in grado di individuare elementi particolari che desideri promuovere. In questa situazione, i criteri personalizzati consentono di fornire un set specifico di articoli consigliati per un determinato elemento o categoria chiave.

Per creare criteri personalizzati, definisci e importa la mappatura desiderata tra l’elemento o la categoria chiave e gli elementi consigliati. Questo processo è descritto nella [documentazione sui criteri personalizzati](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/recommendations-csv.html). Come indicato in tale documentazione, puoi creare, modificare ed eliminare i criteri personalizzati tramite l’interfaccia utente di Target. Tuttavia, Target fornisce anche un set di API di criteri personalizzati che consentono una gestione più dettagliata dei criteri personalizzati.

>[!WARNING]
>
>Per i criteri personalizzati, esegui tutte le azioni (crea, modifica, elimina) per un dato criterio personalizzato utilizzando le API, oppure tutte le azioni (crea, modifica, elimina) utilizzando l’interfaccia utente. La gestione dei criteri personalizzati tramite una combinazione di interfaccia utente e API può causare conflitti tra le informazioni o risultati imprevisti. Ad esempio, la creazione di un criterio personalizzato nell’interfaccia utente ma la successiva modifica tramite API non rifletterà gli aggiornamenti nell’interfaccia utente, anche se verrà aggiornato nel back-end, come visibile tramite l’API.

## Creare criteri personalizzati

Per creare criteri personalizzati utilizzando l&#39;API [Crea criteri personalizzati](https://developer.adobe.com/target/administer/recommendations-api/#operation/createCriteriaCustom), la sintassi è:

`POST https://mc.adobe.io/{{TENANT_ID}}/target/recs/criteria/custom`

>[!WARNING]
>
>I criteri personalizzati creati utilizzando l’API Crea criteri personalizzati, come descritto in questo esercizio, verranno visualizzati nell’interfaccia utente e persisteranno. Non potrai modificarli o eliminarli dall’interfaccia utente. Puoi modificarli o eliminarli **tramite API**, ma in entrambi i casi continueranno a essere visualizzati nell&#39;interfaccia utente di Target. Per mantenere l&#39;opzione di modifica o eliminazione dall&#39;interfaccia utente, crea i criteri personalizzati utilizzando l&#39;interfaccia utente per [la documentazione](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/recommendations-csv.html), anziché utilizzare l&#39;API Crea criteri personalizzati.

Procedi solo con i seguenti passaggi dopo aver letto l’avviso precedente e hai familiarità con la creazione di nuovi criteri personalizzati che non possono essere successivamente eliminati dall’interfaccia utente.

1. Verificare che `TENANT_ID` e `API_KEY` per **[!UICONTROL Create custom criteria]** facciano riferimento alle variabili di ambiente Postman stabilite in precedenza. Utilizza l’immagine seguente per il confronto.

   ![CreaCriteriPersonalizzati1](assets/CreateCustomCriteria1.png)

1. Aggiungi **Body** come **raw** JSON che definisce la posizione del file CSV dei criteri personalizzato. Utilizza come modello l&#39;esempio fornito nella documentazione di [Crea API per criteri personalizzati](https://developer.adobe.com/target/administer/recommendations-api/#operation/getAllCriteriaCustom), fornendo `environmentId` e altri valori, a seconda delle necessità. In questo esempio viene utilizzato LAST_PURCHASED come chiave.

   ![CreaCriteriPersonalizzati2](assets/CreateCustomCriteria2.png)

1. Invia la richiesta e osserva la risposta, che contiene i dettagli dei criteri personalizzati appena creati.

   ![CreaCriteriPersonalizzati3](assets/CreateCustomCriteria3.png)

1. Per verificare che i criteri personalizzati siano stati creati, passa a **[!UICONTROL Recommendations > Criteria]** all&#39;interno di Adobe Target e cerca i criteri in base al nome, oppure utilizza **[!UICONTROL List Custom Criteria API]** nel passaggio successivo.

   ![CreaCriteriPersonalizzati4](assets/CreateCustomCriteria4.png)

In questo caso, abbiamo un errore. Esaminiamo l&#39;errore esaminando più attentamente i criteri personalizzati, utilizzando **[!UICONTROL List Custom Criteria API]**.

## Elencare criteri personalizzati

Per recuperare un elenco di tutti i criteri personalizzati con i relativi dettagli, utilizzare l&#39;[Elenco criteri personalizzati API](https://developer.adobe.com/target/administer/recommendations-api/#operation/getAllCriteriaCustom). La sintassi è:

`GET https://mc.adobe.io/{{TENANT_ID}}/target/recs/criteria/custom`

1. Verificare `TENANT_ID` e `API_KEY` come prima e inviare la richiesta. Nella risposta, prendi nota dell’ID del criterio personalizzato, nonché dei dettagli relativi al messaggio di errore annotato in precedenza.
   ![ListCustomCriteria](assets/ListCustomCriteria.png)

In questo caso, l’errore si è verificato perché le informazioni sul server non sono corrette, il che significa che Target non è in grado di accedere al file CSV contenente la definizione dei criteri personalizzati. Modifichiamo i criteri personalizzati per correggere questo problema.

## Modifica criteri personalizzati

Per modificare i dettagli di una definizione di criteri personalizzati, utilizzare [Modifica API criteri personalizzati](https://developer.adobe.com/target/administer/recommendations-api/#operation/updateCriteriaCustom). La sintassi è:

`POST https://mc.adobe.io/{{TENANT_ID}}/target/recs/criteria/custom/:criteriaId`

1. Verificare `TENANT_ID` e `API_KEY`, come in precedenza.
   ![ModificaCriteriPersonalizzati1](assets/EditCustomCriteria1.png)

1. Specifica l’ID del criterio personalizzato (singolo) che desideri modificare.
   ![ModificaCriteriPersonalizzati2](assets/EditCustomCriteria2.png)

1. Nel corpo, fornisci il JSON aggiornato con le informazioni corrette sul server. Per questo passaggio, specifica l’accesso FTP a un server a cui puoi accedere.
   ![ModificaCriteriPersonalizzati3](assets/EditCustomCriteria3.png)

1. Invia la richiesta e annota la risposta.
   ![ModificaCriteriPersonalizzati4](assets/EditCustomCriteria4.png)

Verifichiamo il successo dei criteri personalizzati aggiornati, utilizzando **[!UICONTROL Get Custom Criteria API]**.

## Ottieni criteri personalizzati

Per visualizzare i dettagli dei criteri personalizzati per un criterio personalizzato specifico, utilizzare [Ottieni API criteri personalizzati](https://developer.adobe.com/target/administer/recommendations-api/#operation/getCriteriaCustom). La sintassi è:

`GET https://mc.adobe.io/{{TENANT_ID}}/target/recs/criteria/custom/:criteriaId`

1. Specifica l’ID del criterio personalizzato di cui desideri ottenere i dettagli. Invia la richiesta e rivedi la risposta.
   ![GetCustomCriteria.png](assets/GetCustomCriteria.png)
1. Verifica il completamento. Nel nostro caso, verifica che non vi siano altri errori FTP.
   ![GetCustomCriteria1.png](assets/GetCustomCriteria1.png)
1. (Facoltativo) Verifica che l’aggiornamento rifletta accuratamente l’interfaccia utente.
   ![GetCustomCriteria2.png](assets/GetCustomCriteria2.png)

## Elimina criteri personalizzati

Utilizzando l&#39;ID criteri indicato in precedenza, elimina i criteri personalizzati utilizzando l&#39;[Elimina API criteri personalizzati](https://developer.adobe.com/target/administer/recommendations-api/#operation/deleteCriteriaCustom). La sintassi è:

`DELETE https://mc.adobe.io/{{TENANT_ID}}/target/recs/criteria/custom/:criteriaId`

1. Specifica l’ID del criterio personalizzato (singolo) da eliminare. Fare clic su **[!UICONTROL Send]**.
   ![EliminaCriteriPersonalizzati1](assets/DeleteCustomCriteria1.png)

1. Verifica che i criteri siano stati eliminati utilizzando Ottieni criteri personalizzati.
   ![EliminaCriteriPersonalizzati2](assets/DeleteCustomCriteria2.png)
In questo caso, l’errore 404 previsto indica che non è possibile trovare i criteri eliminati.

>[!NOTE]
>
>Come promemoria, i criteri non verranno rimossi dall’interfaccia utente di Target anche se sono stati eliminati, perché sono stati creati utilizzando l’API Create Custom Criteria.

Congratulazioni! Ora puoi creare, elencare, modificare, eliminare e ottenere dettagli sui criteri personalizzati, utilizzando l’API Recommendations. Nella sezione successiva, utilizzerai l’API di consegna di Target per recuperare i consigli.

&lt;!— [Avanti &quot;Recupera Recommendations con l&#39;API di distribuzione lato server&quot; >](fetch-recs-server-side-delivery-api.md) —>
