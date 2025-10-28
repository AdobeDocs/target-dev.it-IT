---
title: API di aggiornamento del profilo bulk di Adobe Target
description: Scopri come utilizzare  [!DNL Adobe Target] [!UICONTROL Bulk Profile Update API] per inviare i dati del profilo di più visitatori a  [!DNL Target]  per l'utilizzo nel targeting.
feature: APIs/SDKs
contributors: https://github.com/icaraps
exl-id: 0f38d109-5273-4f73-9488-80eca115d44d
source-git-commit: 38ed32560170e5a8f472aa191bb5a24d4e13cde7
workflow-type: tm+mt
source-wordcount: '1078'
ht-degree: 6%

---

# [!DNL Adobe Target Bulk Profile Update API]

[!DNL Adobe Target] [!UICONTROL Bulk Profile Update API] consente di aggiornare i profili utente per più visitatori a un sito Web in blocco utilizzando un file batch.

Utilizzando [!UICONTROL Bulk Profile Update API], puoi inviare in modo comodo dati dettagliati del profilo visitatore sotto forma di parametri di profilo per molti utenti a [!DNL Target] da qualsiasi origine esterna. Le fonti esterne possono includere sistemi CRM (Customer Relationship Management) o POS (Point of Sale), che di solito non sono disponibili su una pagina web.

| Versione | Esempio di URL | Funzioni |
| --- | --- | --- |
| v1 | `http://CLIENTCODE.tt.omtrdc.net/m2/CLIENTCODE/profile/batchUpdate` | Supporto solo per l’aggiornamento in blocco dei profili. |
| v2 | `http://CLIENTCODE.tt.omtrdc.net/m2/CLIENTCODE/v2/profile/batchUpdate` | <ul><li>Crea profilo se non trovato.</li><li>Aggiornamento dello stato per riga.</li></ul> |

>[!NOTE]
>
>La versione 2 (v2) di [!DNL Bulk Profile Update API] è la versione corrente. Tuttavia, [!DNL Target] continua a supportare la versione 1 (v1).
>
>* Se l&#39;implementazione di [!DNL Target] utilizza [!DNL Experience Cloud ID] (ECID) come identificatore di profilo per i visitatori anonimi, non utilizzare `pcId` come chiave in un file batch versione 2 (v2). L&#39;utilizzo di `pcId` con v2 di [!DNL Bulk Profile Update API] è destinato solo alle implementazioni autonome di [!DNL Target] che non si basano su ECID.
>
>* Se l&#39;implementazione utilizza ECID per l&#39;identificazione del profilo e si desidera utilizzare `pcId` come chiave nel file batch, utilizzare la versione 1 (v1) dell&#39;API.
>
>* Se l&#39;implementazione utilizza `thirdPartyId` per l&#39;identificazione del profilo, utilizza la versione 2 (v2) dell&#39;API con `thirdPartyId` come chiave.

## Vantaggi di [!UICONTROL Bulk Profile Update API]

* Nessun limite al numero di attributi del profilo.
* Gli attributi del profilo inviati tramite il sito possono essere aggiornati tramite l’API e in modo opposto.

## Avvertenze

* La dimensione del file batch deve essere inferiore a 50 MB. Inoltre, il numero totale di righe non deve superare 500000 righe per upload.
* Gli aggiornamenti in genere si verificano in meno di un’ora, ma la visualizzazione potrebbe richiedere fino a 24 ore.
* Non esiste alcun limite al numero o alle righe che è possibile caricare in un periodo di 24 ore nei batch successivi. Tuttavia, il processo di ingestione potrebbe essere limitato durante l&#39;orario di ufficio per garantire che altri processi vengano eseguiti in modo efficiente.
* Le chiamate all’aggiornamento collettivo v2 senza chiamate mbox tra gli stessi thirdPartyIds sovrascrivono le proprietà aggiornate nella prima chiamata di aggiornamento collettivo.
* [!DNL Adobe] non garantisce che il 100% dei dati del profilo batch sia integrato e mantenuto in Target e sia quindi disponibile per l&#39;utilizzo nel targeting. Nella progettazione corrente, esiste la possibilità che una piccola percentuale di dati (fino allo 0,1% dei batch di produzione di grandi dimensioni) non venga caricata o conservata.

## File batch

Per aggiornare i dati del profilo in blocco, crea un file batch. Il file batch è un file di testo con valori separati da virgole simili al seguente file di esempio.

``` ```
batch=pcId,param1,param2,param3,param4
123,value1
124,value1,,,value4
125,,value2
126,value1,value2,value3,value4
``` ```

>[!NOTE]
>
>Il parametro `batch=` è obbligatorio e deve essere specificato all&#39;inizio del file.

Si fa riferimento a questo file nella chiamata POST ai server [!DNL Target] per elaborare il file. Durante la creazione del file batch, tenere presente quanto segue:

