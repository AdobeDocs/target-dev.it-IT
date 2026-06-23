---
keywords: adobe.target.trackEvent, trackEvent, trackevent, track event, at.js, funzioni, funzione, preventDefault, preventdefault, prevent default, adobe.target.trackEvent
description: Utilizza la funzione [!UICONTROL adobe.target.trackEvent()] per la libreria JavaScript di  [!DNL Adobe Target] at.js per attivare una richiesta per segnalare le azioni dell'utente, ad esempio clic e conversioni sul sito.
title: Come si utilizza la funzione [!UICONTROL adobe.target.trackEvent()]?
feature: at.js
exl-id: 9a55e4f1-d7f9-47c1-867c-2ce06fb26f9f
TQID: https://experienceleague.adobe.com/Jib9C5FvmsgIF6CA-0UbdMdnMiXxQCkU2-O3Zys3vrY
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2:
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 4d0e7f9f2887db71229061fa64b2633a84c6d054
workflow-type: tm+mt
source-wordcount: 336
ht-degree: 59%

---

# [!UICONTROL adobe.target.trackEvent(options)]

Questa funzione genera una richiesta per segnalare le azioni dell&#39;utente, ad esempio clic e conversioni. Non recapita le attività nella risposta.

Queste chiamate mbox registrazione eventi possono essere utilizzate per definire le metriche nelle attività. Per ulteriori informazioni, consulta [Metriche di successo](https://experienceleague.adobe.com/docs/target/using/activities/success-metrics/success-metrics.html?lang=it) e [Tracciare le conversioni](../how-to-deployatjs/implement-target-without-a-tag-manager.md#track-conversions).

Di seguito sono riportati i dettagli API:

| Chiave | Tipo | Obbligatorio | Descrizione |
|--- |--- |--- |--- |
| mbox | Stringa | Sì | Nome Mbox<P>**Nota**: se una chiamata [!UICONTROL trackEvent()] viene attivata con un nome mbox già attivato sulla pagina, l&#39;identificatore SDID di [!UICONTROL trackEvent()] viene reimpostato e sarà diverso dalle chiamate [!DNL Target] sulla pagina. Tuttavia, l&#39;attivazione di una chiamata [!UICONTROL trackEvent()] con un nome mbox diverso mantiene l&#39;identificatore SDID della chiamata [!UICONTROL trackEvent()] coerente con le chiamate [!UICONTROL Page Load Request]/[!UICONTROL triggerView()] sulla pagina. |
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

