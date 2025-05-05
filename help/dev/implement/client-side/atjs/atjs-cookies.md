---
keywords: at.js, 2.0, 1.x, cookie
description: Dettagli sulla gestione dei cookie da parte di  [!DNL Adobe Target] at.js 2.x e at.js 1.x
title: Cookie di at.js
feature: at.js
exl-id: 154a844a-6855-4af7-8aed-0719b4c389f5
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '1716'
ht-degree: 72%

---

# Cookie di at.js

Informazioni su at.js 2.x e at.js 1.*x*: comportamento dei cookie.

## Comportamento dei cookie at.js 2.x

Per at.js versione 2.x (fino alla versione 2.10.0, ma non inclusa), *sono supportati solo i cookie di prime parti*. Proprio come in at.js 1.*x*, il cookie di prime parti &quot;mbox&quot; è memorizzato in `clientdomain.com`, dove `clientdomain` è il tuo dominio.

at.js genera un ID sessione e lo memorizza nel cookie. La prima risposta contiene tutte le informazioni relative all’attività, nonché il `TNT` o `PC ID` generato dai server [!DNL Target]. at.js quindi scrive il `TNT/PC ID` nel cookie.

Il cookie di prima parte `AMCV_###@AdobeOrg` è sempre impostato dal servizio ID Experience Cloud, anche se `ECID` è passato in [!DNL Target] richieste.

>[!NOTE]
>
>Per at.js versione 2.10.0 e successive, è disponibile il supporto sia per i cookie di prima parte che per quelli tra domini.

### Supporto per cookie di terze parti e monitoraggio tra più domini

Il monitoraggio tra più domini consente di visualizzare sessioni su due siti correlati, ma con domini diversi, come una sessione singola. Ad esempio, potresti creare un&#39;attività [!DNL Target] che si espande su `siteA.com` e `siteB.com` con il visitatore che resta nella stessa esperienza quando passa da un dominio all&#39;altro. Questa funzionalità è legata alcomportamento dei cookie di terze parti e di prima parte di at.js 1.*x*.

>[!NOTE]
>
>Per at.js versione 2.10.0 e successive, è disponibile il supporto sia dei cookie di terze parti che del tracciamento tra domini diversi.


## at.js 1.Comportamento cookie *x*

Per di at.js 1.*x*, il comportamento del cookie dipende dal fatto di essere un cookie di prima parte, un cookie di terze parti con un cookie di prima parte oppure un cookie di terze parti da solo.

### Quando utilizzare un cookie di prima parte o di terze parti

Il cookie da utilizzare dipende dalla configurazione del sito. Per comprendere i cookie di prima parte e di terze parti, è utile capire come funziona [!DNL Target]. Consulta [Come [!DNL Adobe Target] funziona](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html?lang=it) per ulteriori informazioni.

I casi d’uso principali per i cookie sono tre:

1. Un dominio.

   Tutta l’attività di test si svolge all’interno di un dominio di primo livello (`www.domain.com`, `store.domain.com`, `anysub.domain.com`, ecc.).

   utilizza solo cookie di prima parte. Questa è l’impostazione predefinita.

1. Gli utenti passano da un dominio all’altro e tu intendi tenere traccia e sottoporre a test il loro comportamento tra i diversi domini.

   Esempio: un utente accede al sito per acquistare, ma finalizza l’acquisto attraverso i negozi Yahoo. Tre approcci possibili (consulta il rappresentante commerciale di riferimento per determinare quello più adatto):

   * Abilitare i cookie di prima parte e di terze parti.
   * Abilitare solo i cookie di terze parti (molto raro, ma ha il vantaggio di mantenere il cookie at.js fuori dal tuo dominio).
   * Abilitare solo i cookie di prima parte e trasmettere il parametro `mboxSession` quando si passa a un altro dominio.

     Il parametro `mboxSession` deve essere trasmesso a una pagina di destinazione con riferimento a at.js. Non può trattarsi di una pagina di reindirizzamento intermedia.

