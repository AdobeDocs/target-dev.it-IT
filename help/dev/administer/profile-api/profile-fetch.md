---
title: Recuperare i profili
description: Scopri come utilizzare le API del profilo di Adobe Target per recuperare i dati dei visitatori da utilizzare in [!DNL Target].
contributors: https://github.com/icaraps
feature: APIs/SDKs
exl-id: b422ae68-49b3-4d60-9ea4-0fa67b6934b0
source-git-commit: b8ccfdcaff6aa17a325727df0a9ffd716e44519b
workflow-type: tm+mt
source-wordcount: '290'
ht-degree: 0%

---

# Recuperare i profili

Un profilo [!DNL Target] può essere recuperato in tre modi: utilizzando un `[!DNL Experience Cloud Visitor ID]` (`ECID`), `tntid` o `thirdPartyId`.

## Utilizzo di un [!DNL Experience Cloud Visitor ID] (ECID)

È possibile recuperare un profilo basato su `ECID`. Il metodo HTTP deve essere GET.

L’URL si presenta come nell’esempio seguente:

```
https://<clientCode>.tt.omtrdc.net/rest/v1/profiles/marketingCloudVisitorId/<ECID>?client=<clientCode>
```

Sostituisci `<clientCode>` con [!DNL Target] [!UICONTROL Client Code] e `<ECID>` con [!DNL Experience Cloud Visitor ID] ([!DNL Marketing Cloud Visitor ID]).

## Utilizzo di un tntid

[!DNL Target] assegna automaticamente un `tntid` per ogni richiesta.

L&#39;esempio seguente mostra il formato della richiesta per recuperare un profilo utilizzando un `tntid`:

```
https://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/your-tnt-id?client=<your-client-code>
```

Sostituisci `<your-client-code>` e `your-tnt-id` e attiva una richiesta GET. Di seguito è riportato un esempio di chiamata di recupero del profilo che utilizza `tntid`:

```
https://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/111492025094307-353046?client=<your-client-code>
```

## Utilizzo di un thirdPartyId

È possibile aumentare i profili [!DNL Adobe Target] con il proprio identificatore (ad esempio: ID CRM, `uuid`, numero di iscrizione e così via).

Consulta [Aggiornare i profili](/help/dev/administer/profile-api/profile-api-overview.md) per scoprire come allegare un `thirdPartyId` al tuo profilo.

L&#39;esempio seguente mostra il formato della richiesta per recuperare un profilo utilizzando un `thirdPartyId`:

```
https://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/thirdPartyId/your-thirdpartyid?client=<your-client-code>
```

Sostituisci `<your-client-code>` e `your-thirdpartyid` e attiva una richiesta GET. Di seguito è riportato un esempio di chiamata di recupero del profilo che utilizza [!UICONTROL thirdpartyid]:

```
https://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/thirdPartyId/a1-mbox3rdPartyId?client=<your-client-code>
```

Quando viene effettuata questa chiamata, [!DNL Target] tenta di individuare prima il profilo nel cluster indicato nella richiesta Edge o dove si trova il profilo e restituisce il contenuto. I contenuti del profilo vengono restituiti in formato JSON.

## Autenticazione

È possibile proteggere [!DNL Target Profile API] attivando l&#39;autenticazione dall&#39;interfaccia utente [!DNL Target] come descritto qui. Una volta attivata l’autenticazione, tutte le richieste API di profilo devono avere il token di autenticazione profilo impostato nelle intestazioni della richiesta. Il token può essere generato utilizzando l&#39;interfaccia utente [!DNL Target] o i passaggi descritti in precedenza nella sezione [Token di autenticazione profilo](https://developers.adobetarget.com/api/#authentication-tokens){target=_blank}.

## Misurazione

Queste chiamate non vengono conteggiate per le chiamate mbox.

## Gestione degli errori

Nel caso di una chiamata a `/thirdPartyId` con `thirdPartyId` specificato non valido o scaduto:

```
{"status" : 404, "message" : "No profile found for client <client_code> with third party id=<third_party_id>"}
```

Se il profilo non può essere individuato o è scaduto:

```
{"status" : 404, "message" : "No profile found for client <client_code> with mboxPC=<mbox_pc>"}
```
