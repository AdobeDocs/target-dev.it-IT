---
keywords: faq at.js, domande frequenti at.js, faq, visualizzazione momentanea di altri contenuti, caricatore, loader, caricatore di pagina, cross domain, dimensione del file, dimensione file, dominio x, at.js e mbox.js, solo x, per diversi domini, safari, app singola pagina, selettori mancanti, selettori, applicazione a pagina singola, tt.omtrdc.net, spa, Adobe Experience Manager, AEM, indirizzo ip, httponly, HttpOnly, protetto, ip, dominio cookie
description: Risposte alle domande frequenti sulla libreria JavaScript at.js di  [!DNL Adobe Target] .
title: Quali sono le domande più comuni su at.js, e le relative risposte?
feature: at.js
exl-id: 362ccc5b-8731-46c0-bc52-3e55c273e216
source-git-commit: 448c43c0c10e22ad054f4ee98bfc282f8c96cdcb
workflow-type: tm+mt
source-wordcount: '2923'
ht-degree: 67%

---

# Domande frequenti su at.js

Risposte alle domande frequenti sulla libreria JavaScript at.js di [!DNL Adobe Target].

## Quali vantaggi offre at.js rispetto a mbox.js?

La libreria at.js sostituisce mbox.js. La libreria mbox.js non è più supportata. Tuttavia, per la maggior parte delle persone, at.js offre vantaggi rispetto a mbox.js.

Ad esempio, at.js migliora i tempi di caricamento delle pagine per le implementazioni web, migliora la sicurezza e fornisce migliori opzioni di implementazione per le applicazioni a pagina singola.

Nella figura seguente vengono illustrate le prestazioni di caricamento delle pagine utilizzando mbox.js e at.js.

