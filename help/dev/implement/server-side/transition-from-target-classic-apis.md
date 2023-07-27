---
keywords: api, adobe i/o, classic, adobe developer console
description: Scopri come effettuare la transizione dal [!DNL Adobe Target Classic] API per [!DNL Target] API su [!DNL Adobe Developer Console].
title: Come posso effettuare la transizione da [!DNL Target Classic] API per [!DNL Target] API su [!DNL Adobe Developer Console]?
feature: APIs/SDKs
exl-id: b84e3767-89ad-4e2d-9bb4-7e31bffbc285
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 34%

---

# Transizione da [!DNL Target Classic] API per [!DNL Target] API su [!DNL Adobe Developer Console]

Informazioni utili per passare dal [!DNL Target Classic] API per [!DNL Target] API su [[!DNL Adobe Developer Console]](https://developer.adobe.com/console/home).

Con la disattivazione di [!DNL Adobe Target Classic], le API connesse al tuo [!DNL Target Classic] anche l&#39;account è stato reso non disponibile. Questo articolo ti aiuterà a eseguire la transizione [!DNL Target Classic] Integrazioni basate su API al [!DNL Target] API basate su [[!DNL Adobe Developer Console]](https://developer.adobe.com/console/home).

Per ulteriori informazioni su [!DNL Target] API, consulta [[!DNL Target] API](/help/dev/before-administer/target-api-overview.md). Per ulteriori informazioni su [!DNL Target] SDK, consulta [[!DNL Target] Implementazione lato server](/help/dev/implement/server-side/server-side-overview.md)

## Terminologia 

| Termine | Descrizione |
|--- |--- |
| API classica | API collegate al tuo [!DNL Target Classic] account. Queste chiamate API richiedono l’autenticazione con nome utente e password e utilizzano il nome host `testandtarget.omniture.com`. Se le chiamate API contengono un nome utente e una password nell’URL della richiesta, devi passare al [!DNL Adobe Developer Console] API. |
| [[!DNL Adobe Developer Console]](https://developer.adobe.com/console/home) | Il [!DNL Adobe Developer Console] è il gateway per [!DNL Target] API. Queste API sono collegate al tuo [!DNL Target Standard/Premium] account. Il [!DNL Target] API su [!DNL Adobe Developer Console] utilizza un [Autenticazione basata su JWT](../../before-administer/configure-authentication.md), standard di settore per le API aziendali sicure. |

## Linea temporale

Le seguenti API sono state dismesse quando [!DNL Target Classic] è stato disattivato:

| Data | Dettagli |
|--- |--- |
| 17 ottobre 2017 | Tutti i metodi delle API che eseguono un’operazione di scrittura (`saveCampaign`, `copyCampaign`, `saveHTMLOfferContent` e `setCampaignState`) sono stati ritirati.<P>È la stessa data in cui tutti [!DNL Target Classic] gli account utente sono stati impostati sullo stato di sola lettura. |
| 14 novembre 2017 | Le API rimanenti sono state disattivate. Questa è la data in cui [!DNL Target Classic] l&#39;interfaccia utente è stata disattivata |

[!DNL Recommendations Classic] Le API non sono state interessate da questa timeline.

## Metodi equivalenti

Nella tabella seguente è riportato l&#39;equivalente [!DNL Adobe Developer Console] Metodi API per i metodi API Classic. Il [!DNL Adobe Developer Console] Le API di restituiscono JSON, mentre le API classiche restituiscono XML.

Il [!DNL Adobe Developer Console] I metodi API sono collegati alla sezione corrispondente nel sito della documentazione API. Ogni metodo di API è corredato da esempio. È inoltre possibile utilizzare [!DNL Target] Insieme Postman dell’API amministratore che contiene esempi di chiamate API per tutti i metodi API di Adobe su [!DNL Adobe Developer Console].

| Raggruppamento | Metodo API classico | [!DNL Adobe Developer Console] Metodo API | Note |
|--- |--- |--- |--- |
| Campagna/attività | Crea campagna | [Crea attività AB](https://developers.adobetarget.com/api/#create-ab-activity)<P>[Crea attività XT](https://developers.adobetarget.com/api/#create-xt-activity) | Le nuove API forniscono metodi di creazione separati per AB e XT |
|  | Aggiorna campagna | [Aggiorna l&#39;attività AB](https://developers.adobetarget.com/api/#update-ab-activity)<P>[Aggiorna l&#39;attività XT](https://developers.adobetarget.com/api/#update-xt-activity) |  |
|  | Copia campagna | N/D | Utilizza le API Crea attività |
|  | Elenco campagne | [Elenco attività](https://developers.adobetarget.com/api/#list-activities) |  |
|  | Stato campagna | [Aggiorna stato attività](https://developers.adobetarget.com/api/#update-activity-state) |  |
|  | Visualizza campagna | [Ottieni l&#39;attività AB da ID](https://developers.adobetarget.com/api/#get-ab-activity-by-id)<P>[Ottieni l&#39;attività XT per ID](https://developers.adobetarget.com/api/#get-xt-activity-by-id) |  |
|  | ID campagna di terze parti | N/D | Se si utilizza un thirdpartyID, puoi utilizzare i metodi di attività rilevanti |
| Offerte | Crea offerta | [Crea offerta](https://developers.adobetarget.com/api/#create-offer) |  |
|  | Ottieni offerta | [Ottieni offerta per ID](https://developers.adobetarget.com/api/#get-offer-by-id) |  |
|  | Elenco offerte | [Elenco offerte](https://developers.adobetarget.com/api/#list-offers) |  |
|  | Elenco cartelle | N/D | Le cartelle non sono supportate in [!DNL Target Standard/Premium] |
| Generazione di rapporti | Rapporto prestazioni campagna | [Ottieni rapporto prestazioni AB](https://developers.adobetarget.com/api/#get-ab-performance-report)<P>[Ottieni rapporto prestazioni XT](https://developers.adobetarget.com/api/#get-xt-performance-report) |  |
|  | Rapporto audit | [Ottieni rapporto audit](https://developers.adobetarget.com/api/#get-audit-report) |  |
|  | Rapporto contenuto 1-1 | [Ottieni rapporto prestazioni AP](https://developers.adobetarget.com/api/#get-ap-activity-performance-report) |  |
| Impostazioni account | Ottieni gruppi host | [Elenco ambienti](https://developers.adobetarget.com/api/#list-environments) |  |

## Eccezioni

Se hai bisogno di un&#39;eccezione, contatta il tuo Customer Success Manager.

## Aiuto

Contatta [!DNL Adobe Target Client Care] (tt-support@adobe.com) in caso di domande o se hai bisogno di aiuto nella transizione dalle API Classic a [!DNL Target] API su [!DNL Adobe Developer Console].
