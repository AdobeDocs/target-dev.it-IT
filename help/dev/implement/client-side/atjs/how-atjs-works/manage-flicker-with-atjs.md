---
keywords: visualizzazione momentanea di altri contenuti, at.js, implementazione, asincrona, asincrona, sincrona, $8
description: Scopri come at.js e [!DNL Target] evita sfarfallii (il contenuto predefinito viene visualizzato momentaneamente prima di essere sostituito dal contenuto dell’attività) durante il caricamento della pagina o dell’app.
title: Come gestisce at.js la visualizzazione momentanea di altri contenuti?
feature: at.js
exl-id: 8aacf254-ec3d-4831-89bb-db7f163b3869
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '693'
ht-degree: 63%

---

# Gestione at.js della visualizzazione momentanea di altri contenuti

Informazioni su come [!DNL Adobe Target] La libreria JavaScript at.js impedisce la visualizzazione momentanea di altri contenuti durante il caricamento della pagina o dell’app.

La visualizzazione momentanea di altri contenuti si verifica quando il contenuto predefinito viene momentaneamente visualizzato ai visitatori prima che venga sostituito dal contenuto dell’attività. Tale visualizzazione momentanea è da evitare perché può confondere i visitatori.

## Utilizzo di una mbox globale creata automaticamente

Se abiliti l’impostazione [Creazione automatica di una mbox globale](/help/dev/implement/client-side/atjs/global-mbox/customize-global-mbox.md) quando configuri at.js, at.js gestisce la visualizzazione momentanea di altri contenuti modificando l’impostazione di opacità durante il caricamento della pagina. Quando si carica, at.js cambia l’impostazione di opacità dell’elemento `<body>` Su “0”, rendendo la pagina inizialmente invisibile ai visitatori. Dopo una risposta da [!DNL Target] viene ricevuto, o se si verifica un errore con [!DNL Target] richiesta rilevata: at.js reimposta l&#39;opacità su &quot;1&quot;. In questo modo il visitatore vede la pagina solo dopo l&#39;applicazione del contenuto delle attività.

Se si abilita l&#39;impostazione, quando si configura at.js, at.js imposta l&#39;elemento HTML “BODY” con un valore di opacità pari a 0. Dopo una risposta da [!DNL Target] Ricevuto, at.js reimposta l&#39;opacità HTML BODY su 1.

L&#39;opzione opacità impostata su 0 mantiene il contenuto della pagina nascosto per impedire la visualizzazione momentanea di altri contenuti, ma il browser esegue ancora il rendering della pagina e carica tutte le risorse necessarie come CSS, immagini e così via.

Se `opacity: 0` non funziona nell’implementazione, puoi anche gestire la visualizzazione momentanea di altri contenuti personalizzando `bodyHiddenStyle` e impostarlo su `body {visibility:hidden !important}`. È possibile utilizzare `body {opacity:0 !important}` o `body {visibility:hidden !important}`, a seconda di quale sia il migliore per il tuo caso specifico.

La figura seguente mostra le chiamate per Nascondi corpo e Mostra corpo, sia in at.js 1.*x* che in at.js 2.x.

**at.js 2.x**

