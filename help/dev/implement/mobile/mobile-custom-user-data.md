---
keywords: app mobile, inviare dati app mobile, app mobile target mobile, dati personalizzati utente mobile, app mobile, dati personalizzati app mobile
description: Scopri come inviare informazioni aggiuntive sulla posizione o sull’utente a [!DNL Adobe Target] come coppie nome-valore per aiutarti a creare tipi di pubblico personalizzati.
title: Come posso inviare dati utente personalizzati in un’app iOS?
feature: Implement Mobile
exl-id: 9cf8e8fd-1898-43b1-b339-d7a21cb35d57
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 62%

---

# iOS - Inviare dati utente personalizzati

Puoi inviare ulteriori informazioni sulla posizione o sull’utente a [!DNL Target] come coppie nome-valore.

>[!IMPORTANT]
>
>Supporto per [!DNL Adobe Mobile] versione 4.*x* SDK è terminato il 31 agosto 2021 e non è più consigliato per [!DNL Adobe Target] utenti di dispositivi mobili.
>
>Il [SDK di Adobe Experience Platform per le app mobili](https://developer.adobe.com/client-sdks/documentation/){target=_blank} è la soluzione consigliata per [!DNL Adobe Experience Cloud] soluzioni e servizi nelle app mobili.

Queste informazioni possono essere utilizzate per creare tipi di pubblico personalizzati (ad esempio, utenti che distano oltre 25.000 km) e generare rapporti.

Esistono due tipi di parametri che è possibile inviare con un [!DNL Target] chiama:

* **Parametri mbox**: i parametri mbox non sono persistenti tra sessioni diverse.
* **Parametri del profilo**: i parametri di profilo sono memorizzati nell’archivio del profilo visitatore e rimangono costanti per tutte le sessioni. I parametri mbox non persistono. Mentre alcuni tasti sono riservati, i parametri profilo e mbox possono essere coppie chiave-valore personalizzate.

Sebbene esistano alcune chiavi riservate, entrambi i parametri profilo e mbox possono contenere coppie chiave-valore personalizzate.

1. Crea dizionario.

   Innanzitutto, crea un dizionario con i valori inviati da passare a [!DNL Target]. Per motivi di convenienza, aggiungilo all&#39;interno del metodo `welcomeMessageCampaign` in modo da non doverti preoccupare dell&#39;obbiettivo.

   Di seguito è riportato un dizionario di esempio. Puoi copiarlo e incollarlo all’interno di `(void)welcomeMessageCampaign`. I valori per le chiavi come `userLevel` e `userMiles` sono inseriti direttamente in questo esempio. In generale, si passano le variabili corrispondenti.

   ```
   NSDictionary *targetParams = [[NSDictionary alloc] initWithObjectsAndKeys: 
                                 @"platinum",@"userLevel", 
                                 @26500,@"userMiles", 
                                 @"1067007",@"entity.id", 
                                 @"dealsapp.qa", @"host", 
                                 @"fashion",@"entity.categoryId", 
                                 @"millenial", @"profile.persona", 
                                 @"cohort_5", @"profile.cohort", 
                                 nil];
   ```

   * Le chiavi con il profilo prefisso (ad esempio, `profile.persona`) sono memorizzate nel profilo dell&#39;utente.

     Questi attributi di profilo possono essere utilizzati in diverse attività e canali.

   * Le chiavi che non hanno alcun prefisso (ad esempio, `userMiles`) sono parametri mbox.

     Questi parametri sono disponibili solo durante la sessione.

   * Le chiavi con l&#39;entità prefisso (ad esempio,`entity.category.id`) vengono utilizzate per i consigli di prodotti.

1. Verifica i dati.
   1. In applicazione `didFinishLaunchingWithOptions`, rimuovi il commento o aggiungi `[ADBMobile setDebugLogging:YES];`.

      Così vengono stampati dettagliati log di debug.
   1. Genera l’app.
   1. Verifica che i parametri vengano passati nella chiamata di destinazione.

      Cerca il nome della posizione di destinazione nella console di debug. Verrà visualizzata una chiamata a `YOUR-CLIENT-CODE.tt.omtrdc.net` con tutti i parametri appena passati.

      (Fare clic sull&#39;immagine per espanderla a larghezza intera.)

      ![Posizione di destinazione nella console di debug](/help/dev/implement/mobile/assets/mobile-debug.png "Posizione di destinazione nella console di debug"){zoomable=&quot;yes&quot;}

   Puoi creare tipi di pubblico e limitare o indirizzare la visualizzazione del contenuto utilizzando questi parametri in [!DNL Target].
