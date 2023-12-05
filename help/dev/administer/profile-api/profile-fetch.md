---
title: Recuperare i profili
description: Scopri come utilizzare le API di profilo di Adobe Target per recuperare i dati dei visitatori da utilizzare in [!DNL Target].
contributors: https://github.com/icaraps
feature: APIs/SDKs
source-git-commit: e5a1c38d448cb7446b7b26cd0dc882976ba94dd3
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 0%

---

# Aggiornare profili

A [!DNL Target] il profilo può essere recuperato in due modi: utilizzando un `tntid` o un `thirdPartyId`.

## Utilizzo di un tntid

[!DNL Target] assegna automaticamente un `tntid` per ogni richiesta.

L’esempio seguente mostra il formato della richiesta per recuperare un profilo utilizzando un `tntid`:

```
https://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/your-tnt-id?client=<your-client-code>
```

Sostituisci `<your-client-code>` e `your-tnt-id` e attiva una richiesta GET. Esempio di chiamata di recupero del profilo tramite un `tntid`;

```
http://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/111492025094307-353046?client=<your-client-code>
```

## Utilizzo di un thirdPartyId

[!DNL Adobe Target] I profili possono essere migliorati con il tuo identificatore (ad esempio: ID CRM, `uuid`, numero di iscrizione e così via).

Consulta [Aggiornare profili](/help/dev/administer/profile-api/profile-api-overview.md) per scoprire come allegare un `thirdPartyId` al tuo profilo.

L’esempio seguente mostra il formato della richiesta per recuperare un profilo utilizzando un `thirdPartyId`:

```
https://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/thirdPartyId/your-thirdpartyid?client=<your-client-code>
```

Sostituisci `<your-client-code>` e `your-thirdpartyid` e attiva una richiesta GET. Esempio di chiamata di recupero del profilo tramite un [!UICONTROL thirdpartyid]:

```
https://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/thirdPartyId/a1-mbox3rdPartyId?client=<your-client-code>
```

Quando viene effettuata questa chiamata, [!DNL Target] tenta di individuare il profilo prima nel cluster indicato nella richiesta edge, oppure ovunque si trovi il profilo, quindi restituisce il contenuto. I contenuti del profilo vengono restituiti in formato JSON.

## Autenticazione

Il [!DNL Target Profile API] può essere protetto attivando l’autenticazione dalla [!DNL Target] Interfaccia utente come descritto qui. Una volta attivata l’autenticazione, tutte le richieste API di profilo devono avere il token di autenticazione profilo impostato nelle intestazioni della richiesta. Il token stesso può essere generato utilizzando [!DNL Target] o utilizzando i passaggi descritti in precedenza nella [Token di autenticazione profilo](https://developers.adobetarget.com/api/#authentication-tokens){target=_blank} sezione.

## Misurazione

Queste chiamate non vengono conteggiate per le chiamate mbox.

## Gestione degli errori

Nel caso di una chiamata a `/thirdPartyId` con un valore non valido o scaduto `thirdPartyId` specificato:

```
{"status" : 404, "message" : "No profile found for client <client_code> with third party id=<third_party_id>"}
```

Se il profilo non può essere individuato o è scaduto:

```
{"status" : 404, "message" : "No profile found for client <client_code> with mboxPC=<mbox_pc>"}
```
