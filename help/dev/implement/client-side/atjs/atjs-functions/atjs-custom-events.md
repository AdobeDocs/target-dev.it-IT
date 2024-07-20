---
keywords: eventi personalizzati, at.js, richiesta non riuscita, richiesta riuscita, rendering del contenuto non riuscito, rendering del contenuto riuscito, libreria caricata, inizio richiesta, inizio rendering del contenuto, rendering del contenuto senza offerte, reindirizzamento rendering del contenuto, eventi personalizzati2
description: Utilizza eventi personalizzati per la libreria JavaScript at.js di  [!DNL Adobe Target]  per ricevere notifiche quando una richiesta o un'offerta mbox ha esito negativo o positivo.
title: Come si utilizzano gli eventi personalizzati at.js?
feature: at.js
exl-id: a4baed9a-9eb8-4343-9834-709b03e44ca2
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '655'
ht-degree: 71%

---

# Eventi personalizzati at.js

Informazioni su `at.js custom events`, che consente di sapere quando una richiesta o un’offerta mbox ha esito negativo o positivo.

Storicamente, mbox.js (ora obsoleto) non ha permesso ad altri codici JavaScript in esecuzione sulla pagina di sapere cosa succede dietro le quinte. Con l&#39;avanzamento di at.js, abbiamo avuto un&#39;occasione unica per risolvere questo problema.

I nostri clienti richiedono di essere informati in diversi scenari, tra cui:

* Una richiesta mbox non riuscita a causa di timeout, codice di stato errato, errore di analisi JSON, ecc.
* Una richiesta mbox riuscita.
* Offerta di rendering fallita a causa di elemento mbox di wrapping mancante, selettore che non può essere trovato, ecc.
* Offerta di rendering riuscita. Sono state applicate modifiche DOM.

Gli eventi predefiniti hanno una struttura che consente di estrarre i dati necessari, in base al tipo di evento.

Per assicurarti che gli eventi possano essere usati in scenari diversi, gli eventi personalizzati hanno un oggetto payload che viene assegnato alla proprietà di dettaglio dell&#39;oggetto evento (che viene passato al gestore). Anche per evitare di passare stringhe come nomi di eventi, gli eventi sono esposti come costanti usando lo spazio dei nomi `adobe.target.event`.

## Struttura