1. Stai utilizzando solo adbox o Flashbox su un sito di terze parti.

   Due approcci possibili (contatta il responsabile dell’assistenza clienti di riferimento per determinare quello più adatto):

   * Abilitare i cookie di prima parte e di terze parti.

     I cookie di prima parte e di terze parti sono necessari per Flashbox e risorse creative dinamiche.

   * Abilitare solo i cookie di terze parti.

     Questo approccio è adatto solo nel raro caso in cui le implementazioni adBox vengano utilizzate senza destinazione sul sito.

### Comportamento del cookie di prima parte

Il cookie di prima parte viene memorizzato in `clientdomain.com`, dove `clientdomain` rappresenta il tuo dominio.

at.js genera un `mboxSession ID` e lo memorizza nel cookie. La prima risposta contiene l’offerta, nonché il JavaScript per memorizzare l’`mboxPC ID` generato dall’applicazione, nel cookie.

>[!NOTE]
>
>Il cookie di prima parte `AMCV_###@AdobeOrg` è sempre impostato con l&#39;ID visitatore Experience Cloud.

### Comportamento dei cookie di terze parti

Il cookie di terze parti viene memorizzato in `clientcode.tt.omtrdc.net` e il cookie di prima parte in `clientdomain.com`, dove `clientdomain` rappresenta il tuo dominio.

at.js genera un `mboxSession ID`. La prima richiesta di posizione restituisce intestazioni di risposta HTTP che tentano di impostare cookie di terze parti denominati `mboxSession` e `mboxPC` e viene inviata nuovamente una richiesta di reindirizzamento con un parametro aggiuntivo (`mboxXDomainCheck=true`).

Se il browser accetta cookie di terze parti, la richiesta di reindirizzamento li include e viene restituita l’offerta.

Se il browser rifiuta i cookie di terze parti, la richiesta di reindirizzamento non include i cookie e viene visualizzato il contenuto predefinito per tutte le posizioni nella pagina. Data l’assenza di cookie impostati, lo stesso processo di cui sopra si ripete a ogni richiesta di pagina.

### Comportamento dei cookie di terze parti e di prima parte

Il cookie di terze parti viene memorizzato in `clientcode.tt.omtrdc.net` e il cookie di prima parte in `clientdomain.com`, dove `clientdomain` rappresenta il tuo dominio.

at.js genera un `mboxSession ID`. La prima richiesta di posizione restituisce intestazioni di risposta HTTP che tentano di impostare cookie di terze parti denominati `mboxSession` e `mboxPC` e viene inviata nuovamente una richiesta di reindirizzamento con un parametro aggiuntivo (`mboxXDomainCheck=true`).

Se il browser accetta cookie di terze parti, la richiesta di reindirizzamento li include e viene restituita l’offerta.

Alcuni browser rifiutano i cookie di terze parti. Se il cookie di terze parti è bloccato, il cookie di prima parte continua a funzionare. [!DNL Target] tenta di impostare il cookie di terze parti e, se non è in grado di farlo, [!DNL Target] può solo tenere traccia del dominio specifico del cliente. Il rilevamento tra domini diversi non funziona se il cookie di terze parti è bloccato, a meno che `mboxSession` non venga aggiunto al collegamento che attraversa più domini. In questo caso, un altro cookie di prima parte viene impostato e sincronizzato con il cookie di prima parte del dominio precedente.

## Impostazioni dei cookie

Il cookie dispone di diverse impostazioni predefinite. Puoi modificare queste impostazioni, se necessario, fatta eccezione per la durata dei cookie. In caso di modifica delle impostazioni dei cookie, rivolgiti al rappresentante di riferimento per il tuo account.

