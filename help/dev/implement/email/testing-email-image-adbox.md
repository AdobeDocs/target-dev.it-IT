---
keywords: e-mail, adbox, e-mail immagine adbox
description: Scopri come utilizzare [!DNL Adobe Target] per testare dinamicamente le immagini nelle e-mail e persino modificarle al volo quando qualcuno apre l’e-mail.
title: Come posso testare un AdBox dell’immagine di un’e-mail?
feature: Implement Email
exl-id: 4512741a-567f-41bb-9721-3e1c4f5302e1
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '417'
ht-degree: 79%

---

# Test AdBox di un&#39;immagine per e-mail

Esegui un test dinamico delle immagini in un’e-mail, e cambiale al volo quando qualcuno apre l’e-mail.

I redirector possono essere utilizzati nelle e-mail per monitorare i clic e controllare dinamicamente quale pagina di destinazione viene raggiunta dagli utenti.

Il test dell&#39;immagine di una e-mail si ottiene attraverso l&#39;utilizzo di versioni modificate di AdBox. Poiché i client di posta elettronica non consentono l&#39;impostazione dei cookie, per ogni messaggio di posta elettronica deve essere generato un identificatore univoco. Questo numero viene aggiunto all&#39;URL dell&#39;AdBox e a qualsiasi redirector utilizzato nell&#39;email per tenere traccia dei clic dall&#39;e-mail.

>[!NOTE]
>
>Sebbene [!DNL Target] possa distribuire tecnicamente l&#39;ottimizzazione dell&#39;immagine in fase di apertura, ogni client di posta elettronica gestisce la memorizzazione nella cache in modo diverso, in modo che l&#39;esito positivo possa variare. Indipendentemente dal provider del servizio e-mail utilizzato, il client e-mail utilizzato dall&#39;utente finale per aprire l&#39;e-mail (app Gmail, Outlook, Hotmail e così via) determina se l&#39;immagine è effettivamente racchiusa in fase di apertura. Ad esempio, Gmail memorizza nella cache la maggior parte degli elementi, quindi l&#39;immagine che l&#39;utente finale visualizza è legata al momento in cui Gmail sceglie di richiamarla e memorizzarla nella cache dell&#39;immagine.

**Codice di esempio per un&#39;AdBox dell&#39;immagine di un&#39;e-mail:**

```
<img src="https://{clientcode}.tt.omtrdc.net/m2/​{clientcode}/ubox/​image?
mbox={email_header}&
mboxDefault=​{http%3A%2F%2Fwww.domain.com%2Fheader.jpg}&
mboxXDomain=disabled&
mboxSession={123456}&
mboxPC={123456}" border=:"0"/>
```

I valori qui di seguito sono specifici per il tuo caso:

| Valore | Descrizione |
|--- |--- |
| clientcode | Il codice cliente della tua azienda. Trovalo nel tuo at.js elencato come `clientCode='yourclientcode'`. Tutto minuscolo e senza caratteri speciali. |
| image | Tipo di offerta. È sempre “image” per gli annunci grafici e “page” per i redirector. |
| email_header | Il nome della AdBox. |
| `mboxDefault=http%3A%2F%2Fwww.domain.com%2Fheader.jpg` | Obbligatorio. Sostituisci l’URL con il contenuto predefinito appropriato per la AdBox. Deve essere un riferimento assoluto e deve essere codificato nell’URL. |
| `mboxXDomain=disabled` | Indica a [!DNL Target] di non tentare di impostare un cookie. |
| `mboxSession=123456` e `mboxPC=123456` | Due valori richiesti da [!DNL Target] per unire il profilo di questo utente al profilo esistente per il sito. 123456 è l&#39;identificatore univoco generato per e-mail. Inserire in modo dinamico questo valore in ogni URL AdBox e redirector. Questo numero deve essere univoco per ogni e-mail inviata a ciascuna persona. Se un&#39;e-mail settimanale viene inviata a 1000 persone, è necessario generare 1000 ID univoci.<br />L&#39;identificatore univoco per ogni e-mail deve essere assegnato a mboxSession e mboxPC in ciascun URL della AdBox e del redirector. Il formato consigliato per l’identificatore è timestamp-NNNNN, dove NNNNN è un numero casuale di 5 cifre, ma funziona qualsiasi formato alfanumerico. Alcuni servizi e-mail di massa e qualsiasi linguaggio di programmazione sono in grado di generare questo identificatore univoco. |
