---
title: Utilizzare getOffers() in [!DNL Adobe Target] quando si utilizza l’SDK Java
description: Scopri come utilizzare getOffers() per eseguire una decisione e recuperare un’esperienza da [!DNL Adobe Target].
feature: APIs/SDKs
exl-id: 9d7bf956-9d6a-4b4f-a401-2e6814f17f3d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '654'
ht-degree: 13%

---

# Ottieni offerte (Java)

## Descrizione

`getOffers()` viene utilizzato per eseguire una decisione e recuperare un’esperienza da [!DNL Adobe Target].

## Metodo

### getOffers

Il `TargetClient.getOffers` la firma del metodo viene visualizzata come segue.

**Richiesta**

```javascript {line-numbers="true"}
TargetDeliveryResponse TargetClient.getOffers(TargetDeliveryRequest request)
```

TargetDeliveryRequest viene creato tramite `TargetDeliveryRequest.builder`.

**Risposta**

```javascript {line-numbers="true"}
TargetDeliveryRequestBuilder TargetDeliveryRequest.builder()
```

## Parametri

Il `[!UICONTROL TargetDeliveryRequestBuilder]` L&#39;oggetto ha la seguente struttura:

| Nome | Tipo | Obbligatorio | Descrizione |
| --- | --- | --- | --- |
| Contesto | Contesto | Sì | Specifica il contesto per la richiesta |
| sessionId |  | Stringa | No | Utilizzato per il collegamento di più [!DNL Target] richieste |
| thirdPartyId | Stringa | No | Identificatore della tua azienda per l&#39;utente che puoi inviare con ogni chiamata |
| cookie | Elenco | No | Elenco dei cookie restituiti in precedenza [!DNL Target] richiesta dello stesso utente. |
| customerIds | Mappa | No | ID cliente in formato compatibile con VisitorId |
| esegui | ExecuteRequest | No | Richiesta PageLoad o mboxes da eseguire. Verrà immediatamente valutato sul lato server |
| preacquisizione | PrefetchRequest | No | Visualizzazioni, PageLoad o mbox richiedono la preacquisizione. Restituisce con il token di notifica da restituire al momento della conversione. |
| Notifiche | Elenco | No | Utilizzato per inviare notifiche relative al contenuto prerecuperato visualizzato |
| requestId | Stringa | No | L’ID della richiesta che verrà restituito nella risposta. Generato automaticamente se non presente. |
| impressionId | Stringa | No | Se presente, la seconda e le richieste successive con lo stesso ID non incrementeranno le impression ad attività/metriche. Generato automaticamente se non presente. |
| environmentId | Lungo | No | ID ambiente client valido. Se non viene specificato, l’host verrà determinato in base all’host fornito. |
| proprietà | Proprietà | No | Specifica la at_property tramite il campo token. Può essere utilizzato per controllare l’ambito della consegna. |
| traccia | Traccia | No | Abilita la traccia per l’API di consegna. |
| qaMode | QAMode | No | Utilizza questo oggetto per abilitare la modalità di controllo qualità nella richiesta. |
| locationHint | Stringa | No | [!DNL Target] hint di posizione cluster edge. Utilizzato per eseguire il targeting di un determinato cluster Edge per questa richiesta. |
| visitatore | Visitatore | No | Utilizzato per fornire un oggetto API visitatore personalizzato. |
| id | VisitorId | No | Oggetto contenente gli identificatori del visitatore. Esempio: tntId, thirdParyId, mcId, customerIds. |
| experienceCloud | Experience Cloud | No | Specifica le integrazioni con Audienci Manager e Analytics. Compilato automaticamente utilizzando i cookie, se non fornito. |
| tntId | Stringa | No | Identificatore primario in [!DNL Target] per un utente. Recuperato da targetCookies. Generato automaticamente se non specificato. |
| mcId | Stringa | No | Utilizzato per unire e condividere dati tra diversi [!DNL Adobe] solutions(ECID). Recuperato da targetCookies. Generato automaticamente se non specificato. |
| trackingServer | Stringa | No | Il server Adobe Analytics per [!DNL Adobe Target] e [!DNL Adobe Analytics] per unire correttamente i dati. |
| trackingServerSecure | Stringa | No | Il [!UICONTROL Adobe Analytics Secure Server] affinché [!DNL Adobe Target] e [!DNL Adobe Analytics] per unire correttamente i dati. |
| decisioningMethod | DecisioningMethod | No | Può essere utilizzato per impostare in modo esplicito il metodo di decisione ON_DEVICE o HYBRID per le decisioni sul dispositivo |

