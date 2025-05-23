---
keywords: versioni at.js, versioni at.js, app a pagina singola, spa, domini diversi, tra domini diversi
description: Scopri come aggiornare da [!DNL Adobe Target] at.js 1.x a at.js 2.x. Esamina i diagrammi di flusso del sistema, scopri le funzioni nuove e obsolete e altro ancora.
title: Come posso effettuare l’aggiornamento da at.js versione 1.x alla versione 2.x?
feature: at.js
exl-id: fbfa5743-0fa5-44c6-89b3-fdee9b50e126
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '2939'
ht-degree: 57%

---

# Aggiornamento da at.js 1.*x* in at.js 2.*x*

La versione più recente di at.js in [!DNL Adobe Target] offre set di funzioni avanzate che consentono di eseguire personalizzazioni su tecnologie lato client di nuova generazione. Questa nuova versione si concentra sull&#39;aggiornamento di at.js per garantire interazioni in sintonia con le applicazioni a pagina singola.

Di seguito sono riportati alcuni vantaggi dell’utilizzo di at.js 2.*x* non disponibili nelle versioni precedenti:

* La capacità di memorizzare nella cache tutte le offerte al caricamento di pagina per ridurre più chiamate al server a una singola chiamata al server.
* Migliora enormemente le esperienze degli utenti finali sul sito, in quanto le offerte appaiono immediatamente tramite la cache senza l’implementazione di chiamate al server tradizionali.
* Una semplice riga di codice e una configurazione per sviluppatori una tantum per consentire agli esperti di marketing di creare ed eseguire attività [!UICONTROL A/B Test] e [!UICONTROL Experience Targeting] (XT) tramite il Compositore esperienza visivo sull’SPA.

## at.js 2.*x* diagrammi di sistema

I seguenti diagrammi ti aiutano a comprendere il flusso di lavoro di at.js 2.*x* con visualizzazioni e miglioramento dell&#39;integrazione SPA. Per una migliore introduzione dei concetti utilizzati in at.js 2.*x*, vedi [Implementazione di un&#39;applicazione a pagina singola](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md).

