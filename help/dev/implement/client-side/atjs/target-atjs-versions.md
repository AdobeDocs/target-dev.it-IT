---
keywords: Versioni di at.js, versioni di at.js, note sulla versione
description: Visualizza i dettagli sulle modifiche in ogni versione di [!DNL Adobe Target] Libreria JavaScript at.js.
title: Cosa è incluso in ogni versione di at.js?
feature: at.js
exl-id: 609dacba-2ab8-45e9-b189-928d59938c98
source-git-commit: c5d1f477a2561a59efb3250e35563b48822fbe59
workflow-type: tm+mt
source-wordcount: '4697'
ht-degree: 72%

---

# Dettagli sulle versioni di at.js

Dettagli sulle modifiche in ogni versione della libreria JavaScript at.js di [!DNL Adobe Target].

>[!IMPORTANT]
>
>[!DNL Adobe Target] supporta sia at.js 1.*x* e at.js 2.*x*.
>
>at.js 1.*x* è entrato in modalità di manutenzione. Il [!DNL Target] se necessario, il team rilascia correzioni di bug e patch di sicurezza.
>
>Il [!DNL Target] team fornisce supporto completo per at.js 2.*x* e rilascia regolarmente correzioni di bug, patch di sicurezza, funzioni e ottimizzazione delle prestazioni.
>
>Effettua l’aggiornamento alle versioni più recenti di 1.*x* o 2.*x* per ottenere correzioni di bug e patch di sicurezza per i problemi rilevati in qualsiasi versione secondaria precedente della versione principale corrispondente.