I valori di ciascun campo devono essere conformi a *[!UICONTROL API di consegna della visualizzazione di Target]* specifica della richiesta. Per ulteriori informazioni su *[!UICONTROL API di consegna della visualizzazione di Target]*, vedi [http://developers.adobetarget.com/api/#view-delivery-overview](http://developers.adobetarget.com/api/#view-delivery-overview)


## Risposta

Il `TargetDeliveryResponse` restituito da `TargetClient.getOffers(`) ha la seguente struttura:

| Nome | Tipo | Descrizione |
| --- | --- | --- |
| richiesta | TargetDeliveryRequest&#x200B; | *[!DNL Target]* richiesta |
| risposta | DeliveryResponse | *[!DNL Target]* risposta |
| cookie | Elenco | Elenco dei metadati di sessione per questo utente. Deve essere passato alla successiva richiesta di destinazione per questo utente. |
| visitorState | Mappa | Stato del visitatore da impostare sul lato client per l’utilizzo da parte dell’API visitatore |
| responseStatus | ResponseStatus | Oggetto che rappresenta lo stato della risposta |

Il `ResponseStatus` nella risposta contiene i campi seguenti:

| Nome | Tipo | Descrizione |
| --- | --- | --- |
| status | int | Stato HTTP restituito da [!DNL Target] |
| message | Stringa | Messaggio di stato se lo stato HTTP non è 200 |
| remoteMboxes | Elenco di stringhe | Utilizzato per il decisioning sul dispositivo. Contiene un elenco di mbox con attività remote che non possono essere decise completamente su dispositivo. |
| remoteViews | Elenco di stringhe | Utilizzato per il decisioning sul dispositivo. Contiene un elenco di visualizzazioni con attività remote che non possono essere decise interamente su dispositivo. |

Il `TargetCookie` l’oggetto utilizzato per salvare i dati per la sessione utente ha la seguente struttura:

| Nome | Tipo | Descrizione |
| --- | --- | --- |
| name | Stringa | Nome cookie |
| value | Stringa | Valore cookie, il valore verrà convertito in stringa |
| maxAge | Numero | L’opzione maxAge è utile per impostare le scadenze relative al tempo corrente in secondi |

Non devi preoccuparti di scadere i cookie. Target gestisce maxAge all’interno dell’SDK.

## Esempio

**Richiesta**

```javascript {line-numbers="true"}
ClientConfig clientConfig = ClientConfig.builder()
        .client("acmeclient")
        .organizationId("1234567890@AdobeOrg")
        .build();

TargetClient targetJavaClient = TargetClient.create(clientConfig);

List<MboxRequest> mboxRequests = new ArrayList<>();
mboxRequests.add((MboxRequest) new MboxRequest().name("a1-serverside-ab").index(1));

TargetDeliveryRequest targetDeliveryRequest = TargetDeliveryRequest.builder()
        .context(new Context().channel(ChannelType.WEB))
        .execute(new ExecuteRequest().setMboxes(mboxRequests))
        .build();
```

**Risposta**

```javascript {line-numbers="true"}
TargetDeliveryResponse targetResponse = targetJavaClient.getOffers(targetDeliveryRequest);
```
