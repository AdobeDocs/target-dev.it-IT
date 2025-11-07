---
keywords: gdpr, ue, unione europea, privacy, faq, domande frequenti, california consumer privacy act, ccpa, privacy, protezione dei dati, opt-out, opt out, government, regulation, gdpr5, gdpr6, gdpr7, gdpr8, gdpr9, eu0, eu1, eu2, eu3, eu4, eu5
description: Scopri Target e il Regolamento generale sulla protezione dei dati (RGPD) dell’Unione Europea, il California Consumer Privacy Act (CCPA) e altri requisiti sulla privacy.
title: In che modo Target gestisce le normative sulla privacy e la protezione dei dati?
feature: Privacy & Security
exl-id: 40bac3c5-8e6f-4a90-ac0c-eddce1dbe6c0
source-git-commit: 67cc93cf697f8d5bca6fedb3ae974e4012347a0b
workflow-type: tm+mt
source-wordcount: '2329'
ht-degree: 62%

---

# Normative sulla privacy e la protezione dei dati

Informazioni sul Regolamento generale sulla protezione dei dati (GDPR) dell’Unione Europea, sul California Consumer Privacy Act (CCPA) e su altri requisiti internazionali relativi alla privacy. Scopri come queste normative influiscono sulla tua organizzazione e su Adobe Target.

## Panoramica sulla privacy e sul Regolamento generale sulla protezione dei dati (GDPR)

