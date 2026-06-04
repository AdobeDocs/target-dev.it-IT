---
title: API di aggiornamento a profilo singolo di Adobe Target
description: Scopri come utilizzare l'API  [!DNL Adobe Target] [!UICONTROL Aggiornamento singolo profilo] per inviare i dati del profilo di un singolo visitatore a [!DNL Target].
feature: APIs/SDKs
contributors: https://github.com/icaraps
exl-id: 4e022db3-215f-461b-9222-38ce2f2dbc28
TQID: https://experienceleague.adobe.com/HEjGkrgixufe9wQvaPAljSlZRSaF-idgwKYWs3cuoJ0
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 6fac79420aef0a73c109b2c19f363266c1f8027a
workflow-type: tm+mt
source-wordcount: 396
ht-degree: 4%

---

# [!DNL Adobe Target Single Profile Update API]

L&#39;API [!DNL Adobe Target] [!UICONTROL Aggiornamento profilo singolo] consente di inviare un aggiornamento profilo per un singolo utente. L&#39;API [!UICONTROL Aggiornamento profilo singolo] è quasi identica all&#39;API [!UICONTROL Aggiornamento profilo bulk], ma un profilo visitatore viene aggiornato alla volta, in linea con la chiamata API invece che con un file .cvs.

L&#39;API [!UICONTROL Aggiornamento profilo singolo] e viene generalmente utilizzata quando deve verificarsi un aggiornamento in relazione a una transazione che si verifica in un canale che non ha implementato [!DNL Target]. Ad esempio, desideri aggiornare il profilo di un singolo visitatore che esegue un’azione offline. Le azioni possono includere il raggiungimento di un call center, il finanziamento di un prestito, l’utilizzo di una carta fedeltà in negozio, l’accesso a un chiosco e così via.

I vantaggi dell&#39;[!UICONTROL API di aggiornamento a profilo singolo] includono:

* Nessun limite al numero di attributi del profilo.
* Gli attributi del profilo inviati tramite il sito possono essere aggiornati tramite l’API e in modo opposto.

## Avvertenze

* L&#39;API [!UICONTROL Single Profile Update] è limitata all&#39;esecuzione di 1 milione di aggiornamenti in qualsiasi periodo continuo di 24 ore.
* Gli aggiornamenti in genere si verificano in meno di un’ora, ma la visualizzazione potrebbe richiedere fino a 24 ore.

  Se devi inviare altri aggiornamenti o richiedere l&#39;elaborazione degli aggiornamenti in tempi più brevi, puoi inviare gli aggiornamenti del profilo transazionale tramite l&#39;aggiornamento lato client (preferito) o tramite l&#39;[!DNL Adobe Target] [API di consegna](/help/dev/implement/delivery-api/overview.md) lato server.

* L&#39;[!UICONTROL API di aggiornamento a profilo singolo] è un&#39;API server-to-server e non è progettata per funzionare all&#39;interno di una pagina Web. Per aggiornare un profilo visitatore dalla pagina Web, puoi utilizzare la funzione [trackEvent()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md) o l&#39;[API di consegna](/help/dev/implement/delivery-api/overview.md).

## Formato

Specificare i parametri del profilo nel formato `profile.paramName=value`.

Per aggiornare il profilo per un `pcId`, utilizzare:

```
https://<your-client-code>.tt.omtrdc.net/m2/client/profile/update?mboxPC=1368007744041-575948.01_00&profile.attr=0&profile.attr2=1...
```

Per aggiornare il profilo per un `mbox3rdPartyId`, utilizzare:

```
shell http://<your-client-code>.tt.omtrdc.net/m2/client/profile/update?mbox3rdPartyId=123456&profile.attr=0&profile.attr2=1...
```

L&#39;API [!UICONTROL Aggiornamento profilo singolo] è solo per aggiornamenti. Se non viene trovato nulla, non viene creato alcun profilo.

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
