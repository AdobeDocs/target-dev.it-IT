---
title: Recuperare Recommendations con l’API di consegna
description: Questo articolo guida gli sviluppatori attraverso i passaggi necessari per recuperare i contenuti dei consigli utilizzando l’API di distribuzione di Adobe Target.
feature: APIs/SDKs, Recommendations, Administration & Configuration
kt: 3815
thumbnail: null
author: Judy Kim
exl-id: 9b391f42-2922-48e0-ad7e-10edd6125be6
source-git-commit: ba53161b2ec51af3d90994773034790feb51099c
workflow-type: tm+mt
source-wordcount: '1507'
ht-degree: 1%

---

# Recupero di Recommendations con l’API di consegna

Le API Recommendations di Adobe Target e Adobe Target possono essere utilizzate per fornire risposte alle pagine web, ma possono anche essere utilizzate in esperienze non basate su HTML, tra cui app, schermi, console, e-mail, chioschi e altri dispositivi di visualizzazione. In altre parole, quando non è possibile utilizzare le librerie di Target e JavaScript, il [API di consegna di Target](/help/dev/implement/delivery-api/overview.md) consente comunque di accedere a tutte le funzionalità di Target, per distribuire esperienze personalizzate.

>[!NOTE]
>
>Quando si richiedono contenuti contenenti consigli effettivi (prodotti o elementi consigliati), utilizza l’API di consegna di Target.

Per recuperare i consigli, invia una chiamata POST API di consegna Adobe Target con le informazioni contestuali appropriate, che possono includere un ID utente (da utilizzare con i consigli specifici del profilo, come gli elementi visualizzati di recente dall’utente), il nome mbox pertinente, i parametri mbox, i parametri del profilo o altri attributi. La risposta includerà il file entity.ids consigliato (e potrebbe includere altri dati di entità) in formato JSON o HTML, che potrà quindi essere visualizzato nel dispositivo.

Il [API di consegna](/help/dev/implement/delivery-api/overview.md) per Adobe Target espone tutte le funzioni esistenti fornite da una richiesta Target standard.

L’API di consegna:

* Consente di recuperare esperienze o offerte per una posizione e un pubblico in modo RESTful.
* Non richiede alcuna autenticazione.
* Solo POST.
* Non elabora cookie o chiamate di reindirizzamento.
* Non richiede o riconosce &quot;ruoli utente&quot;. Recupera semplicemente i contenuti o segnala gli eventi ai server edge di Target.

Per utilizzare l’API di consegna per fornire esperienze Target, inclusi i consigli, segui questi passaggi:

1. Crea un’attività Target (A/B, XT, AP o Recommendations) utilizzando il Compositore basato su moduli (non il Compositore esperienza visivo).
1. Utilizza l’API di consegna per ottenere una risposta per le richieste generate dall’attività Target appena creata.

&lt;!— D: Perché sono entrambe le fasi necessarie a questo scopo? Se hai definito un consiglio basato su moduli per una mbox, qual è il vantaggio di avere ANCHE il passaggio API di consegna in per recuperare i risultati? Perché non puoi semplicemente far sì che la registrazione basata su moduli distribuisca i risultati nel dispositivo di destinazione? ?? R: Vedi il caso d’uso seguente... è il momento in cui desideri &quot;intercettare&quot; i risultati in sospeso per fare più cose prima di visualizzarli. Confronti in tempo reale con i livelli di inventario. --->

## Creare un consiglio utilizzando il Compositore esperienza basato su moduli

Per creare consigli che possono essere utilizzati con l’API di consegna, utilizza [Compositore basato su moduli](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html).

1. Innanzitutto, crea e salva una progettazione basata su JSON da utilizzare nei consigli. Per un esempio di JSON, oltre a informazioni generali su come restituire le risposte JSON durante la configurazione di un’attività basata su modulo, consulta la documentazione su [Creazione di design per consigli](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-design/create-design.html). In questo esempio, la progettazione è denominata *JSON semplice.*
   ![server-side-create-recs-json-design.png](assets/server-side-create-recs-json-design.png)

1. In Target, passa a **[!UICONTROL Attività]** > **[!UICONTROL Crea attività]** > **[!UICONTROL Recommendations]**, quindi seleziona **[!UICONTROL Modulo]**.

   ![server-side-create-recs.png](assets/server-side-create-recs.png)

1. Seleziona una proprietà e fai clic su **[!UICONTROL Successivo]**.
1. Definisci il percorso in cui desideri che gli utenti ricevano la risposta del consiglio. L’esempio seguente utilizza una posizione denominata *api_charter*. Seleziona la progettazione basata su JSON, creata in precedenza, denominata *JSON semplice.*
   ![server-side-create-recs-form.png](assets/server-side-create-recs-form1.png)
