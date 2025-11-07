---
title: Implementazione di un'applicazione a pagina singola per [!DNL Adobe Experience Platform Web SDK]
description: Scopri come creare un'implementazione di applicazione a pagina singola (SPA) di  [!DNL Adobe Experience Platform Web SDK]using [!DNL Target].
keywords: target;adobe target;visualizzazioni xdm; visualizzazioni;applicazioni a pagina singola;SPA;ciclo di vita SPA;lato client;test AB;AB;Targeting esperienza;XT;VEC
feature: AEP Web SDK
exl-id: 17e71e47-c7cc-421a-bc9c-53f45f587449
source-git-commit: 67cc93cf697f8d5bca6fedb3ae974e4012347a0b
workflow-type: tm+mt
source-wordcount: '1680'
ht-degree: 2%

---

# Implementazione di un&#39;applicazione a pagina singola

[!DNL Adobe Experience Platform Web SDK] offre funzionalità avanzate che consentono all&#39;azienda di eseguire personalizzazioni su tecnologie lato client di nuova generazione, ad esempio applicazioni a pagina singola (SPA).

I siti web tradizionali utilizzavano modelli di navigazione &quot;da pagina a pagina&quot;, detti anche Applicazioni a più pagine. In questi siti web le progettazioni sono strettamente collegate agli URL e lo spostamento tra le pagine richiedeva un caricamento di pagina completo.

Le applicazioni web moderne, come le applicazioni a pagina singola, hanno invece adottato un modello che attiva rapidamente il rendering dell’interfaccia utente del browser, spesso indipendente dai ricaricamenti delle pagine. Queste esperienze vengono attivate dalle interazioni dei clienti, ad esempio scorrimento, clic e movimenti del cursore. Con l’evolversi dei paradigmi del web moderno, la rilevanza degli eventi generici tradizionali, come il caricamento di una pagina, per distribuire personalizzazione e sperimentazione non funziona più.

![Diagramma che mostra il ciclo di vita delle applicazioni a pagina singola rispetto al ciclo di vita delle pagine tradizionali.](/help/dev/implement/client-side/aep-web-sdk/assets/spa-vs-traditional-lifecycle.png)

## Vantaggi di [!DNL Experience Platform Web SDK] per le applicazioni a pagina singola

Di seguito sono riportati alcuni vantaggi dell&#39;utilizzo di [!DNL Adobe Experience Platform Web SDK] per le applicazioni a pagina singola:

* Capacità di memorizzare nella cache tutte le offerte al caricamento di pagina per ridurre più chiamate al server a una singola chiamata al server.
* Migliora l’esperienza utente sul sito perché le offerte vengono visualizzate immediatamente tramite la cache senza il tempo di ritardo introdotto dalle chiamate al server tradizionali.
* Una singola riga di codice e una configurazione per sviluppatori una tantum consentono agli addetti al marketing di creare ed eseguire attività [!UICONTROL A/B Test] e [!UICONTROL Experience Targeting] (XT) tramite il [!UICONTROL Visual Experience Composer] (VEC) nell&#39;applicazione a pagina singola.

## Visualizzazioni XDM e applicazioni a pagina singola

Il Compositore esperienza visivo [!UICONTROL Adobe Target] per applicazioni a pagina singola sfrutta un concetto denominato [!UICONTROL Views]: un gruppo logico di elementi visivi che insieme formano un&#39;esperienza SPA. Un’applicazione a pagina singola può, pertanto, essere considerata come transizione attraverso le viste, anziché gli URL, in base alle interazioni dell’utente. In genere, un elemento [!UICONTROL View] può rappresentare un intero sito o elementi visivi raggruppati all&#39;interno di un sito.

Per ulteriori informazioni sulle visualizzazioni, nell&#39;esempio seguente viene utilizzato un ipotetico sito di e-commerce online implementato in [!DNL React] per esplorare l&#39;esempio [!UICONTROL Views].

