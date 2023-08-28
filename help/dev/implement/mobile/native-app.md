---
keywords: app mobile,sdk aep,app nativa,visualizzazioni web,nativa;swift,sdk mobile adobe experience platform,sdk mobile,codice nativo
description: Scopri come implementare [!DNL Adobe Target] con [!DNL AEP Mobile SDK] in un’app nativa con visualizzazioni web.
title: Implementare [!DNL Adobe Target] in un’app mobile che utilizza codice nativo con visualizzazioni web
feature: Implement Mobile
role: Developer
source-git-commit: c0fda36cb5472d71438c47b8b484716003da4214
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 0%

---


# Implementare [!DNL Target] con [!DNL AEP Mobile SDK] in un’app nativa con visualizzazioni web

Questo articolo condivide le best practice per l’implementazione di [!DNL Adobe Target] in un’app mobile che utilizza codice nativo con visualizzazioni web utilizzando [!DNL Adobe Experience Platform Mobile SDK].

Questo articolo utilizza un&#39;app iOS di esempio che utilizza [[!DNL Adobe Experience Platform Mobile SDK]](https://developer.adobe.com/client-sdks/documentation/getting-started/){target=_blank} and a [!DNL Target] integration written in [Swift from the GitHub repository](https://github.com/adobe/aep-sdk-app/){target=_blank}.

Nel mondo reale, è probabile che l’app aziendale utilizzi le visualizzazioni web nell’app mobile. Una visualizzazione web è un contenitore che carica una pagina web utilizzando un URL. Il contenitore è simile a una finestra del browser senza controlli. In iOS, il contenitore per la visualizzazione web funziona come un browser Safari durante l’elaborazione di pagine web.

## Prerequisiti

Per iniziare a utilizzare [!DNL Adobe Experience Platform Mobile SDK], è necessario eseguire alcune attività preliminari.

