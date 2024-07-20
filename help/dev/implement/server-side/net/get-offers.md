---
title: Utilizzare getOffers() in [!DNL Adobe Target] quando si utilizza .NET SDK
description: Scopri come utilizzare getOffers() per eseguire una decisione e recuperare un'esperienza da [!DNL Adobe Target].
feature: APIs/SDKs
exl-id: 4d1d1cbd-c7e5-4146-9fea-08e01923874d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 13%

---

# Ottieni offerte (.NET)

## Descrizione

`GetOffers()` viene utilizzato per eseguire una decisione e recuperare un&#39;esperienza da [!DNL Adobe Target].

## Metodo

Firma del metodo `TargetClient.GetOffers`.

## \.NET

```dotnet {line-numbers="true"}
TargetDeliveryResponse TargetClient.GetOffers(TargetDeliveryRequest request)
```

`TargetDeliveryRequest` è stato creato utilizzando `TargetDeliveryRequest.Builder`.

## \.NET

```dotnet {line-numbers="true"}
TargetDeliveryRequest.Builder TargetDeliveryRequest.Builder()
```

## Parametri

L&#39;oggetto `TargetDeliveryRequest.Builder` ha la seguente struttura:

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
| mcId | Stringa | No | Utilizzato per unire e condividere dati tra diverse soluzioni Adobe (ECID). Recuperato da targetCookies. Generato automaticamente se non specificato. |
| trackingServer | Stringa | No | Il server Adobe Analytics affinché [!DNL Adobe Target] e [!DNL Adobe Analytics] uniscano correttamente i dati. |
| trackingServerSecure | Stringa | No | [!UICONTROL Adobe Analytics Secure Server] affinché [!DNL Adobe Target] e [!DNL Adobe Analytics] uniscano correttamente i dati. |
| decisioningMethod | DecisioningMethod | No | Può essere utilizzato per impostare in modo esplicito il metodo di decisione ON_DEVICE o HYBRID per le decisioni sul dispositivo |

I valori di ciascun campo devono essere conformi alla specifica della richiesta [API di consegna Target](/help/dev/implement/delivery-api/overview.md).

## Risposta

`TargetDeliveryResponse` restituito da `TargetClient.GetOffers()` ha la seguente struttura:

| Nome | Tipo | Descrizione |
| --- | --- | --- |
| Richiesta | TargetDeliveryRequest&#x200B; | [Richiesta API di consegna Target](/help/dev/implement/delivery-api/overview.md) |
| Risposta | DeliveryResponse&#x200B; | [Risposta API di consegna Target](/help/dev/implement/delivery-api/overview.md)* |
| Stato | HttpStatusCode | Codice di stato HTTP della risposta |
| Messaggio | stringa | Messaggio di stato di risposta o messaggio di errore |
| Posizioni | Posizioni | Nomi di posizione [!DNL Target], inclusi il nome della mbox globale e mbox/visualizzazioni per cui è disponibile solo il decisioning remoto |
| GetCookies | Dizionario | Restituisce un dizionario di metadati di sessione per questo utente. Questo deve essere passato nella prossima richiesta [!DNL Target] per questo utente. |
| VisitorState | Icona ID | Stato del visitatore da impostare sul lato client per l&#39;inizializzazione della libreria JavaScript dell&#39;API del visitatore |

L&#39;oggetto `TargetCookie` utilizzato per il salvataggio dei dati per la sessione utente ha la seguente struttura:

| Nome | Tipo | Descrizione |
| --- | --- | --- |
| Nome | stringa | Nome cookie |
| Valore | stringa | Valore cookie |
| MaxAge | int | L&#39;opzione `MaxAge` è utile per impostare le scadenze relative al tempo corrente in secondi |

Non devi preoccuparti di scadere i cookie. [!DNL Target] gestisce `MaxAge` nell&#39;SDK.

## Esempio

## \.NET

```dotnet {line-numbers="true"}
var targetClientConfig = new TargetClientConfig.Builder("acmeClient", "ABCDEF012345677890ABCDEF0@AdobeOrg")
    .Build();

var targetClient = TargetClient.Create(targetClientConfig);

var mboxRequests = new List<MboxRequest> { new (index: 1, name: "a1-serverside-ab") };

var targetDeliveryRequest = new TargetDeliveryRequest.Builder()
    .SetExecute(new ExecuteRequest(mboxes: mboxRequests))
    .Build();

var targetResponse = targetClient.GetOffers(targetDeliveryRequest);
```
