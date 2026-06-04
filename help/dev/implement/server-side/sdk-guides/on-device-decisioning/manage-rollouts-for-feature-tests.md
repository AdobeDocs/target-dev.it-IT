---
title: Gestire i rollout per i test delle funzioni
description: Scopri come gestire i rollout per i test di funzionalità utilizzando [!UICONTROL decisioning sul dispositivo].
feature: APIs/SDKs
exl-id: caa91728-6ac0-4583-a594-0c8fe616342d
TQID: https://experienceleague.adobe.com/soG8leVV3R4Y4FSns5oIJ43oziIhtOb2zJ5bkFYxeo0
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 596
ht-degree: 1%

---

# Gestire i rollout per i test delle funzioni

## Riepilogo dei passaggi

1. Abilita [!UICONTROL decisioning sul dispositivo] per la tua organizzazione
1. Crea un&#39;attività [!UICONTROL Test A/B]
1. Definire la funzione e le impostazioni di rollout
1. Implementare ed eseguire il rendering della funzione nell’applicazione
1. Implementa il tracciamento degli eventi nell’applicazione
1. Attivare l’attività A/B
1. Regola rollout e allocazione del traffico in base alle esigenze

## &#x200B;1. Abilita [!UICONTROL decisioning sul dispositivo] per la tua organizzazione

L’abilitazione del decisioning sul dispositivo garantisce che un’attività A/B venga eseguita con latenza vicina allo zero. Per abilitare questa funzione, passa a **[!UICONTROL Amministrazione]** > **[!UICONTROL Implementazione]** > **[!UICONTROL Dettagli account]** in [!DNL Adobe Target] e abilita l&#39;opzione **[!UICONTROL Decisioning sul dispositivo]**.

![Alt immagine](assets/asset-odd-toggle.png)

>[!NOTE]
>
>Per abilitare o disabilitare l&#39;opzione [!UICONTROL Decisioning sul dispositivo], è necessario disporre del ruolo utente [utente](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/user-management.html?lang=it) amministratore o approvatore.

Dopo aver attivato l&#39;interruttore [!UICONTROL Decisioning sul dispositivo], [!DNL Adobe Target] inizia a generare *artefatti regola* per il client.

## &#x200B;2. Crea un&#39;attività [!UICONTROL Test A/B]

1. In [!DNL Adobe Target], passa alla pagina **[!UICONTROL Attività]**, quindi seleziona **[!UICONTROL Crea attività]** > **[!UICONTROL Test A/B]**.

   ![Alt immagine](assets/asset-ab.png)

1. Nella finestra modale **[!UICONTROL Crea attività test A/B]**, lascia selezionata l&#39;opzione predefinita **[!UICONTROL Web]** (1), seleziona **[!UICONTROL Modulo]** come Compositore esperienza (2), seleziona **[!UICONTROL Workspace predefinito]** con **[!UICONTROL Nessuna restrizione proprietà]** (3), quindi fai clic su **[!UICONTROL Avanti]** (4).

   ![Alt immagine](assets/asset-form.png)

## &#x200B;3. Definire la funzione e le impostazioni di rollout

Nel passaggio **[!UICONTROL Esperienze]** della creazione di attività, specifica un nome per l&#39;attività (1). Immetti il nome della posizione (2) all&#39;interno dell&#39;applicazione in cui desideri gestire i rollout per la funzione. Ad esempio, `ondevice-rollout` o `homepage-addtocart-rollout` sono nomi di posizione che indicano le destinazioni per la gestione dei rollout di funzionalità. Nell&#39;esempio seguente, `ondevice-rollout` è la posizione definita per l&#39;Esperienza A. Facoltativamente, puoi aggiungere perfezionamenti del pubblico (4) per limitare la qualifica all’attività.

![Alt immagine](assets/asset-location-rollout.png)

1. Nella sezione **[!UICONTROL Contenuto]** della stessa pagina, seleziona **[!UICONTROL Crea offerta JSON]** nel menu a discesa (1), come illustrato.

   ![Alt immagine](assets/asset-offer.png)

1. Nella casella di testo **[!UICONTROL Dati JSON]** visualizzata, immetti la variabile del flag di funzione per la funzione che intendi eseguire il rollout con questa attività nell&#39;Esperienza A (1), utilizzando un oggetto JSON valido (2).

   ![Alt immagine](assets/asset-json-a-rollout.png)

