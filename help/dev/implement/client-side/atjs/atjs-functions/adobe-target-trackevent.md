---
keywords: adobe.target.trackEvent, trackEvent, trackevent, track event, at.js, funzioni, funzione, preventDefault, preventdefault, prevent default, adobe.target.trackEvent
description: Utilizza la funzione [!UICONTROL adobe.target.trackEvent()] per la libreria JavaScript di  [!DNL Adobe Target] at.js per attivare una richiesta per segnalare le azioni dell'utente, ad esempio clic e conversioni sul sito.
title: Come si utilizza la funzione [!UICONTROL adobe.target.trackEvent()]?
feature: at.js
exl-id: 9a55e4f1-d7f9-47c1-867c-2ce06fb26f9f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 62%

---

# [!UICONTROL adobe.target.trackEvent(options)]

Questa funzione genera una richiesta per segnalare le azioni dell&#39;utente, ad esempio clic e conversioni. Non recapita le attività nella risposta.

Queste chiamate mbox registrazione eventi possono essere utilizzate per definire le metriche nelle attività. Per ulteriori informazioni, consulta [Metriche di successo](https://experienceleague.adobe.com/docs/target/using/activities/success-metrics/success-metrics.html) e [Tracciare le conversioni](../how-to-deployatjs/implement-target-without-a-tag-manager.md#track-conversions).

Di seguito sono riportati i dettagli API:

| Chiave | Tipo | Obbligatorio | Descrizione |
|--- |--- |--- |--- |
| mbox | Stringa | Sì | Nome Mbox<P>**Nota**: se viene attivata una chiamata [!UICONTROL trackEvent()] con un nome mbox già attivato sulla pagina, l&#39;identificatore SDID di [!UICONTROL trackEvent()] viene reimpostato e sarà diverso dalle chiamate di [!DNL Target] sulla pagina. Tuttavia, l&#39;attivazione di una chiamata [!UICONTROL trackEvent()] con un nome mbox diverso mantiene l&#39;identificatore SDID della chiamata [!UICONTROL trackEvent()] coerente con le chiamate [!UICONTROL Page Load Request]/[!UICONTROL triggerView()] sulla pagina. |
| selector | Stringa | No | Selettori CSS utilizzati per trovare gli elementi HTML. I listener di eventi verranno allegati agli elementi trovati. |
| type | Stringa | No | Rappresenta un tipo di evento registrato. Può trattarsi sia di eventi HTML noti come: click, mouseown, ecc, così come di eventi HTML personalizzati. |
| preventDefault | Booleano | No | Indica se utilizzare `[!UICONTROL event.preventDefault()]` nella chiamata di ritorno del listener di eventi. Predefinito su false.<P>**Nota**: sono supportati solo `[!UICONTROL form[submit]]` e `a[click]`. Altri scenari non sono supportati a causa della complessità e delle enormi quantità di scenari da supportare. |
| params | Oggetto | No | Parametri mbox. Un oggetto di coppie chiave-valore che presenta la struttura seguente:<P>`{ "param1": "value1", "param2": "value2"}` |
| timeout | Numero | No | Timeout in millisecondi.<P>Se non viene specificato, viene utilizzato il valore predefinito:<P>`...timeoutInSeconds: 0.15...}` |
| success | Funzione | No | Funzione di callback utilizzata per indicare che quell’evento è stato segnalato. |
| error | Funzione | No | Funzione di callback utilizzata per indicare che non è stato possibile segnalare quell’evento. |

## Esempio

```javascript {line-numbers="true"}
<a href="https://asite.com">click me!</a> 
```

più codice JavaScript per assegnare `trackEvent`:

```javascript {line-numbers="true"}
<script> 
$('a').click(function(event){ 
  adobe.target.trackEvent({'mbox':'homePageHero'}) 
}); 
</script> 
```

Oppure:

```javascript {line-numbers="true"}
adobe.target.trackEvent({ 
    "mbox": "clicked-cta", 
    "params": { 
        "param1": "value1" 
    } 
});
```

>[!WARNING]
>
>Se i campi obbligatori non sono impostati, non viene eseguita alcuna richiesta e viene generato un errore.
