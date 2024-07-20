---
title: Come utilizzare le richieste asincrone nell'SDK Java  [!DNL Adobe Target]
description: Scopri come l'SDK di Java  [!DNL Target]  supporta le richieste asincrone, riducendo a zero il tempo di destinazione effettivo.
feature: APIs/SDKs
exl-id: e11f8d16-76f6-4d39-822a-34a1cf7f623f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 3%

---

# Richieste asincrone (Java)

## Descrizione

Uno dei vantaggi dell&#39;integrazione lato server è la possibilità di sfruttare l&#39;enorme larghezza di banda e le risorse informatiche disponibili sul lato server utilizzando il parallelismo. L&#39;SDK Java [!DNL Target] supporta le richieste asincrone, che possono ridurre il tempo di destinazione effettivo a zero.

## Metodi supportati

### Metodi

```javascript {line-numbers="true"}
CompletableFuture<TargetDeliveryResponse> getOffersAsync(TargetDeliveryRequest request);
CompletableFuture<ResponseStatus> sendNotificationsAsync(TargetDeliveryRequest request);
CompletableFuture<Attributes> getAttributesAsync(TargetDeliveryRequest targetRequest, String ...mboxes);
```

## Esempio

Un esempio di controller applicazione `Spring` potrebbe essere simile al seguente:

### Controller di esempio

```javascript {line-numbers="true"}
@RestController
public class TargetRestController {

    @Autowired
    private TargetClient targetJavaClient;

    @GetMapping("/mboxTargetOnlyAsync")
        public TargetDeliveryResponse mboxTargetOnlyAsync(
                @RequestParam(name = "mbox", defaultValue = "server-side-mbox") String mbox,
                HttpServletRequest request, HttpServletResponse response) {
            ExecuteRequest executeRequest = new ExecuteRequest()
                    .mboxes(getMboxRequests(mbox));

            TargetDeliveryRequest targetDeliveryRequest = TargetDeliveryRequest.builder()
                    .context(getContext(request))
                    .execute(executeRequest)
                    .cookies(getTargetCookies(request.getCookies()))
                    .build();
            CompletableFuture<TargetDeliveryResponse> targetResponseAsync =
                    targetJavaClient.getOffersAsync(targetDeliveryRequest);
            targetResponseAsync.thenAccept(tr -> setCookies(tr.getCookies(), response));
            simulateIO();
            TargetDeliveryResponse targetResponse = targetResponseAsync.join();
            return targetResponse;
        }

    /**
     * Function for simulating network calls like other microservices and database calls
     */
    private void simulateIO() {
        try {
            Thread.sleep(200L);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

}
```

L&#39;esempio presuppone che [abbiate inizializzato l&#39;SDK](initialize-sdk.md) come molla e che abbiate [metodi di utilità](utility-methods.md) disponibili.

La richiesta [!DNL Target] viene attivata prima di `simulateIO` e al momento dell&#39;esecuzione anche il risultato di destinazione dovrebbe essere pronto. Anche se non lo è, nella maggior parte dei casi si otterranno risparmi significativi.
