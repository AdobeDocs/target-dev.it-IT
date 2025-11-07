---
title: Utilizzare getOffers() in [!DNL Adobe Target] con Java SDK
description: Scopri come utilizzare getOffers() per eseguire una decisione e recuperare un'esperienza da [!DNL Adobe Target].
feature: APIs/SDKs
exl-id: 9d7bf956-9d6a-4b4f-a401-2e6814f17f3d
source-git-commit: 67cc93cf697f8d5bca6fedb3ae974e4012347a0b
workflow-type: tm+mt
source-wordcount: '635'
ht-degree: 13%

---

# Ottieni offerte (Java)

## Descrizione

`getOffers()` viene utilizzato per eseguire una decisione e recuperare un&#39;esperienza da [!DNL Adobe Target].

## Metodo

### getOffers

La firma del metodo `TargetClient.getOffers` viene visualizzata come segue.

**Richiesta**

```javascript {line-numbers="true"}
TargetDeliveryResponse TargetClient.getOffers(TargetDeliveryRequest request)
```

TargetDeliveryRequest creato utilizzando `TargetDeliveryRequest.builder`.

**Risposta**

```javascript {line-numbers="true"}
TargetDeliveryRequestBuilder TargetDeliveryRequest.builder()
```

## Parametri

L&#39;oggetto `[!UICONTROL TargetDeliveryRequestBuilder]` ha la seguente struttura:

| Nome | Tipo | Obbligatorio | Descrizione |
| --- | --- | --- | --- |
| Contesto | Contesto | Sì | Specifica il contesto per la richiesta |
| sessionId | Stringa | No | Utilizzato per collegare più richieste [!DNL Target] |
| thirdPartyId | Stringa | No | Identificatore della tua azienda per l&#39;utente che puoi inviare con ogni chiamata |
| cookie | Elenco | No | Elenco di cookie restituiti nella precedente richiesta [!DNL Target] dello stesso utente. |
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
| locationHint | Stringa | No | hint posizione cluster edge [!DNL Target]. Utilizzato per eseguire il targeting di un determinato cluster Edge per questa richiesta. |
| visitatore | Visitatore | No | Utilizzato per fornire un oggetto API visitatore personalizzato. |
| ID | VisitorId | No | Oggetto contenente gli identificatori del visitatore. Esempio: tntId, thirdParyId, mcId, customerIds. |
| experienceCloud | Experience Cloud | No | Specifica le integrazioni con Audience Manager e Analytics. Compilato automaticamente utilizzando i cookie, se non fornito. |
| tntId | Stringa | No | Identificatore primario in [!DNL Target] per un utente. Recuperato da targetCookies. Generato automaticamente se non specificato. |
| mcId | Stringa | No | Utilizzato per unire e condividere dati tra diverse [!DNL Adobe] soluzioni (ECID). Recuperato da targetCookies. Generato automaticamente se non specificato. |
| trackingServer | Stringa | No | Il server Adobe Analytics affinché [!DNL Adobe Target] e [!DNL Adobe Analytics] uniscano correttamente i dati. |
| trackingServerSecure | Stringa | No | [!UICONTROL Adobe Analytics Secure Server] affinché [!DNL Adobe Target] e [!DNL Adobe Analytics] uniscano correttamente i dati. |
| decisioningMethod | DecisioningMethod | No | Può essere utilizzato per impostare in modo esplicito il metodo di decisione ON_DEVICE o HYBRID per le decisioni sul dispositivo |

I valori di ciascun campo devono essere conformi alla specifica della richiesta *[!UICONTROL Target View Delivery API]*. Per ulteriori informazioni su *[!UICONTROL Target View Delivery API]*, vedere [http://developers.adobetarget.com/api/#view-delivery-overview](http://developers.adobetarget.com/api/#view-delivery-overview)


## Risposta

`TargetDeliveryResponse` restituito da `TargetClient.getOffers(`) ha la seguente struttura:

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

L&#39;oggetto `TargetCookie` utilizzato per il salvataggio dei dati per la sessione utente ha la seguente struttura:

| Nome | Tipo | Descrizione |
| --- | --- | --- |
| name | Stringa | Nome cookie |
| value | Stringa | Valore cookie, il valore verrà convertito in stringa |
| maxAge | Numero | L’opzione maxAge è utile per impostare le scadenze relative al tempo corrente in secondi |

Non devi preoccuparti di scadere i cookie. Target gestisce maxAge all’interno di SDK.

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
