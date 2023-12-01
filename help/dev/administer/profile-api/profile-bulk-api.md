---
title: API di aggiornamento del profilo bulk di Adobe Target
description: Scopri come utilizzare [!DNL Adobe Target] [!UICONTROL API di aggiornamento del profilo bulk] per inviare dati di profilo di più visitatori a [!DNL Target].
feature: APIs/SDKs
contributors: https://github.com/icaraps
source-git-commit: 6f7d9875e3b73352ead3a55e40a4b2f81f3d4400
workflow-type: tm+mt
source-wordcount: '690'
ht-degree: 9%

---

# [!DNL Adobe Target Bulk Profile Update API]

Il [!DNL Adobe Target] [!UICONTROL API di aggiornamento del profilo bulk] consente di aggiornare in blocco i profili utente di più visitatori a un sito web utilizzando un file batch.

Utilizzo di [!UICONTROL API di aggiornamento del profilo bulk], puoi inviare in modo comodo dati dettagliati del profilo del visitatore sotto forma di parametri di profilo per molti utenti a [!DNL Target] da qualsiasi origine esterna. Le origini esterne possono includere sistemi CRM (Customer Relationship Management) o POS (Point of Sale), che in genere non sono disponibili in una pagina Web.

| Versione | Esempio di URL | Funzioni |
| --- | --- | --- |
| v1 | `http://CLIENTCODE.tt.omtrdc.net/m2/ CLIENTCODE/profile/batchUpdate` | Supporto solo per l’aggiornamento in blocco dei profili. |
| v2 | `http://CLIENTCODE.tt.omtrdc.net/m2/ CLIENTCODE/v2/profile/batchUpdate` | <ul><li>Crea profilo se non trovato.</li><li>Aggiornamento dello stato per riga.</li></ul> |

>[!NOTE]
>
>Versione 2 (v2) del [!UICONTROL API di aggiornamento del profilo bulk] è la versione corrente. Tuttavia, [!DNL Target] supporta ancora la versione 1 (v1).

## Avvertenze

* La dimensione del file batch deve essere inferiore a 50 MB. Inoltre, il numero totale di righe non deve superare 500000 righe per upload.
* Non esiste alcun limite al numero o alle righe che è possibile caricare in un periodo di 24 ore nei batch successivi. Tuttavia, il processo di ingestione potrebbe essere limitato durante l&#39;orario di ufficio per garantire che altri processi vengano eseguiti in modo efficiente.
* Le chiamate all’aggiornamento collettivo v2 senza chiamate mbox tra gli stessi thirdPartyIds sovrascrivono le proprietà aggiornate nella prima chiamata di aggiornamento collettivo.

## File batch

Per aggiornare i dati del profilo in blocco, crea un file batch. Il file batch è un file di testo con valori separati da virgole simili al seguente file di esempio.

``````
batch=pcId, param1, param2, param3, param4 123, value1 124, value1,,, value4 125,, value2 126, value1, value2, value3, value4
``````

Fai riferimento a questo file nella chiamata POST a [!DNL Target] server per elaborare il file. Durante la creazione del file batch, tenere presente quanto segue:

* Nella prima riga del file devono essere specificate le intestazioni di colonna.
* La prima intestazione deve essere `pcId` o `thirdPartyId`. Il [!UICONTROL ID visitatore Marketing Cloud] non è supportato. [!UICONTROL pcId] è un [!DNL Target]ID visitatore generato da. `thirdPartyId` è un ID specificato dall’applicazione client e trasmesso a [!DNL Target] tramite una chiamata mbox come `mbox3rdPartyId`. Deve essere qui indicato come `thirdPartyId`.
* I parametri e i valori specificati nel file batch devono essere codificati tramite URL utilizzando UTF-8 per motivi di sicurezza. I parametri e i valori possono essere inoltrati ad altri nodi edge per l’elaborazione tramite richieste HTTP.
* I parametri devono essere nel formato `paramName` solo. I parametri vengono visualizzati in [!DNL Target] as `profile.paramName`.
* Se sta usando [!UICONTROL API di aggiornamento del profilo bulk] v2, non è necessario specificare tutti i valori dei parametri per ciascuno `pcId`. I profili vengono creati per qualsiasi `pcId` o `mbox3rdPartyId` che non si trova in [!DNL Target]. Se utilizzi v1, i profili non vengono creati per pcIds o mbox3rdPartyIds mancanti.
* La dimensione del file batch deve essere inferiore a 50 MB. Inoltre, il numero totale di righe non deve superare 500,000. Questo limite assicura che i server non vengano inondati da troppe richieste.
* Puoi inviare più file. Tuttavia, la somma totale delle righe di tutti i file inviati in un giorno non deve superare un milione per ogni client.
* Non esiste alcun limite al numero di attributi caricati. Tuttavia, la dimensione complessiva di un profilo, inclusi i dati di sistema, non deve superare i 2000 KB. [!DNL Adobe] consiglia di utilizzare meno di 1000 KB di spazio di archiviazione per gli attributi del profilo.
* I parametri e i valori fanno distinzione tra maiuscole e minuscole.

## richiesta HTTP POST

Effettuare una richiesta HTTP POST a [!DNL Target] server perimetrali per elaborare il file. Di seguito è riportato un esempio di richiesta HTTP POST per il file batch.txt utilizzando il comando curl:

``````
curl -X POST --data-binary @BATCH.TXT http://CLIENTCODE.tt.omtrdc.net/m2/ CLIENTCODE/v2/profile/batchUpdate
``````

Dove:

BATCH.TXT è il nome del file. CLIENTCODE è il codice [!DNL Target] codice client.

Se non conosci il tuo codice cliente, nel [!DNL Target] clic interfaccia utente **[!UICONTROL Amministrazione]** > **[!UICONTROL Implementazione]**. Il codice client viene visualizzato nel [!UICONTROL Dettagli account] sezione.

### Inspect la risposta

v2 restituisce uno stato profilo per profilo e v1 restituisce solo lo stato complessivo. La risposta include un collegamento a un URL diverso con il messaggio di successo profilo per profilo.

### Esempio di risposta

```
true http://mboxedge19.tt.omtrdc.net/m2/demo/v2/profile/batchStatus?batchId=demo-1845664501&m2Node=00 Batch submitted for processing
```

In caso di errore, la risposta contiene `success=false` e un messaggio dettagliato dell’errore.

Una risposta corretta si presenta come segue:

``````
demo-1845664501 1436187396849-250353.03_03 success 2403081156529-351655.03_03 success 2403081156529-351656.03_03 success 1436187396849-250351.01_00 success 
``````

I valori previsti per i campi di stato sono:

**success**: profilo aggiornato. Se il profilo non è stato trovato, ne è stato creato uno con i valori del batch.
**errore**: il profilo non è stato aggiornato o creato a causa di un errore, un’eccezione o una perdita di messaggi.
**in sospeso**: il profilo non è ancora stato aggiornato o creato.