| Impostazione | Informazioni |
|--- |--- |
| Nome cookie | mbox. |
| Dominio cookie | Il primo e il secondo livello dei domini da cui viene distribuito il contenuto. Dato che viene distribuito dal dominio della società, il cookie è un cookie dei siti Web visualizzati. Esempio: `mycompany.com`. |
| Dominio server | `clientcode.tt.omtrdc.net`, utilizzando il codice cliente per il tuo account. |
| Durata cookie | Il cookie rimane sul browser del visitatore per due anni dal suo ultimo accesso.<P>L&#39;impostazione `deviceIdLifetime` può essere sovrascritta in [at.js versione 2.3.1 o successiva](../atjs/target-atjs-versions.md). Per ulteriori informazioni, consulta [targetGlobalSettings()](../../../implement/client-side/atjs/atjs-functions/targetglobalsettings.md). |
| Policy P3P | Il cookie viene pubblicato con una policy P3P, come richiesto dall’impostazione predefinita nella maggior parte dei browser. Una policy P3P indica a un browser chi sta erogando il cookie e come verranno utilizzate le informazioni. |

Nel cookie sono conservati una serie di valori per la gestione dell’esperienza dei visitatori in relazione alle campagne:

| Valore | Definizione |
|--- |--- |
| session ID | Un ID univoco per una sessione utente. Per impostazione predefinita, la sua durata è di 30 minuti. |
| pc ID | Un ID semi-permanente per il browser di un visitatore. Dura 14 giorni. |
| check | Un semplice valore di test utilizzato per determinare se un visitatore supporta i cookie. Impostato ogni volta che un visitatore richiede una pagina. |
| disable | Impostato se il tempo di caricamento del visitatore supera il timeout configurato nel file at.js o Adobe Experience Platform Web SDK. Per impostazione predefinita, la sua durata è di 1 ora. |

## Impatto su [!DNL Target] per i visitatori di Safari a causa di modifiche nel monitoraggio di Apple WebKit

Considera quanto segue:

### Come funziona il tracciamento di [!DNL Adobe Target]?

| Cookie | Dettagli |
|--- |--- |
| Domini di prima parte | Questa è l&#39;implementazione standard per i clienti [!DNL Target].  I cookie &quot;mbox&quot; sono impostati nel dominio del cliente. |
| Tracciamento di terze parti | Il monitoraggio di terze parti è importante per i casi di utilizzo di pubblicità e targeting in [!DNL Target] e in [!DNL Adobe Audience Manager] (AAM).  Il tracciamento di terze parti richiede tecniche di scripting tra siti.  [!DNL Target] utilizza due cookie, &quot;mboxSession&quot; e &quot;mboxPC&quot;, impostati nel dominio `clientcode.tt.omtrd.net`. |

### Qual è l’approccio di Apple?

Da Apple:

“Intelligent Tracking Prevention è una nuova funzionalità di WebKit che riduce il tracciamento tra siti limitando ulteriormente i cookie e altri dati del sito Web.”

“Questo è ciò che viene chiamato tracciamento tra siti e il cookie utilizzato da `example-tracker.com` è chiamato un cookie di terze parti. Durante i test abbiamo trovato siti Web popolari con oltre 70 tracker simili, che raccolgono in silenzio dati sugli utenti.”

