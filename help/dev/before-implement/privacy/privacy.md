---
keywords: privacy, indirizzo ip, geosegmentazione, opt out, opt-out, opt-out, privacy dei dati, normative governative, regolamenti, rgpd, ccpa, privacy, informazioni personali identificabili, PII
description: Scopri come [!DNL Adobe Target] è conforme alle leggi sulla privacy dei dati applicabili, inclusa la raccolta e la gestione di indirizzi IP, PII e istruzioni di rinuncia.
title: In che modo Target gestisce i problemi relativi alla privacy, inclusi i PII?
feature: Privacy & Security
exl-id: 4330e034-2483-4a25-9c87-48dbef6fc9de
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '716'
ht-degree: 58%

---

# Privacy

In [!DNL Adobe Target] sono stati inclusi processi e impostazioni che ti consentono di utilizzare [!DNL Target] in conformità con le leggi sulla privacy dei dati.

## Raccolta di indirizzi IP e informazioni personali (PII, Personally Identifiable Information)

L&#39;indirizzo IP di un visitatore del tuo sito Web viene trasmesso a un DPC (Adobe Data Processing Center). A seconda della configurazione di rete per il visitatore, l’indirizzo IP non rappresenta necessariamente l’indirizzo IP del computer del visitatore. Potrebbe essere ad esempio l’indirizzo IP esterno di un firewall con traduzione degli indirizzi di rete (Network Address Translation, NAT), di un proxy HTTP o di un gateway Internet.

>[!IMPORTANT]
>
>[!DNL Target] non memorizza indirizzi IP dell’utente o informazioni personali (PII, Personally Identifiable Information). Gli indirizzi IP vengono utilizzati solo da [!DNL Target] durante la sessione (in memoria, senza persistenza).

## Sostituzione dell’ultimo ottetto di indirizzi IP

Adobe ha sviluppato un’impostazione &quot;privacy by design&quot; che gli utenti possono abilitare, ad Adobe [!DNL Target]. Quando è attivata, Adobe [!DNL Target] offusca immediatamente l’ultimo ottetto (l’ultima porzione) dell’indirizzo IP nel momento in cui l’indirizzo IP viene raccolto. Questa forma di anonimizzazione viene eseguita prima di qualsiasi elaborazione dell’indirizzo IP, inclusa l’operazione di lookup geografico.

Quando questa funzione è abilitata, l’indirizzo IP è reso sufficientemente anonimo da non essere più identificabile come dato personale. Di conseguenza, [!DNL Target] può essere utilizzato in conformità con le leggi sulla privacy dei dati in paesi che non consentono la raccolta di informazioni personali. L’ottenimento di informazioni a livello di città sarà probabilmente influenzato in modo significativo dall’oscuramento dell’indirizzo IP. L’ottenimento di informazioni a livello di area e nazionale dovrebbe essere influenzato solo leggermente.

Le seguenti impostazioni sono disponibili nel [!DNL Target] Interfaccia utente di **[!UICONTROL Amministrazione]** > **[!UICONTROL Implementazione]**:

* [!UICONTROL Offuscamento dell’ultimo ottetto]: [!DNL Target] nasconde l’ultimo ottetto dell’indirizzo IP.
* [!UICONTROL Offuscamento dell’intero IP]: [!DNL Target] nasconde l&#39;intero indirizzo IP.
* [!UICONTROL Nessuno]: [!DNL Target] non nasconde alcuna parte dell’indirizzo IP.

  ![obfuscate-ip-options](assets/obfuscate-ip.png)

[!DNL Target] riceve l’indirizzo IP completo e lo offusca (se impostato su Last octet o Entire IP) come specificato. [!DNL Target] mantiene quindi in memoria l’indirizzo IP offuscato solo durante la sessione corrente.

## Geosegmentazione

Se attivi l’oscuramento dell’ultimo ottetto dell’indirizzo IP, i restanti valori dell’indirizzo IP possono essere analizzati utilizzando i rapporti in [!DNL Target]. Se l’ultimo ottetto dell’indirizzo IP non viene oscurato, l’indirizzo IP completo può essere analizzato in [!DNL Target]. Puoi utilizzare la funzione di geosegmentazione per mappare la posizione dei visitatori per area geografica. I dati di geosegmentazione sono granulari solo a livello di città o di codice postale, e non a livello individuale.

Se gli indirizzi IP sono completamente offuscati, GeoSegmentation e il target geografico non sono disponibili.

## Collegamento per la rinuncia

Puoi aggiungere un collegamento di rinuncia ai siti per consentire ai visitatori di rinunciare a tutte le operazioni di conteggio e di distribuzione dei contenuti.

1. Aggiungi al sito il seguente collegamento:

   `<a href="https://clientcode.tt.omtrdc.net/optout"> Your Opt Out Language Here</a>`

1. (Condizionale) Se utilizzi CNAME, il collegamento deve contenere il messaggio &quot;client=`clientcode` parametro, ad esempio:
   `https://my.cname.domain/optout?client=clientcode`.

1. Sostituisci `clientcode` con il tuo codice cliente e aggiungi il testo o l’immagine da collegare all’URL di rinuncia.

Il visitatore che fa clic sul collegamento sarà escluso da qualsiasi richiesta mbox richiamata dalle relative sessioni di navigazione fino all’eliminazione dei cookie o per due anni (a seconda di quale dei due eventi si verifica prima). Questo funziona impostando un cookie per il visitatore denominato `disableClient` nel dominio `clientcode.tt.omtrdc.net`.

Anche se utilizzi un’implementazione di cookie di prima parte, la rinuncia viene impostata tramite un cookie di terze parti. Se il client utilizza solo un cookie di prima parte, [!DNL Target] controlla se è impostato un cookie di rinuncia.

## Normative sulla privacy e la protezione dei dati

Consulta [Privacy and data protection regulations](/help/dev/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.md) (Normative sulla privacy e la protezione dei dati) per informazioni sul regolamento generale sulla protezione dei dati (RGPD) dell’Unione Europea, sul California Consumer Privacy Act (CCPA) e altri requisiti internazionali sulla privacy e su come queste normative influiscono sulla tua organizzazione e su [!DNL Target].

## Raccolta di dati sull’utilizzo delle funzioni

I dati di utilizzo delle singole funzioni vengono raccolti a scopo di Adobe interno per identificare se [!DNL Target] le funzioni vengono eseguite come previsto o per identificare le funzioni utilizzate poco. Vengono raccolte varie misurazioni della latenza per contribuire a risolvere i problemi legati alle prestazioni. I dati personali non vengono raccolti.

Puoi rinunciare al reporting dei dati di utilizzo nei nostri SDK impostando `telemetryEnabled` su false nelle opzioni di inizializzazione del client. Per ulteriori informazioni, consulta [telemetryEnabled in targetGlobalSettings](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#telemetryenabled).
