---
title: API di aggiornamento a profilo singolo di Adobe Target
description: Scopri come utilizzare [!DNL Adobe Target] [!UICONTROL API di aggiornamento a profilo singolo] per inviare i dati del profilo di un singolo visitatore a [!DNL Target].
feature: APIs/SDKs
contributors: https://github.com/icaraps
source-git-commit: 81bff85a9d1fe28ca267c471a470da95568fd06d
workflow-type: tm+mt
source-wordcount: '349'
ht-degree: 4%

---

# [!DNL Adobe Target Single Profile Update API]

Il [!DNL Adobe Target] [!UICONTROL API di aggiornamento a profilo singolo] consente di inviare un aggiornamento del profilo per un singolo utente. Il [!UICONTROL API di aggiornamento a profilo singolo] è quasi identico al [!UICONTROL API di aggiornamento del profilo bulk], ma un profilo visitatore viene aggiornato alla volta, in linea con la chiamata API invece che con un file .cvs.

Il [!UICONTROL API di aggiornamento a profilo singolo] ed è generalmente utilizzato quando si deve verificare un aggiornamento in relazione a una transazione che si verifica in un canale che non è stato implementato [!DNL Target]. Ad esempio, desideri aggiornare il profilo di un singolo visitatore che esegue un’azione offline. Le azioni possono includere il raggiungimento di un call center, il finanziamento di un prestito, l’utilizzo di una carta fedeltà in negozio, l’accesso a un chiosco e così via.

Vantaggi della [!UICONTROL API di aggiornamento a profilo singolo] include:

* Nessun limite al numero di attributi del profilo.
* Gli attributi del profilo inviati tramite il sito possono essere aggiornati tramite l’API e in modo opposto.

## Avvertenze

* Il [!UICONTROL API di aggiornamento a profilo singolo] è limitato all’esecuzione di 1 milione di aggiornamenti in qualsiasi periodo continuo di 24 ore.
* Gli aggiornamenti in genere si verificano in meno di un’ora, ma la visualizzazione potrebbe richiedere fino a 24 ore.

  Se devi inviare più aggiornamenti o richiedere l’elaborazione degli aggiornamenti in tempi più brevi, puoi inviare gli aggiornamenti del profilo transazionale tramite aggiornamento lato client (opzione preferita) o tramite [!DNL Adobe Target] lato server [API di consegna](/help/dev/implement/delivery-api/overview.md).

## Formato

Specificare i parametri del profilo nel formato `profile.paramName=value`.

Per aggiornare il profilo per un `pcId`, utilizza:

``````
https://<your-client-code>.tt.omtrdc.net/m2/client/profile/update?mboxPC=1368007744041-575948.01_00&profile.attr=0&profile.attr2=1...
``````

Per aggiornare il profilo per un `mbox3rdPartyId`, utilizza:

``````
shell http://<your-client-code>.tt.omtrdc.net/m2/client/profile/update?mbox3rdPartyId=123456&profile.attr=0&profile.attr2=1...
``````

Il [!UICONTROL API di aggiornamento a profilo singolo] è solo per aggiornamenti. Se non viene trovato nulla, non viene creato alcun profilo.

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