| Approccio | Dettagli |
|--- |--- |
| Intelligent Tracking Prevention | Per ulteriori informazioni, consulta [Intelligent Tracking Prevention](https://webkit.org/blog/7675/intelligent-tracking-prevention/) sul sito web del motore di ricerca open source WebKit. |
| Cookie | Come Safari gestisce i cookie:<ul><li>I cookie di terze parti che non si trovano in un dominio a cui l’utente accede direttamente non vengono mai salvati. Questo comportamento non è una novità. I cookie di terze parti già non sono supportati in Safari.</li><li>I cookie di terze parti impostati in un dominio a cui l’utente accede direttamente vengono eliminati dopo 24 ore.</li><li>I cookie di prima parte vengono eliminati dopo 30 giorni se il dominio di prima parte è stato classificato come dominio che traccia gli utenti tra siti diversi. Questo problema può essere applicato alle grandi aziende che inviano utenti a diversi domini online. Apple non ha chiarito come esattamente questi domini saranno classificati o come un dominio può determinare se è stato classificato come dominio che traccia gli utenti tra siti diversi.</li></ul> |
| Apprendimento automatico per identificare i domini che sono tra siti diversi | Da Apple:<P>Classificatore di apprendimento automatico: viene utilizzato un modello di apprendimento automatico per classificare quali domini principali controllati privatamente hanno la capacità di tracciare l’utente tra siti diversi, sulla base delle statistiche raccolte. Dalle varie statistiche raccolte, sono emersi tre vettori con un segnale forte per la classificazione basata sulle pratiche di tracciamento correnti: sottorisorsa per il numero di domini univoci, sottostruttura per il numero di domini univoci e infine numero di domini univoci di reindirizzamento. La raccolta e la classificazione di tutti i dati avviene sul dispositivo.<P>Tuttavia, se l’utente interagisce con example.com come dominio principale, spesso denominato dominio di prima parte, la funzione Intelligent Tracking Prevention lo considera come un segnale di interesse dell’utente verso il sito web e regola temporaneamente il suo comportamento come illustrato nella seguente timeline:<P>Se l&#39;utente ha interagito con example.com nelle ultime 24 ore, i suoi cookie saranno disponibili quando `example.com` è di terze parti. Questo consente scenari di login come &quot;Accedi a X con il mio account Y&quot;.<ul><li>Domini che vengono visitati come dominio di livello superiore non saranno interessati. Ad esempio, siti come OKTA</li><li>Identifica i domini che sono sottodomini o frame secondari della pagina corrente su più domini univoci.</li></ul> |

### Che impatto ha sulle soluzioni Adobe?

| Funzionalità interessate | Dettagli |
|--- |--- |
| Supporto per rinuncia | Le modifiche di tracciamento WebKit di Apple interrompono il supporto dell’opzione di rinuncia.<P>La rinuncia a [!DNL Target] utilizza un cookie nel dominio `clientcode.tt.omtrdc.net`. Per ulteriori dettagli, consulta [Privacy](/help/dev/before-implement/privacy/privacy.md).<P>[!DNL Target] supporta due rinunce:<ul><li>Uno per il cliente (il cliente gestisce il collegamento di rinuncia).</li><li>Uno tramite Adobe che rifiuta all&#39;utente tutte le funzionalità [!DNL Target] per tutti i clienti.</li></ul>Entrambi i metodi utilizzano il cookie di terze parti. |
| [!DNL Target] attività | I clienti possono scegliere la durata del profilo [&#128279;](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/visitor-profile-lifetime.html?lang=it) per i loro account [!DNL Target], fino a 90 giorni. Il problema è che se la durata del profilo dell&#39;account è più lunga di 30 giorni e il cookie di prima parte viene eliminato perché il dominio del cliente è stato contrassegnato come dominio di tracciamento degli utenti tra siti diversi, i visitatori Safari saranno interessati dalle seguenti aree in [!DNL Target]:<P>**[!DNL Target]report**: se un utente Safari entra in un&#39;attività, ritorna dopo 30 giorni e poi effettua la conversione, l&#39;utente conta come due visitatori e una conversione.<P>Questo comportamento è lo stesso per le attività che utilizzano [!DNL Analytics] come origine per la generazione di rapporti (A4T).<P>**Profilo e appartenenza all&#39;attività**:<ul><li>I dati del profilo vengono cancellati quando scade il cookie di prima parte.</li><li>L’appartenenza all’attività viene cancellata quando scade il cookie di prima parte.</li><li> [!DNL Target] non funziona in Safari per gli account che utilizzano un&#39;implementazione di cookie di terze parti o di cookie di prima parte e di terze parti. Questo comportamento non è una novità. Safari non consente i cookie di terze parti già da qualche tempo.</li></ul><P>**Suggerimenti**: se si teme che il dominio del cliente possa essere contrassegnato come dominio che tiene traccia dei visitatori attraverso sessioni diverse, è più sicuro impostare la durata del profilo su un massimo di 30 giorni in [!DNL Target]. In tal modo gli utenti verranno tracciati allo stesso modo in Safari e negli altri motori browser. |