(Fare clic sull&#39;immagine per espanderla a larghezza intera.)

![Flusso di Target con at.js 2.x](/help/dev/implement/client-side/assets/system-diagram-atjs-20.png "Flusso di Target con at.js 2.x"){zoomable="yes"}

| Chiamata | Dettagli |
| --- | --- |
| 1 | La chiamata restituisce l&#39;[!UICONTROL Experience Cloud ID] se l’utente è autenticato; un’altra chiamata sincronizza l’ID cliente. |
| 2 | La libreria at.js viene caricata in modo sincrono e nasconde il corpo del documento.<P>at.js si carica anche in modo asincrono con un’opzione che nasconde lo snippet implementato sulla pagina. |
| 3 | Si effettua una richiesta di caricamento della pagina, con tutti i parametri configurati (MCID, SDID e ID cliente). |
| 4 | Gli script di profilo vengono eseguiti e quindi inseriti nell’archivio profili. L&#39;archivio richiede tipi di pubblico idonei dalla libreria Pubblico (ad esempio, pubblici condivisi da [!DNL Adobe Analytics], [!DNL Audience Manager], ecc.).<P>Gli attributi del cliente vengono inviati all’archivio profili in un processo batch. |
| 5 | In base ai parametri di richiesta dell’URL e ai dati di profilo, [!DNL Target] determina le attività ed esperienze da restituire al visitatore per la pagina corrente e le visualizzazioni future. |
| 6 | Il contenuto di destinazione viene rinviato alla pagina, includendo facoltativamente i valori di profilo per ulteriore personalizzazione.<P>Il contenuto mirato sulla pagina corrente viene mostrato il più rapidamente possibile senza che venga visualizzato momentaneamente il contenuto predefinito.<P>Contenuto mirato per le viste mostrate come risultato delle azioni dell&#39;utente in un SPA memorizzato nella cache del browser, in modo da applicarlo immediatamente senza una chiamata al server aggiuntiva quando si attivano le viste tramite `triggerView()`. |
| 7 | I dati [!UICONTROL Analytics] vengono inviati ai server di raccolta dati. |
| 8 | I dati di destinazione vengono confrontati con i dati [!UICONTROL Analytics] tramite SDID ed elaborati nell&#39;archivio dei report [!UICONTROL Analytics].<P>I dati di [!UICONTROL Analytics] possono quindi essere visualizzati sia in [!UICONTROL Analytics] che in [!DNL Target] tramite i report [!UICONTROL Analytics for Target] (A4T). |

Ora, ovunque si implementi `triggerView()` nell’applicazione a pagina singola, le visualizzazioni e le azioni vengono recuperate dalla cache e mostrate all’utente senza una chiamata al server. `triggerView()` invia anche una richiesta di notifica al backend [!DNL Target] per incrementare e registrare i conteggi delle impression.

(Fare clic sull&#39;immagine per espanderla a larghezza intera.)

![Flusso di Target at.js 2.*x* triggerView](/help/dev/implement/client-side/assets/atjs-20-triggerview.png "Flusso di Target at.js 2.*x* triggerView"){zoomable="yes"}

| Chiamata | Dettagli |
| --- | --- |
| 1 | Si richiama `triggerView()` nell’applicazione a pagina singola per eseguire il rendering della visualizzazione e applicare azioni per modificare gli elementi visuali. |
| 2 | Il contenuto mirato per la visualizzazione viene letto dalla cache. |
| 3 | Il contenuto mirato viene mostrato il più rapidamente possibile senza che venga visualizzato momentaneamente il contenuto predefinito. |
| 4 | Si invia la richiesta di notifica all&#39;archivio profili di [!DNL Target] per conteggiare il visitatore nell&#39;attività e nelle metriche incrementali. |
| 5 | [!UICONTROL Analytics] dati inviati ai server di raccolta dati. |
| 6 | I dati di [!DNL Target] corrispondono ai dati di [!UICONTROL Analytics] tramite SDID ed vengono elaborati nell&#39;archivio dei report di [!UICONTROL Analytics]. I dati di [!UICONTROL Analytics] possono quindi essere visualizzati sia in [!UICONTROL Analytics] che in [!DNL Target] tramite i rapporti A4T. |

## Implementare at.js 2.*x*

Implementare at.js 2.*x* tramite tag nell&#39;estensione [Adobe Experience Platform](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md).

>[!NOTE]
>
>La distribuzione di at.js utilizzando i tag in [!DNL Adobe Experience Platform] è il metodo preferito.
>
>Oppure
>
>Scarica manualmente at.js 2.*x* utilizzando l&#39;interfaccia utente [!DNL Target] e distribuendola con il metodo [scelto](/help/dev/implement/client-side/atjs/how-to-deployatjs/how-to-deployatjs.md).

## Funzioni at.js obsolete

In at.js 2 sono state rimosse diverse funzioni.*x*.

>[!WARNING]
>
>Se queste funzioni obsolete sono ancora in uso sul tuo sito quando utilizzi at.js 2.*x* è distribuito. Verranno visualizzati avvisi nella console. L’approccio consigliato durante l’aggiornamento consiste nel testare la distribuzione di at.js 2.*x* in un ambiente di pre-produzione e assicurati di controllare tutti gli avvisi registrati nella console e tradurre le funzioni obsolete in nuove funzioni introdotte in at.js 2.*x*.

Di seguito puoi trovare le funzioni obsolete e i loro equivalenti. Per un elenco completo delle funzioni, consulta [Funzioni di at.js](/help/dev/implement/client-side/atjs/atjs-functions/atjs-functions.md).

>[!NOTE]
>
>at.js 2.*x* non nasconde più preventivamente in automatico `mboxDefault` elementi contrassegnati. I clienti dovranno quindi nasconderli manualmente sul sito o tramite uno strumento di gestione dei tag.

### mboxCreate(mbox,params)

**Descrizione**:

Esegue una richiesta e applica l’offerta all’elemento DIV più vicino con il nome della classe `mboxDefault`.

**Esempio**:

```html {line-numbers="true"}
<div class="mboxDefault">
  default content to replace by offer
</div>
<script>
  mboxCreate('mboxName','param1=value1','param2=value2');
</script>
```

**Equivalente in at.js 2.*x***

Un’alternativa a `mboxCreate(mbox, params)` è `getOffer()` e `applyOffer()`.

**Esempio**:

```html {line-numbers="true"}
<div class="mboxDefault"> 
  default content to replace by offer 
</div> 
<script> 
  var el = document.currentScript.previousElementSibling;
  adobe.target.getOffer({
    mbox: "mboxName",
    params: {
      param1: "value1",
      param2: "value2"
    },
    success: function(offer) {
      adobe.target.applyOffer({
        mbox: "mboxName",
        selector: el,
        offer: offer
      });
    },
    error: function(error) {
      console.error(error);
      el.style.visibility = "visible";
    }
  });
</script> 
```

### mboxDefine() e mboxUpdate()

**Descrizione**:

Crea una mappatura interna tra un elemento e un nome mbox, ma non esegue la richiesta. Utilizzata insieme a `mboxUpdate()`, che esegue la richiesta e applica l’offerta all’elemento identificato dal nodeId in `mboxDefine()`. Può essere utilizzato anche per aggiornare una mbox iniziata da `mboxCreate`.

**Esempio**:

```html {line-numbers="true"}
<div id="someId" class="mboxDefault"></div>
<script>
 mboxDefine('someId','mboxName','param1=value1','param2=value2');
 mboxUpdate('mboxName','param3=value3','param4=value4');
</script>
```

**Equivalente in at.js 2.*x***:

Un’alternativa a `mboxDefine()` e `mboxUpdate` è `getOffer()` e `applyOffer()`, con l’opzione del selettore utilizzata in `applyOffer()`. Questo approccio consente di mappare l’offerta su un elemento utilizzando qualsiasi selettore CSS, non solo uno con un ID.

**Esempio**:

```html {line-numbers="true"}
<div id="someId" class="mboxDefault"> 
  default content to replace by offer 
</div> 
<script> 
  adobe.target.getOffer({
    mbox: "mboxName",
    params: {
      param1: "value1",
      param2: "value2",
      param3: "value3",
      param4: "value4" 
    },
    success: function(offer) {
      adobe.target.applyOffer({
        mbox: "mboxName",
        selector: "#someId",
        offer: offer
      });
    },
    error: function(error) {
      console.error(error);
      var el = document.getElementById("someId");
      el.style.visibility = "visible";
    }
  });
</script>
```

### adobe.target.registerExtension()

**Descrizione**:

Fornisce un metodo standard per registrare un’estensione specifica.

Questa funzione non è più supportata. Non utilizzarla.

## Riepilogo delle funzioni obsolete, nuove e supportate in at.js 2.*x*

| Metodo | Supportate? | Nuovo? | Obsoleta?<P>(apparirà il contenuto predefinito) |
| --- | --- | --- | --- |
| `getOffer()` | Sì |  |  |
| `getOffers()` |  | Sì |  |
| `applyOffer()` | Sì |  |  |
| `applyOffers()` |  | Sì |  |
| `triggerView()` |  | Sì |  |
| `trackEvent()` | Sì |  |  |
| `mboxCreate()` |  |  | Sì |
| `mboxDefine()`<P>`mboxUpdate()` |  |  | Sì |
| `targetGlobalSettings()` | Sì |  |  |
| `Data Providers` | Sì |  |  |
| `targetPageParams()` | Sì |  |  |
| `targetPageParamsAll()` | Sì |  |  |
| `registerExtension()` |  |  | Sì |
| `At.js Custom Events` | Sì |  |  |

## Limitazioni e callout

Tieni presente le limitazioni e i callout seguenti:

### Tracciamento delle conversioni

I clienti che usano `mboxCreate()` per il tracciamento delle conversione devono utilizzare `trackEvent()` o `getOffer()`.

### Consegna delle offerte

Se `mboxCreate()` non viene sosituito con `getOffer()` o `applyOffer()`, la consegna delle offerte potrebbe non riuscire.

### È possibile usare at.js 2.*x* in alcune pagine, e at.js 1.*x* si trova su altre pagine?

Sì, il profilo del visitatore è mantenuto su più pagine utilizzando diverse versioni e librerie. Il formato del cookie è lo stesso.

### Nuovo utilizzo dell’API in at.js 2.*x*

at.js 2.*x* utilizza una nuova API, che chiamiamo API Delivery. Per eseguire il debug se at.js sta chiamando correttamente il server perimetrale [!DNL Target], è possibile filtrare la scheda Rete degli strumenti per sviluppatori del browser in &quot;delivery&quot;, &quot;`tt.omtrdc.net`&quot; o nel codice client. Noterai inoltre che [!DNL Target] invia un payload JSON invece di coppie chiave-valore.

### Mbox globale [!DNL Target] non più utilizzata

In at.js 2.*x*, non vedrai più &quot;`target-global-mbox`&quot; chiaramente nelle chiamate di rete. La sintassi &quot;`target-global-mbox`&quot; è stata sostituita con &quot;`execute > pageLoad`&quot; nel payload JSON inviato ai server [!DNL Target], come illustrato di seguito:

```json {line-numbers="true"}
{
  "id": {
    // ...
  },
  "context": {
    "channel": "web",
    // ...
  },
  "execute": {
    "pageLoad": {}
  }
}
```

In sostanza, il concetto di mbox globale era stato introdotto per comunicare a [!DNL Target] se recuperare o meno offerte e contenuti al caricamento delle pagine. Nella nostra versione più recente, lo abbiamo reso più esplicito.

### Il nome della mbox globale in at.js ha ancora importanza?

I clienti possono specificare un nome di mbox globale tramite **[!UICONTROL Target]** > **[!UICONTROL Administration]** > **[!UICONTROL Implementation]** > **[!UICONTROL Edit at.js Settings]**. I server perimetrali [!DNL Target] utilizzano questa impostazione per tradurre execute > pageload nel nome della mbox globale visualizzato nell’interfaccia utente [!DNL Target]. Questo consente ai clienti di continuare a utilizzare le API lato server, il compositore basato su moduli, gli script di profilo e creare tipi di pubblico utilizzando il nome della mbox globale. Inoltre, ti consigliamo vivamente di configurare lo stesso nome della mbox globale anche sulla pagina **[!UICONTROL Administration]** > **[!UICONTROL Visual Experience Composer]**, nel caso in cui si disponga di pagine che utilizzano ancora at.js 1.*x*, come illustrato nelle illustrazioni seguenti.

![Modificare la finestra di dialogo at.js](../assets/modify-atjs.png)

e

![Mbox globale personalizzata](../assets/custom-global-mbox.png)

### È necessario attivare l’impostazione di creazione automatica di una mbox globale con at.js 2.*x*?

Nella maggior parte dei casi, sì. Questa impostazione comunica a at.js 2.*x* per attivare una richiesta ai server perimetrali [!DNL Target] al caricamento della pagina. Questa impostazione deve essere attiva perché la mbox globale si traduce in execution > pageLoad, ma anche se desideri attivare una richiesta al caricamento della pagina.

### Le attività del Compositore esperienza visivo esistenti continueranno a funzionare, anche se il nome della mbox globale di Target non è specificato a partire da at.js 2.*x*?

Sì, perché execute > pageLoad è trattato sul backend [!DNL Target] come `target-global-mbox`.

### Se le mie attività basate su moduli sono mirate per `target-global-mbox`, queste continueranno a funzionare?

Sì, perché execute > pageLoad è trattato sui server edge di [!DNL Target] come `target-global-mbox`.

### at.js 2 supportato e non supportato.Impostazioni *x*

| Impostazione | Supportate? |
| --- | --- |
| X-Domain | No |
| Creazione automatica di una mbox globale | Sì |
| Nome mbox globale | Sì |

### Supporto del tracciamento tra più domini in at.js 2.x

Il tracciamento tra più domini consente di unire i visitatori di domini diversi. Poiché per ciascun dominio deve essere creato un nuovo cookie, è difficile tenere traccia dei visitatori che passano da un dominio a un altro. Per eseguire il tracciamento tra più domini, [!DNL Target] utilizza usa un cookie di terze parti. Ciò ti consente di creare un&#39;attività [!DNL Target] che si estende su `siteA.com` e `siteB.com` e i visitatori rimangono nella stessa esperienza quando passano da un dominio all&#39;altro. Questa funzionalità è legata al comportamento dei cookie di terze parti e di prima parte di [!DNL Target].

>[!NOTE]
>
>Il tracciamento tra più domini è supportato a partire dalla versione 2.10 di at.js, ma non è incluso come funzionalità integrata in at.js 2.*x* prima della versione 2.10. Il tracciamento tra più domini è supportato in at.js 2.*x* tramite la libreria Experience Cloud ID (ECID) v4.3.0+.

In [!DNL Target], il cookie di terze parti è memorizzato in `<CLIENTCODE>.tt.omtrdc.net`. Il cookie di prime parti è memorizzato in `clientdomain.com`. La prima richiesta restituisce intestazioni di risposta HTTP che tentano di impostare cookie di terze parti denominati `mboxSession` e `mboxPC`, mentre viene inviata nuovamente una richiesta di reindirizzamento con un parametro aggiuntivo (`mboxXDomainCheck=true`). Se il browser accetta cookie di terze parti, la richiesta di reindirizzamento li include e viene restituita l’esperienza. Questo flusso di lavoro è possibile perché utilizziamo il metodo HTTP GET.

In at.js 2.*x*, GET HTTP non utilizzato. Invece, HTTP POST viene utilizzato tramite at.js 2.*x* per inviare payload JSON a [!DNL Target] server Edge. L’utilizzo di HTTP POST comporta la richiesta di reindirizzamento per verificare se un browser supporta i cookie di terze parti. Infatti le richieste HTTP GET sono transazioni idempotenti, mentre HTTP POST non lo è e non deve essere ripetuto arbitrariamente. Di conseguenza, il tracciamento tra più domini in at.js 2.*x* (prima della versione 2.10) non è supportato come funzionalità integrata. Solo at.js 1.*x* supporta il tracciamento tra più domini come funzionalità integrata.

Per utilizzare il tracciamento tra domini diversi per at.js v2.10 o versione successiva, puoi effettuare una delle seguenti operazioni:

1. Installa la libreria [ECID v4.3.0+](https://experienceleague.adobe.com/docs/id-service/using/release-notes/release-notes.html?lang=it) in combinazione con at.js 2.*x*. La libreria ECID consente di gestire gli ID persistenti utilizzati per identificare lo stesso visitatore su domini diversi. Dopo l’installazione della libreria ECID v4.3.0+ e di at.js 2.*x*, potrai creare attività che si estendono su più domini singoli e tenere traccia degli utenti. È importante notare che questa funzionalità funziona solo dopo la scadenza della sessione.

1. Invece di installare la libreria ECID, se disponi di at.js v2.10 o versione successiva, puoi abilitare l&#39;impostazione Tra domini diversi nell&#39;interfaccia utente [!DNL Target] in **[!UICONTROL Administration]** > **[!UICONTROL Implementation]**. In alternativa, è possibile impostare l&#39;opzione _crossDomain_ su _enabled_ nel codice at.js.

Utilizzare il tracciamento tra domini diversi per le versioni di at.js v2.*x* prima della versione 2.10, è possibile implementare l&#39;opzione #1 sopra (installare la libreria ECID).

### Supporto della creazione automatica di una mbox globale

Questa impostazione comunica a at.js 2.*x* per attivare una richiesta ai server perimetrali [!DNL Target] al caricamento della pagina. Poiché la mbox globale è convertita in execute > pageLoad e interpretata dai server edge di [!DNL Target], i clienti devono attivarla se desiderano avviare una richiesta al caricamento della pagina.

### Supporto del nome mbox globale

I clienti possono specificare un nome di mbox globale tramite **[!UICONTROL Target]** > **[!UICONTROL Administration]** > **[!UICONTROL Implementation]** > **[!UICONTROL Edit]**. I server perimetrali [!DNL Target] utilizzano questa impostazione per tradurre execute > pageLoad nel nome della mbox globale immesso. Questo consente ai clienti di continuare a utilizzare le API lato server, il compositore basato su moduli, gli script di profilo e creare tipi di pubblico mirati per la mbox globale.

### I seguenti eventi personalizzati at.js sono applicabili a `triggerView()` o valgono solo per `applyOffer()` o `applyOffers()`?

* `adobe.target.event.CONTENT_RENDERING_FAILED`
* `adobe.target.event.CONTENT_RENDERING_SUCCEEDED`
* `adobe.target.event.CONTENT_RENDERING_NO_OFFERS`
* `adobe.target.event.CONTENT_RENDERING_REDIRECT`

Sì, gli eventi personalizzati at.js sono applicabili anche a `triggerView()`.

### Quando richiamo `triggerView()` con &lbrace;`"page" : "true"`&rbrace;, dice che invierà una notifica al backend [!DNL Target] e aumenterà le impression. Questo provocherà anche l’esecuzione degli script di profilo?

Quando si effettua una chiamata di preacquisizione al backend [!DNL Target], avviene l’esecuzione degli script di profilo. Successivamente, i dati del profilo interessati saranno crittografati e trasmessi nuovamente al lato client. Dopo la chiamata di `triggerView()` con `{"page": "true"}`, si invierà una notifica insieme ai dati di profilo codificati. Questo si verifica quando il backend [!DNL Target] deciderà di decodificare i dati di profilo e archiviarli nei database.

### Per gestire la visualizzazione temporanea di altri contenuti è necessario aggiungere codice per nasconderli preventivamente prima di richiamare `triggerView()`?

No, non è necessario aggiungere codice per nascondere contenuti preventivamente prima di richiamare `triggerView()`. at.js 2.*x* gestisce la logica relativa ai contenuti nascosti anticipatamente e visualizzati temporaneamente prima di mostrare e applicare la visualizzazione.

### Quale at.js 1.I parametri *x* per la creazione di tipi di pubblico non sono supportati in at.js 2.*x*?

I seguenti parametri di at.js 1.x sono *NOT* attualmente supportati per la creazione di tipi di pubblico quando si utilizza at.js 2.*x*:

* browserHeight
* browserWidth
* browserTimeOffset
* screenHeight
* screenWidth
* screenOrientation
* colorDepth
* devicePixelRatio
* vst.* parametri (vedere di seguito)

### at.js 2.*x* non supporta la creazione di tipi di pubblico utilizzando i parametri vst.* parametri

Clienti su at.js 1.*x* ha potuto usare vst.* Parametri mbox per creare tipi di pubblico. Si è trattato di un effetto collaterale non previsto di come at.js 1.*x* ha inviato parametri mbox al back-end [!DNL Target]. Dopo la migrazione a at.js 2.*x*, non è più possibile creare tipi di pubblico utilizzando questi parametri perché at.js 2.*x* invia i parametri mbox in modo diverso.

## Compatibilità di at.js

Le tabelle seguenti contengono una spiegazione di at.js. 2.*x* compatibilità con diversi tipi di attività, integrazioni, funzionalità e funzioni di at.js.

### Tipi di attività

| Type (Tipo) | Supportate? |
| --- | --- |
| [!UICONTROL A/B Test] | Sì |
| [!UICONTROL Auto-Allocate] | Sì |
| [!UICONTROL Auto-Target] | Sì |
| [!UICONTROL Experience Targeting] | Sì |
| [!UICONTROL Multivariate Test] | Sì |
| [!UICONTROL Automated Personalization] | Sì |
| [!DNL Recommendations] | Sì |

>[!NOTE]
>
>[!UICONTROL Auto-Target] attività sono supportate tramite at.js 2.*x* e il Compositore esperienza visivo quando tutte le modifiche vengono applicate a `Page Load Event`. Quando vengono aggiunte modifiche a particolari viste, sono supportate solo le attività [!UICONTROL A/B Test], [!UICONTROL Auto-Allocate] e [!UICONTROL Experience Targeting] (XT).

### Integrazioni

| Type (Tipo) | Supportate? |
| --- | --- |
| [!UICONTROL Analytics for Target] (A4T) | Sì |
| Tipi di pubblico | Sì |
| Attributi del cliente | Sì |
| Frammenti esperienza AEM | Sì |
| [Estensione Adobe Experience Platform](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md) | Sì |
| Strumento di debug | Sì |
| Auditor | Le regole per at.js 2 non sono ancora state aggiornate.*x* |
| Supporto di Opt-in per [RGPD](/help/dev/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.md) | Questa funzione è supportata in [at.js versione 2.1.0](/help/dev/implement/client-side/atjs/target-atjs-versions.md#atjs-version-210-june-3-2019) o successiva. |
| Personalization avanzato AEM con tecnologia [!DNL Adobe Target] | No |

### Funzioni

| Funzione | Supportate? |
| --- | --- |
| X-Domain | No |
| Proprietà/Aree di lavoro | Sì |
| Collegamenti QA | Sì |
| Compositore esperienza basato su moduli | Sì |
| Compositore esperienza visivo | Sì |
| Codice personalizzato | Sì |
| [Token di risposta](#response-tokens) | Sì |
| Tracciamento dei clic | Sì |
| Consegna di più attività | Sì |
| targetGlobalSettings | Sì (ma non dominio x) |
| Metodi at.js | È disponibile il supporto per tutto, tranne<P>`mboxCreate()`<P>`mboxUpdate()`<P>`mboxDefine()`<P>che visualizzerà il contenuto predefinito. |

### Parametri stringa di query

| Parametro | Supportate? |
| --- | --- |
| `?mboxDisable` | Sì |
| `?mboxDisable` | Sì |
| `?mboxTrace` | Sì |
| `?mboxSession` | No |
| `?mboxOverride.browserIp` | Sì |

## Token di risposta

at.js 2.*x*, esattamente come at.js 1.*x*, utilizza l’evento personalizzato `at-request-succeeded` per ottenere i token di risposta. Per esempi di codice con l’evento `at-request-succeeded` personalizzato, consulta [Token di risposta](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html?lang=it).

## Mappatura payload dei parametri at.js 1.*x* parametri in at.js 2.Mappatura payload *x*

Questa sezione delinea le mappature tra at.js 1.*x* che in at.js 2.*x*.

Prima di passare alla mappatura dei parametri, gli endpoint utilizzati da queste versioni della libreria sono cambiati:

* at.js 1.*x* - `http://<client code>.tt.omtrdc.net/m2/<client code>/mbox/json`
* at.js 2.*x* - `http://<client code>.tt.omtrdc.net/rest/v1/delivery`

Un’altra differenza significativa è che:

* at.js 1.*x* - Il codice client è parte del percorso
* at.js 2.*x* - Il codice client viene inviato come parametro della stringa di query, ad esempio:
  `http://<client code>.tt.omtrdc.net/rest/v1/delivery?client=democlient`

Le seguenti sezioni riportano tutte nell’elenco il parametro di at.js 1.Parametro *x*, descrizione e 2 corrispondente.Payload JSON *x* (se applicabile):

### at_property

(parametro di at.js 1.*x*)

Utilizzato per le [autorizzazioni per gli utenti Enterprise](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/property-channel.html?lang=it).

```json {line-numbers="true"}
{
  ....
  "property": {
    "token": "1213213123122313121"
  }
  ....
}
```

### mboxHost

(parametro di at.js 1.*x*)

Dominio della pagina in cui viene eseguita la libreria [!DNL Target].

at.js 2.Payload JSON *x*:

```json {line-numbers="true"}
{
  "context": {
    "browser": {
       "host": "test.com"
    }
  }
}
```

### webGLRenderer

(parametro di at.js 1.*x*)

Funzionalità del rendering GL WEB del browser. Questo viene usato dal nostro meccanismo di rilevamento del dispositivo per determinare se il dispositivo del visitatore è un computer desktop, un iPhone, un dispositivo Android, ecc.

Payload JSON di at.js 2.Payload JSON *x*:

```json {line-numbers="true"}
{
  "context": {
    "browser": {
       "webGLRenderer": "AMD Radeon Pro 560X OpenGL Engine"
    }
  }
}
```

### mboxURL

(parametro di at.js 1.*x*)

URL della pagina.

Payload JSON di at.js 2.Payload JSON *x*:

```json {line-numbers="true"}
{
  "context": {
    "address": {
       "url": "http://test.com"
    }
  }
}
```

### mboxReferrer

(parametro di at.js 1.*x*)

Riferimento (provenienza) della pagina.

Payload JSON di at.js 2.Payload JSON *x*:

```json {line-numbers="true"}
{
  "context": {
    "address": {
       "referringUrl": "http://google.com"
    }
  }
}
```

### mbox (il nome) è uguale a mbox globale

(parametro di at.js 1.*x*)

L’API della consegna non dispone più di un concetto mbox globale. Nel payload JSON devi usare `execute > pageLoad`.

Payload JSON di at.js 2.Payload JSON *x*:

```json {line-numbers="true"}
{
  "execute": {
    "pageLoad": {
       "parameters": ....
       "profileParameters": ...
       .....
    }
  }
}
```

### mbox (il nome) *non* è uguale a mbox globale

(parametro di at.js 1.*x*)

Per utilizzare il nome di una mbox, trasmettilo a `execute > mboxes`. Una mbox richiede un indice e un nome.

Payload JSON di at.js 2.Payload JSON *x*:

```json {line-numbers="true"}
{
  "execute": {
    "mboxes": [{
       "index": 0,
       "name": "some-mbox",
       "parameters": ....
       "profileParameters": ...
       .....
    }]
  }
}
```

### mboxId

(parametro di at.js 1.*x*)

Non più utilizzato.

### mboxCount

(parametro di at.js 1.*x*)

Non più utilizzato.

### mboxRid

(parametro di at.js 1.*x*)

ID della richiesta utilizzato dai sistemi a valle per facilitare il debug.

Payload JSON di at.js 2.Payload JSON *x*:

```json {line-numbers="true"}
{
  "requestId": "2412234442342"
  ....
}
```

### mboxTime

(parametro di at.js 1.*x*)

Non più utilizzato.

### mboxSession

(parametro di at.js 1.*x*)

L’ID della sessione viene inviato come parametro della stringa di query (`sessionId`) all’endpoint API di consegna.

### mboxPC

(parametro di at.js 1.*x*)

L’ID TNT viene trasmesso in `id > tntId`.

Payload JSON di at.js 2.Payload JSON *x*:

```json {line-numbers="true"}
{
  "id": {
    "tntId": "ca5ddd7e33504c58b70d45d0368bcc70.21_3"
  }
  ....
}
```

### mboxMCGVID

(parametro di at.js 1.*x*)

L’ID del visitatore di Experience Cloud viene trasmesso in `id > marketingCloudVisitorId`.

Payload JSON di at.js 2.Payload JSON *x*:

```json {line-numbers="true"}
{
  "id": {
    "marketingCloudVisitorId": "797110122341429343505"
  }
  ....
}
```

### `vst.aaaa.id` e `vst.aaaa.authState`

(parametro di at.js 1.*x*)

Gli ID cliente devono essere trasmessi in `id > customerIds`.

Payload JSON di at.js 2.Payload JSON *x*:

```json {line-numbers="true"}
{
  "id": {
    "customerIds": [{
       "id": "1232131",
       "integrationCode": "aaaa",
       "authenticatedState": "....."
     }]
  }
  ....
}
```

### mbox3rdPartyId

(parametro di at.js 1.*x*)

ID di terze parti del cliente utilizzato per collegare diversi ID [!DNL Target].

at.js 2.Payload JSON *x*:

```json {line-numbers="true"}
{
  "id": {
    "thirdPartyId": "1232312323123"
  }
  ....
}
```

### mboxMCSDID

(parametro di at.js 1.*x*)

SDID, noto anche come ID di dati supplementari. Deve essere trasmesso in `experienceCloud > analytics > supplementalDataId`.

Payload JSON di at.js 2.Payload JSON *x*:

```json {line-numbers="true"}
{
  "experienceCloud": {
    "analytics": {
      "supplementalDataId": "1212321132123131"
    }
  }
  ....
}
```

### vst.trk

(parametro di at.js 1.*x*)

Server di tracciamento [!UICONTROL Analytics]. Deve essere trasmesso in `experienceCloud > analytics > trackingServer`.

Payload JSON di at.js 2.Payload JSON *x*:

```json {line-numbers="true"}
{
  "experienceCloud": {
    "analytics": {
      "trackingServer": "analytics.test.com"
    }
  }
  ....
}
```

### vst.trks

(parametro di at.js 1.*x*)

Server di tracciamento sicuro di Analytics. Deve essere trasmesso in `experienceCloud > analytics > trackingServerSecure`.

Payload JSON di at.js 2.Payload JSON *x*:

```json {line-numbers="true"}
{
  "experienceCloud": {
    "analytics": {
      "trackingServerSecure": "secure-analytics.test.com"
    }
  }
  ....
}
```

### mboxMCGLH

(parametro di at.js 1.*x*)

Hint di posizione di Audience Manager. Deve essere trasmesso in `experienceCloud > audienceManager > locationHint`.

Payload JSON di at.js 2.Payload JSON *x*:

```json {line-numbers="true"}
{
  "experienceCloud": {
    "audienceManager": {
      "locationHint": 9
    }
  }
  ....
}
```

### mboxAAMB

(parametro di at.js 1.*x*)

Blob Audience Manager. Deve essere trasmesso in `experienceCloud > audienceManager > blob`.

Payload JSON di at.js 2.Payload JSON *x*:

```json {line-numbers="true"}
{
  "experienceCloud": {
    "audienceManager": {
      "blob": "2142342343242342"
    }
  }
  ....
}
```

### mboxVersion

(parametro di at.js 1.*x*)

La versione viene inviata come parametro della stringa di query tramite il parametro della versione.

## Video di formazione: at.js 2.*x* diagramma architetturale ![Icona panoramica](../../assets/overview.png)

at.js 2.*x* migliora il supporto di Adobe [!DNL Target] per l&#39;SPA e si integra con altre soluzioni Experience Cloud. Questo video spiega come tutti questo elementi funzionano insieme.

>[!VIDEO](https://video.tv.adobe.com/v/26250/?quality=12)

Consulta [Informazioni su at.js 2.*x* funziona](https://experienceleague.adobe.com/docs/target-learn/tutorials/implementation/understanding-how-atjs-20-works.html?lang=it) per ulteriori informazioni.