(Fare clic sull&#39;immagine per espanderla a larghezza intera.)

![Diagramma delle prestazioni delle pagine: confronto tra mbox.js e at.js](/help/dev/implement/client-side/atjs/assets/atjs_versus_mboxjs.png "Diagramma delle prestazioni delle pagine: confronto tra mbox.js e at.js"){zoomable="yes"}

Come illustrato in precedenza, utilizzando mbox.js il contenuto della pagina inizia a caricarsi solo una volta completata la richiesta di [!DNL Target]. Utilizzando at.js, il contenuto della pagina comincia a caricarsi quando viene avviata la richiesta di [!DNL Target], senza attenderne il completamento.

## Qual è l’impatto di at.js e mbox.js sui tempi di caricamento delle pagine?

Molti clienti e consulenti vogliono conoscere l’impatto di at.js e mbox.js sui tempi di caricamento delle pagine, soprattutto nel contesto di nuovi utenti rispetto a utenti di ritorno. Sfortunatamente, è difficile misurare e fornire numeri concreti per quanto riguarda l’influenza di at.js o mbox.js sul tempo di caricamento della pagina a causa dell’implementazione di ciascun cliente.

Tuttavia, se sulla pagina è presente l&#39;API dei visitatori, [!DNL Target] può capire meglio in che modo at.js e mbox.js influenzano il tempo di caricamento delle pagine.

>[!NOTE]
>
>L’API dei visitatori e at.js o mbox.js hanno un impatto sul tempo di caricamento della pagina solo quando si utilizza la mbox globale (a causa della tecnica di celamento del corpo). Le mbox regionali non sono influenzate dall&#39;integrazione delle API dei visitatori.

Le sezioni seguenti illustrano la sequenza di azioni per i visitatori nuovi e per i visitatori di ritorno:

### Visitatori nuovi

1. L&#39;API dei visitatori viene caricata, analizzata ed eseguita.
1. at.js / mbox.js è caricato, analizzato ed eseguito.
1. Se la creazione automatica della mbox globale è abilitata, la libreria JavaScript di [!DNL Target]:

   * Crea un&#39;istanza dell&#39;oggetto Visitatore.
   * La libreria [!DNL Target] tenta di recuperare i dati dell&#39;ID visitatore Experience Cloud.
   * Poiché si tratta di un nuovo visitatore, l’API Visitor genera una richiesta cross-domain a demdex.net.
   * Dopo il recupero dei dati dell&#39;ID visitatore di Experience Cloud, viene avviata una richiesta a [!DNL Target].

### Visitatori di ritorno

1. L&#39;API dei visitatori viene caricata, analizzata ed eseguita.
1. at.js / mbox.js è caricato, analizzato ed eseguito.
1. Se la creazione automatica della mbox globale è abilitata, la libreria JavaScript di [!DNL Target]:

   * Crea un&#39;istanza dell&#39;oggetto Visitatore.
   * La libreria [!DNL Target] tenta di recuperare i dati dell&#39;ID visitatore Experience Cloud.
   * L&#39;API dei visitatori recupera i dati dai cookie.
   * Dopo il recupero dei dati dell&#39;ID visitatore di Experience Cloud, viene avviata una richiesta a [!DNL Target].

>[!NOTE]
>
>Per i nuovi visitatori, quando è presente l&#39;API visitatore, [!DNL Target] deve connettersi più volte per assicurarsi che [!DNL Target] richieste contengano dati di ID visitatore Experience Cloud. Per i visitatori di ritorno, [!DNL Target] si connette a [!DNL Target] solo per recuperare il contenuto personalizzato.

## Perché mi sembra di notare tempi di risposta più lenti dopo l’aggiornamento da una versione precedente di at.js alla versione 1.0.0?

at.js versione 1.0.0 o successiva attiva tutte le richieste in parallelo. Le versioni precedenti eseguono le richieste in modo sequenziale, ovvero le richieste formano una coda e [!DNL Target] aspetta il completamento della richiesta corrente prima di passare alla successiva.

Il modo in cui le versioni precedenti di at.js eseguono le richieste è suscettibile al cosiddetto blocco del primo elemento. In at.js 1.0.0 e versioni successive, [!DNL Target] è passato all&#39;esecuzione parallela delle richieste.

Se controlli la cascata delle schede di rete di at.js 0.9.1, ad esempio, vedrai che la successiva richiesta di [!DNL Target] si avvia solo dopo che la precedente è stata completata. Diverso è il caso con at.js 1.0.0 e versioni successive, dove tutte le richieste partono essenzialmente nello stesso momento.

Per quanto riguarda il tempo di risposta, la sequenza può essere riassunta così:

<ul class="simplelist"> 
 <li> at.js 0.9.1: tempo di risposta di tutte le [!DNL Target] richieste = somma del tempo di risposta delle richieste </li> 
 <li> at.js 1.0.0 e versioni successive: tempo di risposta di tutte le [!DNL Target] richieste = tempo di risposta massimo delle richieste </li> 
</ul>

La libreria at.js versione 1.0.0 completa le richieste più rapidamente. Inoltre, le richieste at.js sono asincrone, pertanto [!DNL Target] non blocca il rendering della pagina. Anche se il completamento delle richieste impiega qualche secondo, la pagina renderizzata sarà comunque visibile, anche se alcune parti della pagina resteranno vuote finché [!DNL Target] non avrà ricevuto una risposta dal server Edge di [!DNL Target].

## È possibile caricare la libreria di [!DNL Target] in modo asincrono?

La versione di at.js 1.0.0 permette di caricare la libreria di [!DNL Target] in modo asincrono.

Per caricare at.js in modo asincrono:

* L’approccio consigliato è tramite tag in Adobe Experience Platform.
* Puoi anche caricare at.js in modo asincrono aggiungendo l’attributo async al tag script che carica at.js. Ad esempio, puoi usare un codice simile al seguente:

  ```
  <script src="<URL to at.js>" async></script>
  ```

* Puoi anche caricare at.js in modo asincrono utilizzando questo codice:

  ```
  var script = document.createElement('script'); 
  script.async = true; 
  script.src = "<URL to at.js>"; 
  document.head.appendChild(script);
  ```

Caricare at.js in modo asincrono è un ottimo modo per evitare di bloccare il rendering del browser; tuttavia, questa tecnica può portare alla visualizzazione momentanea di altri contenuti della pagina web.

Puoi evitare sfarfallii utilizzando uno snippet che nasconde preventivamente la pagina (o specifiche porzioni), quindi la rivela dopo il caricamento di at.js e della richiesta globale. Lo snippet deve essere aggiunto prima del caricamento di at.js.

Se distribuisci at.js tramite un’implementazione asincrona di [!UICONTROL Adobe Experience Platform], assicurati di includere lo snippet per nascondere le pagine direttamente, prima di implementare [!DNL Target] utilizzando il codice di incorporamento di [!UICONTROL Adobe Experience Platform].

Durante l’implementazione di at.js tramite un’implementazione sincrona di DTM, puoi aggiungere lo snippet tramite una regola di caricamento della pagina attivata nella parte superiore della pagina.

Per ulteriori informazioni, consulta [Gestione at.js della visualizzazione momentanea di altri contenuti](/help/dev/implement/client-side/atjs/how-atjs-works/manage-flicker-with-atjs.md).

## at.js è compatibile con l’integrazione [!DNL Adobe Experience Manager] (Experience Manager)?

[!DNL Adobe Experience Manager] 6.2 con FP-11577 (o versioni successive) ora supporta le implementazioni at.js con la relativa integrazione [!UICONTROL Adobe Target Cloud Services].

## Come posso evitare la visualizzazione momentanea di altri contenuti al caricamento pagina, utilizzando at.js?

[!DNL Target] offre diversi modi per evitare la visualizzazione momentanea di altri contenuti al caricamento della pagina. Per ulteriori informazioni, consulta [Impedire la visualizzazione momentanea di altri contenuti con at.js](/help/dev/implement/client-side/atjs/how-atjs-works/manage-flicker-with-atjs.md).

## Qual è la dimensione del file di at.js?

Il file di at.js che scarichi è approssimativamente 109 KB. Tuttavia, poiché la maggior parte dei server comprime automaticamente i file per renderli di dimensioni più piccole, at.js è circa 34 KB quando è compresso (utilizzando GZIP o un altro metodo) sul server e caricato dagli utenti che visitano il tuo sito. Le impostazioni di compressione sul server in cui è stato installato at.js determinano la dimensione effettiva.

## Perché at.js è più grande di mbox.js?

Le implementazioni di at.js utilizzano una sola libreria ( at.js), mentre quelle di mbox.js usano due librerie ( mbox.js e target.js). Quindi un confronto più equo è quello tra at.js e mbox.js *più* `target.js`. Confrontando le dimensioni compresse con GZIP delle due versioni, at.js versione 1.2 è di 34 KB e mbox.js versione 63 è di 26,2 KB. &grave;&grave;

at.js è più grande perché effettua molta più analisi DOM rispetto a mbox.js. Questo è necessario perché at.js ottiene dati “grezzi” dalla risposta JSON e deve interpretarli. mbox.js utilizzava invece `document.write()` ed era il browser a eseguire l’analisi.

Nonostante le dimensioni più grandi del file, i nostri test indicano che le pagine vengono caricate più velocemente con at.js rispetto a mbox.js. Inoltre, at.js è superiore dal punto di vista della sicurezza perché non carica file aggiuntivi in modo dinamico né utilizza `document.write`.

## at.js include jQuery? Posso rimuovere questa parte di at.js se sul mio sito ho già jQuery?

at.js attualmente utilizza parti di jQuery e quindi vedrai la notifica della licenza MIT nella parte superiore di at.js. jQuery non è esposto e non interferisce con la libreria jQuery che hai già sulla pagina, che potrebbe essere una versione diversa. La rimozione del codice jQuery all’interno di at.js non è supportata.

## at.js supporta Safari e l’attraversamento di più domini impostato su “solo x”?

No, se l’attraversamento di più domini è impostato su &quot;solo x&quot; e in Safari i cookie di terze parti sono disabilitati, allora sia mbox.js che at.js impostano un cookie disabilitato e non viene eseguita alcuna richiesta mbox per quel particolare dominio del client.

Per supportare i visitatori Safari, un dominio X migliore sarebbe &quot;disabilitato&quot; (imposta solo un cookie di prima parte) o &quot;abilitato&quot; (imposta solo un cookie di prima parte su Safari, mentre imposta i cookie di prima e terze parti su altri browser).

## È possibile utilizzare Target [!UICONTROL Visual Experience Composer] (VEC) nelle applicazioni a pagina singola?

Sì, puoi utilizzare il Compositore esperienza visivo se utilizzi at.js 2.x. Per maggiori informazioni, consulta [Compositore esperienza visivo per applicazione a singola pagina (SPA)](https://experienceleague.adobe.com/docs/target/using/experiences/spa-visual-experience-composer.html).

## Posso utilizzare il debugger di Adobe Experience Cloud con le implementazioni di at.js?

Sì. È inoltre possibile utilizzare mboxTrace per il debug o gli Strumenti per sviluppatori del browser per controllare le richieste di rete, filtrando “mbox” per isolare le chiamate mbox.

## Posso usare caratteri speciali nei nomi delle mbox con at.js?

Sì, come con mbox.js.

## Perché le mie mbox non vengono lanciate sulle mie pagine web?

I [!DNL Target] clienti di utilizzano talvolta istanze basate su cloud con [!DNL Target] per test o semplici prove di concetto. Questi domini, e molti altri, fanno parte dell’[elenco dei suffissi pubblici](https://publicsuffix.org/list/public_suffix_list.dat).

I browser moderni non salvano i cookie se usi questi domini, a meno che non personalizzi l’impostazione `cookieDomain` utilizzando targetGlobalSettings(). Per ulteriori informazioni, vedere [Utilizzo di istanze basate su cloud con [!DNL Target]](/help/dev/implement/client-side/target-debugging-atjs/targeting-using-cloud-based-instances.md).

## Gli indirizzi IP possono essere utilizzati come dominio dei cookie quando si utilizza at.js?

Sì, se utilizzi [at.js versione 1.2 o successive](/help/dev/implement/client-side/atjs/target-atjs-versions.md). Tuttavia, Adobe consiglia vivamente di utilizzare sempre l’ultima versione.

>[!NOTE]
>
>Gli esempi seguenti non sono necessari se si utilizza at.js versione 1.2 o successiva.

A seconda di come utilizzi [targetGlobalSettings](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md), potrebbe essere necessario apportare ulteriori modifiche al codice dopo aver scaricato at.js. Ad esempio, se ti servivano impostazioni leggermente diverse per le tue implementazioni di [!DNL Target] su vari siti web e non hai potuto definirle in modo dinamico con JavaScript personalizzato, apporta tali personalizzazioni manualmente dopo aver scaricato il file e prima di caricarlo sul rispettivo sito web.

Gli esempi seguenti consentono di utilizzare la funzione di at.js `targetGlobalSettings()` per inserire uno snippet di codice per supportare gli indirizzi IP:

Esempio per un solo indirizzo IP:

```
if (window.location.hostname === '123.456.78.9') { 
    window.targetGlobalSettings = window.targetGlobalSettings || {}; 
    window.targetGlobalSettings.cookieDomain = window.location.hostname; 
}
```

Esempio per un intervallo di indirizzi IP:

```
if (/^123\.456\.78\..*/g.test(window.location.hostname)) { 
    window.targetGlobalSettings = window.targetGlobalSettings || {}; 
    window.targetGlobalSettings.cookieDomain = window.location.hostname; 
}
```

## Perché ricevo messaggi di avviso, ad esempio “azioni con selettori mancanti”?

Questi messaggi non sono legati alla funzionalità at.js. La libreria at.js tenta di segnalare tutto ciò che non si trova nel DOM.

Di seguito sono riportate le possibili cause principali per questo messaggio di avviso:

* La pagina viene generata in modo dinamico e at.js non è in grado di trovare l’elemento.
* La pagina viene creata lentamente (a causa di una rete lenta) e at.js non riesce a trovare il selettore nel DOM.
* La struttura di pagina su cui è in esecuzione l’attività è stata modificata. Se riapri l’attività nel Compositore esperienza visivo dovrebbe comparire un messaggio di avviso. È necessario aggiornare l’attività in modo che tutti gli elementi necessari possano essere trovati.
* La pagina sottostante fa parte di un’applicazione a pagina singola (SPA) oppure la pagina contiene elementi che appaiono più in basso e il &quot;meccanismo di polling selettivo&quot; di at.js non riesce a trovarli. Può essere utile aumentare il valore di `selectorsPollingTimeout`. Per ulteriori informazioni, consulta [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md).
* Qualsiasi metrica di rilevamento dei clic tenta di aggiungersi a ogni pagina, indipendentemente dall’URL su cui è stata impostata la metrica. Anche se innocua, questa situazione fa apparire molti di questi messaggi.

  Per ottenere risultati ottimali, scarica e utilizza la [versione più recente di at.js](/help/dev/implement/client-side/atjs/target-atjs-versions.md). Per ulteriori informazioni su come scaricare at.js, consulta la sezione [Scaricare at.js utilizzando la [!DNL Target] interfaccia](how-to-deployatjs/implement-target-without-a-tag-manager.md#download-atjs-using-the-target-interface) nell&#39;articolo [*Come distribuire at.js* > *Implementare [!DNL Target] senza un sistema per la gestione dei tag*](how-to-deployatjs/implement-target-without-a-tag-manager.md).

## Le chiamate server di [!DNL Target] sono indirizzate al dominio tt.omtrdc.net: di che si tratta?

tt.omtrdc.net è il nome di dominio della rete EDGE di Adobe, utilizzato per ricevere tutte le chiamate server per [!DNL Target].

## Perché at.js non utilizza sempre i flag dei cookie HttpOnly e Secure?

HttpOnly può essere impostato solo tramite codice basato su server. [!DNL Target]I cookie, come mbox, vengono creati e salvati tramite il codice JavaScript, quindi non può utilizzare il flag di [!DNL Target] cookie HttpOnly. [!DNL Target] utilizza il flag HttpOnly impostato per i cookie di terze parti impostati lato server quando è abilitato l’attraversamento di più domini.

È possibile impostare il flag Secure tramite JavaScript solo se la pagina è stata caricata tramite HTTPS. Se la pagina viene inizialmente caricata tramite HTTP, JavaScript non può impostare questo flag. Inoltre, se viene utilizzato il flag Secure, il cookie sarà disponibile solo nelle pagine HTTPS. Per le pagine caricate tramite HTTPS, [!DNL Target] imposta gli attributi Secure e SameSite=None.

Per garantire che [!DNL Target] possa tracciare correttamente gli utenti e che i cookie siano generati lato client, [!DNL Target] non utilizza nessuno di questi flag, tranne nelle situazioni sopra menzionate.

## In che modo at.js gestisce problemi di sicurezza come attacchi XSS e MITM?

La comunicazione con la rete Adobe Edge, abilitata da at.js, avviene solo su HTTPS purché l&#39;opzione `secureOnly` sia impostata su true nella funzione targetGlobalSettings() ([targetGlobalSettings](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md)), altrimenti at.js può passare da HTTP a HTTPS in base al protocollo della pagina.

Le seguenti intestazioni vengono applicate per impostazione predefinita:
* HTTP Strict Transport Security (HSTS)
* Protezione X-XSS
* Opzioni tipo di contenuto X
* Referrer-Policy

È possibile applicare tutte le intestazioni già utilizzate nelle pagine client. Un modo comune per farlo è tramite &quot;HTTP Request Header Authorization&quot;. L’Assistenza clienti di Adobe può fornire ulteriori consigli su metodi e pratiche ottimali.

Inoltre, le richieste a Adobe Edge Network sono pubbliche (in quanto sono progettate per essere effettuate dai browser dei visitatori) e non contengono dettagli visibili del visitatore (contengono solo un ID visitatore). Queste richieste consegnano esperienze ai visitatori e contengono dettagli su ciò che un visitatore dovrebbe vedere sulla pagina.

Tieni presente che per i token di risposta e gli ID sessione trasmessi in queste richieste:

* Tiene traccia delle sessioni di comunicazione
* Sono composti da caratteri casuali
* Gli ID sessione sono validi per 30 minuti
* I token di risposta possono essere disabilitati ([Token di risposta](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html))
* Sono utili solo nell’ambiente delle soluzioni Adobi.

È previsto che nelle richieste at.js venga visualizzata l’intestazione `Access-Control-Allow-Origin` con valore &quot;*&quot;, poiché sono pubbliche, non è richiesta l’autenticazione e è necessario accedere alla rete Adobe Edge da qualsiasi dominio tramite chiamate JavaScript.

Tuttavia, i criteri sulla sicurezza dei contenuti (Content Security Policy, CSP) devono essere applicati sulla pagina. Per ulteriori informazioni sui requisiti CSP per at.js, consulta [Criteri di sicurezza del contenuto](/help/dev/before-implement/privacy/content-security-policy.md) e [targetGlobalSettings](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md).

## Con quale frequenza at.js attiva una richiesta di rete?

[!DNL Target] esegue tutte le sue decisioni sul lato server. Ciò significa che at.js at.js genera una richiesta di rete ogni volta che la pagina si ricarica o viene richiamata un&#39;API pubblica di at.js.

## Nel migliore dei casi, possiamo aspettarci che l&#39;utente non verifichi effetti visibili sul caricamento della pagina dovuti al fatto che si nasconde, sostituisce e visualizza il contenuto?

at.js cerca di evitare di nascondere anticipatamente HTML BODY o altri elementi DOM per un periodo di tempo prolungato, ma ciò dipende dalle condizioni di rete e dalla configurazione dell’attività. at.js offre [impostazioni](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md) che è possibile utilizzare per personalizzare lo stile CSS per nascondere il BODY, in modo tale che invece di svuotare l’intero BODY HTML sia possibile nascondere anticipatamente solo alcune parti della pagina. L&#39;aspettativa è che quelle parti contengano elementi DOM che devono essere “personalizzati”.

## Qual è la sequenza di eventi in uno scenario medio in cui un utente si qualifica per un’attività?

La richiesta at.js è un `XMLHttpRequest` asincrono, quindi eseguiamo i seguenti passaggi:

1. La pagina viene caricata.
1. at.js nasconde anticipatamente il BODY HTML. È presente un&#39;impostazione per nascondere anticipatamente un particolare contenitore invece del BODY HTML.
1. La richiesta at.js viene attivata.
1. Una volta ricevuta la risposta di [!DNL Target], [!DNL Target] estrae i selettori CSS.
1. Utilizzando i selettori CSS, [!DNL Target] crea tag STYLE per nascondere anticipatamente gli elementi DOM che saranno personalizzati.
1. Il BODY HTML che nasconde anticipatamente STYLE viene rimosso.
1. [!DNL Target] avvia il polling per gli elementi DOM.
1. Se viene trovato un elemento DOM, [!DNL Target] applica le modifiche DOM e l’elemento che nasconde anticipatamente STYLE viene rimosso.
1. Se gli elementi DOM non vengono trovati, un timeout globale rivela gli elementi per evitare di avere una pagina interrotta.

## Quanto spesso il contenuto della pagina è completamente caricato e visibile quando at.js rivela infine l’elemento che l’attività sta modificando?

Considerando lo scenario precedente, quanto spesso il contenuto della pagina è completamente caricato e visibile quando at.js rivela infine l’elemento che l’attività sta modificando? In altre parole, la pagina è completamente visibile ad eccezione del contenuto dell&#39;attività, che viene poi rivelato leggermente dopo il resto del contenuto.

at.js non blocca il rendering della pagina. Un utente potrebbe notare alcune aree vuote nella pagina che rappresentano elementi che verranno personalizzati da [!DNL Target]. Se il contenuto da applicare non contiene molte risorse remote, come SCRIPT o IMG, il rendering dovrebbe essere eseguito rapidamente.

## In che modo una pagina interamente salvata nella cache influirà sullo scenario precedente? Sarebbe più probabile che il contenuto dell’attività diventi visibile notevolmente dopo il caricamento del resto del contenuto della pagina?

Se una pagina viene salvata nella cache in una rete CDN vicina alla posizione dell’utente, ma non vicina al server Edge di [!DNL Target], l’utente potrebbe notare alcuni ritardi. I server Edge di [!DNL Target] sono ben distribuiti in tutto il mondo, quindi nella maggior parte dei casi questo non è un problema.

## È possibile che un’immagine protagonista venga visualizzata e poi scambiata dopo un breve ritardo?

Considerando lo scenario seguente:

Il timeout di [!DNL Target] è di cinque secondi. Un utente carica una pagina che ha un&#39;attività per personalizzare un&#39;immagine protagonista. at.js invia la richiesta per determinare se c&#39;è un&#39;attività da applicare, ma non è presente una risposta iniziale. Supponiamo che l’utente veda il contenuto regolare dell’immagine principale, perché non è stata ricevuta alcuna risposta da [!DNL Target] sull’esistenza di un’attività associata. Dopo quattro secondi, [!DNL Target] restituisce una risposta con il contenuto dell’attività.

A questo punto, è possibile che la versione alternativa venga mostrata? Perciò dopo quattro secondi, l&#39;immagine protagonista potrebbe essere scambiata e l&#39;utente potrebbe notare questo scambio di immagini?

Inizialmente, l&#39;elemento DOM dell’immagine protagonista è nascosto. Una volta ricevuta una risposta da [!DNL Target], at.js applica le modifiche DOM, come la sostituzione dell’IMG e la visualizzazione dell’immagine principale personalizzata.

## Quale doctype HTML richiede at.js?

at.js richiede il doctype HTML5.

La sintassi è:

`<!DOCTYPE html>`

Il doctype HTML5 assicura che la pagina venga caricata in modalità standard. Durante il caricamento in modalità quirks, alcune API JS dalle quali dipende at.js sono disabilitate. [!DNL Target] disabilita at.js in modalità quirks.

## at.js funziona in un ambiente di app Ionic.

Questa implementazione non è mai stata testata, poiché at.js non era destinato a funzionare in un ambiente non web. [!DNL Adobe] consiglia i relativi [SDK per le implementazioni mobili](/help/dev/implement/mobile/overview.md).
