---
keywords: prehide SDK, sfarfallio, anti-sfarfallio, pre-hiding, pre-hiding, alloy, at.js, implementazione, consenso, CMP, posizionamento script, inline, esterno, selezione SDK
description: Scopri come integrare  [!DNL Adobe Target] Prehide SDK per eliminare la visualizzazione momentanea di contenuti non personalizzati (visualizzazione momentanea di altri contenuti) durante il caricamento della pagina. SDK funziona sia con Adobe Alloy (Web SDK) che con at.js.
title: Guida all’integrazione di Prehide SDK
feature: Implementation
hide: true
source-git-commit: 2f7a53b667990474dfab7ca66a8ea93d2e946548
workflow-type: tm+mt
source-wordcount: '1007'
ht-degree: 0%

---


# Guida all’integrazione di Prehide SDK

Una piccola libreria sincrona JavaScript che impedisce lo sfarfallio visivo causato dalla personalizzazione [!DNL Adobe Target] che riscrive un rendering intermedio della pagina. Aggiungi un tag `<script>` in linea nella parte superiore di `<head>` e la pagina viene visualizzata solo dopo che il contenuto personalizzato è pronto o il timer di sicurezza si attiva.

## Passaggi dell’integrazione {#integration-steps}

1. Scarica il bundle.

   Utilizzare l&#39;interfaccia utente di Gestione sfarfallio per scaricare `prehide.min.js`. Il file è preconfigurato con il codice client e il timeout di protezione, quindi non è necessario alcun blocco `PrehideConfig`.

1. Incorporalo in linea nella parte superiore di `<head>`.

   Incolla il contenuto di `prehide.min.js` direttamente all&#39;interno di un tag `<script>` in linea come primo elemento figlio di `<head>`. Vedi [In linea rispetto a esterno](#inline-vs-external) per informazioni sul motivo per cui è preferibile utilizzare la modalità in linea.

   ```html
   <!-- 1. Prehide SDK: must be FIRST in <head> and BEFORE any Adobe SDK -->
   <script>
     // paste the contents of prehide.min.js here
   </script>
   
   <!-- 2. Then load Alloy.js OR at.js -->
   <script src="https://your-cdn/alloy.min.js"></script>
   ```

1. (Facoltativo) Aggiungi un blocco di configurazione runtime.

   Obbligatorio solo se ospiti autonomamente il bundle senza scaricare tramite l’interfaccia utente o se devi ignorare la scelta di SDK. Posiziona il blocco di configurazione prima dello script prehide:

   ```html
   <script>
     window.PrehideConfig = {
       sdk: "alloy"            // or "atjs" (defaults to "alloy")
     };
   </script>
   <script> /* prehide.min.js inline contents */ </script>
   <script src="https://your-cdn/alloy.min.js"></script>
   ```

1. (Facoltativo) Consenso per la revoca del consenso.

   Se l&#39;implementazione utilizza una piattaforma di gestione del consenso (CMP, Consent Management Platform), chiama `window.Prehide.setConsent(...)` non appena è noto lo stato del consenso. Consulta [Gestione del consenso](#consent-management).

1. Verifica.

   Apri DevTools e conferma che `<style id="alloy-prehiding">` (o `at-body-style` per at.js) sia visualizzato in `<head>` sul primo disegno e venga rimosso al termine del SDK. Eseguire `window.Prehide.getState()` nella console per verificare lo stato del runtime.

## Dove posizionare lo script {#script-placement}

>[!IMPORTANT]
>
>Prehide SDK deve essere eseguito prima di Alloy/at.js. Se Alloy viene caricato per primo, la pagina esegue il rendering del contenuto non personalizzato e quindi esegue nuovamente il rendering. Questo è esattamente lo sfarfallio che questo SDK è progettato per prevenire.
></br>>Non aggiungere `async` o `defer` al tag di script Prehide SDK. È necessaria l’esecuzione sincrona in modo che la regola di nasconderlo venga inserita prima che il browser inizi a disporre la pagina.

Il SDK di previsualizzazione deve essere visualizzato prima nel documento rispetto al SDK [!DNL Adobe Target] che si ripulisce dopo. L&#39;ordine di caricamento non è negoziabile:

```html
<!doctype html>
<html>
<head>
  <!-- ① Prehide SDK FIRST. Inline. Synchronous. No async/defer. -->
  <script> /* prehide.min.js inline contents */ </script>

  <!-- ② Alloy or at.js loads next -->
  <script src="https://cdn.alloy.adobe.com/alloy.min.js"></script>

  <!-- ③ Then everything else: meta, css, third-party tags, ... -->
  <link rel="stylesheet" href="main.css">
</head>
<body> ... your page ... </body>
</html>
```

