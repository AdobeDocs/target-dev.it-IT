---
title: Utilizza  [!DNL Adobe Target] con [!DNL Web SDK] per la personalizzazione.
description: Scopri come eseguire il rendering dei contenuti personalizzati con  [!DNL Experience Platform Web SDK] using [!DNL Adobe Target].
feature: AEP Web SDK
source-git-commit: 1fe6adc25604612ed9fa090f1f68c18b9c0bdf63
workflow-type: tm+mt
source-wordcount: '1342'
ht-degree: 3%

---

# Usa [!DNL Adobe Target] e [!DNL Web SDK] per la personalizzazione

[!DNL Adobe Experience Platform] [!DNL Web SDK] può inviare ed eseguire il rendering di esperienze personalizzate gestite in [!DNL Adobe Target] al canale web. È possibile utilizzare un editor di WYSIWYG, denominato [Compositore esperienza visivo](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html), o un&#39;interfaccia non visiva, il [Compositore esperienza basato su moduli](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html), per creare, attivare e distribuire le attività e le esperienze di personalizzazione.

>[!IMPORTANT]
>
>Scopri come migrare l&#39;implementazione di [!DNL Target] in [!DNL Experience Platform Web SDK] con l&#39;esercitazione [Migrare Target da at.js 2.x ad Experience Platform Web SDK](https://experienceleague.adobe.com/docs/platform-learn/migrate-target-to-websdk/introduction.html?lang=it).
>
>Scopri come implementare [!DNL Target] per la prima volta con l&#39;esercitazione [Implementare Adobe Experience Cloud con Web SDK](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/overview.html?lang=it). Per informazioni specifiche di [!DNL Target], vedere la sezione dell&#39;esercitazione dal titolo [Configurare Target con Experience Platform Web SDK](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/applications-setup/setup-target.html).

Le seguenti funzionalità sono state testate e sono attualmente supportate in [!DNL Target]:

