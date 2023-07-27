---
keywords: Implementazione, at.js non JavaScript, adbox, redirector, mbox
description: Scopri come implementare [!DNL Adobe Target] in scenari non JavaScript, ad esempio l’utilizzo di un AdBox o di un redirector.
title: Come posso implementare [!DNL Target] per e-mail?
feature: Implement Email
exl-id: dda00b75-5d58-4405-ae58-75e7883a30ed
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '436'
ht-degree: 69%

---

# E-mail: implementare [!DNL Target]

Informazioni sull’implementazione di [!DNL Target] in scenari non JavaScript, ad esempio l’utilizzo di un AdBox o di un redirector.

È possibile tenere traccia delle visite agli annunci e ad altri contenuti fuori sito. Puoi inoltre identificare lo stesso utente sul sito e all&#39;esterno di esso, per fornirgli un&#39;esperienza web coerente. Utilizzando un singolo URL, l’AdBox consente il test senza JavaScript o at.js.

Un AdBox è utile per i siti che non hanno at.js, come gli affiliati. Se la tua attività richiede una creatività dinamica (ad esempio, nell&#39;annuncio è necessario mostrare un prodotto che è stato abbandonato nel carrello), non è possibile utilizzare un AdBox.

Gli annunci AdBox e i Redirector possono essere utilizzati con qualsiasi tipo di attività. La tabella seguente mette a confronto AdBox e Redirector, e il loro utilizzo:

| | Finalità | Caso di utilizzo | Struttura URL | Tipo di offerta | Contenuto dell&#39;offerta |
|--- |--- |--- |--- |--- |--- |
| AdBox | Restituzione di immagini diverse all&#39;annuncio | Per modificare il contenuto di un annuncio | `clientcode&#x200B;.tt.&#x200B;omtrdc&#x200B;.net/&#x200B;m2&#x200B;/&#x200B;clientcode/ubox/&#x200B;image?` | Offerta di reindirizzamento | URL per un&#39;immagine |
| Redirector | Reindirizza un visitatore a una pagina Web diversa | Per modificare la pagina di destinazione di un annuncio | `clientcode&#x200B;.tt.omtrdc.net/&#x200B;m2/clientcode&#x200B;/ubox/page?` | Offerta di reindirizzamento | URL per una pagina |

## Best practice per la sicurezza

Tieni presente che con Redirector puoi essere esposto a un rischio di vulnerabilità di reindirizzamento aperto. Per evitare l’uso non autorizzato dei collegamenti redirector da parte di terze parti, ti consigliamo di utilizzare &quot;host autorizzati&quot; per inserire nell&#39;elenco Consentiti i domini URL di reindirizzamento predefiniti. [!DNL Target] utilizza gli host per inserire nell&#39;elenco Consentiti i domini a cui desideri consentire i reindirizzamenti. Per ulteriori informazioni, consulta [Creare elenchi Consentiti che specificano gli host autorizzati per l’invio di chiamate mbox a  [!DNL Target]](https://experienceleague.adobe.com/docs/target/using/administer/hosts.html#allowlist) in *Host*.

## Vincoli

* Non vi è timeout lato client come per le mbox standard. Se [!DNL Target] è completamente inattivo, i visitatori dell’annuncio non visualizzeranno il contenuto, nemmeno quello predefinito.
* I cookie di terze parti vengono utilizzati per tenere traccia delle visite. Se i PCId sono diversi, per impostazione predefinita lil profilo di terza parte del visitatore viene unito a eventuali profili di prima parte esistente.
* Per utilizzare i cookie di prima parte su AdBox, dovrai portare la sessione mBox nell&#39;URL. A tale scopo, rivolgiti al rappresentante del tuo account.
* Per utilizzare i cookie di prima parte per tenere traccia dei clic sugli annunci, passa la sessione mbox nell&#39;URL. A tale scopo, rivolgiti al rappresentante del tuo account.
* Per utilizzare più di un AdBox nella stessa pagina, devi passare la sessione mbox nell&#39;URL. A tale scopo, rivolgiti al rappresentante del tuo account. Potresti avere un AdBox e un link Redirector nella stessa pagina (perché il Redirector è in realtà su una seconda pagina).