* Nella prima riga del file devono essere specificate le intestazioni di colonna.
* La prima intestazione deve essere `pcId` o `thirdPartyId`. [!UICONTROL Marketing Cloud visitor ID] non è supportato. [!UICONTROL pcId] è un visitorID generato da [!DNL Target]. `thirdPartyId` è un ID specificato dall&#39;applicazione client, passato a [!DNL Target] tramite una chiamata mbox come `mbox3rdPartyId`. Deve essere indicato qui come `thirdPartyId`.
* I parametri e i valori specificati nel file batch devono essere codificati tramite URL utilizzando UTF-8 per motivi di sicurezza. I parametri e i valori possono essere inoltrati ad altri nodi edge per l’elaborazione tramite richieste HTTP.
* I parametri devono essere solo nel formato `paramName`. I parametri vengono visualizzati in [!DNL Target] come `profile.paramName`.
* Se si utilizza [!UICONTROL Bulk Profile Update API] v2, non è necessario specificare tutti i valori dei parametri per ogni `pcId`. I profili creati per qualsiasi `pcId` o `mbox3rdPartyId` non trovato in [!DNL Target]. Se utilizzi v1, i profili non vengono creati per pcIds o mbox3rdPartyIds mancanti.
* La dimensione del file batch deve essere inferiore a 50 MB. Inoltre, il numero totale di righe non deve superare 500,000. Questo limite assicura che i server non vengano inondati da troppe richieste.
* Puoi inviare più file. Tuttavia, la somma totale delle righe di tutti i file inviati in un giorno non deve superare un milione per ogni client.
* Non esiste alcuna restrizione sul numero di attributi che è possibile caricare. Tuttavia, la dimensione totale dei dati del profilo esterno, che include Attributi del cliente, API del profilo, parametri del profilo In-Mbox e output degli script di profilo, non deve superare i 64 KB.
* I parametri e i valori fanno distinzione tra maiuscole e minuscole.

## richiesta HTTP POST

Effettuare una richiesta HTTP POST ai server perimetrali [!DNL Target] per elaborare il file. Di seguito è riportato un esempio di richiesta HTTP POST per il file batch.txt utilizzando il comando curl:

``` ```
curl -X POST --data-binary @BATCH.TXT http://CLIENTCODE.tt.omtrdc.net/m2/CLIENTCODE/v2/profile/batchUpdate
``` ```

Dove:

BATCH.TXT è il nome del file. CLIENTCODE è il codice client [!DNL Target].

Se non conosci il tuo codice client, nell&#39;interfaccia utente di [!DNL Target] fai clic su **[!UICONTROL Administration]** > **[!UICONTROL Implementation]**. Il codice client è visualizzato nella sezione [!UICONTROL Account Details].

### Ispezionare la risposta

L’API Profiles restituisce lo stato di invio del batch per l’elaborazione insieme a un collegamento in &quot;batchStatus&quot; a un URL diverso che mostra lo stato generale di un particolare processo batch.

### Esempio di risposta API

Il seguente codice acquisito è un esempio di risposta API Profiles:

```
<response>
    <success>true</success>
    <batchStatus>http://mboxedge45.tt.omtrdc.net/m2/demo/profile/batchStatus?batchId=demo-1701473848678-13029383</batchStatus>
    <message>Batch submitted for processing</message>
</response>
```

Se si verifica un errore, la risposta contiene `success=false` e un messaggio dettagliato per l&#39;errore.

### Risposta di stato batch predefinita

Una risposta predefinita corretta quando si fa clic sul collegamento URL `batchStatus` sopra riportato è simile alla seguente:

```
<response><batchId>demo4-1701473848678-13029383</batchId><status>complete</status><batchSize>1</batchSize></response>
```

I valori previsti per i campi di stato sono:

| Stato | Dettagli |
| --- | --- |
| [!UICONTROL complete] | La richiesta di aggiornamento batch del profilo è stata completata. |
| [!UICONTROL incomplete] | La richiesta di aggiornamento batch del profilo è ancora in fase di elaborazione e non è stata completata. |
| [!UICONTROL stuck] | La richiesta di aggiornamento batch del profilo è bloccata e non è stato possibile completarla. |

### Risposta URL dettagliata sullo stato del batch

È possibile ottenere una risposta più dettagliata passando un parametro `showDetails=true` all&#39;URL `batchStatus` precedente.

Ad esempio:

```
http://mboxedge45.tt.omtrdc.net/m2/demo/profile/batchStatus?batchId=demo-1701473848678-13029383&showDetails=true
```

#### Risposta dettagliata

```
<response>
    <batchId>demo4-1701473848678-13029383</batchId>
    <status>complete</status>
    <batchSize>1</batchSize>
    <consumedCount>1</consumedCount>
    <successfulUpdates>1</successfulUpdates>
    <profilesNotFound>0</profilesNotFound>
    <failedUpdates>0</failedUpdates>
</response>
```

## Chiarimento sulla gestione dei valori vuoti in [!DNL Bulk Profile Update API]

Quando si utilizza [!DNL Target] [!DNL Bulk Profile Update API] (v1 o v2), è importante comprendere in che modo il sistema gestisce i valori vuoti dei parametri o degli attributi.

### Comportamento previsto

L’invio di valori vuoti (&quot;&quot;, campi nulli o mancanti) per parametri o attributi esistenti non reimposta o elimina tali valori nell’archivio dei profili. Questo è progettato.

I valori vuoti vengono ignorati: l’API filtra i valori vuoti durante l’elaborazione per evitare aggiornamenti inutili o inutili.

**Nessuna cancellazione dei dati esistenti**: se un parametro ha già un valore, l&#39;invio di un valore vuoto lo lascia invariato.

**I batch solo vuoti vengono ignorati**: se un batch contiene solo valori vuoti o nulli, viene completamente ignorato e non vengono applicati aggiornamenti.

### Note aggiuntive

Questo comportamento si applica sia alla v1 che alla v2 di [!DNL Bulk Profile Update API].

Il tentativo di cancellare o rimuovere un attributo inviando un valore vuoto non ha alcun effetto.

Il supporto per la rimozione esplicita degli attributi è pianificato per una versione futura (v3) dell’API, ma non è attualmente disponibile.