* [Test A/B](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html)
* [Generazione di rapporti sulle impression e sulle conversioni A4T](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html)
* [Attività di Automated Personalization](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html)
* [Attività Targeting esperienza](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html)
* [Test multivariati (TMV)](https://experienceleague.adobe.com/docs/target/using/activities/multivariate-test/multivariate-testing.html)
* [Attività Consigli](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations.html)
* [Generazione di rapporti sulle impression e le conversioni di Target nativi](https://experienceleague.adobe.com/docs/target/using/reports/reports.html)
* [Supporto VEC](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html)

## Diagramma del sistema di [!DNL Web SDK]

Il diagramma seguente consente di comprendere il flusso di lavoro di [!DNL Target] e [!DNL Web SDK] Edge Decisioning.

![Diagramma di Adobe Target Edge Decisioning con Experience Platform Web SDK](/help/dev/implement/client-side/aep-web-sdk/assets/target-platform-web-sdk-new.png)

| Chiamata | Dettagli |
| --- | --- |
| 1 | Il dispositivo carica [!DNL Web SDK]. [!DNL Web SDK] invia una richiesta ad Edge Network con i dati XDM, l&#39;ID ambiente Datastreams, i parametri immessi e l&#39;ID cliente (facoltativo). La pagina (o i contenitori) è pre-nascosta. |
| 2 | Edge Network invia la richiesta ai servizi Edge di per arricchirla con l’ID visitatore, il consenso e altre informazioni contestuali sul visitatore, come la geolocalizzazione e i nomi descrittivi dei dispositivi. |
| 3 | Edge Network invia la richiesta di personalizzazione arricchita al server Edge [!DNL Target] con l&#39;ID visitatore e i parametri immessi. |
| 4 | Gli script di profilo vengono eseguiti e quindi inseriti nell&#39;archivio dei profili [!DNL Target]. L&#39;archiviazione del profilo recupera i segmenti da [!UICONTROL Audience Library] (ad esempio, i segmenti condivisi da [!DNL Adobe Analytics], [!DNL Adobe Audience Manager], [!DNL Adobe Experience Platform]). |
| 5 | In base ai parametri di richiesta dell&#39;URL e ai dati di profilo, [!DNL Target] determina le attività ed esperienze da visualizzare per il visitatore per la visualizzazione della pagina corrente e per le visualizzazioni preacquisite future. [!DNL Target] invia quindi di nuovo questo messaggio ad Edge Network. |
| 6 | a. Edge Network invia nuovamente la risposta di personalizzazione alla pagina, includendo facoltativamente i valori di profilo per ulteriore personalizzazione. Il contenuto personalizzato nella pagina corrente viene mostrato il più rapidamente possibile senza che venga visualizzato momentaneamente il contenuto predefinito.<br> b. Il contenuto personalizzato per le viste mostrate come risultato delle azioni dell’utente in un’applicazione a pagina singola viene memorizzato in cache, in modo da applicarlo immediatamente senza una chiamata al server aggiuntiva quando si attivano le viste. <br>c. L’Edge Network invia l’ID visitatore e altri valori nei cookie, come consenso, ID sessione, identità, controllo dei cookie, personalizzazione. |
| 7 | Il Web SDK invia la notifica dal dispositivo all&#39;Edge Network. |
| 8 | Edge Network inoltra i dettagli di [!UICONTROL Analytics for Target] (A4T) (metadati di attività, esperienza e conversione) al server Edge di [!DNL Analytics]. |

## Abilitazione di [!DNL Adobe Target]

Per abilitare [!DNL Target], eseguire le operazioni seguenti:

1. Abilita [!DNL Target] nel [flusso di dati](https://experienceleague.adobe.com/en/docs/experience-platform/datastreams/overview) con il codice client appropriato.
1. Aggiungi l&#39;opzione `renderDecisions` ai tuoi eventi.

Quindi, facoltativamente, puoi anche aggiungere le seguenti opzioni:

* **`decisionScopes`**: recupera attività specifiche (utili per le attività create con il compositore basato su moduli) aggiungendo questa opzione agli eventi.
* **[Frammento pre-hiding](https://experienceleague.adobe.com/en/docs/experience-platform/web-sdk/personalization/manage-flicker)**: consente di nascondere solo alcune parti della pagina.

## Utilizzo del Compositore esperienza visivo [!UICONTROL Adobe Target]

Per utilizzare il Compositore esperienza visivo con un&#39;implementazione [!DNL Web SDK], installa e attiva l&#39;estensione [Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-target-vec-helper/) o [Chrome](https://experienceleague.adobe.com/en/docs/target/using/experiences/vec/troubleshoot-composer/visual-editing-helper-extension) VEC Helper.

Per ulteriori informazioni, consulta [Estensione helper per Compositore esperienza visivo](https://experienceleague.adobe.com/docs/target/using/experiences/vec/troubleshoot-composer/vec-helper-browser-extension.html) nella *guida di Adobe Target*.

## Rendering di contenuti personalizzati

Per ulteriori informazioni, consulta [Rendering del contenuto di personalizzazione](https://experienceleague.adobe.com/en/docs/experience-platform/web-sdk/personalization/rendering-personalization-content).

## Pubblico in XDM

Quando definisci i tipi di pubblico per le attività [!DNL Target] distribuite tramite [!DNL Web SDK], è necessario definire e utilizzare [XDM](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html). Dopo aver definito schemi, classi e gruppi di campi dello schema XDM, puoi creare una regola di pubblico [!DNL Target] definita dai dati XDM per il targeting. All&#39;interno di [!DNL Target], i dati XDM vengono visualizzati in [!UICONTROL Audience Builder] come parametro personalizzato. XDM viene serializzato utilizzando la notazione del punto (ad esempio, `web.webPageDetails.name`).

Se hai [!DNL Target] attività con tipi di pubblico predefiniti che utilizzano parametri personalizzati o un profilo utente, non vengono distribuite correttamente tramite SDK. Invece di utilizzare parametri personalizzati o il profilo utente, devi utilizzare XDM. Tuttavia, sono presenti campi di targeting del pubblico predefiniti supportati tramite [!DNL Web SDK] che non richiedono XDM. Questi campi sono disponibili nell&#39;interfaccia utente di [!DNL Target] che non richiedono XDM:

* Libreria di Target
* Geo
* Rete
* Sistema operativo
* Pagine del sito
* Browser
* Origini del traffico
* Arco temporale

Per ulteriori informazioni, consulta [Categorie di pubblico](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/target-rules.html) nella *guida di Adobe Target*.

### Token di risposta

I token di risposta vengono utilizzati per inviare metadati a terze parti come Google o Facebook. Vengono restituiti i token di risposta
nel campo `meta` entro `propositions` > `items`.

Ecco un esempio:

```json
{
  "id": "AT:eyJhY3Rpdml0eUlkIjoiMTI2NzM2IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
  "scope": "__view__",
  "scopeDetails": ...,
  "renderAttempted": true,
  "items": [
    {
      "id": "0",
      "schema": "https://ns.adobe.com/personalization/dom-action",
      "meta": {
        "experience.id": "0",
        "activity.id": "126736",
        "offer.name": "Default Content",
        "offer.id": "0"
      }
    }
  ]
}
```

Per raccogliere i token di risposta, è necessario sottoscrivere la promessa `alloy.sendEvent`, eseguire iterazioni in `propositions` ed estrarre i dettagli da `items` -> `meta`.

Ogni `proposition` ha un campo booleano `renderAttempted` che indica se è stato eseguito il rendering di `proposition`. Vedi l’esempio seguente:

```js
alloy("sendEvent",
  {
    "renderDecisions": true,
    "decisionScopes": [
      "hero-container"
    ]
  }).then(result => {
    const { propositions } = result;

    // filter rendered propositions
    const renderedPropositions = propositions.filter(proposition => proposition.renderAttempted === true);

    // collect the item metadata that represents the response tokens
    const collectMetaData = (items) => {
      return items.filter(item => item.meta !== undefined).map(item => item.meta);
    }

    const pageLoadResponseTokens = renderedPropositions
      .map(proposition => collectMetaData(proposition.items))
      .filter(e => e.length > 0)
      .flatMap(e => e);
  });
  
```

Quando è abilitato il rendering automatico, l’array delle proposte contiene:

#### Al caricamento della pagina:

* Compositore basato su moduli `propositions` con flag `renderAttempted` impostato su `false`
* Proposte basate sul Compositore esperienza visivo con flag `renderAttempted` impostato su `true`
* Proposte basate sul Compositore esperienza visivo per una visualizzazione applicazioni a pagina singola con il flag `renderAttempted` impostato su `true`

#### Alla visualizzazione - modifica (per le viste memorizzate nella cache):

* Proposte basate sul Compositore esperienza visivo per una visualizzazione applicazioni a pagina singola con il flag `renderAttempted` impostato su `true`

Quando il rendering automatico è disattivato, l’array delle proposte contiene:

#### Al caricamento della pagina:

* [!DNL Form-based Composer] basato su `propositions` con flag `renderAttempted` impostato su `false`
* Proposizioni basate su [!DNL Visual Experience Composer] con flag `renderAttempted` impostato su `false`
* Proposte basate su [!DNL Visual Experience Composer] per la visualizzazione di un&#39;applicazione a pagina singola con il flag `renderAttempted` impostato su `false`

#### Alla visualizzazione - modifica (per le viste memorizzate nella cache):

* Proposte basate sul Compositore esperienza visivo per la visualizzazione Applicazione a pagina singola con il flag `renderAttempted` impostato su `false`

### Aggiornamento di profilo singolo

[!DNL Web SDK] consente di aggiornare il profilo al profilo [!DNL Target] e a [!DNL Web SDK] come evento esperienza.

Per aggiornare un profilo [!DNL Target], assicurarsi che i dati del profilo vengano passati con:

* Sotto `"data {"`
* Sotto `"__adobe.target"`
* Prefisso `"profile."`

| Chiave | Tipo | Descrizione |
| --- | --- | --- |
| `renderDecisions` | Booleano | Indica al componente di personalizzazione se deve interpretare le azioni DOM |
| `decisionScopes` | Array `<String>` | Un elenco di ambiti per cui recuperare le decisioni |
| `xdm` | Oggetto | Dati formattati in XDM che vengono inseriti in Web SDK come evento esperienza |
| `data` | Oggetto | Coppie chiave/valore arbitrarie inviate a [!DNL Target] soluzioni nella classe di destinazione. |

<!--Typical [!DNL Web SDK] code using this command looks like the following:-->

**Ritarda il salvataggio dei parametri di entità o profilo fino a quando il contenuto non viene visualizzato all&#39;utente finale**

Per ritardare la registrazione degli attributi nel profilo fino alla visualizzazione del contenuto, impostare `data.adobe.target._save=false` nella richiesta.

Ad esempio, il sito web contiene tre ambiti decisionali corrispondenti a tre collegamenti per categorie sul sito web (uomini, donne e bambini) e desideri tenere traccia della categoria che l’utente ha eventualmente visitato. Invia queste richieste con il flag `__save` impostato su `false` per evitare di rendere persistente la categoria al momento della richiesta del contenuto. Dopo aver visualizzato il contenuto, invia il payload corretto (inclusi `eventToken` e `stateToken`) per gli attributi corrispondenti da registrare.

L’esempio seguente invia un messaggio in stile trackEvent, esegue script di profilo, salva gli attributi e registra immediatamente l’evento.

```js
alloy("sendEvent", {
    "renderDecisions": true,
    "xdm": { /* Experience Event XDM data */ },
    "data": {
        "__adobe": {
            "target": {
                " __save": true|false,
                //defaults to true if omitted
                "profile.gender": "female",
                "profile.age": 30,
                "entity.name": "T-shirt",
                "entity.id": "1234"
            }
        }
    }
})
```

>[!NOTE]
>
>Se la direttiva `__save` viene omessa, il salvataggio degli attributi di profilo ed entità avviene immediatamente. La direttiva `__save` è rilevante solo per gli attributi di profilo e i dettagli di entità.

## Richiedi consigli

Nella tabella seguente sono elencati gli attributi [!DNL Recommendations] e se ciascuno di essi è supportato tramite [!DNL Web SDK]:

| Categoria | Attributo | Stato del supporto |
| --- | --- | --- |
| Recommendations - Attributi di entità predefiniti | entity.id | Supportato |
|  | entity.name | Supportato |
|  | entity.categoryId | Supportato |
|  | entity.pageUrl | Supportato |
|  | entity.thumbnailUrl | Supportato |
|  | entity.message | Supportato |
|  | entity.value | Supportato |
|  | entity.inventory | Supportato |
|  | entity.brand | Supportato |
|  | entity.margin | Supportato |
|  | entity.event.detailsOnly | Supportato |
| Recommendations - Attributi di entità personalizzati | entity.yourCustomAttributeName | Supportato |
| Consigli - Parametri mbox/pagina riservati | excludedIds | Supportato |
|  | cartIds | Supportato |
|  | productPurchasedId | Supportato |
| Pagina o categoria di elementi per affinità tra categorie | user.categoryId | Supportato |

**Come inviare gli attributi Recommendations a [!DNL Target]:**

```js
alloy("sendEvent", {
  "renderDecisions": true,
  "data": {
    "__adobe": {
      "target": {
        "entity.id": "123",
        "entity.genre": "Drama"
      }
    }
  }
});
```

## Visualizzare le metriche di conversione mbox {#display-mbox-conversion-metrics}

L&#39;esempio seguente mostra come tenere traccia delle conversioni di mbox di visualizzazione e inviare i parametri di profilo a [!DNL Target], senza richiedere l&#39;idoneità per alcun contenuto o attività.

```js
alloy("sendEvent", {
    "xdm": {
        "_experience": {
            "decisioning": {
                "propositions": [{
                    "scope": "conversion-step-1" //example scope name
                }],
                "propositionEventType": {
                    "display": 1
                }
            }
        },
        "eventType": "decisioning.propositionDisplay"
    }
});
```


| Proprietà | Descrizione |
|---------|----------|
| `xdm._experience.decisioning.propositions[x].scope` | L’ambito a cui associare la metrica di successo (che la attribuirà a un’attività specifica sul lato Target). |
| `xdm._experience.decisioning.propositions[x].eventType` | Stringa che descrive il tipo di evento previsto. Imposta su `"decisioning.propositionDisplay"` per questo caso d&#39;uso. |

## Eseguire il debug

mboxTrace e mboxDebug sono stati dichiarati obsoleti. Utilizza invece un metodo di [debug di Web SDK](https://experienceleague.adobe.com/en/docs/experience-platform/web-sdk/use-cases/debugging).

## Terminologia 

**Proposte**: in [!DNL Target], le proposte sono correlate all&#39;esperienza selezionata da un&#39;attività.

**Schema**: lo schema di una decisione è il tipo di offerta in [!DNL Target].

**Ambito**: ambito della decisione. In [!DNL Target], l&#39;ambito è la mbox. Mbox globale: ambito `__view__`.

**XDM**: XDM viene serializzato in notazione con punto e quindi inserito in [!DNL Target] come parametri mbox.
