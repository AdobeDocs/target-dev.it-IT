---
keywords: app mobile, posizione di app mobile, target app mobile, posizioni di destinazione mobile, metriche di successo di app mobile
description: Visualizza il codice di esempio per scoprire come creare posizioni e metriche di successo nelle app iOS, in modo da poter utilizzare  [!DNL Adobe Target]  per personalizzare e ottimizzare l'app.
title: Come posso creare  [!DNL Target] posizioni e metriche di successo in un'app iOS?
feature: Implement Mobile
exl-id: 755c8b26-5c60-48fc-9e7e-5e97a25edb78
TQID: https://experienceleague.adobe.com/frolzqCgdL0iz5Z3E8OaJmP6yiVq7jEYiWn6LD4bocA
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 469
ht-degree: 63%

---

# iOS - crea una posizione [!DNL Target] e una metrica di successo

Per utilizzare [!DNL Target] nell&#39;app mobile, crea una posizione e una metrica di successo.

>[!IMPORTANT]
>
>Il supporto per gli SDK [!DNL Adobe Mobile] versione 4.*x* è terminato il 31 agosto 2021 e non è più consigliato per gli utenti di dispositivi mobili [!DNL Adobe Target].
>
>[Adobe Experience Platform SDK for Mobile Apps](https://developer.adobe.com/client-sdks/documentation/){target=_blank} è la soluzione consigliata per alimentare le soluzioni e i servizi [!DNL Adobe Experience Cloud] nelle app mobili.

Questa sezione include il codice di esempio che può essere utilizzato come modello per l&#39;app. Gli esempi in questa sezione contengono il codice per iOS. Gli stessi modelli si applicano ad Android. La sintassi specifica per Android si trova nella guida [Android SDK 4.x per soluzioni Experience Cloud](https://experienceleague.adobe.com/docs/mobile-services/android/target-android/target-main.html).

>[!NOTE]
>
>Per un elenco di tutti i metodi [!DNL Target] disponibili, consulta la [documentazione di Mobile](https://experienceleague.adobe.com/docs/mobile-services/ios/target-ios/c-target-methods.html).

Per creare una posizione [!DNL Target] nell&#39;app e effettuare una richiesta, esistono due metodi principali:

* `targetCreateRequestWithName`
* `targetLoadRequest`

1. Crea un percorso [!DNL Target].

   Ecco una chiamata di esempio per creare una richiesta:

   ```
   // make your request 
   ADBTargetLocationRequest *myRequest = [ADBMobile targetCreateRequestWithName:@"heroBanner" 
                                                    defaultContent:@"default.png" 
                                                    parameters:nil];
   ```

   | Parametro | Descrizione |
   |---|---|
   | `ADBTargetLocationRequest *myRequest` | Sostituisci `myRequest` con il nome della tua `targetLocation` nell’app. |
   | `targetCreateRequestWithName:@"heroBanner"` | Sostituisci `heroBanner` con il nome della `targetLocation` in Target. È lo stesso del nome mbox. Il banner hero viene visualizzato nell&#39;interfaccia di Target. |
   | `defaultContent:@"default.png"` | Sostituisci `default.png` con il valore utilizzato dall&#39;app nel caso in cui Target non risponda. |
   | `parameters:nil` | Specifica il profilo o i parametri mbox. Per ulteriori informazioni, consulta la sezione “passaggio di dati personalizzati”. |

   Ecco una chiamata di esempio per caricare la richiesta:

   ```
   // load your request 
   [ADBMobile targetLoadRequest:myRequest callback:^(NSString *content) { 
                                        // do something with content 
                                        heroImage.image = [UIImage imageNamed:content]; 
   }];
   ```

   | Parametro | Descrizione |
   |---|---|
   | `targetLoadRequest:myRequest` | Sostituisci `myRequest` con il nome della tua `targetLocation` nell’app. |
   | `NSString *content` | Sostituisci contenuto della richiesta con il contenuto effettivo che ritorna da Adobe. La stringa può essere XML, JSON o semplice. Utilizza questa sezione del codice per definire le variabili, impostare i percorsi delle immagini, visualizzare i flussi di regolazione, i punti di transazione o qualsiasi altra operazione che desideri eseguire. Target restituirà il contenuto immesso nell&#39;interfaccia utente nello stesso formato. |
   | `heroImage.image = [UIImage imageNamed:content];` | Ad esempio: seleziona il contenuto e imposta il percorso per un&#39;immagine hero. |

1. Creare una metrica di successo.

   Il metodo `targetCreateOrderConfirmRequestWithName` può essere utilizzato per monitorare una metrica di conversione/successo nell&#39;app.

   ```
   ADBTargetLocationRequest *req = [ADBMobile targetCreateOrderConfirmRequestWithName: "orderConfirm" 
                                              orderId: orderId 
                                              orderTotal: @"39.95" 
                                              productPurchasedId: _galleryItem.title 
                                              parameters: nil]; 
   [ADBMobile targetLoadRequest: req callback: nil];
   ```

   | Parametro | Descrizione |
   |---|---|
   | `orderId` | Sostituisci con una variabile dinamica che rappresenta un ID ordine univoco. |
   | `@"39.95"` | Sostituisci con una variabile dinamica che rappresenta un totale di ordine univoco. |
   | `_galleryItem.title` | Sostituisci con una variabile dinamica che rappresenta un elenco delimitato da virgole dei prodotti acquistati. |
   | `parameters: nil` | Dizionario facoltativo di parametri addizionali. |

1. Genera l’app.

   Risultato passaggio: dopo aver creato correttamente una posizione di destinazione e aver applicato i tag a una metrica di successo, crea un test A/B. L&#39;attività può essere creata utilizzando il Compositore esperienza basato su moduli.