Per ulteriori informazioni, consulta [Adobe Target](https://developer.adobe.com/client-sdks/documentation/adobe-target/){target=_blank} in the [[!DNL Adobe Experience Platform Mobile SDK]](https://developer.adobe.com/client-sdks/documentation/){target=_blank} documentazione.

## Sincronizzare il codice nativo con le visualizzazioni web

La sfida nell&#39;implementazione [!DNL Target] in un’app nativa con visualizzazioni web è che il [!DNL Adobe Experience Platform Mobile SDK] ha già generato tutti gli identificatori necessari richiesti per [!DNL Adobe] soluzioni per lavorare senza problemi. Tuttavia, gli identificatori non sono ancora visibili alle visualizzazioni web perché non si trovano nell’ambiente nativo della piattaforma. Pertanto, devi creare un ponte per trasmettere alcuni identificatori SDK alle visualizzazioni web in modo che l’identità del visitatore persista nell’ambiente web. In caso contrario, si verificheranno visite duplicate e questo influirà sulla generazione dei rapporti.

Fortunatamente, il [!DNL Adobe Experience Platform Mobile SDK] fornisce un metodo pratico per generare [!DNL Adobe] parametri necessari affinché le visualizzazioni web utilizzino e persistano per lo stesso visitatore, come mostrato nel seguente codice di esempio:

```swift
Identity.appendTo(url: URL(string: url), completion: {appendedURL, error in
  print("appendedURL \(String(describing: appendedURL))")
  // load the url with ECID on the main thread
  DispatchQueue.main.async {
    let request = NSMutableURLRequest(url: appendedURL!)
    self.webView.load(request as URLRequest)
  }
});
```

Per ulteriori informazioni su `Identity.appendTo` e per visualizzare un esempio di come utilizzare il metodo, vedere [Swift > Esempio](https://developer.adobe.com/client-sdks/documentation/mobile-core/identity/tabs/api-reference/){target=_blank} nel *Documentazione di Mobile SDK*.

Utilizzo di `Identity.appendTo`, questo URL:

```
https://vadymus.github.io/ateng/at-order-confirmation/index.html?a=1&b=2
```

si trasforma in:

```
https://vadymus.github.io/ateng/at-order-confirmation/index.html?a=1&b=2&adobe_mc=TS%3D1660667205%7CMCMID%3D69624092487065093697422606480535692677%7CMCORGID%3DEB9CAE8B56E003697F000101%40AdobeOrg
```

Come potete vedere, c&#39;è `adobe_mc` Parametro aggiunto all&#39;URL. Questo parametro contiene valori codificati per:

* TS=1660667205: il timestamp corrente. Questo timestamp assicura che la visualizzazione web non riceva valori scaduti.
* MCMID=69624092487065093697422606480535692677: Il [!UICONTROL ID EXPERIENCE CLOUD] (ECID) Noto anche come MID o [!UICONTROL ID MARKETING CLOUD] obbligatorio per [!DNL Adobe] identificazione dei visitatori tra soluzioni.
* MCORGID=EB9CAE8B56E003697F000101@AdobeOrg: La [!UICONTROL ID organizzazione Adobe].

Il `Identity.getUrlVariables` è un’alternativa [!DNL Adobe Experience Platform Mobile SDK] metodo che restituisce una stringa con formato appropriato che contiene [!DNL Experience Cloud Identity Service] Variabili URL. Per ulteriori informazioni, consulta [getUrlVariables](https://developer.adobe.com/client-sdks/documentation/mobile-core/identity/api-reference/#geturlvariables){target=_blank} nel *Riferimento API di identità*.

## Passa il [!DNL Target] ID sessione per esperienza nella stessa sessione

È necessario un ulteriore passaggio per rendere [!DNL Target] il percorso di utenti funziona perfettamente tra le visualizzazioni native e web. Questo passaggio include l’estrazione e il passaggio di [!DNL Target] ID sessione da [!DNL Adobe Experience Platform Mobile SDK] alle visualizzazioni web dell’app mobile.

Il `Target.getSessionId` estrae l’ID sessione che può essere passato all’URL della visualizzazione web come `mboxSession` parametro:

```swift
Target.getSessionId { (id, err) in
    // read Target sessionId
}
```

## Eseguire il test nelle visualizzazioni web

I collegamenti di anteprima web vengono generati in [!UICONTROL Dettagli attività] pagina facendo clic sul pulsante [[!UICONTROL CONTROLLO DI QUALITÀ ADOBE] link](/help/dev/implement/mobile/target-mobile-preview.md) per visualizzare un pop-up per copiare ogni collegamento di anteprima esperienza, simile al seguente:

```
?at_preview_token=mhFIzJSF7JWb-RsnakpBqi_s83Sl64hZp928VWpkwvI&at_preview_index=1_1&at_preview_listed_activities_only=true
```

I collegamenti di anteprima web contengono elementi aggiuntivi `at_preview_index` e `at_preview_listed_activities_only` parametri. Copia questi parametri per creare collegamenti di anteprima facili da usare sui dispositivi mobili con parametri di collegamento web.

Ad esempio:

```
com.adobe.targetmobile://?at_preview_token=mhFIzJSF7JWb-RsnakpBqhBwj-TiIlZsRTx_1QQuiXLIJFdpSLeEZwKGPUyy57O_&at_preview_index=1_1&at_preview_listed_activities_only=true
```

Dopo aver aperto il collegamento in un browser iOS Safari, l’app acquisisce l’URL nel `AppDelegate` classe simile all&#39;esempio seguente:

```swift
func application(_ app: UIApplication, open url: URL, options: [UIApplicationOpenURLOptionsKey : Any] = [:]) -> Bool {
  print("url= \(String(describing: url.absoluteString))")
  //...
```

Ora che hai acquisito tutti i parametri necessari nell’app, puoi passarli al web quando necessario:

```swift
Identity.appendTo(url: URL(string: url), completion: {appendedURL, error in
  let urlWithWebPreviewLink = appendedURL + "&" + myPreviewLinkFromAppDelegate
```

L’output finale per il collegamento di visualizzazione web potrebbe essere simile al seguente:

```
https://vadymus.github.io/ateng/at-order-confirmation/index.html?a=1&b=2&adobe_mc=TS%3D1660667205%7CMCMID%3D69624092487065093697422606480535692677%7CMCORGID%3DEB9CAE8B56E003697F000101%40AdobeOrg&at_preview_token=mhFIzJSF7JWb-RsnakpBqi_s83Sl64hZp928VWpkwvI&at_preview_index=1_1&at_preview_listed_activities_only=true
```
