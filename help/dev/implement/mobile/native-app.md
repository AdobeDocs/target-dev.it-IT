---
keywords: app mobile,sdk aep,app nativa,visualizzazioni web,nativa;swift,sdk mobile adobe experience platform,sdk mobile,codice nativo
description: Scopri come implementare [!DNL Adobe Target] con [!DNL AEP Mobile SDK] in un'app nativa con visualizzazioni web.
title: Implementare [!DNL Adobe Target] in un'app mobile che utilizza codice nativo con visualizzazioni Web
feature: Implement Mobile
role: Developer
exl-id: 3dd2e1d7-c744-4ba8-aaa4-6c2fe64d01fa
source-git-commit: 50ee7e66e30c0f8367763a63b6fde5977d30cfe7
workflow-type: tm+mt
source-wordcount: '561'
ht-degree: 0%

---

# Implementa [!DNL Target] con [!DNL AEP Mobile SDK] in un&#39;app nativa con visualizzazioni Web

Questo articolo condivide le best practice per l&#39;implementazione di [!DNL Adobe Target] in un&#39;app mobile che utilizza codice nativo con visualizzazioni Web tramite [!DNL Adobe Experience Platform Mobile SDK].

Questo articolo utilizza un&#39;app iOS di esempio che utilizza l&#39;integrazione [[!DNL Adobe Experience Platform Mobile SDK]](https://developer.adobe.com/client-sdks/documentation/getting-started/){target=_blank} e [!DNL Target] scritta in [Swift dall&#39;archivio GitHub](https://github.com/adobe/aep-sdk-app/){target=_blank}.

Nel mondo reale, è probabile che l’app aziendale utilizzi le visualizzazioni web nell’app mobile. Una visualizzazione web è un contenitore che carica una pagina web utilizzando un URL. Il contenitore è simile a una finestra del browser senza controlli. In iOS, il contenitore per la visualizzazione web funziona come un browser Safari durante l’elaborazione di pagine web.

## Prerequisiti

Per iniziare a utilizzare [!DNL Adobe Experience Platform Mobile SDK], è necessario eseguire alcune attività preliminari.

Per ulteriori informazioni, vedere [Adobe Target](https://developer.adobe.com/client-sdks/documentation/adobe-target/){target=_blank} nella documentazione di [[!DNL Adobe Experience Platform Mobile SDK]](https://developer.adobe.com/client-sdks/documentation/){target=_blank}.

## Sincronizzare il codice nativo con le visualizzazioni web

Il problema quando si implementa [!DNL Target] in un&#39;app nativa con visualizzazioni Web è che [!DNL Adobe Experience Platform Mobile SDK] ha già generato tutti gli identificatori necessari per il corretto funzionamento delle soluzioni [!DNL Adobe]. Tuttavia, gli identificatori non sono ancora visibili alle visualizzazioni web perché non si trovano nell’ambiente nativo della piattaforma. Pertanto, devi creare un ponte per trasmettere alcuni identificatori SDK alle visualizzazioni web in modo che l’identità del visitatore persista nell’ambiente web. In caso contrario, si verificheranno visite duplicate e questo influirà sulla generazione dei rapporti.

Fortunatamente, [!DNL Adobe Experience Platform Mobile SDK] fornisce un metodo pratico per generare [!DNL Adobe] parametri necessari affinché le visualizzazioni web possano utilizzare e mantenere per lo stesso visitatore, come illustrato nel seguente codice di esempio:

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

Per ulteriori informazioni sul metodo `Identity.appendTo` e per visualizzare un esempio di come utilizzare il metodo, vedere [Swift > Esempio](https://developer.adobe.com/client-sdks/documentation/mobile-core/identity/tabs/api-reference/){target=_blank} nella *documentazione SDK per dispositivi mobili*.

Utilizzando `Identity.appendTo`, questo URL:

```
https://vadymus.github.io/ateng/at-order-confirmation/index.html?a=1&b=2
```

si trasforma in:

```
https://vadymus.github.io/ateng/at-order-confirmation/index.html?a=1&b=2&adobe_mc=TS%3D1660667205%7CMCMID%3D69624092487065093697422606480535692677%7CMCORGID%3DEB9CAE8B56E003697F000101%40AdobeOrg
```

Come puoi vedere, è stato aggiunto il parametro `adobe_mc` all&#39;URL. Questo parametro contiene valori codificati per:

* TS=1660667205: il timestamp corrente. Questo timestamp assicura che la visualizzazione web non riceva valori scaduti.
* MCMID=69624092487065093697422606480535692677: [!UICONTROL Experience Cloud ID] (ECID). Anche noto come MID o [!UICONTROL Marketing Cloud ID] richiesto per l&#39;identificazione di [!DNL Adobe] visitatori tra soluzioni.
* MCORGID=EB9CAE8B56E003697F000101@AdobeOrg: [!UICONTROL Adobe Organization ID].

`Identity.getUrlVariables` è un metodo [!DNL Adobe Experience Platform Mobile SDK] alternativo che restituisce una stringa con formato appropriato contenente le variabili URL [!DNL Experience Cloud Identity Service]. Per ulteriori informazioni, vedere [getUrlVariables](https://developer.adobe.com/client-sdks/documentation/mobile-core/identity/api-reference/#geturlvariables){target=_blank} nel *riferimento API identità*.

## Passa l&#39;ID sessione [!DNL Target] per l&#39;esperienza della stessa sessione

È necessario un ulteriore passaggio per consentire al percorso di utenti [!DNL Target] di funzionare senza problemi nelle visualizzazioni native e web. Questo passaggio include l&#39;estrazione e il passaggio dell&#39;ID sessione [!DNL Target] da [!DNL Adobe Experience Platform Mobile SDK] alle visualizzazioni Web dell&#39;app mobile.

`Target.getSessionId` estrae l&#39;ID sessione che può essere passato all&#39;URL della visualizzazione Web come parametro `mboxSession`:

```swift
Target.getSessionId { (id, err) in
    // read Target sessionId
}
```

## Eseguire il test nelle visualizzazioni web

I collegamenti di anteprima Web vengono generati nella pagina [!UICONTROL Activity detail] facendo clic sul collegamento [[!UICONTROL Adobe QA]](/help/dev/implement/mobile/target-mobile-preview.md) per visualizzare un popup per copiare ogni collegamento di anteprima esperienza, simile al seguente:

```
?at_preview_token=mhFIzJSF7JWb-RsnakpBqi_s83Sl64hZp928VWpkwvI&at_preview_index=1_1&at_preview_listed_activities_only=true
```

I collegamenti di anteprima Web contengono ulteriori `at_preview_index` e `at_preview_listed_activities_only` parametri. Copia questi parametri per creare collegamenti di anteprima facili da usare sui dispositivi mobili con parametri di collegamento web.

Ad esempio:

```
com.adobe.targetmobile://?at_preview_token=mhFIzJSF7JWb-RsnakpBqhBwj-TiIlZsRTx_1QQuiXLIJFdpSLeEZwKGPUyy57O_&at_preview_index=1_1&at_preview_listed_activities_only=true
```

Dopo aver aperto il collegamento in un browser iOS Safari, l&#39;app acquisisce l&#39;URL nella classe `AppDelegate` in modo simile all&#39;esempio seguente:

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