| Chiave | Tipo | Descrizione |
|--- |--- |--- |
| type | Stringa | Ci sono diversi scenari per cui si desidera ricevere notifica per contribuire a monitorare, eseguire il debug e personalizzare l&#39;interazione con at.js.<p>Ogni evento personalizzato elencato di seguito contiene due formati: una “costante” e un “valore stringa”.<ul><li>**Costanti**: aggiunte a `adobe.target.event.`, includono trattini bassi e lettere solo maiuscole. Per abbonarti a eventi personalizzati *dopo* i carichi di at.js ma *prima* che la risposta mbox sia stata ricevuta, utilizza la costante.</li><li>**Valori stringa**: in minuscolo e contengono trattini. Per abbonarti a eventi personalizzati *prima* dei carichi at.js, utilizza il valore stringa.</li></ul>**Richiesta non riuscita**<p>Costante: `adobe.target.event.REQUEST_FAILED`<p>Valore stringa: `at-request-failed`<p>Descrizione: una richiesta mbox non riuscita a causa di timeout, codice di stato errato, errore di analisi JSON, ecc.<p>**Richiesta riuscita**<p>Costante: `adobe.target.event.REQUEST_SUCCEEDED`<p>Valore stringa: `at-request-succeeded`<p>Descrizione: una richiesta mbox ha avuto esito positivo.<p>**Rendering del contenuto non riuscito**<p>Costante: `adobe.target.event.CONTENT_RENDERING_FAILED`<p>Valore stringa: `at-content-rendering-failed`<p>Descrizione: offerta di rendering fallita a causa di elemento mbox di wrapping mancante, selettore che non può essere trovato, ecc.<p>**Rendering del contenuto riuscito**<p>Costante: `adobe.target.event.CONTENT_RENDERING_SUCCEEDED`<p>Valore stringa: `at-content-rendering-succeeded`<p>Descrizione: l&#39;offerta di rendering ha avuto esito positivo. Sono state applicate modifiche DOM.<p>**Libreria caricata**<p>Costante: `adobe.target.event.LIBRARY_LOADED`<p>Valore stringa: `at-library-loaded`<p>Descrizione: questo evento è ideale per le attività di monitoraggio quando at.js è stato completamente caricato. È possibile utilizzare questo evento per personalizzare l&#39;esecuzione della mbox globale. È anche possibile utilizzare questo evento per disattivare la mbox globale e quindi ascoltare l&#39;attivazione della mbox globale da parte dell&#39;evento in un secondo momento.<p>**Avvio richiesta**<p>Costante: `adobe.target.event.REQUEST_START`<p>Valore stringa: `at-request-start`<p>Descrizione: questo evento viene attivato prima dell&#39;esecuzione di una richiesta HTTP. È possibile utilizzare questo evento per le misurazioni delle prestazioni utilizzando l&#39;API di timing delle risorse.<p>**Avvio rendering del contenuto**<p>Costante: `adobe.target.event.CONTENT_RENDERING_START`<p>Valore stringa: `at-content-rendering-start`<p>Descrizione: questo evento viene attivato prima dell&#39;avvio del polling del selettore e viene eseguito il rendering del contenuto nella pagina. È possibile utilizzare questo evento per tenere traccia dello stato di rendering del contenuto.<p>**Rendering del contenuto nessuna offerta**<p>Costante: `adobe.target.event.CONTENT_RENDERING_NO_OFFERS`<p>Valore stringa: `at-content-rendering-no-offers`<p>Descrizione: questo evento viene attivato quando non vengono restituite offerte.<p>**Reindirizzamento del rendering del contenuto**<p>Costante: `adobe.target.event.CONTENT_RENDERING_REDIRECT`<p>Valore stringa: `at-content-rendering-redirect`<p>Descrizione: questo evento si attiva quando un&#39;offerta è un reindirizzamento e [!DNL Target] reindirizzerà a un URL diverso. |
| mbox | Stringa | Nome Mbox |
| message | Stringa | Contiene una descrizione leggibile, come l&#39;accaduto, il messaggio di errore, ecc. |
| tracking | Oggetto | Contiene `sessionId` e `deviceId`. In alcuni casi, `deviceId` potrebbe mancare perché [!DNL Target] non è riuscito a recuperarlo dal server Edge. |
| type | Stringa | **Artefatto di decisioning sul dispositivo completato**<p>Costante:<p>`adobe.target.event.ARTIFACT_DOWNLOAD_SUCCEEDED`<p>Valore stringa: `artifactDownloadSucceeded`<p>Descrizione: chiamato quando l’artefatto del decisioning sul dispositivo viene scaricato correttamente.<p>**Artefatto di decisioning sul dispositivo non riuscito**<p>Costante: `adobe.target.event.ARTIFACT_DOWNLOAD_FAILED`<p>Valore stringa: `artifactDownloadFailed`<p>Descrizione: chiamato quando non è stato possibile scaricare l’artefatto del decisioning sul dispositivo. |

## Utilizzo

```javascript {line-numbers="true"}
document.addEventListener(adobe.target.event.REQUEST_SUCCEEDED, function(event) { 
  console.log('Event', event); 
});
```

## Video di formazione: Token di risposta ed eventi personalizzati at.js ![Icona esercitazione](../../../assets/tutorial.png)

Guarda il video seguente per scoprire come utilizzare i token di risposta e gli eventi personalizzati at.js per condividere le informazioni del profilo da [!DNL Target] a sistemi di terze parti.

>[!VIDEO](https://video.tv.adobe.com/v/23253/?quality=12)