Tag in [Adobe Experience Platform](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md) sono il metodo preferito per aggiornare at.js. Gli sviluppatori di estensioni aggiungono continuamente nuove funzioni alle loro estensioni e spesso correggono i bug. Questi aggiornamenti vengono assemblati in nuove versioni di un’estensione e sono resi disponibili nel catalogo Adobe Experience Platform come aggiornamenti. Per ulteriori informazioni, consulta [Aggiornamenti delle estensioni](https://experienceleague.adobe.com/docs/experience-platform/tags/ui/extensions/extension-upgrade.html) nel *Panoramica sui tag* guide.6+

## at.js versione 2.11.2 (26 ottobre 2023)

* È stato risolto un problema che causava incoerenze nei token di risposta inviati su eventi personalizzati.

## at.js versione 2.11.1 (13 ottobre 2023)

* È stato risolto un problema che causava errori non rilevati durante la modalità quirks di una pagina in cui è in esecuzione at.js.

## at.js versione 2.11.0 (10 ottobre 2023)

* È stato aggiunto il supporto per l’impostazione di [!DNL Adobe Experience Platform] (AEP) `sandboxId` e `sandboxName` in `targetGlobalSettings`, che viene passato all’API di consegna il `getOffer/getOffers` chiamate.
* Correzione DOM shadow per concatenamento `:eq()` nei selettori.

## at.js versione 2.10.3 (12 settembre 2023)

* È stato risolto un problema che attivava in modo errato il `at-content-rendering-succeeded` evento personalizzato quando non viene eseguito il rendering di offerte. L’evento corretto, `at-content-rendering-no-offers`, è ora attivato.
* Aggiunto `eventToken` e `responseTokens` a oggetto errore per `at-content-rendering-failed` evento personalizzato.

## at.js versione 2.10.2 (7 marzo 2023)

* È stato risolto un problema che causava la restituzione di un errore della funzione `trackEvent`.

## at.js versione 2.10.1 (2 febbraio 2023)

* È stato corretto un bug a causa del quale le attività che coinvolgono regole di pubblico contenenti parametri nel cui nome è presente il carattere punto non restituivano l’esperienza prevista per le decisioni sul dispositivo.
* È stato corretto un bug introdotto in at.js 2.6.0 in cui at.js attivava una chiamata di consegna, anche quando mboxDisable era abilitato.

## at.js versione 2.10.0 (19 settembre 2022)

* È stato aggiunto il supporto per cookie di terze parti.

## at.js versione 2.9.0 (27 maggio 2022)

* È stato aggiunto il supporto [per i User Agent Client Hints](user-agent-and-client-hints.md).
* È stato corretto un errore per cui più richieste di mbox sulla stessa pagina avevano ID di impression diversi.

## at.js versione 2.8.1 (28 gennaio 2022)

* È stato corretto un problema che impediva la mappatura di `pageLoad` su target-global-mbox nella funzione Decisioning sul dispositivo in modalità di esecuzione ibrida.
* È stato risolto un problema relativo ai dettagli di analisi per richieste mbox.
* Sono state aggiornate le dipendenze di sviluppo per correggere alcune vulnerabilità di sicurezza.

## at.js versione 2.8.0 (7 gennaio 2022)

La [!DNL Target] libreria JavaScript at.js ora raccoglie dati di telemetria relativi all’utilizzo delle funzioni e alle prestazioni. I dati personali non vengono raccolti. La rinuncia a questa funzione è disponibile impostando `telemetryEnabled` false in `targetGlobalSettings`. Per ulteriori informazioni, consulta [telemetryEnabled in targetGlobalSettings](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#telemetryenabled).

## at.js versione 2.7.0 (28 ottobre 2021)

Questa versione contiene i seguenti miglioramenti:

* È stato aggiunto il supporto per [Componenti web](https://developer.mozilla.org/en-US/docs/Web/Web_Components). Questa versione di at.js è necessaria per creare e testare esperienze e offerte personalizzate su elementi personalizzati e su elementi al loro interno. Questa funzionalità è inclusa in [!DNL Target Standard/Premium] versione 21.10.5.

## at.js 1.8.3 (21 settembre 2021)

Questa versione contiene le seguenti modifiche:

* È stato rimosso il `reactor-window` e `reactor-document` moduli Adobe Experience Platform Launch per garantire il corretto funzionamento della build del Platform launch per i clienti che hanno `window.default` o `document-default` impostata.
* at.js 1.8.3 ora imposta in modo esplicito `Samesite=None` e `Secure` per garantire che i cookie del dominio di terze parti siano impostati correttamente.

## at.js 2.6.1 (16 agosto 2021)

* Correzione di un bug relativo all’errore “Nessun artefatto nella cache disponibile per la modalità ibrida” quando si utilizzano le decisioni sul dispositivo.

## at.js 2.6.0 (16 luglio 2021)

* Ai cookie è stato aggiunto un attributo sicuro ogni volta che le impostazioni at.js `secureOnly` sono impostate su `true`.
* Ora quando si utilizza `triggerView()` sono disponibili i token di risposta.
* È stato risolto un problema relativo all’evento `CONTENT_RENDERING_NO_OFFERS`. Ora questo evento viene attivato correttamente ogni volta che non viene restituito alcun contenuto da [!DNL Target].
* [!UICONTROL Analytics for Target] I dettagli delle metriche di clic di (A4T) vengono restituiti correttamente quando si utilizza `prefetch` richieste.
* La generazione di UUID non utilizza più `Math.random()`, ma si basa su `window.crypto`.
* La scadenza dei cookie `sessionId` viene estesa correttamente a ogni chiamata di rete.
* L’inizializzazione della cache di visualizzazione Applicazione a pagina singola viene ora gestita correttamente e rispetta le impostazioni `viewsEnabled`. Impostazione `viewsEnabled` al `false` ora disattiva il valore `triggerView()` funzione. Consulta [Ordine delle operazioni per il caricamento della pagina iniziale](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md#order).

## at.js 2.5.0 (13 maggio 2021)

Questa versione di at.js include i miglioramenti e le modifiche seguenti:

* Supporto di [Decisioning sul dispositivo](/help/dev/implement/client-side/atjs/on-device-decisioning/on-device-decisioning.md) per at.js.
* Supporto dei [collegamenti di anteprima](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html) per le attività di Automated Personalization

A partire da questa versione, non è più supportato Microsoft Internet Explorer 10 o versioni successive.

## at.js 2.4.1 (23 marzo 2021)

Questa release di at.js è una versione di manutenzione e include i miglioramenti e le correzioni seguenti:

* È stato risolto un problema a causa del quale `targetPageParams` veniva incluso nelle richieste mbox. `targetPageParams` deve essere incluso solo nelle richieste `pageLoad`. (TNT-40247)
* Sono stati ottimizzati i valori globali di finestre e documenti a cui fa riferimento l’estensione Adobe Experience Platform. (TNT-37124)

## at.js 2.4.0 (14 gennaio 2021)

Questa versione di at.js è una versione di manutenzione che include i miglioramenti e le correzioni seguenti:

* Aggiunge il supporto per l’ID di piattaforma/profilo unificato agli ID cliente dell’API Delivery.
* È stata corretta l’iniezione di tag di stile non valida.

## at.js 2.3.3 (13 novembre 2020)

Questa versione di at.js è una versione di manutenzione e include la correzione seguente:

* È stato risolto un problema relativo al tracciamento dei clic mbox e A4T. Con 0n-clic, [!DNL Target] è stata attivata una chiamata API di consegna con i parametri mbox e mbox corretti. Tuttavia, l&#39;identificatore SDID non corrisponde a quello presente nel [!DNL Analytics] non si è verificato alcun hit di unione e conversione. (TNT-38372)

## at.js 2.3.2 (24 luglio 2020)

Questa versione di at.js è una versione di manutenzione e include la correzione seguente:

* È stato corretto un bug a causa del quale uno script o un codice aggiungeva proprietà predefinite alla finestra o al documento.

## at.js 1.8.2 (15 giugno 2020)

Questa versione di at.js è una versione di manutenzione e include la correzione seguente:

* È stato risolto un problema che si verificava con l’utilizzo di CNAME e Edge override, at.js 1.*x* poteva creare il dominio del server in modo errato, causando un errore nella richiesta di [!DNL Target]. (TNT-35064)

## Versioni di at.js 2.3.1 (15 giugno 2020)

Questa release di at.js è una versione di manutenzione e include i miglioramenti e le correzioni seguenti:

* L’impostazione `deviceIdLifetime` ora può essere sostituita tramite [targetGlobalSettings](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md). (TNT-36349)
* È stato risolto un problema che si verificava con l’utilizzo di CNAME e Edge override, at.js 2.*x* poteva creare il dominio del server in modo errato, causando un errore nella richiesta di [!DNL Target]. (TNT-35065)
* È stato risolto un problema che si verificava con l’utilizzo di [!DNL Target] l&#39;estensione v2 e [!UICONTROL Adobe Analytics Launch] estensione, [!DNL Target] ha ritardato il [!DNL Analytics] `sendBeacon` chiamare. (TNT-36407, TNT-35990, TNT-36000)

## at.js versione 2.3.0 (25 marzo 2020)

Questa release di at.js è una versione di manutenzione e include i miglioramenti e le correzioni seguenti:

* Supporto per l’impostazione dei nonce dell’informativa sulla sicurezza dei contenuti sui tag SCRIPT e STYLE aggiunti al DOM della pagina durante l’applicazione dei tag consegnati [!DNL Target] offerte. I clienti possono impostare `targetGlobalSettings.cspScriptNonce` e `targetGlobalSettings.cspStyleNonce` in modo che at.js possa impostare i nonce degli script e dei tag di stile corrispondenti sulle offerte applicate. Consulta  [targetGlobalSettings](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md) per ulteriori dettagli.
* È stato risolto un problema che si verificava durante la compilazione di at.js con il compilatore Google Closure per la distribuzione di Google Tag Manager.
* Rinominato il cookie di controllo at.js da `check` a `at_check` per evitare conflitti con le implementazioni dei clienti.

## at.js versione 1.8.1 (25 marzo 2020)

Questa release di at.js è una versione di manutenzione e include i miglioramenti e le correzioni seguenti:

* Rinominato il cookie di controllo at.js da `check` a `at_check` per evitare conflitti con le implementazioni dei clienti.

## at.js versione 2.2.0 (10 ottobre 2019)

Questa versione di at.js include i miglioramenti e le correzioni seguenti:

* È stato risolto un problema a causa del quale il tracciamento dei clic non segnalava le conversioni in [!DNL Analytics for Target] (A4T) quando [!DNL Adobe Analytics] codice non presente negli elementi della pagina.
* Prestazioni migliorate quando si utilizzano sia Experience Cloud ID Service (ECID) v4.4 che at.js 2.2 sulle pagine web.
* In precedenza, l’ECID effettuava due chiamate di blocco prima che at.js potesse recuperare le esperienze. Questa funzione è stata ridotta a una singola chiamata, il che migliora notevolmente le prestazioni.
* È stata corretta l’elaborazione della vista precaricata errata, a causa della quale i token evento delle offerte predefinite non venivano inclusi nelle notifiche inviate.

>[!NOTE]
>
>Aggiorna l’estensione ECID alla versione 4.4 per approfittare di questo miglioramento delle prestazioni.

* La versione 2.2 di at.js fornisce anche una nuova impostazione denominata `serverState`. Questa impostazione può essere utilizzata per ottimizzare le prestazioni della pagina quando un’integrazione ibrida di [!DNL Target] è implementato. L’integrazione ibrida significa che utilizzi sia at.js v2.2+ sul lato client che l’API di consegna o un [!DNL Target] SDK lato server per distribuire le esperienze. `serverState` consente a at.js v2.2+ di applicare le esperienze direttamente dal contenuto recuperato sul lato server e restituito al client come parte della pagina trasmessa. Per ulteriori informazioni, consulta “serverState” in [targetGlobalSettings](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#serverstate).

## at.js versione 1.8.0 (10 ottobre 2019)

Questa versione di at.js include i miglioramenti e le correzioni seguenti:

* Prestazioni migliorate quando si utilizzano sia Experience Cloud ID Service (ECID) v4.4 che at.js 1.8 sulle pagine web.
* In precedenza, l’ECID effettuava due chiamate di blocco prima che at.js potesse recuperare le esperienze. Questa funzione è stata ridotta a una singola chiamata, il che migliora notevolmente le prestazioni.

>[!NOTE]
>
>Aggiorna l’estensione ECID alla versione 4.4 per approfittare di questo miglioramento delle prestazioni.

## at.js versione 2.1.1 (24 luglio 2019)

Questa release di at.js è una versione di manutenzione e include i miglioramenti e le correzioni seguenti:

(I codici tra parentesi sono per uso interno di Adobe.)

* È stato risolto un problema a causa del quale venivano attivati più beacon quando si utilizzava la metrica Tracciamento dei clic nella pagina Obiettivi e impostazioni nel Compositore esperienza visivo. (TNT-32812)
* È stato corretto un problema a causa del quale `triggerView()` non poteva eseguire il rendering delle offerte più di una volta. (TNT-32780)
* È stato corretto un problema relativo a `triggerView()` per fare sì che la richiesta contenga informazioni Experience Cloud ID (ECID). (TNT-32776)
* È stato risolto un problema a causa del quale la notifica `triggerView()` non veniva attivata nonostante l’assenza di visualizzazioni salvate. (TNT-32614)
* È stato risolto un problema che provocava un errore a causa dell’utilizzo di decodeURIcomponent e si verificavano problemi se l’URL conteneva un parametro di stringa query non valido. (TNT-32710)
* Il flag beacon ora è impostato su “true” nel contesto di richieste di consegna inviate tramite l’API `Navigator.sendBeacon()`. (TNT-32683)
* È stato risolto un problema che, per alcuni clienti, impediva la visualizzazione delle offerte Consigli sui siti web. I clienti potevano vedere il contenuto dell’offerta nella chiamata API di consegna, ma l’offerta non veniva applicata al sito web. (TNT-32680)
* È stato risolto un problema di funzionamento imprevisto del tracciamento dei clic tra più esperienze. (TNT-32644)
* È stato risolto un problema che impediva ad at.js di applicare la seconda metrica dopo un errore nel rendering della prima metrica. (TNT-32628)
* È stato risolto un problema nel passare `mbox3rdPartyId` utilizzando la funzione `targetPageParams`, a causa del quale il payload della richiesta non veniva incluso nei parametri della query o nel payload della richiesta. (TNT-32613)
* È stato risolto un problema a causa del quale, nei browser basati su Chromium (incluso Google Chrome), venivano bloccate le risposte alle notifiche di visualizzazione o clic. (TNT-32290)

## at.js versione 2.1.0 (3 giugno 2019)

Questa versione include i miglioramenti e le funzioni seguenti:

* **Supporto di Adobe Opt-in**: Adobe Opt-in è un metodo per semplificare le integrazioni delle soluzioni Adobe con le piattaforme di gestione del consenso. Per ulteriori informazioni su Adobe Opt-in, consulta [Privacy e Regolamento generale sulla protezione dei dati (RGPD)](/help/dev/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.md).

* **Conformità allo standard di settore CSP**: at.js non utilizza più eval() per eseguire JavaScript.

* **Registrazione analisi lato client**: fornisci ai clienti pieno controllo su come desiderano inviare dati analitici a [!DNL Adobe Analytics], lato client o lato server.

  Per ulteriori informazioni, consulta [Lato client [!DNL Analytics] registrazione](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/before-implement.html#client-side).

* **Invio di notifiche**: consenti agli sviluppatori di inviare notifiche quando un’esperienza viene riprodotto dal codice anziché utilizzando `applyOffer()` o `applyOffers()`.

  Per ulteriori informazioni, consulta [adobe.target.sendNotifications(options)](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-sendnotifications-atjs-21.md).

* **Dimensione di at.js ridotta di circa il 24%**: la dimensione di at.js è stata ridotta di circa il 24%. Le dimensioni ridotte del file migliorano le prestazioni di caricamento delle pagine e riducono il tempo necessario al download di at.js sulla pagina.

## at.js versione 2.0.1 (19 marzo 2019)

Questa è una versione di manutenzione e include i miglioramenti e le correzioni seguenti:

(I codici tra parentesi sono per uso interno di Adobe.)

* È stata risolta una situazione di tipo “race condition” nel codice di polling DOM che causava eccezioni JavaScript per alcuni clienti. (TNT-31869)
* Le notifiche di rendering delle visualizzazioni sono state disaccoppiate dai gestori di eventi di tracciamento dei clic. Inizialmente, [!DNL Target] non ha inviato notifiche se non è stato possibile allegare gestori di eventi di clic appartenenti a una visualizzazione sottoposta a rendering. [!DNL Target] invia ora una notifica della visualizzazione anche quando non vengono trovati elementi di clic. (TNT-31969)
* È stato risolto un problema per cui il flag di reindirizzamento dell’evento della richiesta riuscita era sempre impostato su true. (TNT-31907)
* È stato risolto un problema che causava la registrazione con successo dell’azione di riorganizzazione del Compositore esperienza visivo, anche quando mancavano elementi. (TNT-31924)
* È stato risolto un problema che causava la mancata inclusione del token di proprietà delle Autorizzazioni Enterprise per alcuni clienti. (TNT-31999)

## at.js versione 1.7.1 (19 marzo 2019)

Questa è una versione di manutenzione e include la seguente correzione:

(I codici tra parentesi sono per uso interno di Adobe.)

* È stata risolta una situazione di tipo “race condition” nel codice di polling DOM che causava eccezioni JavaScript per alcuni clienti. (TNT-31869)

## at.js versione 2.0.0

at.js 2.x offre set di funzioni avanzate che consentono di eseguire personalizzazioni su tecnologie lato client di nuova generazione. Questa nuova versione si concentra sull&#39;aggiornamento di at.js per garantire interazioni in sintonia con le applicazioni a pagina singola.

Di seguito sono riportati alcuni vantaggi dell’utilizzo di at.js 2.x che non sono disponibili nelle versioni precedenti:

* La capacità di memorizzare nella cache tutte le offerte al caricamento di pagina per ridurre più chiamate al server a una singola chiamata al server.
* Migliora enormemente le esperienze degli utenti finali sul sito, in quanto le offerte appaiono immediatamente tramite la cache senza l’implementazione di chiamate al server tradizionali.
* Una semplice riga di codice e una configurazione per sviluppatori una tantum per consentire agli esperti di marketing di creare ed eseguire attività A/B e di esperienza (XT) tramite il Compositore esperienza visivo sulle applicazioni a pagina singola.

at.js 2.x introduce le seguenti nuove funzioni:

* getOffers()
* applyOffers()
* triggerView()

Le seguenti funzioni sono state rese obsolete con l’introduzione di at.js 2.x:

* mboxCreate()
* mboxDefine
* registerExtension()

Per ulteriori informazioni, consulta [Aggiornamento da at.js 1.x a at.js 2.x](/help/dev/implement/client-side/atjs/upgrading-from-atjs-1x-to-atjs-20.md) e [Funzioni di at.js](/help/dev/implement/client-side/atjs/atjs-functions/atjs-functions.md).

>[!NOTE]
>
>Se hai bisogno del supporto di Adobe Opt-in per [Regolamento generale sulla protezione dei dati](/help/dev/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.md) (RGPD), al momento devi utilizzare at.js 1.7.0 o at.js 2.1.0 o versione successiva.

## at.js versione 1.7.0

at.js 1.7.0 include il supporto di Adobe Opt-In. Adobe Opt-in è un metodo per semplificare le integrazioni delle soluzioni Adobe con le piattaforme di gestione del consenso.

Per ulteriori informazioni su Adobe Opt-in, consulta [Privacy e Regolamento generale sulla protezione dei dati](/help/dev/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.md) (RGPD).

Questa versione risolve anche un problema in cui [!DNL Target] potrebbe sostituire i parametri degli URL di reindirizzamento con parametri provenienti dall’URL di reindirizzamento.

>[!NOTE]
>
>Se hai bisogno del supporto di Adobe Opt-In per rispettare i requisiti del regolamento RGPD, al momento devi utilizzare at.js 1.7.0, at.js 2.1.0 o versione successiva.

## at.js versione 1.6.4

at.js 1.6.4 è una versione di manutenzione e risolve il seguente problema:

* È stata corretta una situazione di tipo “race condition” critica in Microsoft Internet Explorer 11 che causava l’applicazione di offerte duplicate.

## at.js versione 1.6.3

at.js versione 1.6.3 include le correzioni e i miglioramenti seguenti:

* I selettori sono ora in sequenza CSS se contengono ID o classi CSS che iniziano con una cifra, due trattini o un trattino seguito da una cifra (ad esempio #-123). (TNT-31061)
* Risolto un problema introdotto con at.js 1.6.2 per cui le offerte del Compositore esperienza visivo di diverse attività che si applicavano allo stesso selettore CSS non rispettavano la priorità delle attività. (TNT-31052)
* Corretto un problema con il timeout di una promessa in ambienti in cui non era presente alcun supporto nativo per le promesse. (TNT-30974)
* L’acquisizione e la segnalazione dei problemi ora avviene correttamente tramite l’evento di rendering del contenuto non riuscito. In precedenza, il sistema poteva segnalare la corretta esecuzione di JavaScript anche in caso contrario. (TNT-30599)

## at.js versione 1.6.2

Questa è una versione di manutenzione e risolve il seguente problema:

* Risolto un problema che portava a un ciclo “async” infinito in alcuni siti clienti.

>[!WARNING]
>
>Inoltre, la versione 1.6.2 di at.js contiene tutti i miglioramenti e le correzioni inclusi nelle versioni 1.6.1 e 1.6.0 di at.js. Queste versioni non sono più disponibili per il download. Si consiglia di eseguire l&#39;aggiornamento alla versione 1.6.2 se si utilizza la versione 1.6.1 o 1.6.0

Ecco i miglioramenti e le correzioni che sono stati inclusi nella versione 1.6.1 di at.js:

* Risolto un problema in at.js 1.6.0 che causava la duplicazione delle esperienze dei consigli in Microsoft Internet Explorer 11. (TNT-30593)
* at.js ora assicura che la logica di sovrascrittura Edge verifichi l&#39;esistenza di un cookie del cluster Edge per evitare un numero di bordi diverso se un utente salta i bordi durante una sessione. (TNT-30563)
* Risolto un problema che impediva ad at.js di eseguire azioni successive se il contenuto HTML conteneva codice JS non valido. at.js ora registra l&#39;errore ed effettua il rendering delle azioni rimanenti senza problemi. (TNT-30546)
* Modifiche apportate affinché vi sia un&#39;eccezione quando una pagina di reindirizzamento si riqualifica per un&#39;attività di reindirizzamento. (TNT-30532)
* Risolto un problema che impediva la propagazione del timeout della richiesta corretta dalla richiesta API getOffer(). (TNT-30498)
* Risolto un problema che impediva ad at.js 1.6.0 di salvare i cookie durante l&#39;utilizzo del protocollo file. (TNT-30454)
* È stato risolto un problema a causa del quale non tutte le esperienze venivano distribuite con i reindirizzamenti durante l’utilizzo di [!DNL Analytics for Target] (A4T). (TNT-30444)
* È stato risolto un problema che causava la visualizzazione della pagina dopo il [!DNL Target] chiamata riuscita. (TNT-30358)

Ecco i miglioramenti e le correzioni che sono stati inclusi nella versione 1.6.0 di at.js:

* Le offerte di reindirizzamento ora sono supportate automaticamente nella [!UICONTROL Analytics for Target] (A4T). La soluzione lato client è stata rimossa. (TNT-30247)
* Il routing Edge lato client è ora abilitato per impostazione predefinita. (TNT-30261)
* Risolto un problema con il rendering delle azioni del Compositore esperienza visivo in presenza di dipendenze tra le azioni. (TNT-30248)

## at.js versione 1.5.0

È ora disponibile la versione 1.5.0 di at.js.

* I dettagli dell’evento `at-request-succeeded` contengono il flag di reindirizzamento. Questo flag può essere utilizzato per determinare se la pagina verrà reindirizzata a un URL diverso. Se desideri conoscere l&#39;URL, abbonati a `at-content-rendering-redirect`. (TNT-29834)
* È stato risolto un problema che ha causato il mancato funzionamento di `window.targetGlobalSettings.enabled` con un&#39;eccezione di esecuzione se è stato impostato su falso. (TNT-29829)
* È stato risolto un problema che ha causato un errore durante il caricamento nel Compositore esperienza visivo se si utilizza il codice personalizzato per una richiesta mbox globale di abilitazione e si nasconde il corpo. (TNT-29795)
* È stato aggiunto il supporto per `screenOrientation`, `devicePixelRatio` e `webGLRenderer`. Questi nuovi [!DNL Target] I parametri di richiesta vengono utilizzati per iPhone X e altri sistemi di rilevamento dispositivi moderni. Per ulteriori informazioni, consulta [Mobile](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html). (TNT-29781)
* È stato risolto un problema per cui il suggerimento di posizione di Adobe Audience Manager (AAM) non veniva sempre inviato. (TNT-29695)
* Per i browser che lo supportano, at.js 1.5.0 passa a MutationObserver per la votazione del selettore. Le versioni precedenti a at.js 1.0.0 utilizzavano un polyfill MutationObserver, che si è dimostrato problematico. Per evitare problemi di polyfill, la versione 1.5.0 utilizza il seguente pseudo codice per decidere quale meccanismo di programmazione utilizzare:

  ```
  if MutationObserver is supported
    scheduler = MutationObserver
  else if document is visible
    scheduler = requestAnimationFrame
  else
    scheduler = setTimeout
  ```

## at.js versione 1.3.0

È ora disponibile la versione 1.3.0 di at.js.

* I seguenti nuovi eventi sono disponibili per facilitare il tracciamento, il debug e la personalizzazione dell’interazione con at.js:

   * LIBRARY_LOADED
   * REQUEST_START
   * CONTENT_RENDERING_START
   * CONTENT_RENDERING_NO_OFFERS
   * CONTENT_RENDERING_REDIRECT

  Per ulteriori informazioni, consulta [Eventi personalizzati at.js](/help/dev/implement/client-side/atjs/atjs-functions/atjs-custom-events.md).

* È possibile integrare una richiesta at.js con parametri aggiuntivi provenienti da provider di dati. Aggiungi i provider di dati a `window.targetGlobalSettings` nella `dataProviders key`.

  Per ulteriori informazioni, vedi [Provider di dati](atjs-functions/targetglobalsettings.md#data-providers).

* Le richieste at.js ora utilizzano GET, ma passeranno a POST quando la dimensione dell’URL supererà i 2048 caratteri. La nuova proprietà `urlSizeLimit` consente di aumentare questo limite, se necessario. Questa modifica consente [!DNL Target] per allineare at.js a AppMeasurement, che utilizza la stessa tecnica.
* [!DNL Target] ora impone di utilizzare la chiave `mbox` nella funzione `adobe.target.applyOffer(options)`. Questa chiave era già richiesta in passato, ma [!DNL Target] ora ne impone l’utilizzo per garantire che [!DNL Target] dispone di una convalida corretta e i clienti utilizzano correttamente la funzione.
* at.js offre una funzionalità migliorata per il tracciamento di eventi e clic. at.js utilizza `navigator.sendBeacon()` per inviare i dati di tracciamento degli eventi; se `navigator.sendBeacon()` non è supportato, si basa su richieste XHR sincrone. Questa alternativa riguarda principalmente Internet Explorer 10 e 11 e alcune versioni di Safari. Con iOS 11.3, in Safari verrà aggiunto il supporto di `navigator.sendBeacon()`.
* at.js ora può renderizzare le offerte anche quando una pagina viene aperta nelle schede in background. Alcuni [!DNL Target] I clienti hanno riscontrato un problema quando `requestAnimationFrame()` è stato disattivato a causa del comportamento di limitazione del browser per le schede in background.
* In questa versione sono stati aggiunti svariati miglioramenti a livello di prestazioni, tra cui call stack più brevi durante l’ispezione di un profilo CPU in Chrome.
* at.js 1.3.0 non supporta più la consegna di contenuti in Microsoft Internet Explorer 9. Per un elenco dei browser supportati, consulta [Browser supportati](/help/dev/before-implement/supported-browsers.md). Da adesso in poi, tutte le richieste vengono eseguite tramite `XMLHttpRequest` con supporto CORS, senza richieste JSONP. Questa modifica migliora notevolmente la sicurezza.

## at.js versione 1.2.3 

È ora disponibile la versione 1.2.3 di at.js.

* Aggiunge il supporto per le offerte JSON. Le offerte JSON sono supportate solo nelle attività create utilizzando il Compositore esperienza basato su moduli. Attualmente le offerte JSON possono essere utilizzate solo tramite chiamate API dirette. Consulta [Creare offerte JSON](https://experienceleague.adobe.com/docs/target/using/experiences/offers/create-json-offer.html).

## at.js versione 1.2.2

È ora disponibile la versione 1.2.2 di at.js.

* È stato risolto un problema che restituiva un errore JavaScript quando il [!DNL Target] libreria caricata su una pagina utilizzando la modalità QUIRKS. (TNT-28312)
* È stato risolto un problema che causava [!DNL Target] tracciamento dei clic per interrompere [!DNL Analytics] chiamate di raccolta dati. (TNT-28261)
* È stato corretto un problema che impediva il corretto funzionamento di `getOffer() params` se `targetPageParams()` restituiva una stringa vuota. (TNT-28359)
* È stato risolto un problema nella generazione dell’ID di sessione quando si utilizza solo x. (TNT-28361)

## at.js versione 1.2.1 

È ora disponibile la versione 1.2.1 di at.js.

* È stato risolto un problema che impediva il tracciamento dei clic su un collegamento con target=&quot;_blank&quot;. [!DNL Target] dall’apertura del collegamento in una nuova scheda.

## at.js versione 1.2.0

La versione 1.2 di at.js è ora disponibile in una versione di manutenzione che contiene per lo più correzioni di bug. 

* È stato risolto un problema che impediva azioni predefinite per i casi speciali di tracciamento dei clic. (TNT-28089)
* È stato risolto un problema che impediva a di aprire il collegamento in una nuova scheda durante il tracciamento dei clic con un collegamento contenente `target="_blank"`. [!DNL Target] (TNT-28072)
* Gli indirizzi IP possono essere utilizzati come dominio dei cookie. (TNT-28002)
* È stato risolto un problema che causava la visualizzazione momentanea di altro contenuto nelle offerte di reindirizzamento aventi una mbox globale o altre mbox regionali. (TNT-27978)
* È stato risolto un problema in [!UICONTROL Targeting esperienza] Configurazione dell’attività non riuscita nel Compositore esperienza visivo quando si passa da Sfoglia a Componi. (TNT-27942)
* È stata risolta la gestione errata delle classi di stile che causava la visualizzazione momentanea di altro contenuto sugli elementi contrassegnati per il monitoraggio dei clic. (TNT-27896)
* È stato corretto un problema a causa del quale i parametri mbox globali si mescolavano con gli altri parametri mbox. (TNT-27846)
* Sono state apportate modifiche per garantire che Handlebar, Mustache e altre librerie di modelli lato client siano gestite correttamente da at.js. (TNT-27831)
* Sono state apportate modifiche per garantire che `sdidParamExpiry` sia inizializzato correttamente e passato all’API visitatore. Questa è una regressione che è stata aggiunta a `at.js 1.1.0`. Non influisce sulle versioni precedenti di at.js. Questa modifica riguarda soltanto i clienti che usano offerte di reindirizzamento e A4T. (TNT-27791)
* Sono state apportate modifiche per garantire che `SCRIPT` venga eseguito indipendentemente dal tipo di attributo utilizzato. (TNT-27865)

## at.js versione 1.1.0 

**Data:** 2 agosto 2017

I miglioramenti e le correzioni seguenti sono inclusi nella versione 1.1 di at.js:

* È stata aggiunta la gestione dei token di risposta. Per ulteriori informazioni, consulta [Token di risposta](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html).
* È stato risolto un problema in modo che `document.currentScript polyfill` non interferisca con Angular 1.X.
* Sono state apportate modifiche per garantire che il tracciamento dei clic non interferisca con la proprietà di visibilità. Gli elementi di tracciamento dei clic sono contrassegnati con la classe CSS `at-element-click-tracking` anziché `at-element-marker`.

## at.js versione 1.0.0

**Data:** 7 luglio 2017

I miglioramenti e le correzioni seguenti sono inclusi nella versione 1.0 di at.js:

* Supporto per il caricamento in modo asincrono di at.js per caricare le pagine più velocemente.
* Supporto che consente di pre-nascondere il contenuto della pagina durante il caricamento di at.js in modo asincrono.
* Miglioramento della messaggistica di errore quando la consegna del contenuto è disattivata.
* Miglioramenti delle prestazioni durante la distribuzione di più attività.
* Supporto per il compressore YUI.
* Segnalazione di bug ed errori per eventi personalizzati durante la consegna delle attività.
* Correzione di problemi di prestazione in Microsoft Internet Explorer 11.
* Risoluzione della funzione `getOffer()` che dava un errore su alcuni siti web.
* Carica [!DNL Target] libreria in modo asincrono. Per ulteriori informazioni, consulta [Domande frequenti su at.js](/help/dev/implement/client-side/atjs/target-atjs-faq.md).

## at.js versione 0.9.7

**Data:** 22 maggio 2017

I miglioramenti e le correzioni seguenti sono inclusi nella versione 0.9.7 di at.js:

* Correzione di un problema relativo a una chiave di asset mancante dalle azioni `insertAfter` e `insertBefore` nel Compositore esperienza visivo. Questi problemi erano correlati alla migrazione da offerte visive a modelli di offerte.

## at.js versione 0.9.6 

**Data:** 13 aprile 2017

I miglioramenti e le correzioni seguenti sono inclusi nella versione 0.9.6 di at.js:

* Supporto per le offerte di reindirizzamento per A4T. Dopo aver scaricato e installato la versione 0.9.6 di at.js, è possibile utilizzare le offerte di reindirizzamento in attività che utilizzano [!UICONTROL Adobe Analytics come origine per la generazione di rapporti per Target] (A4T). Oltre alla versione 0.9.6 di at.js, l’implementazione deve soddisfare altri requisiti minimi al fine di usare le offerte di reindirizzamento e A4T. Per ulteriori informazioni e importanti informazioni aggiuntive che dovresti conoscere, consulta [Offerte di reindirizzamento: domande frequenti su A4T](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-faq/a4t-faq-redirect-offers.html).
* Prima della versione 0.9.6 di at.js, quando l’API visitatore era presente nella pagina e `visitorApiTimeout` impostazione troppo aggressiva, [!DNL Target] potrebbe verificarsi una situazione in cui non sono stati inviati dati MCID nel [!DNL Target] richiesta. Questo poteva portare a problemi come la presenza di dati parziali in [!DNL Analytics] durante l’utilizzo di A4T.

  Questo comportamento è stato modificato nella versione 0.9.6 di at.js, anche se `visitorApiTimeout` è impostato su 1 ms, [!DNL Target] tenterà di raccogliere SDID, server di tracciamento e dati degli ID cliente, per inviarli nel [!DNL Target] richiesta.

* È stata aggiunta l’impostazione `selectorsPollingTimeout`. Per ulteriori informazioni, consulta [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md).
* Il formato della risposta da `getOffer()` è stato modificato. Per ulteriori informazioni, consulta [adobe.target.getOffer(options)](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffer.md).
* La funzione di log della console è stata aggiunta per le dichiarazioni `<!DOCTYPE>` non supportate.
* È stato risolto un problema a causa del quale [!DNL Target Classic] i plug-in non venivano applicati correttamente quando venivano distribuite offerte multiple predefinite a un singolo mbox. (TGT-22664)
* È stata migliorata la configurazione dei cookie per i domini di primo livello a due lettere (TLD) per garantire che il cookie mbox venga configurato correttamente per questi domini (ad esempio, test.no, autodrives.ca e così via).
* L’algoritmo per l’estrazione del dominio di primo livello che deve essere utilizzato quando si salvano i cookie è stato modificato nella versione 0.9.6. di at.js. A causa di questa modifica, i cookie non possono essere salvati in indirizzi IP. Il più delle volte, gli indirizzi IP vengono utilizzati per scopi di test, ma come soluzioni alternative è possibile utilizzare le voci DNS, regolare il file host in una casella locale.
* Risolta la gestione delle azioni Sposta e Ridisponi nel caso in cui le proprietà siano valori stringa anziché numeri interi.

## at.js versione 0.9.4

**Data:** 19 gennaio 2017

* I nomi mbox possono ora contenere caratteri speciali, tra cui il simbolo e commerciale (&amp;). 

  Per un elenco dei caratteri speciali consentiti, consulta [Configurazione at.js](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md).

* È stata aggiunta l’impostazione `secureOnly` che indica se at.js deve utilizzare solo HTTPS o può passare da HTTP a HTTPS in base al protocollo della pagina. Si tratta di un’impostazione avanzata con impostazione predefinita False e può essere bypassata tramite `targetGlobalSettings`.
* L&#39;opzione Supporto di browser legacy è disponibile nelle versioni 0.9.3 e precedenti di at.js. Questa opzione è stata rimossa nella versione 0.9.4 di at.js.

## at.js versione 0.9.3

**Data:** 10 ottobre 2016

* Le chiamate mbox si attivano in Microsoft Internet Explorer 11 quando i browser legacy sono disabilitati nelle impostazioni at.js.
* Il contenuto predefinito viene renderizzato in caso di mancata riuscita di un’offerta remota dinamica (ad esempio, se l’URL non è corretto e genera un errore 404).
* Gli elementi vengono visualizzati rapidamente qualora non sia possibile trovare nel DOM i selettori per il tracciamento dei clic del Compositore esperienza visivo.

## at.js versione 0.9.2

**Data:** 21 settembre 2016

* È stata aggiunta un’impostazione `optoutEnabled` per abilitare o disabilitare la rinuncia a Device Graph. Se questa impostazione si trova su `true` e il visitatore ha rinunciato al monitoraggio, il browser del visitatore non effettuerà chiamate mbox. Device Graph è attualmente in versione beta. Questa impostazione si trova su `false` come impostazione predefinita ma deve trovarsi su `true` se si sta utilizzando Device Graph (Grafico dispositivo). 
* È stato aggiunto il supporto di `CustomEvent` per il meccanismo di notifica. In precedenza, il meccanismo di notifica degli eventi at.js non poteva essere utilizzato tramite API DOM standard, come ad esempio `document.addEventListener()`. Ora è possibile utilizzare `document.addEventListener()` per effettuare la sottoscrizione a eventi at.js, come ad esempio eventi di richiesta e di rendering del contenuto.
* È stato risolto un problema relativo alle offerte create nel Compositore esperienza visivo. Prima di questa versione, [!DNL Target] nasconde i selettori e li mostra solo quando tutti i selettori corrispondono. In at.js 0.9.2 [!DNL Target] mostra i selettori non appena corrispondono.

## at.js versione 0.9.1

**Data:** 14 luglio 2016

* Fornisce a at.js un timeout per il servizio ID visitatore, indipendente dal timeout proprio del servizio.
* Corregge un problema in 0.9.0 che interessava le implementazioni che utilizzavano at.js su alcune pagine e mbox.js (ora obsoleto) su altre pagine.
* Se usa [!DNL Adobe Analytics] come origine per la generazione di rapporti dell’attività, non è necessario specificare un server di tracciamento durante la creazione di attività se si utilizza mbox.js versione 61 (o successiva) o at.js versione 0.9.1 (o successiva). La libreria at.js invia automaticamente i valori del server di tracciamento a [!DNL Target]. Durante la creazione di attività, puoi lasciare vuoto il campo Server di monitoraggio nella pagina Obiettivi e impostazioni.

## at.js versione 0.9.0

**[!DNL Target]Versione:** 16.6.1.

**Data:** 23 giugno 2016

* Corregge un problema di schermo bianco quando si utilizzano le offerte di Compositore esperienza visivo. Chiunque utilizzi at.js dovrebbe effettuare l’aggiornamento a questa nuova versione.
* Nuova API `registerExtension`.

  Questa nuova API fornisce agli sviluppatori l’accesso a determinati moduli jQuery utilizzati in at.js per sviluppare estensioni (plug-in) per la libreria. Ci sono alcune implicazioni per questo cambiamento. Questo impatta solo gli utenti che utilizzano le seguenti funzionalità:

   * L’API `getSettings()` è stata rimossa, ma la stessa funzionalità è disponibile utilizzando `registerExtension()`.
   * L’API `getTracking()` è stata rimossa, ma la stessa funzionalità è disponibile utilizzando `registerExtension()`.

   * Le estensioni esistenti (ad esempio le estensioni AngularJS) devono essere aggiornate per utilizzare l’approccio `registerExtension()`.

* Nuova API di notifica at.js.

  L&#39;obiettivo di questo sistema di notifica è quello di fornire ulteriori informazioni su ciò che at.js sta facendo sulla pagina e quando si verificano dei problemi. Un problema comune riscontrato con Compositore esperienza visivo si verifica quando una nuova versione IT modifica la pagina, un selettore del compositore non funziona più correttamente e il test smette di consegnare correttamente il contenuto. Un obiettivo di questo sistema di notifica è quello di rendere noto alla pagina questo problema di consegna, così gli sviluppatori possono accedere a tali informazioni, passarle a un sistema come [!DNL Adobe Analytics] e avvisare i responsabili business che il loro test non ha funzionato.

* Nuovo metodo API `targetGlobalSettings()`.

  È possibile modificare le impostazioni nella libreria at.js anziché configurarle nella [!DNL Target Standard/Premium] Interfaccia utente o utilizzando le API REST.

## at.js versione 0.8.0

**Data:** 5 maggio 2016.

Questa è la prima versione ufficiale della libreria at.js.

at.js è una nuova libreria di implementazione per [!DNL Target], progettata sia per le tipiche implementazioni web sia per le applicazioni a pagina singola.

at.js sostituisce mbox.js per le implementazioni di [!DNL Adobe Target].

Ad esempio, at.js migliora i tempi di caricamento delle pagine per le implementazioni web, migliora la sicurezza e fornisce migliori opzioni di implementazione per le applicazioni a pagina singola.

La libreria at.js include anche i componenti inclusi in target.js, quindi cessano le chiamate a target.js.

Quando implementi at.js, tieni presente quanto segue:

* Le versioni di Internet Explorer precedenti a 8 non sono supportate.
* L’implementazione asincrona potrebbe impedire il funzionamento delle integrazioni legacy come il plug-in [!UICONTROL Test&amp;Target a SiteCatalyst].
* [!DNL Target]I plug-in che fanno riferimento a oggetti e metodi mbox.js non sono supportati.
* Tutte le chiamate a [!DNL Target] vengono effettuate tramite XMLHTTPRequest e il contenuto viene restituito tramite JSON.