(Fare clic sull&#39;immagine per espanderla a larghezza intera.)

![Flusso di Target: richiesta di caricamento pagina di at.js](/help/dev/implement/client-side/assets/atjs-20-flow-page-load-request.png "Flusso di Target: richiesta di caricamento pagina di at.js"){zoomable=&quot;yes&quot;}

**at.js 1.*x***

(Fare clic sull&#39;immagine per espanderla a larghezza intera.)

![Flusso di Target: mbox globale creata automaticamente](/help/dev/implement/client-side/atjs/how-atjs-works/assets/target-flow2.png "Flusso di Target: mbox globale creata automaticamente"){zoomable=&quot;yes&quot;}

Per ulteriori informazioni sull’override di `bodyHiddenStyle`, consulta [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md).

## Gestione della visualizzazione momentanea di altri contenuti durante il caricamento di at.js in modo asincrono

Caricare at.js in modo asincrono è un ottimo modo per evitare di bloccare il rendering del browser; tuttavia, questa tecnica può portare alla visualizzazione momentanea di altri contenuti della pagina web.

È possibile evitare questo fenomeno utilizzando uno snippet per nascondere le pagine che sarà visibile dopo che Target avrà personalizzato gli elementi HTML rilevanti.

at.js può essere caricato in modo asincrono, direttamente incorporato nella pagina o tramite un gestore di tag (ad esempio Adobe Experience Platform Launch).

Se at.js è incorporato nella pagina, lo snippet deve essere aggiunto prima di caricare at.js. Se carichi at.js tramite un gestore di tag, che viene caricato anche in modo asincrono, devi aggiungere lo snippet prima di caricare il gestore di tag. Se il gestore di tag viene caricato in modo sincrono, lo script potrebbe essere incluso nel gestore di tag prima di at.js.

Lo snippet di codice per nascondere le pagine è il seguente:

```
;(function(win, doc, style, timeout) {
  var STYLE_ID = 'at-body-style';

  function getParent() {
    return doc.getElementsByTagName('head')[0];
  }

  function addStyle(parent, id, def) {
    if (!parent) {
      return;
    }

    var style = doc.createElement('style');
    style.id = id;
    style.innerHTML = def;
    parent.appendChild(style);
  }

  function removeStyle(parent, id) {
    if (!parent) {
      return;
    }

    var style = doc.getElementById(id);

    if (!style) {
      return;
    }

    parent.removeChild(style);
  }

  addStyle(getParent(), STYLE_ID, style);
  setTimeout(function() {
    removeStyle(getParent(), STYLE_ID);
  }, timeout);
}(window, document, "body {opacity: 0 !important}", 3000));
```

Per impostazione predefinita, lo snippet nasconde l’intera sezione BODY nell’HTML. In alcuni casi, puoi voler nascondere alcuni elementi HTML e non l’intera pagina. Per farlo, puoi personalizzare il parametro di stile. Puoi sostituirlo con qualcosa che nasconde preventivamente solo particolari zone della pagina.

Ad esempio, se hai due aree identificate dagli ID container-1 e container-2, puoi sostituire lo stile come segue:

```
#container-1, #container-2 {opacity: 0 !important}
```

Anziché il valore predefinito:

```
body {opacity: 0 !important}
```

## Gestione della visualizzazione momentanea di altri contenuti in at.js 2.x per triggerView()

Quando si utilizza `triggerView()` per visualizzare contenuti mirati nell’applicazione a pagina singola, la gestione della visualizzazione momentanea di altri contenuti è preconfigurata. Questo significa che non è necessario aggiungere la logica preliminare manualmente. Al contrario, at.js 2.x nasconde preventivamente la posizione in cui la visualizzazione deve essere mostrata prima di applicare il contenuto mirato.

## Gestire la visualizzazione momentanea di altri contenuti con getOffer() e applyOffer()

Poiché sia `getOffer()` che `applyOffer()` sono API di basso livello, non è incorporato alcun controllo sulla visualizzazione momentanea di altri contenuti. Puoi passare un selettore o un elemento HTML come opzione ad `applyOffer()`; in questo caso `applyOffer()` aggiunge il contenuto dell’attività a questo particolare elemento; tuttavia, è necessario assicurarsi che l’elemento sia correttamente nascosto prima di richiamare `getOffer()` e `applyOffer()`.

```
document.documentElement.style.opacity = "0";
 
adobe.target.getOffer({
    mbox: 'target-global-mbox',
    success: function(offer) {
        adobe.target.applyOffer({
            mbox: 'target-global-mbox',
            offer: offer
        });
 
        document.documentElement.style.opacity = "1";
    },
    error: function() {
        document.documentElement.style.opacity = "1";        
    }
});
```

## Utilizzo di una mbox regionale con mboxCreate() in at.js 1.x (non supportato in at.js 2.x)

Se utilizzi un’implementazione di mbox regionale, utilizza `mboxCreate()` con il provisioning della pagina configurato in modo simile al seguente codice di esempio:

```
<div class="mboxDefault">
Some default content
</div>
<script>
mboxCreate('some-mbox');
</script>
```

Se il provisioning delle tue pagine viene eseguito correttamente, at.js gestisce la visualizzazione momentanea di altri contenuti cambiando opportunamente la proprietà CSS “visibility” dell’elemento con la classe mboxDefault.