Il 25 maggio 2018 è entrato in vigore il regolamento GDPR dell’Unione Europea. Per ulteriori informazioni su cosa implica per te, consulta la pagina relativa al [GDPR e la tua azienda](https://business.adobe.com/it/privacy/general-data-protection-regulation.html).

Quando Adobe fornisce software e servizi alle aziende, agisce come Incaricato del trattamento dei dati, per ognuno dei dati personali trattati e memorizzati nell&#39;ambito della fornitura di tali servizi. In qualità di Incaricato del trattamento dei dati, Adobe tratta i dati personali in conformità alle autorizzazioni e alle istruzioni dell&#39;azienda (ad esempio, come stabilito nell&#39;accordo con Adobe).

In qualità di Titolare del trattamento dei dati, l&#39;utente determina i dati personali che Adobe tratta e memorizza per suo conto. Se si utilizzano le soluzioni Adobe Experience Cloud, Adobe potrebbe ospitare i dati personali per conto dell&#39;utente, a seconda delle soluzioni utilizzate e delle informazioni che si sceglie di inviare all&#39;account Adobe Experience Cloud. Per un elenco dettagliato di esempi, consulta [Privacy di Adobe Experience Cloud](https://www.adobe.com/it/privacy/experience-cloud.html#collect).

Adobe Experience Cloud fornisce API a supporto del regolamento RGPD che consentono ai titolari del trattamento di completare le attività seguenti:

* Accedere alle informazioni relative all&#39;interessato memorizzate in Target
* Cancellare le informazioni relative all&#39;interessato memorizzate in Target

Per ulteriori informazioni, consulta:

* [Panoramica di Adobe Privacy Service](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=it)
* [Guida API di Privacy Service](https://experienceleague.adobe.com/docs/experience-platform/privacy/api/overview.html?lang=it)
* [Panoramica dell&#39;interfaccia utente di Privacy Service](https://experienceleague.adobe.com/docs/experience-platform/privacy/ui/overview.html?lang=it)

## California Consumer Privacy Act (CCPA) - Panoramica

Il California Consumer Privacy Act (CCPA) offre ai consumatori della California nuovi diritti relativi alle informazioni personali e impone responsabilità di protezione dei dati a determinate entità che operano in California. Il CCPA è entrato in vigore il 1° gennaio 2020.

Ad alto livello, la legge tutela i californiani con diversi diritti chiave, compreso il diritto a:

* Richiedere informazioni (accesso ai dati)
* Non acconsentire alla vendita di informazioni personali (diritto con definizione ampia sulla possibilità di non acconsentire alla condivisione di informazioni con terze parti)
* Richiedere la cancellazione dei dati personali
* Ricevere informazioni sui dati personali che vengono divulgati o venduti

Se ti sei già preparato alla legge europea sulla privacy (RGPD), alcuni di questi diritti ti potrebbero essere familiari e molto del lavoro già svolto può essere riproposto.

>[!NOTE]
>
>L’accesso e l’eliminazione dei dati per quanto attiene al CCPA seguono lo stesso processo del GDPR.

## consenso Adobe Target e Adobe Experience Platform

Target fornisce supporto per la funzionalità opt-in tramite i tag in Adobe Experience Platform per supportare la strategia di gestione dei consensi. La funzionalità opt-in consente ai clienti di controllare come e quando viene attivato il tag di Target. È inoltre disponibile un’opzione tramite Adobe Experience Platform per pre-approvare il tag di Target. Per abilitare la capacità di utilizzare la funzione di opt-in nella libreria at.js di Target, utilizza `targetGlobalSettings` e aggiungi l&#39;impostazione `optinEnabled=true`. In Adobe Experience Platform, seleziona &quot;abilita&quot; dall’elenco a discesa Opt-in RGPD nella visualizzazione di installazione dell’estensione. Per ulteriori dettagli, vedere [Implementare Target utilizzando Adobe Experience Platform](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md).

Il seguente snippet di codice mostra come abilitare l’impostazione `optinEnabled=true`:

```
window.targetGlobalSettings = {
  optinEnabled: true
};
```

>[!NOTE]
>
>La funzionalità opt-in è supportata in at.js 1.7.0 e in at.js 2.1.0 o versione successiva. La funzionalità opt-in non è supportata in at.js 2.0.0 e 2.0.1.

L’utilizzo di Adobe Experience Platform per gestire l’opt-in rappresenta l’approccio consigliato. Esiste un ulteriore controllo granulare in Adobe Experience Platform per nascondere elementi selezionati della pagina prima dell’attivazione di Target che è utile da utilizzare come parte della strategia di consenso.

Esistono tre scenari da considerare quando si utilizza l’opt-in:

1. **Il tag di Target è stato pre-approvato tramite Adobe Experience Platform (o l&#39;interessato ha già approvato Target):** Il tag di Target non viene trattenuto per il consenso e funziona come previsto.
1. **Il tag di Target NON è pre-approvato e `bodyHidingEnabled` è FALSE:** il tag di Target viene attivato solo dopo il consenso del cliente. Prima della raccolta del consenso, è disponibile solo il contenuto predefinito. Dopo aver ricevuto il consenso, Target viene chiamato e il contenuto personalizzato è disponibile per l&#39;interessato (visitatore). Poiché solo i contenuti predefiniti sono disponibili prima del consenso, è importante usare una strategia appropriata, come ad esempio una pagina iniziale che copre qualsiasi parte della pagina o del contenuto che potrebbe essere personalizzato. Questo processo garantisce che l’esperienza rimanga coerente per l’interessato (visitatore).
1. **Il tag di Target NON è pre-approvato e `bodyHidingEnabled` è TRUE:** il tag di Target viene attivato solo dopo il consenso del cliente. Prima della raccolta del consenso, è disponibile solo il contenuto predefinito. Tuttavia, poiché `bodyHidingEnabled` è impostato su true, `bodyHiddenStyle` determina quale contenuto della pagina è nascosto fino a quando il tag di Target non viene attivato (o l&#39;interessato rifiuta di effettuare l’opt-in, nel qual caso viene visualizzato il contenuto predefinito). Per impostazione predefinita, `bodyHiddenStyle` è impostato su `body { opacity:0;}`, che nasconde il tag corpo HTML. Di seguito si trova la configurazione di pagina consigliata da Adobe affinché l’intero corpo della pagina, a eccezione della finestra di dialogo di gestione del consenso, sia nascosto inserendo il contenuto della pagina in un contenitore e la finestra di dialogo di gestione del consenso in un contenitore separato. Questa configurazione imposta Target affinché nasconda solo il contenitore del contenuto della pagina. Consulta la pagina [Privacy Service overview](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=it&) (Panoramica di Privacy Service).

   La configurazione consigliata della pagina per lo scenario 3 è:

   ```
   <html> 
   <head> 
   //visitor, at.js 
   </head> 
   
   <body> 
   <div id = "consentManagerDialog"> 
   
   //consent manager html dialog goes here 
   </div> 
   
   <div id="pageContent"> 
   // page content goes here 
   </div> 
   
   </body> 
   </html> 
   ```

   Supponendo `bodyHiddenStyle` di:

   ```
   #pageContent { opacity:0;}
   ```

## Domande frequenti sulla privacy e la protezione dei dati

Domande frequenti sul Regolamento generale sulla protezione dei dati (GDPR) dell’Unione Europea, sul California Consumer Privacy Act (CCPA) e su altri requisiti internazionali relativi alla privacy che interessano Target.

### Qual è la politica di Adobe in merito a queste normative?

Adobe adempie o sta adempiendo ai propri obblighi in qualità di Incaricato del trattamento dei dati. Adobe ha una solida base di sicurezza certificata e controlli sulla privacy da progettazione e ha introdotto miglioramenti nei prodotti con anticipo sulla scadenza di maggio 2018. I clienti aziendali hanno la responsabilità di implementare questi miglioramenti e di aggiornare le politiche e le procedure necessarie.

### La mia azienda, Titolare del trattamento dei dati, deve inviare una richiesta relativa al RGPD o CCPA per ciascuna soluzione Adobe Experience Cloud utilizzata?

No, Adobe fornisce una modalità unica per consentire ai titolari del trattamento dei dati di soddisfare i requisiti RGPD e CCPA. Non è necessario che i Titolari del trattamento dei dati inviino la richiesta per ciascuna soluzione.

Tutte le richieste relative ai requisiti RGPD per le soluzioni Experience Cloud, incluso Target, sono effettuate tramite un’API Adobe centralizzata, attualmente denominata API RGPD. L’API completa quindi la richiesta nella suite di soluzioni Experience Cloud per il titolare del trattamento dei dati.

### Quali informazioni Adobe consente ai clienti di eliminare, in risposta a una richiesta dell’interessato/utente?

Le informazioni relative a un singolo visitatore all&#39;interno di Target sono contenute nel Profilo del visitatore di Target. Target consente ai clienti di eliminare tutti i dati associati a un ID nel loro profilo visitatore. Per esempi dei dati del profilo memorizzati da Target, consulta [Profilo visitatore](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html?lang=it).

I dati aggregati o anonimi (ad esempio, i dati di segnalazione) che non identificano una persona particolare oppure i dati che non sono correlati a una persona specifica (ad esempio, i dati sul contenuto) esulano dall’ambito di una richiesta di cancellazione da parte dell’utente.

Anche i profili dei visitatori di Target che sono stati inattivi per 90 giorni vengono cancellati per impostazione predefinita, senza che sia necessaria alcuna azione.

### Quali ID sono supportati per consentire ai clienti di completare una richiesta RGPD o CCPA di accesso e cancellazione per Target?

Target supporta i seguenti tipi di ID per individuare un profilo cliente:

| ID utente | Tipo ID dello spazio dei nomi | ID dello spazio dei nomi | Definizione |
|--- |--- |--- |--- |
| Experience Cloud ID (ECID) | Standard | 4 | Adobe Experience Cloud ID, precedentemente noto come ID visitatore o Experience Cloud ID. È possibile utilizzare l’API JavaScript per individuare questo ID (consulta i dettagli di seguito). |
| ID TnT/ID cookie (TNTID) | Standard | 9 | Identificatore di Target impostato come cookie nel browser del visitatore. È possibile utilizzare l’API JavaScript per individuare questo ID (consulta i dettagli di seguito). |
| ID di terze parti/ID CRM (THIRDPARTYID) | Specifico di Target | N/D | Se si fornisce a Target il proprio CRM o altre informazioni di identificazione univoche per i propri clienti. |

>[!NOTE]
>
>Sebbene Target supporti sia cookie di prima parte sia di terze parti di diversi domini, i cookie di prima parte di Target sono consigliati solo per il RGPD e CCPA.

### Come avviene la gestione del consenso da parte di Target?

I regolamenti GDPR e CCPA non cambiano i requisiti in merito a “quando” devi ottenere il consenso, ma piuttosto “come” ottenerlo. La strategia di consenso di ciascun cliente dipende dalle sue modalità di raccolta e utilizzo dei dati e dalla sua politica sulla privacy. La gestione del consenso non è supportata e non deve essere ottenuta tramite Target per RGPD e CCPA.

Attualmente, Adobe non offre una soluzione per la gestione dei consensi, ma sul mercato sono in fase di sviluppo diversi strumenti per soddisfare alcuni dei nuovi requisiti. Per ulteriori informazioni sugli strumenti generali di tutela della privacy, inclusi i responsabili del consenso, consulta la [relazione del 2017 sui fornitori di tecnologia della privacy](https://iapp.org/media/pdf/resource_center/Tech-Vendor-Directory-1.4.1-electronic.pdf) sul *sito web di IAPP (International Association of Privacy Professionals)*.

Target fornisce supporto per la funzionalità opt-in tramite Adobe Experience Platform per supportare la strategia di gestione dei consensi. La funzionalità opt-in consente ai clienti di controllare come e quando viene attivato il tag di Target. È inoltre disponibile un’opzione tramite Adobe Experience Platform per pre-approvare il tag di Target. L’utilizzo di Adobe Experience Platform per gestire l’opt-in rappresenta l’approccio consigliato. Esiste un ulteriore controllo granulare in Adobe Experience Platform per nascondere alcuni elementi della pagina prima dell’attivazione di Target che potrebbe essere utile da utilizzare come parte della strategia di consenso.

Per ulteriori informazioni su RGPD, CCPA e Adobe Experience Platform, consulta [Libreria JavaScript di Adobe Privacy e RGPD](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=it&). Inoltre, consulta la sezione precedente *Funzionalità di opt-in di Adobe Target e Adobe Experience Platform*.

### `AdobePrivacy.js` invia informazioni all’API GDPR?

AdobePrivacy.js *non* invia queste informazioni all&#39;API. Farlo è compito del cliente. Questa libreria fornisce solo gli ID memorizzati nel browser per un visitatore specifico.

### L’azione `removeIdentities` che cosa elimina?

`removeIdentities` rimuove identità *solo* dal browser, e solo nel caso in cui la soluzione Adobe sia stata implementata.

Ad esempio, Target elimina i cookie che memorizzano gli ID, ma Adobe Audience Manager (AAM) non elimina l’ID demdex memorizzato in un cookie di terze parti.

### Quali informazioni devono essere incluse in una richiesta RGPD o CCPA di Target?

Oltre ai requisiti del Central Privacy Service, un messaggio RGPD o CCPA valido per Target contiene:

```
{ 
    "jobId":"12345AD43E", 
    ... 
    "products":["Target",...], 
    "companyContexts":[ 
        { 
            "namespace":"imsOrgID", 
            "value":"123456789@AdobeOrg" 
        }, 
        ... 
    ], 
    "userContexts":[ 
        { 
            "namespace":"ECID", 
            "namespaceId":4, 
            "type":"standard", 
            "value":"53792210477379708453829363835595041181" 
        } 
        And/OR: 
        { 
            "namespace":"TNTID", 
            "namespaceId":9, 
            "type":"standard", 
            "value":"1234567890" 
        } 
        And/OR: 
        { 
            "namespace":"THIRDPARTYID", 
            "type":"target", 
            "value":"thirdPartyIdName" 
        }, 
        ... 
    ] 
}
```

### Quali tipi di risposte è possibile prevedere da Target mediante l’API GDPR?

| Stato richiesta | Messaggio risposta Target | Scenario |
|--- |--- |--- |
| Elaborazione | Elaborazione | Target ha ricevuto la richiesta GDPR o CCPA e la sta elaborando. |
| Completa | Non applicabile; contesto aziendale non applicabile | L’ID IMS nella richiesta GDPR o CCPA non viene mappato su alcun client di Target.<br />Alcune aziende hanno più ID IMS. Invia l’ID IMS in cui è effettuato il provisioning di Target. |
| Completa | Non applicabile; contesto utente non trovato | L’ID fornito nella richiesta GDPR o CCPA per il visitatore o l’oggetto dati specifico non è presente nell’archivio dei profili di Target.<br />Questo risultato si ottiene anche se si tenta di inviare un tipo di ID spazio dei nomi non supportato da Target (vedere sopra per gli ID supportati). |
| Errore | Messaggio di errore (i dettagli dipendono dal tipo di errore) | Errore durante il recupero o l’eliminazione del profilo dati richiesto.<br />Errore durante il caricamento su Azure della richiesta di accesso. |

### Quale risposta invia Target all&#39;API RGPD per una richiesta di accesso?

Le risposte alle richieste di accesso ai dati contengono un riassunto del profilo di Target per il visitatore in questione. Questo ritorno è inviato all’API RGPD di Experience Cloud, che a sua volta invia una risposta ai Titolari del trattamento dei dati.

Un esempio di risposta API di accesso a Target potrebbe essere il seguente:

```
{ 
    "jobId":"12345AD43E", 
    ... 
    "products":["Target",...], 
    "companyContexts":[ 
        { 
            "namespace":"imsOrgID", 
            "value":"123456789@AdobeOrg" 
        }, 
        ... 
    ], 
    "userContexts":[ 
        { 
            ~"namespace":"ECID", 
            "namespaceId":4, 
            "type":"standard", 
            "value":"53792210477379708453829363835595041181" 
        } 
        And/OR: 
        { 
            ~"namespace":"tntId", 
            "namespaceId":9, 
            "type":"standard", 
            "value":"1234567890" 
        } 
        And/OR: 
        { 
            "namespace":"thirdPartyId", 
            "type":"target", 
            "value":"thirdPartyIdName" 
        }, 
        ... 
    ] 
} 
```

| Campo | Descrizione |
|--- |--- |
| jobId | Indica l’ID del lavoro GDPR o CCPA dall’API GDPR centrale. |
| imsOrgID | Fornisce un identificatore univoco per la tua azienda. |
| namespace | Denominato anche fonte di dati. Consulta “Quali ID sono supportati per aiutare i clienti a completare una richiesta di accesso ed eliminazione GDPR o CCPA per Target?” in questo argomento. |
| type | Tipo di ID per il quale è stato richiesto l’accesso ai dati GDPR o CCPA. Target accetta diversi tipi di ID, alcuni dei quali sono standard e altri specifici di Target. Consulta “Quali ID sono supportati per aiutare i clienti a completare una richiesta di accesso ed eliminazione GDPR o CCPA per Target?” in questo argomento. |
| value | L&#39;ID spazio dei nomi/sorgente dei dati. Consulta “Quali ID sono supportati per aiutare i clienti a completare una richiesta di accesso ed eliminazione GDPR o CCPA per Target?” per i valori accettati. |
| integration code | I codici di integrazione sono nomi semplici per le origini dati e consentono di tracciarle più facilmente rispetto agli ID delle origini dati. |

Quando vengono forniti più valori per identificare i profili, ogni identificatore valido ha un file di profilo. Uno o più file di profilo sono inviati al Azure Blob centrale per RGPD attraverso l’API centrale RGPD, sotto forma di risposta JSON per il profilo di Target.

Un esempio di JSON profilo di Target potrebbe avere l&#39;aspetto seguente:

```
{"profileAttributes": 
 
"Sample_Parameter":{"value":"Gold Loyalty Status","modifiedAt":"2018-04-11T21:44:14.000-04:00"}, 
 
"user.ReturnTimeOfDay":{"value":"44.0","modifiedAt":"2018-04-11T21:44:14.000-04:00"}, 
 
"firstSessionStart":{"value":"1523497450602","modifiedAt":"2018-04-11T21:44:10.000-04:00"}, 
 
"user.sessionCountScript":{"value":"1","modifiedAt":"2018-04-11T21:44:14.000-04:00"} 
   } 
} 
```

La tabella seguente contiene la descrizione dei campi JSON del profilo illustrativo:

| Campo | Descrizione |
|--- |--- |
| Sample_Parameter | Molte informazioni nel profilo di Target sono caricate o fornite direttamente dal Titolare del trattamento dei dati. In questo esempio, è stato caricato un parametro nel profilo di Target, utilizzando l&#39;API di aggiornamento del profilo. Per ulteriori informazioni, consulta [Metodi per ottenere dati in Target](/help/dev/before-implement/methods-to-get-data-into-target/methods-to-get-data-into-target.md). |
| user.ReturnTimeOfDay | Questo campo standard include l’ora del giorno dell’ultima visita di ritorno di un utente. |
| firstSessionStart | Questo campo standard include l’ora del giorno in cui è iniziata la prima sessione dell’utente. |
| user.sessionCountScript | Molte informazioni nel profilo di Target sono caricate o fornite direttamente dal Titolare del trattamento dei dati. In questo esempio, uno script di profilo incrementa il numero di sessioni che il visitatore ha effettuato sul sito del titolare del trattamento dei dati. Per ulteriori informazioni, consulta [Attributi del profilo](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/profile-parameters.html?lang=it). |

>[!NOTE]
>
>Questo esempio di codice è una versione ridotta di un profilo Target JSON, a titolo illustrativo. Molti campi del profilo Target non sono standard. Ciò che viene restituito dipende da quali informazioni si trovano in quel profilo specifico di visitatore.

### Target supporta l’omissione dell’IP?

Se scegli di utilizzarla come parte della strategia di implementazione RGPD o CCPA, Target supporta l’omissione dell’IP. Per ulteriori informazioni, consulta [Privacy](privacy.md#replacement-of-last-octet-of-ip-addresses).

### Devo fare qualcosa per evitare che i miei dati vengano condivisi o venduti a terzi?

Target non consente ai clienti di condividere o vendere dati direttamente da Target a terze parti, quindi non vi è alcuna opzione di rinuncia alla vendita per Target.