1. Fai clic su **[!UICONTROL Avanti]** (1) per passare al passaggio **[!UICONTROL Targeting]** della creazione di attività.

   ![Alt immagine](assets/asset-next-2-t-rollout.png)

1. Nel passaggio **[!UICONTROL Targeting]**, mantieni il pubblico **[!UICONTROL Tutti i visitatori]** (1), per semplicità. Ma regola l&#39;allocazione del traffico (2) al 10%. In questo modo la funzione sarà limitata al 10% dei visitatori del sito. Fai clic su Avanti (3) per passare al passaggio **[!UICONTROL Obiettivi e impostazioni]**.

   ![Alt immagine](assets/asset-next-2-g-rollout.png)

1. Nel passaggio **[!UICONTROL Obiettivi e impostazioni]**, scegli **[!UICONTROL Adobe Target]** (1) come **[!UICONTROL Source per la generazione di rapporti]** per visualizzare i risultati dell&#39;attività nell&#39;interfaccia utente [!DNL Adobe Target].

1. Scegli una **[!UICONTROL metrica obiettivo]** per misurare l&#39;attività. In questo esempio, una conversione corretta si basa sull&#39;acquisto o meno di un articolo da parte dell&#39;utente, come indicato dal fatto che l&#39;utente abbia raggiunto o meno la posizione orderConfirm (2).

1. Fai clic su **[!UICONTROL Salva e chiudi]** (3) per salvare l&#39;attività.

   ![Alt immagine](assets/asset-conv-rollout.png)

## &#x200B;4. Implementare ed eseguire il rendering della funzione nell’applicazione

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
targetClient.getAttributes(["ondevice-rollout"]).then(function(attributes) {
      const featureFlags = attributes.asObject("ondevice-rollout");

      // Your flag variables are now available in the featureFlags object variable.
      //If you failed to qualify for the Activity, you will have an empty object.
      console.log(featureFlags);
    });
```

>[!TAB Java]

```java {line-numbers="true"}
    Attributes attrs = targetJavaClient.getAttributes(targetDeliveryRequest, "ondevice-rollout");
    Map<String, Object> featureFlags = attrs.toMboxMap("ondevice-rollout");
​
    // Your flag variables are now available in the featureFlags object variable.
    //If you failed to qualify for the Activity, you will have an empty object.
    System.out.println(featureFlags);
```

>[!ENDTABS]

## &#x200B;5. Implementa il tracciamento degli eventi nell’applicazione

Dopo aver reso disponibile nell’applicazione la variabile flag di funzione, puoi utilizzarla per abilitare qualsiasi funzione che fa già parte dell’applicazione. Se un visitatore non è idoneo per l’attività, significa che non è stato incluso nel bucket del 10% definito come pubblico.

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
//... Code removed for brevity

if(featureFlags.enable == "yes") { //Fell within 10% traffic
    console.log("Render Feature");
}
else {
    console.log("Disable Feature");
}

// alternatively, the getValue method could be used on the Attributes object.

if(attributes.getValue("ondevice-rollout", "enable") === "yes") { //Fell within 10% traffic
    console.log("Render Feature");
}
else {
    console.log("Disable Feature");
}
```

>[!TAB Java]

```java {line-numbers="true"}
//... Code removed for brevity
​
if("yes".equals(String.valueOf(featureFlags.get("enable")))) { //Fell within 10% traffic
    System.out.println("Render Feature");
}
else {
    System.out.println("Disable Feature");
}
​
// alternatively, the getString method could be used on the Attributes object.
​
if("yes".equals(attrs.getString("ondevice-rollout", "enable"))) { //Fell within 10% traffic
    System.out.println("Render Feature");
}
else {
    System.out.println("Disable Feature");
}
```

>[!ENDTABS]

## &#x200B;6. Attivare l’attività di rollout

![Alt immagine](assets/asset-activate-rollout.png)

## &#x200B;7. Regola rollout e allocazione del traffico in base alle esigenze

Dopo aver attivato l’attività, modificala in qualsiasi momento per aumentare o ridurre l’allocazione del traffico in base alle esigenze.

Aumento dell’allocazione del traffico dal 10% al 50% a causa del successo del rollout iniziale.

![Alt immagine](assets/asset-adjust-rollout.png)
