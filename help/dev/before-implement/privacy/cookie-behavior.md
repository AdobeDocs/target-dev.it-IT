---
keywords: Panoramica e riferimento, webkit, cookie, prime parti, terze parti, prime parti, terze parti,
description: Scopri il comportamento dei cookie  [!DNL Target]  (cookie di prima parte, cookie di terze parti con cookie di prima parte o cookie di terze parti da soli).
title: Dove Posso Trovare Informazioni Su [!DNL Target] Cookie?
feature: at.js
exl-id: d44e02ce-8920-4130-bcad-699ca77c0dad
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '1581'
ht-degree: 50%

---

# [!DNL Target] cookie

Il comportamento del cookie dipende dal fatto di essere un cookie di prima parte, un cookie di terze parti con un cookie di prima parte oppure un cookie di terze parti.

>[!NOTE]
>
>Per informazioni dettagliate sui diversi cookie utilizzati da [!DNL Target], vedere [[!DNL Adobe Target] cookies](https://experienceleague.adobe.com/docs/core-services/interface/administration/ec-cookies/cookies-target.html?lang=it){target=_blank} nella *Guida ai componenti dell&#39;interfaccia centrale di Experience Cloud*.
>
>Questo argomento contiene informazioni su `mboxSession` e `mboxPC`. Le best practice di implementazione consigliano di non collegare o archiviare informazioni riservate con i dati dei cookie: `mboxSession` o `mboxPC`.

Vedi anche [Eliminare il [!DNL Target] cookie](cookie-deleting.md).

## Quando utilizzare un cookie di prima parte o di terze parti

Il cookie da utilizzare dipende dalla configurazione del sito. Per comprendere i cookie di prima parte e di terze parti, è utile capire come funziona [!DNL Target]. Per ulteriori informazioni, vedere [Funzionamento [!DNL Adobe] [!DNL Target]](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html).

I casi d’uso principali per i cookie sono tre:

1. Un dominio.

   Tutti i test si svolgono all&#39;interno di un dominio di primo livello (`www.domain.com`, `store.domain.com`, `anysub.domain.com` e così via).

   Approccio: utilizza solo cookie di prime parti (impostazione predefinita).

1. Gli utenti passano da un dominio all’altro e tu intendi tenere traccia e sottoporre a test il loro comportamento tra i diversi domini.

   Esempio: un utente accede al sito per acquistare, ma finalizza l’acquisto attraverso i negozi Yahoo. Tre approcci possibili (consulta il rappresentante commerciale di riferimento per determinare quello più adatto):

   * Abilitare i cookie di prima parte e di terze parti.
   * Abilita solo i cookie di terze parti (raro, ma ha il vantaggio di mantenere il cookie mbox fuori dal tuo dominio).
   * Abilitare solo i cookie di prima parte e trasmettere il parametro `mboxSession` quando si passa a un altro dominio.

     Il parametro `mboxSession` deve essere passato a una pagina di destinazione e a cui si fa riferimento dalla libreria JavaScript (Adobe Experience Platform Web SDK o at.js). Non può trattarsi di una pagina di reindirizzamento intermedia.

1. Utilizzi solo adbox o Flashbox su un sito di terze parti.

   Due approcci (collabora con il rappresentante del tuo account per determinare quello migliore):

   * Abilitare i cookie di prima parte e di terze parti.

     I cookie di prima parte e di terze parti sono necessari per Flashbox e risorse creative dinamiche.

   * Abilitare solo i cookie di terze parti.

     Questo approccio è adatto solo nel raro caso in cui le implementazioni AdBox vengano utilizzate senza destinazione sul sito.

## Comportamento del cookie di prima parte

Il cookie di prime parti è memorizzato in clientdomain.com, dove `clientdomain` è il tuo dominio.

La libreria JavaScript genera un `mboxSession ID` e lo memorizza nel cookie [!DNL Target]. La prima risposta mbox contiene l&#39;offerta e il JavaScript per memorizzare `mboxPC ID` generato dall&#39;applicazione nel cookie mbox.

>[!NOTE]
>
>Il cookie di prima parte AMCV_###@AdobeOrg è sempre impostato con l’ID visitatore Experience Cloud.

## Comportamento dei cookie di terze parti

Il cookie di terze parti è memorizzato in clientcode.tt.omtrdc.net mentre il cookie di prima parte in clientdomain.com, dove `clientdomain` è il tuo dominio.

La libreria JavaScript genera un `mboxSession ID`. La prima richiesta di posizione restituisce intestazioni di risposta HTTP che tentano di impostare cookie di terze parti denominati `mboxSession` e `mboxPC` e viene inviata nuovamente una richiesta di reindirizzamento con un parametro aggiuntivo (`mboxXDomainCheck=true`).

Se il browser accetta cookie di terze parti, la richiesta di reindirizzamento li include e viene restituita l’offerta.

Se il browser rifiuta i cookie di terze parti, la richiesta di reindirizzamento non include i cookie e viene visualizzato il contenuto predefinito per tutte le posizioni nella pagina. Data l’assenza di cookie impostati, lo stesso processo di cui sopra si ripete a ogni richiesta di pagina.

>[!NOTE]
>
>Il cookie demdex.net viene impostato se i cookie di terze parti non sono bloccati.

## Comportamento dei cookie di terze parti e di prima parte

Il cookie di terze parti è memorizzato in clientcode.tt.omtrdc.net mentre il cookie di prima parte in clientdomain.com, dove `clientdomain` è il tuo dominio.

La libreria JavaScript genera un `mboxSession ID`. La prima richiesta di posizione restituisce intestazioni di risposta HTTP che tentano di impostare cookie di terze parti denominati `mboxSession` e `mboxPC` e viene inviata nuovamente una richiesta di reindirizzamento con un parametro aggiuntivo (`mboxXDomainCheck=true`).

Se il browser accetta cookie di terze parti, la richiesta di reindirizzamento li include e viene restituita l’offerta.

Alcuni browser rifiutano i cookie di terze parti. Se il cookie di terze parti è bloccato, il cookie di prima parte continua a funzionare. [!DNL Target] tenta di impostare il cookie di terze parti e, se non è in grado di farlo, [!DNL Target] può solo tenere traccia del dominio specifico del cliente. Il monitoraggio tra più domini non funziona se il cookie di terze parti è bloccato, a meno che `mboxSession` non venga aggiunto al collegamento che attraversa più domini. In questo caso, un altro cookie di prima parte viene impostato e sincronizzato con il cookie di prima parte del dominio precedente.

## Impostazioni dei cookie

Il cookie dispone di diverse impostazioni predefinite. Se necessario, puoi modificare queste impostazioni, ad eccezione della durata del cookie. In caso di modifica delle impostazioni dei cookie, rivolgiti al rappresentante di riferimento per il tuo account.

| Impostazione | Informazioni |
|--- |--- |
| Nome cookie | mbox. |
| Dominio cookie | Il primo e il secondo livello dei domini da cui viene distribuito il contenuto. Poiché viene distribuito dal dominio della tua azienda, il cookie è un cookie di prime parti.<br />Esempio: `mycompany.com`. |
| Dominio server | `clientcode.tt.omtrdc.net`, utilizzando il codice cliente per il tuo account. |
| Durata cookie | Il cookie rimane sul browser del visitatore per due settimane dall&#39;ultimo accesso. Non puoi modificare la durata del cookie. |
| Policy P3P | Il cookie viene pubblicato con una policy P3P, come richiesto dall’impostazione predefinita nella maggior parte dei browser. Una policy P3P indica a un browser che sta distribuendo il cookie e come vengono utilizzate le informazioni. |

Nel cookie è conservata una serie di valori per la gestione dell’esperienza dei visitatori in relazione alle campagne:

| Valore | Definizione |
|--- |--- |
| session ID | Un ID univoco per una sessione utente. Per impostazione predefinita, l’ID dura 30 minuti. |
| pc ID | Un ID semi-permanente per il browser di un visitatore. Dura 14 giorni. |
| check | Un semplice valore di test utilizzato per determinare se un visitatore supporta i cookie. Impostato ogni volta che un visitatore richiede una pagina. |
| disable | Impostato se il tempo di caricamento del visitatore supera il timeout configurato nel file della libreria di JavaScript. Per impostazione predefinita, la durata di questo valore è di un’ora. |

## Impatto su [!DNL Target] per i visitatori di Safari a causa di modifiche nel monitoraggio di Apple WebKit

**Funzionamento del tracciamento di [!DNL Target]?**

| Cookie | Dettagli |
|--- |--- |
| Domini di prima parte | Implementazione standard per [!DNL Target] clienti. I cookie &quot;mbox&quot; sono impostati nel dominio del cliente. |
| Tracciamento di terze parti | Il monitoraggio di terze parti è importante per i casi di utilizzo di pubblicità e targeting in [!DNL Target] e in [!DNL Adobe Audience Manager] (AAM). Il tracciamento di terze parti richiede tecniche di scripting tra siti. [!DNL Target] utilizza due cookie, &quot;mboxSession&quot; e &quot;mboxPC&quot;, impostati nel dominio `clientcode.tt.omtrd.net`. |
**Qual è l’approccio di Apple?**

Da Apple:

“Intelligent Tracking Prevention è una nuova funzionalità di WebKit che riduce il tracciamento tra siti limitando ulteriormente i cookie e altri dati del sito Web.”

“Questo è ciò che viene chiamato tracciamento tra siti e il cookie utilizzato da `example-tracker.com` è chiamato un cookie di terze parti. Durante i test abbiamo trovato siti Web popolari con oltre 70 tracker simili, che raccolgono in silenzio dati sugli utenti.”

| Approccio | Dettagli |
|--- |--- |
| Intelligent Tracking Prevention | Per ulteriori informazioni, consulta [Intelligent Tracking Prevention](https://webkit.org/blog/7675/intelligent-tracking-prevention/) sul sito web del motore di ricerca open source WebKit. |
| Cookie | Come Safari gestisce i cookie:<ul><li>I cookie di terze parti che non si trovano in un dominio a cui l’utente accede direttamente non vengono mai salvati. Questo comportamento non è una novità. I cookie di terze parti già non sono supportati in Safari.</li><li>I cookie di terze parti impostati in un dominio a cui l’utente accede direttamente vengono eliminati dopo 24 ore.</li><li>I cookie di prima parte vengono eliminati dopo 30 giorni se il dominio di prima parte è stato classificato come dominio che traccia gli utenti tra siti diversi. Questo problema può essere applicato alle grandi aziende che inviano utenti a diversi domini online. Apple non ha chiarito come esattamente questi domini vengano classificati, o come un dominio possa determinare se sono stati classificati come domini di tracciamento degli utenti tra siti diversi.</li></ul> |
| Apprendimento automatico per identificare i domini che sono tra siti diversi | Da Apple:<br />Classificatore di apprendimento automatico: viene utilizzato un modello di apprendimento automatico per classificare quali domini principali controllati privatamente possono tracciare l&#39;utente tra siti diversi, in base alle statistiche raccolte. Dalle varie statistiche raccolte, sono emersi tre vettori con un segnale forte per la classificazione basata sulle pratiche di tracciamento correnti: sottorisorsa per il numero di domini univoci, sottostruttura per il numero di domini univoci e infine numero di domini univoci di reindirizzamento. La raccolta e la classificazione di tutti i dati avviene sul dispositivo.<br />Tuttavia, se l&#39;utente interagisce con `example.com` come dominio principale, spesso denominato dominio di prima parte, la funzione Intelligent Tracking Prevention lo considera come un segnale di interesse dell&#39;utente verso il sito Web e regola temporaneamente il suo comportamento come illustrato nella seguente timeline:<br />Se l&#39;utente ha interagito con `example.com` nelle ultime 24 ore, i suoi cookie sono disponibili quando `example.com` è di terze parti. Questa procedura consente scenari di accesso &quot;Accedi con il mio account X su Y&quot;.<ul><li>I domini che vengono visitati come dominio di primo livello non sono interessati. Ad esempio, siti come OKTA</li><li>Identifica i domini che sono sottodomini o frame secondari della pagina corrente su più domini univoci.</li></ul> |

**Che impatto ha [!DNL Adobe]?**

| Funzionalità interessate | Dettagli |
|--- |--- |
| Supporto per rinuncia | Le modifiche di tracciamento WebKit di Apple interrompono il supporto dell’opzione di rinuncia.<br />La funzione di rinuncia di Target utilizza un cookie nel dominio `clientcode.tt.omtrdc.net`. Per ulteriori dettagli, consulta [Privacy](privacy.md).<br />Target supporta due tipi di rinuncia:<ul><li>Uno per il cliente (il cliente gestisce il collegamento di rinuncia).</li><li>Uno tramite [!DNL Adobe] che rifiuta all&#39;utente tutte le funzionalità di [!DNL Target] per tutti i clienti.</li></ul>Entrambi i metodi utilizzano il cookie di terze parti. |
| Attività di Target | I clienti possono scegliere la durata del profilo [](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/visitor-profile-lifetime.html) per i loro account [!DNL Target] (fino a 90 giorni). Il problema è che, se la durata del profilo dell&#39;account è più lunga di 30 giorni e il cookie di prima parte viene eliminato perché il dominio del cliente è stato contrassegnato come dominio di tracciamento degli utenti tra siti diversi, i visitatori Safari sono interessati dalle seguenti aree in Target:<br />**Rapporti di Target**: se un utente Safari entra in un&#39;attività, ritorna dopo 30 giorni ed effettua la conversione, l&#39;utente conta come due visitatori e una conversione.<br />Questo comportamento è lo stesso per le attività che utilizzano Analytics come origine per la generazione di rapporti (A4T).<br />**Profilo e appartenenza all&#39;attività**:<ul><li>I dati del profilo vengono cancellati quando scade il cookie di prima parte.</li><li>L’appartenenza all’attività viene cancellata quando scade il cookie di prima parte.</li><li> [!DNL Target] non funziona in Safari per gli account che utilizzano un&#39;implementazione di cookie di terze parti o di cookie di prima parte e di terze parti. Questo comportamento non è una novità. Safari non consente cookie di terze parti da un po’ di tempo.</li></ul><br />**Suggerimenti**: se si teme che il dominio del cliente possa essere identificato come dominio che tiene traccia dei visitatori attraverso sessioni diverse, è più sicuro impostare la durata del profilo in Target su un massimo di 30 giorni. Questo limite garantisce che gli utenti vengano tracciati in modo simile in Safari e in tutti gli altri browser. |
