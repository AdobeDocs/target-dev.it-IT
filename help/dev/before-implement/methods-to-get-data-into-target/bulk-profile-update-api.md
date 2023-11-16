---
keywords: implementare, implementare, configurare, configurare, aggiornare il profilo in blocco
description: Inserire dati in [!DNL Target] utilizzando l’API per l’aggiornamento collettivo dei profili.
title: Come posso inserire dati in [!DNL Target] Utilizzare l’API di aggiornamento del profilo bulk?
feature: Implementation
exl-id: 654b13b7-1683-4c44-80e6-7557b9d29f66
source-git-commit: 773f8a2f22cfbf740a5ce68809b38b33b621f3b5
workflow-type: tm+mt
source-wordcount: '404'
ht-degree: 59%

---

# API di aggiornamento del profilo bulk

Tramite API, invia un file .csv a [!DNL Adobe Target] con aggiornamenti del profilo visitatore per molti visitatori. Ogni profilo visitatore può essere aggiornato con più attributi di profilo nella pagina in una chiamata.

Questa opzione è simile a Attributi del cliente e presenta alcune differenze:

* Attributi del cliente utilizzano un caricamento FTP mentre il [!UICONTROL API di aggiornamento del profilo bulk di Target] utilizza un’API HTTP POST.
* I dati degli attributi dei clienti possono essere condivisi con [!DNL Analytics]. [!UICONTROL L&#39;aggiornamento del profilo bulk è utilizzabile solo in Target.]
* Supporto degli attributi del cliente per la creazione di un profilo per un utente [!DNL Target] non ha ancora visto. L’API di aggiornamento del profilo bulk è aggiornata se esistente [!DNL Target] solo profili.
* Gli Attributi del cliente richiedono l’utilizzo dell’ID Experience Cloud (ECID) e l’utilizzo di un ID sorgente, ad esempio l’ID del sistema di gestione delle relazioni con i clienti o l’ID fedeltà.
* L&#39;API di aggiornamento del profilo bulk richiede l&#39;ID TNT o `mbox3rdPartyId`.
* Non puoi inviare i seguenti caratteri in `mbox3rdPartyID`: segno più (+) e barra (/).

## Formato

Il file .csv deve fare riferimento a ogni visitatore tramite il proprio [!DNL Target] PCID o `mbox3rdPartyId`. Experience Cloud ID (ECID) non è supportato. Tutti gli attributi e valori del profilo vengono creati e aggiornati tramite l&#39;API. I dettagli del formato sono disponibili nella documentazione API.

## Casi d’uso di esempio

Il CRM o altro sistema interno archivia dati importanti relativi ai visitatori che si desidera aggiornare costantemente in Target, senza esporre i dati del profilo nell&#39;implementazione della pagina.

## Vantaggi del metodo

Nessun limite al numero di attributi del profilo.

Gli attributi del profilo inviati tramite il sito possono essere aggiornati tramite l&#39;API e viceversa.

## Avvertenze

La dimensione del file batch deve essere inferiore a 50 MB. Inoltre, il numero totale di righe non deve superare 500000 righe per upload.

Gli aggiornamenti in genere si verificano in meno di un’ora, ma la visualizzazione può richiedere fino a 24 ore.

Non esiste alcun limite al numero o alle righe che è possibile caricare per un periodo di 24 ore nei batch successivi. Tuttavia, il processo di ingestione potrebbe essere limitato durante l&#39;orario di ufficio per garantire che altri processi vengano eseguiti in modo efficiente.

Le [chiamate all’aggiornamento collettivo V2](https://developers.adobetarget.com/api/#updating-profiles) senza chiamate mbox tra gli stessi thirdPartyIds sovrascrivono le proprietà aggiornate nella prima chiamata di aggiornamento collettivo.

## Esempi di codice

Consulta [Aggiornamento di profili](https://developers.adobetarget.com/api/#updating-profiles).

### Collegamenti alle informazioni rilevanti

[Aggiornamento di profili](https://developers.adobetarget.com/api/#updating-profiles)