Dopo aver visitato la home page, un’immagine protagonista promuove una vendita di Pasqua e gli ultimi prodotti disponibili sul sito. In questo caso, è possibile definire [!UICONTROL View] per l&#39;intera schermata iniziale. [!UICONTROL View] potrebbe semplicemente essere chiamato &quot;home&quot;.

![Immagine di esempio di un&#39;applicazione a pagina singola in una finestra del browser.](/help/dev/implement/client-side/aep-web-sdk/assets/example-views.png)

Quando il cliente diventa più interessato ai prodotti venduti dall&#39;azienda, decide di fare clic sul collegamento **Prodotti**. Analogamente alla home page, l&#39;intero sito prodotti può essere definito come [!UICONTROL View]. [!UICONTROL View] potrebbe essere denominato &quot;products-all&quot;.

![Immagine di esempio di un&#39;applicazione a pagina singola in una finestra del browser, con tutti i prodotti visualizzati.](/help/dev/implement/client-side/aep-web-sdk/assets/example-products-all.png)

Poiché un [!UICONTROL View] può essere definito come un intero sito o un gruppo di elementi visivi su un sito. I quattro prodotti visualizzati nel sito prodotti possono essere raggruppati e considerati come [!UICONTROL View]. Questa visualizzazione potrebbe essere denominata &quot;products&quot; (prodotti).

![Immagine di esempio di un&#39;applicazione a pagina singola in una finestra del browser, con prodotti di esempio visualizzati.](/help/dev/implement/client-side/aep-web-sdk/assets/example-products.png)

Quando il cliente decide di fare clic sul pulsante **Carica altro** per esplorare altri prodotti sul sito, in questo caso l&#39;URL del sito Web non cambia. Tuttavia, è possibile creare [!UICONTROL View] qui per rappresentare solo la seconda riga di prodotti visualizzati. Il nome [!UICONTROL View] può essere &quot;products-page-2&quot;.

![Immagine di esempio di un&#39;applicazione a pagina singola in una finestra del browser, con prodotti di esempio visualizzati in una pagina aggiuntiva.](/help/dev/implement/client-side/aep-web-sdk/assets/example-load-more.png)

Il cliente decide di acquistare alcuni prodotti dal sito e procede alla schermata di pagamento. Sul sito di pagamento, al cliente vengono offerte le opzioni per scegliere la consegna normale o express. Un [!UICONTROL View] può essere qualsiasi gruppo di elementi visivi su un sito, quindi un [!UICONTROL View] può essere creato per le preferenze di consegna e chiamato &quot;Preferenze di consegna&quot;.

![Immagine di esempio di una pagina di estrazione di un&#39;applicazione a pagina singola in una finestra del browser.](/help/dev/implement/client-side/aep-web-sdk/assets/example-check-out.png)

Il concetto di [!UICONTROL Views] può essere esteso molto oltre questo scenario. Questi scenari sono solo alcuni esempi di [!UICONTROL Views] che è possibile definire in un sito.

## Implementazione di [!UICONTROL XDM Views]

È possibile sfruttare [!UICONTROL XDM Views] in [!DNL Target] per consentire agli addetti al marketing di eseguire test A/B e XT sulle applicazioni a pagina singola tramite [!UICONTROL Visual Experience Composer]. Per completare la configurazione di uno sviluppatore una tantum, è necessario eseguire i passaggi seguenti:

1. Installa [Adobe Experience Platform Web SDK](https://experienceleague.adobe.com/it/docs/experience-platform/web-sdk/install/overview).
2. Determinare tutti i [!UICONTROL XDM Views] nell&#39;applicazione a pagina singola che si desidera personalizzare.
3. Dopo aver definito [!UICONTROL XDM Views], per distribuire le attività A/B o XT del Compositore esperienza visivo, implementare la funzione `sendEvent()` con `renderDecisions` impostato su `true` e i corrispondenti [!UICONTROL XDM View] nell&#39;applicazione a pagina singola. [!UICONTROL XDM View] deve essere passato in `xdm.web.webPageDetails.viewName`. Questo passaggio consente agli esperti di marketing di sfruttare [!UICONTROL Visual Experience Composer] per avviare test A/B e XT per tali XDM.

   ```javascript
   alloy("sendEvent", { 
     "renderDecisions": true, 
     "xdm": { 
       "web": { 
         "webPageDetails": { 
         "viewName":"home" 
         }
       } 
     } 
   });
   ```

>[!NOTE]
>
>Alla prima chiamata `sendEvent()`, tutti i [!UICONTROL XDM Views] di cui eseguire il rendering per l&#39;utente finale vengono recuperati e memorizzati in cache. Le `sendEvent()` chiamate successive con [!UICONTROL XDM Views] trasmesse vengono lette dalla cache e sottoposte a rendering senza una chiamata al server.

## `sendEvent()` esempi di funzioni

In questa sezione vengono illustrati tre esempi che illustrano come richiamare la funzione `sendEvent()` in React per un&#39;ipotetica applicazione a pagina singola per e-commerce.

### Esempio 1: home page per test A/B

Il team marketing desidera eseguire test A/B sull’intera home page.

![Immagine di esempio di un&#39;applicazione a pagina singola in una finestra del browser.](/help/dev/implement/client-side/aep-web-sdk/assets/use-case-1.png)

Per eseguire test A/B sull&#39;intero sito principale, è necessario richiamare `sendEvent()` con XDM `viewName` impostato su `home`:

```jsx
function onViewChange() { 
  
  var viewName = window.location.hash; // or use window.location.pathName if router works on path and not hash 

  viewName = viewName || 'home'; // view name cannot be empty 

  // Sanitize viewName to get rid of any trailing symbols derived from URL 

  if (viewName.startsWith('#') || viewName.startsWith('/')) { 
    viewName = viewName.substr(1); 
  }
   
  alloy("sendEvent", { 
    "renderDecisions": true, 
    "xdm": { 
      "web": { 
        "webPageDetails": { 
          "viewName":"home" 
        } 
      } 
    }
  }); 
} 

// react router v4 

const history = syncHistoryWithStore(createBrowserHistory(), store); 

history.listen(onViewChange); 

// react router v3 

<Router history={hashHistory} onUpdate={onViewChange} > 
```

### Esempio 2: prodotti personalizzati

Il team marketing desidera personalizzare la seconda riga di prodotti cambiando il colore dell&#39;etichetta prezzo in rosso dopo che un utente ha fatto clic su **Carica altro**.

![Immagine di esempio di un&#39;applicazione a pagina singola in una finestra del browser, con offerte personalizzate.](/help/dev/implement/client-side/aep-web-sdk/assets/use-case-2.png)

```jsx
function onViewChange(viewName) { 

  alloy("sendEvent", { 
    "renderDecisions": true, 
    "xdm": { 
      "web": { 
        "webPageDetails": { 
          "viewName": viewName
        }
      } 
    } 
  }); 
} 

class Products extends Component { 
  
  render() { 
    return ( 
      <button type="button" onClick={this.handleLoadMoreClicked}>Load more</button> 
    ); 
  } 

  handleLoadMoreClicked() { 
    var page = this.state.page + 1; // assuming page number is derived from component's state 
    this.setState({page: page}); 
    onViewChange('PRODUCTS-PAGE-' + page); 
  } 

} 
```

### Esempio 3: preferenze di consegna per test A/B

Il team marketing desidera eseguire un test A/B per verificare se cambiare il colore del pulsante da blu a rosso quando si seleziona **Consegna express**. Il team ritiene che questa modifica possa aumentare le conversioni (invece di mantenere il pulsante blu con entrambe le opzioni di consegna).

![Immagine di esempio di un&#39;applicazione a pagina singola in una finestra del browser, con test A/B.](/help/dev/implement/client-side/aep-web-sdk/assets/use-case-3.png)

Per personalizzare il contenuto sul sito a seconda della preferenza di consegna selezionata, è possibile creare un [!UICONTROL View] per ogni preferenza di consegna. Quando è selezionata l&#39;opzione **Consegna normale**, [!UICONTROL View] può essere denominato &quot;checkout-normal&quot;. Se è selezionata l&#39;opzione **Consegna express**, [!UICONTROL View] può essere denominato &quot;checkout-express&quot;.

```jsx
function onViewChange(viewName) { 
  alloy("sendEvent", { 
    "renderDecisions": true, 
    "xdm": { 
      "web": { 
        "webPageDetails": { 
          "viewName": viewName 
        }
      }
    }
  }); 
} 

class Checkout extends Component { 

  render() { 
    return ( 
      <div onChange={this.onDeliveryPreferenceChanged}> 
        <label> 
          <input type="radio" id="normal" name="deliveryPreference" value={"Normal Delivery"} defaultChecked={true}/> 
          <span> Normal Delivery (7-10 business days)</span> 
        </label> 
        <label> 
          <input type="radio" id="express" name="deliveryPreference" value={"Express Delivery"}/> 
          <span> Express Delivery* (2-3 business days)</span> 
        </label> 
      </div> 
    ); 
  } 

  onDeliveryPreferenceChanged(evt) { 
    var selectedPreferenceValue = evt.target.value; 
    onViewChange(selectedPreferenceValue); 
  } 

} 
```

## Utilizzo di [!UICONTROL Visual Experience Composer] per un&#39;applicazione a pagina singola

Dopo aver definito [!UICONTROL XDM Views] e implementato `sendEvent()` con i [!UICONTROL XDM Views] passati, il Compositore esperienza visivo è in grado di rilevare questi [!UICONTROL Views] e consentire agli utenti di creare azioni e modifiche per le attività A/B o XT.

>[!NOTE]
>
>Per utilizzare il Compositore esperienza visivo per l&#39;applicazione a pagina singola, è necessario installare e attivare [Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-target-vec-helper/) o [l&#39;estensione Chrome VEC Helper](https://experienceleague.adobe.com/it/docs/target/using/experiences/vec/troubleshoot-composer/visual-editing-helper-extension).

### Pannello [!UICONTROL Modifications]

Il pannello [!UICONTROL Modifications] acquisisce le azioni create per un particolare [!UICONTROL View]. Tutte le azioni per un [!UICONTROL View] sono raggruppate sotto tale [!UICONTROL View].

### Azioni

Facendo clic su un’azione viene evidenziato l’elemento del sito in cui questa viene applicata. Ogni azione del Compositore esperienza visivo creata in un [!UICONTROL View] presenta le icone seguenti: **Informazioni**, **Modifica**, **Clone**, **Sposta** e **Elimina**. Queste icone sono spiegate più dettagliatamente nella tabella seguente.

| Icona | Descrizione |
|---|---|
| Informazioni | Visualizza i dettagli dell’azione. |
| Modifica | Ti consente di modificare direttamente le proprietà dell’azione. |
| Clona | Clona l&#39;azione in uno o più [!UICONTROL Views] presenti nel pannello [!UICONTROL Modifications] o in uno o più [!UICONTROL Views] a cui sei passato nel Compositore esperienza visivo. L&#39;azione non deve necessariamente essere presente nel pannello [!UICONTROL Modifications].<br/><br/>**Nota:** dopo un&#39;operazione di clonazione, è necessario passare a [!UICONTROL View] nel Compositore esperienza visivo tramite [!UICONTROL Browse] per verificare se l&#39;azione clonata è un&#39;operazione valida. Se l&#39;azione non può essere applicata a [!UICONTROL View], viene visualizzato un errore. |
| Sposta | Sposta l&#39;azione in un [!UICONTROL Page Load Event] o in un altro [!UICONTROL View] già esistente nel pannello [!UICONTROL Modifications].<br/><br/>**Evento di caricamento pagina:** Tutte le azioni corrispondenti all&#39;evento di caricamento pagina vengono applicate al caricamento iniziale della pagina dell&#39;applicazione Web. <br/><br/>**Nota:** dopo un&#39;operazione di spostamento, è necessario passare a [!UICONTROL View] nel Compositore esperienza visivo tramite [!UICONTROL Browse] per verificare se lo spostamento è un&#39;operazione valida. Se l&#39;azione non può essere applicata a [!UICONTROL View], viene visualizzato un errore. |
| Elimina | Elimina l’azione. |

## Esempi di utilizzo del Compositore esperienza visivo per le applicazioni a pagina singola

Questa sezione descrive tre esempi per l&#39;utilizzo di [!UICONTROL Visual Experience Composer] per creare azioni e modifiche per le attività A/B o XT.

### Esempio 1: aggiornare la vista &quot;home&quot;

In precedenza in questo articolo era stato definito un [!UICONTROL View] denominato &quot;home&quot; per l&#39;intero sito principale. Ora il team marketing desidera aggiornare la visualizzazione &quot;home&quot; nei seguenti modi:

* Cambia i pulsanti **Aggiungi al carrello** e **Mi piace** in una tonalità blu più chiara. Questa modifica dovrebbe verificarsi durante il caricamento della pagina perché comporta la modifica di componenti dell’intestazione.
* Modifica l&#39;etichetta **Ultimi prodotti per il 2026** in **Migliori prodotti per il 2026** e modifica il colore del testo in viola.

Per apportare questi aggiornamenti nel Compositore esperienza visivo, seleziona **Componi** e applica le modifiche alla visualizzazione &quot;Home page&quot;.

![Pagina di esempio del Compositore esperienza visivo.](/help/dev/implement/client-side/aep-web-sdk/assets/vec-home.png)

### Esempio 2: modificare le etichette dei prodotti

Per &quot;products-page-2&quot; [!UICONTROL View], il team marketing desidera cambiare l&#39;etichetta **Price** in **Sale Price** e cambiare il colore dell&#39;etichetta in rosso.

Per apportare questi aggiornamenti nel Compositore esperienza visivo, è necessario attenersi ai seguenti passaggi:

1. Seleziona **Sfoglia** nel Compositore esperienza visivo.
2. Seleziona **Prodotti** nella navigazione superiore del sito.
3. Seleziona **Carica altro** una volta per visualizzare la seconda riga di prodotti.
4. Seleziona **Componi** nel Compositore esperienza visivo.
5. Applica le azioni per modificare l&#39;etichetta di testo in **Prezzo di vendita** e il colore in rosso.

![Pagina di esempio del Compositore esperienza visivo con etichette di prodotto.](/help/dev/implement/client-side/aep-web-sdk/assets/vec-products-page-2.png)

### Esempio 3: personalizzare lo stile delle preferenze di consegna

[!UICONTROL Views] può essere definito a livello granulare, ad esempio uno stato o un&#39;opzione da un pulsante di opzione. In precedenza in questo articolo [!UICONTROL Views] erano stati definiti per le preferenze di consegna, &quot;checkout-normal&quot; e &quot;checkout-express&quot;. Il team marketing desidera cambiare il colore del pulsante in rosso per la visualizzazione &quot;checkout-express&quot;.

Per apportare questi aggiornamenti nel Compositore esperienza visivo, è necessario attenersi ai seguenti passaggi:

1. Seleziona **Sfoglia** nel Compositore esperienza visivo.
2. Aggiungi prodotti al carrello sul sito.
3. Seleziona l’icona del carrello nell’angolo in alto a destra del sito.
4. Seleziona **Estrai il tuo ordine**.
5. Selezionare il pulsante di opzione **Consegna express** in **Preferenze di consegna**.
6. Seleziona **Componi** nel Compositore esperienza visivo.
7. Cambia il colore del pulsante **Paga** in rosso.

>[!NOTE]
>
>Il messaggio &quot;checkout-express&quot; [!UICONTROL View] non viene visualizzato nel pannello [!UICONTROL Modifications] finché non viene selezionato il pulsante di opzione **Consegna express**. Questo perché la funzione `sendEvent()` viene eseguita quando viene selezionato il pulsante di opzione **Consegna express**, pertanto il Compositore esperienza visivo non è a conoscenza di &quot;checkout-express&quot; [!UICONTROL View] finché il pulsante di opzione non viene selezionato.

![Il Compositore esperienza visivo mostra il selettore delle preferenze di consegna.](/help/dev/implement/client-side/aep-web-sdk/assets/vec-delivery-preference.png)