1. Salva e attiva il consiglio. Genera risultati. [Quando i risultati sono pronti](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-activity/previewing-and-launching-your-recommendations-activity.html), puoi utilizzare l’API di consegna per recuperarli.

## Utilizzare l’API di consegna

La sintassi per il [API di consegna](/help/dev/implement/delivery-api/overview.md) è:

`POST https://{{CLIENT_CODE}}.tt.omtrdc.net/rest/v1/delivery`

1. Nota che il codice client è obbligatorio. Come promemoria, il codice cliente si trova in Adobe Target passando a **[!UICONTROL Recommendations]** > **[!UICONTROL Impostazioni]**. Osserva **Codice client** valore in **Token API per i consigli** sezione.
   ![client-code.png](assets/client-code.png)
1. Una volta ottenuto il codice client, crea la chiamata API di consegna. L’esempio seguente inizia con **[!UICONTROL Chiamata API di consegna Mbox in batch web]** fornite in [Raccolta Postman API di consegna](../../implement/delivery-api/overview.md/#section/Getting-Started/Postman-Collection), apportando le modifiche necessarie. Ad esempio:
   * il **browser** e **indirizzo** Gli oggetti sono stati rimossi da **Corpo**, in quanto non sono necessari per i casi di utilizzo non HTML
   * *api_charter* è elencato come nome della posizione in questo esempio
   * viene specificato entity.id, in quanto questo consiglio si basa sulla somiglianza dei contenuti, che richiede la trasmissione di una chiave dell&#39;elemento corrente a Target.
     ![server-side-Delivery-API-call.png](assets/server-side-delivery-api-call2.png)
Ricorda di configurare correttamente i parametri di query. Ad esempio, assicurati di specificare `{{CLIENT_CODE}}` secondo necessità. &lt;!— Q: nella sintassi di chiamata aggiornata, entity.id è elencato come profileParameter invece di mboxParameter come nelle versioni precedenti. ---> &lt;!— Q: immagine vecchia ![server-side-create-recs-post.png](assets/server-side-create-recs-post.png) Testo di accompagnamento precedente: &quot;Nota: questo consiglio si basa su prodotti Content Similar basati sul file entity.id inviato tramite mboxParameters.&quot; —>
     ![client-code3](assets/client-code3.png)
1. Invia la richiesta. Questa operazione viene eseguita in base al *api_charter* posizione, con un consiglio attivo in esecuzione, definita con la progettazione JSON che restituirà un elenco di entità consigliate.
1. Ricevi una risposta in base alla progettazione JSON.
   ![server-side-create-recs-json-response2.png](assets/server-side-create-recs-json-response2.png)
La risposta include l’ID della chiave e gli ID delle entità consigliate.

Questo utilizzo dell’API di consegna con Recommendations consente di eseguire passaggi aggiuntivi prima di visualizzare i consigli al visitatore sul dispositivo non HTML. Ad esempio, puoi prendere la risposta dall’API di consegna per eseguire una ricerca aggiuntiva e in tempo reale dei dettagli degli attributi di entità (inventario, prezzo, valutazione e così via) da un altro sistema (come una piattaforma CMS, PIM o e-commerce), prima di visualizzare i risultati finali.

Utilizzando l’approccio descritto in questa guida, puoi ottenere da qualsiasi applicazione di sfruttare la risposta di Target per fornire consigli personalizzati.

## Implementazioni di esempio

Le risorse seguenti forniscono esempi di varie implementazioni non incentrate su HTML. Tieni presente che ogni implementazione sarà unica, a causa del sistema e dei dispositivi coinvolti.

| Risorsa | Dettagli |
| --- | --- |
| [Adobe Target Everywhere - Implementare il lato server o nell’IoT](https://expleague.azureedge.net/labs/L733/index.html) | Adobi Summit 2019 Lab offre un’esperienza pratica per un’applicazione React che sfrutta le API lato server di Adobe Target. |
| [Adobe Target in un’app mobile senza l’SDK di Adobe](https://community.tealiumiq.com/t5/Universal-Data-Hub/Adobe-Target-in-a-Mobile-App-Without-the-Adobe-SDK/ta-p/26753) | Questa guida illustra come configurare Adobe Target nell’app mobile senza installare l’SDK di Adobe. Questa soluzione utilizza la visualizzazione web dell’SDK Tealium e il modulo Remote Commands per inviare e ricevere richieste all’API visitatore Adobe (Experience Cloud) e all’API Adobe Target. |
| [Configurazione dell’estensione Target nel Experience Platform Launch e implementazione delle API di Target](https://developer.adobe.com/client-sdks/documentation/adobe-target/) | Passaggi per configurare l’estensione Target in Experienci Platform Launch, aggiungere l’estensione Target all’app e implementare le API Target per richiedere attività, preacquisire offerte e attivare la modalità di anteprima visiva. |
| [Client nodo Adobe Target](https://www.npmjs.com/package/@adobe/target-nodejs-sdk) | SDK Node.js v1.0 di Target open-source |
| [Panoramica di Server Side](../../implement/server-side/server-side-overview.md) | Informazioni su API di distribuzione lato server di Adobe Target, API di distribuzione in batch lato server, SDK di Node.js e API di Adobe Target Recommendations. |
| [Contenuto Adobe Campaign Recommendations in e-mail](https://medium.com/adobetech/adobe-campaign-content-recommendations-in-email-b51ced771d7f) | Blog che descrive come sfruttare i consigli sui contenuti nelle e-mail tramite Adobe Target e Adobe I/O Runtime in Adobe Campaign. |

## Gestione delle installazioni di Recommendations con le API

Nella maggior parte dei casi, i consigli sono configurati nell’interfaccia utente di Adobe Target e quindi utilizzati o accessibili tramite le API di Target, per motivi come quelli menzionati nelle sezioni precedenti. Questo coordinamento UI-API è comune. Tuttavia, a volte gli utenti possono voler eseguire tutte le azioni tramite API, sia la configurazione che l’utilizzo dei risultati. Anche se molto meno comune, gli utenti possono assolutamente configurare, eseguire, *e* sfrutta interamente i risultati dei consigli utilizzando le API.

Abbiamo imparato in un [sezione precedente](manage-catalog.md) come gestire le entità Recommendations di Adobe Target e distribuirle lato server. Analogamente, il [Console Adobe Developer](https://developer.adobe.com/console/home) consente di gestire criteri, promozioni, raccolte e modelli di progettazione senza dover accedere ad Adobe Target. È possibile trovare un elenco completo di tutte le API di Recommendations [qui](http://developers.adobetarget.com/api/recommendations/), ma ecco un riepilogo come riferimento.

| Risorsa | Dettagli |
| --- | --- |
| [Raccolte](http://developers.adobetarget.com/api/recommendations/#tag/Collections) | Elenca, crea, ottieni, modifica ed elimina raccolte. |
| [Criteri](http://developers.adobetarget.com/api/recommendations/#tag/Criteria) | Elencare e ottenere i criteri. |
| [Progettazioni](http://developers.adobetarget.com/api/recommendations/#tag/Designs) | Elencare, creare, ottenere, modificare, eliminare e convalidare le progettazioni. |
| [Entità](http://developers.adobetarget.com/api/recommendations/#tag/Entities) | Salvare, eliminare e ottenere le entità. |
| [Promozioni](http://developers.adobetarget.com/api/recommendations/#tag/Promotions) | Elenca, crea, ottieni, modifica ed elimina promozioni. |
| [Criteri categoria](http://developers.adobetarget.com/api/recommendations/#tag/Category-Criteria) | Elenca, crea, ottieni, modifica ed elimina criteri di categoria. |
| [Criteri personalizzati](http://developers.adobetarget.com/api/recommendations/#tag/Custom-Criteria) | Elenca, crea, ottieni, modifica ed elimina criteri personalizzati. |
| [Criteri articolo](http://developers.adobetarget.com/api/recommendations/#tag/Item-Criteria) | Elenca, crea, ottieni, modifica ed elimina i criteri degli elementi. |
| [Criteri di popolarità](http://developers.adobetarget.com/api/recommendations/#tag/Popularity-Criteria) | Elencare, creare, ottenere, modificare ed eliminare criteri di popolarità. |
| [Criteri attributo profilo](http://developers.adobetarget.com/api/recommendations/#tag/Profile-Attribute-Criteria) | Elenca, crea, ottieni, modifica ed elimina i criteri degli attributi di profilo. |
| [Criteri recenti](http://developers.adobetarget.com/api/recommendations/#tag/Recent-Criteria) | Elenca, crea, ottieni, modifica ed elimina criteri recenti. |
| [Sequence Criteria](http://developers.adobetarget.com/api/recommendations/#tag/Sequence-Criteria) | Elencare, creare, ottenere, modificare ed eliminare criteri di sequenza. |

## Documentazione di riferimento

* [Documentazione API di consegna di Adobe Target](/help/dev/implement/delivery-api/overview.md)
* [Integrare i Recommendations con l’e-mail](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-faq/integrating-recs-email.html)

## Riepilogo e revisione

Congratulazioni! Completando questa guida, hai imparato a:
* [Gestire il catalogo utilizzando l’API Recommendations](manage-catalog.md)
* [Gestire i criteri personalizzati tramite l’API di Recommendations](manage-custom-criteria.md)
* [Utilizzare l’API di consegna con Recommendations](fetch-recs-server-side-delivery-api.md)