## Inline ed esterno {#inline-vs-external}

Esistono due modi per includere `prehide.min.js`:

| Metodo | Esempio | Note |
| --- | --- | --- |
| In linea (preferito) | `<script>/* full contents of prehide.min.js pasted directly into the page */</script>` | Nessun round trip di rete. SDK viene eseguito prima di eseguire qualsiasi altra operazione. |
| Esterna (solo se non è possibile eseguire l&#39;inline) | `<script src="/static/prehide.min.js"></script>` | Introduce una richiesta di rete di blocco prima del primo rendering. Anche con HTTP/2 e il caching perimetrale, in genere questo costa 30-80 ms. |

### Perché è preferibile utilizzare la modalità in linea

>[!TIP]
>
>Allinea il bundle direttamente in `<script>...</script>` nel modello HTML. Consideralo come un blocco CSS critico: piccolo, in linea e sempre al vertice.

* Nessun recupero con blocco del rendering. Lo scopo di Prehide SDK consiste nell&#39;inserire le regole nascoste *prima* del primo rendering. Un `<script src>` esterno aggiunge un round trip di rete esattamente in quella finestra critica.
* Nessuna nuova modalità di errore. Un file esterno può essere 404, andare in timeout o essere bloccato da un ad-blocker. Impossibile eseguire una copia in linea.
* Il bundle è piccolo (~6 KB). L&#39;inline costa meno di un tipico favicon, e nessun vantaggio di caching è abbastanza grande da superare il round-trip supplementare al primo rendering.
* Compatibile con la cache. Quando è allineato nella risposta di HTML, il SDK viene memorizzato nella cache insieme al resto del documento dal livello di caching esistente (CDN o cache HTTP del browser).
* Pacchetto specifico per il cliente. Il codice client del file scaricato viene inserito automaticamente al momento del download. L’allineamento garantisce che ogni visitatore riceva il bundle personalizzato corretto senza una richiesta aggiuntiva.

## Configurazione {#configuration}

SDK accetta la configurazione da due origini, in ordine di priorità. Viene letto quello disponibile per primo.

### Runtime `window.PrehideConfig` (integrazione manuale)

Per i bundle self-hosted o non modificati, dichiarate un oggetto config prima dell&#39;esecuzione dello script prehide:

```html
<script>
  window.PrehideConfig = {
    sdk: "alloy"             // optional: "alloy" (default) or "atjs"
  };
</script>
```

| Campo | Tipo | Obbligatorio | Descrizione |
| --- | --- | --- | --- |
| `sdk` | `"alloy"` \| `"atjs"` | No | Il SDK di Adobe caricato sulla pagina. Vedi [Selezione SDK](#sdk-selection). |

## Selezione SDK {#sdk-selection}

[!DNL Adobe Target] offre due SDK di consegna: Alloy (il moderno Web SDK) e at.js (la libreria classica). Ogni cerca un *diverso* `<style>` elemento `id` al termine della personalizzazione e lo rimuove per visualizzare la pagina. Prehide SDK deve inserire `id` corrispondente. In caso contrario, la pagina rimane nascosta fino all&#39;attivazione del timer di sicurezza.

| Valore `sdk` | ID tag di stile inserito | Rimosso da | Quando utilizzare |
| --- | --- | --- | --- |
| `"alloy"` *(predefinito)* | `<style id="alloy-prehiding">` | Alloy SDK su personalize-complete | Stai caricando Alloy / Adobe Web SDK su questa pagina. |
| `"atjs"` | `<style id="at-body-style">` | at.js su personalize-complete | Stai caricando la libreria at.js classica su questa pagina. |

>[!NOTE]
>
>Per at.js SDK, è supportata solo la versione 2.x e successive.

### Come impostarlo

```html
<!-- For at.js -->
<script>
  window.PrehideConfig = { sdk: "atjs" };
</script>
<script> /* prehide.min.js inline */ </script>
<script src="https://cdn.adobe.com/.../at.js"></script>
```

>[!WARNING]
>
>Avviso di mancata corrispondenza. L&#39;impostazione di `sdk: "alloy"` durante il caricamento di at.js (o viceversa) indica che SDK non troverà l&#39;elemento di pre-hide da rimuovere. Il timer di guardia alla fine rivelerà la pagina, ma i visitatori sperimenteranno una finestra nascosta più lunga. Imposta sempre `sdk` in modo che corrisponda alla libreria da caricare.

I valori sconosciuti o mancanti tornano a `"alloy"`, pertanto le integrazioni di Alloy esistenti continuano a funzionare senza alcuna modifica alla configurazione.

## Gestione del consenso {#consent-management}

>[!NOTE]
>
>* Il valore del consenso non viene mai archiviato in `window`. Solo la funzione è esposta; lo stato interno rimane privato per SDK.
>* Le transizioni da `"out"` a `"in"` non nascondono nuovamente la pagina, in quanto nascondere nuovamente il contenuto con rendering completo potrebbe causare interruzioni visive.
>* `setConsent` può essere chiamato più volte all&#39;interno di una singola pagina. Ogni chiamata sostituisce lo stato precedente.

Prehide SDK include un’API in grado di riconoscere il consenso per il coordinamento con la tua CMP. L’utilizzo di è facoltativo. Se `setConsent` non viene mai chiamato, SDK si comporta come un&#39;integrazione standard senza consenso.

### Superficie API

```js
// Single function call. Pass a status string.
window.Prehide.setConsent("pending");  // banner shown, awaiting decision
window.Prehide.setConsent("in");       // user accepted personalization
window.Prehide.setConsent("out");      // user declined personalization

// Read-only debug snapshot.
window.Prehide.getState();
// → { sdk, version, consentStatus, consentApiUsed,
//      rulesResolved, hasSelectorsToGuard, guardTimeout }
```

### Funzionamento di ogni stato

| Chiamata | Effetto sul timer di guardia | Effetto sui contenuti nascosti |
| --- | --- | --- |
| `setConsent("pending")` | Timer attivo cancellato. La sicurezza non rivela incendi fino alla risoluzione del consenso. | I selettori rimangono nascosti a tempo indeterminato. |
| `setConsent("in")` | Cancellato, quindi riavviato con il timeout configurato. Attende che le regole vengano risolte se non lo hanno ancora fatto. | Il contenuto rimane nascosto fino a quando il SDK non si personalizza o il timer di guardia non viene attivato. |
| `setConsent("out")` | Cancellato. La pagina viene mostrata immediatamente. | La pagina viene visualizzata immediatamente. Le regole che si risolvono in seguito *non* nascondono nuovamente il contenuto. |
| *(non chiamato)* | Il timer predefinito viene eseguito da `init()` per la durata configurata. | Il contenuto rimane nascosto fino a quando il SDK non si personalizza o il timer di guardia non viene attivato. (modalità precedente compatibile con le versioni precedenti). |

### Schema consigliato: fase in sospeso esplicita

1. Non appena la tua CMP visualizza l&#39;interfaccia utente di consenso, chiama `setConsent("pending")`. Questo cancella il timer di sicurezza in modo che la pagina rimanga nascosta mentre il visitatore sta decidendo, impedendo un lampo di contenuti non personalizzati dietro il banner.

   ```js
   window.Prehide.setConsent("pending");
   ```

1. Quando il visitatore accetta la personalizzazione, chiama `setConsent("in")`. Il timer di sicurezza si riavvia e Alloy/at.js subentra, rivelando la pagina una volta applicata la personalizzazione.

   ```js
   window.Prehide.setConsent("in");
   ```

1. Quando il visitatore rifiuta la personalizzazione, chiama `setConsent("out")`. La pagina viene visualizzata immediatamente e rimane visibile. Le regole CDN che si risolvono in un secondo momento non la nasconderanno di nuovo.

   ```js
   window.Prehide.setConsent("out");
   ```

### Esempio: integrazione in stile OneTrust

```js
// Called once OneTrust has rendered its banner.
function onOneTrustReady() {
  // Pause the guard timer while the banner is visible.
  window.Prehide.setConsent("pending");

  OneTrust.OnConsentChanged(function (event) {
    if (event.consentedToTargeting) {
      window.Prehide.setConsent("in");
    } else {
      window.Prehide.setConsent("out");
    }
  });
}
```

### Esempio: TCF / stile IAB

```js
// Optional: pause the guard while UI is up.
window.Prehide.setConsent("pending");

// Your CMP eventually emits the final TC string.
function onTcData(tcData) {
  const hasTargetConsent = /* derive from tcData */;
  window.Prehide.setConsent(hasTargetConsent ? "in" : "out");
}
```

