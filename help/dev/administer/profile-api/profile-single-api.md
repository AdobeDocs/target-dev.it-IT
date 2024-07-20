---
title: API di aggiornamento a profilo singolo di Adobe Target
description: Scopri come utilizzare  [!DNL Adobe Target] [!UICONTROL Single Profile Update API] per inviare i dati del profilo di un singolo visitatore a  [!DNL Target].
feature: APIs/SDKs
contributors: https://github.com/icaraps
exl-id: 4e022db3-215f-461b-9222-38ce2f2dbc28
source-git-commit: e2462d12cf58ab5a588c13a96df5e6abafb9d675
workflow-type: tm+mt
source-wordcount: '352'
ht-degree: 3%

---

# [!DNL Adobe Target Single Profile Update API]

[!DNL Adobe Target] [!UICONTROL Single Profile Update API] ti consente di inviare un aggiornamento del profilo per un singolo utente. [!UICONTROL Single Profile Update API] è quasi identico a [!UICONTROL Bulk Profile Update API], ma un profilo visitatore viene aggiornato alla volta, in linea con la chiamata API invece che con un file .cvs.

[!UICONTROL Single Profile Update API] e viene generalmente utilizzato quando deve verificarsi un aggiornamento in relazione a una transazione che si verifica in un canale che non ha implementato [!DNL Target]. Ad esempio, desideri aggiornare il profilo di un singolo visitatore che esegue un’azione offline. Le azioni possono includere il raggiungimento di un call center, il finanziamento di un prestito, l’utilizzo di una carta fedeltà in negozio, l’accesso a un chiosco e così via.

I vantaggi di [!UICONTROL Single Profile Update API] includono:

* Nessun limite al numero di attributi del profilo.
* Gli attributi del profilo inviati tramite il sito possono essere aggiornati tramite l’API e in modo opposto.

## Avvertenze

* [!UICONTROL Single Profile Update API] è limitato all&#39;esecuzione di 1 milione di aggiornamenti in qualsiasi periodo continuo di 24 ore.
* Gli aggiornamenti in genere si verificano in meno di un’ora, ma la visualizzazione potrebbe richiedere fino a 24 ore.

  Se devi inviare altri aggiornamenti o richiedere l&#39;elaborazione degli aggiornamenti in tempi più brevi, puoi inviare gli aggiornamenti del profilo transazionale tramite l&#39;aggiornamento lato client (preferito) o tramite l&#39;[!DNL Adobe Target] [API di consegna](/help/dev/implement/delivery-api/overview.md) lato server.

* [!UICONTROL Single Profile Update API] è un&#39;API server-to-server e non è progettato per funzionare all&#39;interno di una pagina Web. Per aggiornare un profilo visitatore dalla pagina Web, puoi utilizzare la funzione [trackEvent()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md) o l&#39;[API di consegna](/help/dev/implement/delivery-api/overview.md).

## Formato

Specificare i parametri del profilo nel formato `profile.paramName=value`.

Per aggiornare il profilo per un `pcId`, utilizzare:

``````
https://<your-client-code>.tt.omtrdc.net/m2/client/profile/update?mboxPC=1368007744041-575948.01_00&profile.attr=0&profile.attr2=1...
``````

Per aggiornare il profilo per un `mbox3rdPartyId`, utilizzare:

``````
shell http://<your-client-code>.tt.omtrdc.net/m2/client/profile/update?mbox3rdPartyId=123456&profile.attr=0&profile.attr2=1...
``````

[!UICONTROL Single Profile Update API] è solo per aggiornamenti. Se non viene trovato nulla, non viene creato alcun profilo.

## Note

* I parametri e i valori devono essere codificati in URL utilizzando UTF-8.
* Il formato del parametro è `profile.paramName`.
* Non tutti i valori dei parametri devono esistere per tutti i pcIds e mbox3rdPartyIds.
* I parametri e i valori fanno distinzione tra maiuscole e minuscole.
* Sono supportati sia GET che POST.
* Le attuali limitazioni di dimensione per il limite sono di 8 KB per GET e 60 KB per POST.

## Risposta

Un esempio di risposta per le richieste di cui sopra è simile al seguente:

`trueRequest successfully submitted`

Questa risposta indica che è stata inviata e verrà elaborata a breve.
