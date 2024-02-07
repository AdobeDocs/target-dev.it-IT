---
title: Integrazione con i segmenti AAM Experience Cloud
description: Integrazione con Experience Cloud, integrazione Audienci Manager
keywords: api di consegna, lato server, lato server, integrazione, audience manager, aam
exl-id: c21e0200-23ba-4a0b-adf4-38e03c087f00
feature: Implement Server-side
source-git-commit: e3f14e97fa48ffb1f07b29aca5711d16e75faa80
workflow-type: tm+mt
source-wordcount: '417'
ht-degree: 4%

---

# Segmenti AAM

[!DNL Adobe Audience Manager] i segmenti possono essere utilizzati tramite [!DNL Adobe Target] SDK. Per sfruttare i segmenti AAM, è necessario fornire i seguenti campi:

>[!NOTE]
>
>I segmenti AAM non sono supportati per le attività di decisione su dispositivo.

| Nome campo | Obbligatorio | Descrizione |
| --- | --- | --- |
| `locationHint` | Sì | DCS Location Hint viene utilizzato per determinare quale endpoint DCS AAM colpire per recuperare il profilo. Deve essere >= 1. |
| `marketingCloudVisitorId` | Sì | ID visitatore di Marketing Cloud |
| `blob` | Sì | Il BLOB dell’AAM viene utilizzato per inviare dati aggiuntivi all’AAM. Non deve essere vuoto e le dimensioni devono essere &lt;= 1024. |

L’SDK compilerà automaticamente questi campi quando crei un `getOffers` chiamata al metodo, ma dovrai verificare che sia fornito un cookie visitatore valido. Per ottenere questo cookie, devi implementare VisitorAPI.js nel browser.

## Guida all’implementazione

### Utilizzo dei cookie

I cookie vengono utilizzati per correlare [!DNL Adobe Audience Manager] richieste con [!DNL Adobe Target] richieste. Si tratta dei cookie utilizzati in questa implementazione.

| Cookie | Nome | Descrizione |
| --- | --- | --- |
| cookie visitatore | `AMCVS_XXXXXXXXXXXXXXXXXXXXXXXX%40AdobeOrg` | Questo cookie è impostato da `VisitorAPI.js` quando viene inizializzato con `visitorState` dal target `getOffers` risposta. |
| cookie di destinazione | `mbox` | Il server web deve impostare questo cookie utilizzando il nome e il valore di `targetCookie` dal target `getOffers` risposta. |

### Panoramica dei passaggi

Supponiamo che un utente inserisca un URL in un browser che invia una richiesta al server web. Nell’evadere tale richiesta:

1. Il server legge i cookie del visitatore e di destinazione dalla richiesta.
1. Il server effettua una chiamata al `getOffers` metodo del [!DNL Target] SDK, specificando i cookie visitatore e target, se disponibili.
1. Quando `getOffers` chiamata completata, valori per `targetCookie` e `visitorState` dalla risposta.
   1. Nella risposta viene impostato un cookie con valori ricavati da `targetCookie`. Questa operazione viene eseguita utilizzando `Set-Cookie` intestazione di risposta, che indica al browser di rendere persistente il cookie di target.
   1. Viene preparata una risposta di HTML che inizializza `VisitorAPI.js` e passa `visitorState` dalla risposta del target.
1. La risposta del HTML viene caricata nel browser...
   1. `VisitorAPI.js` è incluso nell&#39;intestazione del documento.
   1. VisitorAPI è inizializzato con `visitorState` dal `getOffers` Risposta SDK. In questo modo il cookie visitatore verrà impostato nel browser e quindi inviato al server per le richieste successive.

### Esempio di codice

Il codice di esempio seguente implementa ciascuno dei passaggi descritti in precedenza. Ogni passaggio viene visualizzato nel codice come commento in linea accanto alla relativa implementazione.

#### Node.js

Questo esempio si basa su [express, framework web Node.js](https://expressjs.com/).

>[!BEGINTABS]

>[!TAB server.js]

```js {line-numbers="true"}
const fs = require("fs");
const express = require("express");
const cookieParser = require("cookie-parser");
const Handlebars = require("handlebars");
const TargetClient = require("@adobe/target-nodejs-sdk");

const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg",
  timeout: 10000,
  logger: console,
};
const targetClient = TargetClient.create(CONFIG);
const TEMPLATE = fs.readFileSync(`${__dirname}/index.handlebars`).toString();
const handlebarsTemplate = Handlebars.compile(TEMPLATE);

Handlebars.registerHelper("toJSON", function (object) {
  return new Handlebars.SafeString(JSON.stringify(object, null, 4));
});

const app = express();
app.use(cookieParser());
app.use(express.static(__dirname + "/public"));

app.get("/", async (req, res) => {
  // The server reads the visitor and target cookies from the request.
  const visitorCookie =
    req.cookies[
      encodeURIComponent(
        TargetClient.getVisitorCookieName(CONFIG.organizationId)
      )
    ];
  const targetCookie = req.cookies[TargetClient.TargetCookieName];
  const address = { url: req.headers.host + req.originalUrl };

  const targetRequest = {
    execute: {
      mboxes: [
        { name: "homepage", index: 1, address },
        { name: "SummerShoesOffer", index: 2, address },
        { name: "SummerDressOffer", index: 3, address }
      ],
    },
  };

  res.set({
    "Content-Type": "text/html",
    Expires: new Date().toUTCString(),
  });

  try {
    // The server makes a call to the `getOffers` method of the Target SDK specifying the visitor and target cookies if available.
    const targetResponse = await targetClient.getOffers({
      request: targetRequest,
      visitorCookie,
      targetCookie,
    });

    // When the `getOffers` call is fulfilled, values for `targetCookie` and `visitorState` from the response are used.
    // A cookie is set on the response with values taken from `targetCookie`.  This is done using the `Set-Cookie` response header which tells the browser to persist the target cookie.
    res.cookie(
      targetResponse.targetCookie.name,
      targetResponse.targetCookie.value,
      { maxAge: targetResponse.targetCookie.maxAge * 1000 }
    );

    // An HTML response is prepared that initializes VisitorAPI.js and passes in `visitorState` from the target response.
    const html = handlebarsTemplate({
      organizationId: CONFIG.organizationId,
      targetResponse,
    });

    res.status(200).send(html);
  } catch (error) {
    console.error("Target:", error);
    res.status(500).send(error);
  }
});

app.listen(3000, function () {
  console.log("Listening on port 3000 and watching!");
});
```

>[!TAB index.handlebars]

```html {line-numbers="true"}
<!doctype html>
<html>
<head>
  <meta charset="UTF-8">
  <title>ECID (Visitor API) Integration Sample</title>

  <!-- VisitorAPI.js is included in the document header. -->
  <script src="VisitorAPI.js"></script>
  <script>
    // VisitorAPI is initialized with visitorState from the `getOffers` SDK response. This will cause the visitor cookie to be set in the browser so it will be sent to the server on subsequent requests.
    Visitor.getInstance("{{organizationId}}", {serverState: {{toJSON targetResponse.visitorState}} });
  </script>
</head>
<body>
  <h1>response</h1>
  <pre>{{toJSON targetResponse}}</pre>
</body>
</html>
```

>[!ENDTABS]

#### Java

In questo esempio si utilizza [Spring, un framework web Java](https://spring.io/).

>[!BEGINTABS]

>[!TAB ClientSampleApplication.java]

```java {line-numbers="true"}
@SpringBootApplication
public class ClientSampleApplication {

    public static void main(String[] args) {
        System.setProperty(SimpleLogger.DEFAULT_LOG_LEVEL_KEY, "DEBUG");
        SpringApplication.run(ClientSampleApplication.class, args);
    }

    @Bean
    TargetClient marketingCloudClient() {
        ClientConfig clientConfig = ClientConfig.builder()
                .client("acmeclient")
                .organizationId("1234567890@AdobeOrg")
                .defaultDecisioningMethod(DecisioningMethod.SERVER_SIDE)
                .build();

        return TargetClient.create(clientConfig);
    }
}
```

>[!TAB TargetController.java]

```java {line-numbers="true"}
@Controller
@RequestMapping("/")
public class TargetController {

    @Autowired
    private TargetClientService targetClientService;

    @GetMapping
    public String index(Model model, HttpServletRequest request, HttpServletResponse response) {
        // The server reads the visitor and target cookies from the request.
        List<TargetCookie> targetCookies = getTargetCookies(request.getCookies());

        Address address = getAddress(request);

        List<MboxRequest> mboxRequests = new ArrayList<>();
        mboxRequests.add((MboxRequest) new MboxRequest().name("homepage").index(1).address(address));
        mboxRequests.add((MboxRequest) new MboxRequest().name("SummerShoesOffer").index(2).address(address));
        mboxRequests.add((MboxRequest) new MboxRequest().name("SummerDressOffer").index(3).address(address));

        TargetDeliveryResponse targetDeliveryResponse = targetClientService.getOffers(mboxRequests, targetCookies, request,
                response);

        // An HTML response is prepared that initializes VisitorAPI.js and passes in `visitorState` from the target response.
        model.addAttribute("visitorState", targetDeliveryResponse.getVisitorState());
        model.addAttribute("targetResponse", targetDeliveryResponse);
        model.addAttribute("organizationId", "1234567890@AdobeOrg");

        return "index";
    }
}
```

>[!TAB TargetClientService.java]

```java {line-numbers="true"}
@Service
public class TargetClientService {

    private final TargetClient targetJavaClient;

    public TargetClientService(TargetClient targetJavaClient) {
        this.targetJavaClient = targetJavaClient;
    }

    public TargetDeliveryResponse getOffers(List<MboxRequest> executeMboxes, List<TargetCookie> cookies, HttpServletRequest request, HttpServletResponse response) {

        Context context = getContext(request);
        ExecuteRequest executeRequest = new ExecuteRequest();
        executeRequest.setMboxes(executeMboxes);

        TargetDeliveryRequest targetDeliveryRequest = TargetDeliveryRequest.builder()
                .context(context)
                .execute(executeRequest)
                .cookies(cookies)
                .build();

        // The server makes a call to the `getOffers` method of the Target SDK specifying the visitor and target cookies if available.
        TargetDeliveryResponse targetResponse = targetJavaClient.getOffers(targetDeliveryRequest);

        // When the `getOffers` call is fulfilled, values for `targetCookie` and `visitorState` from the response are used.
        // A cookie is set on the response with values taken from `targetCookie`.  This is done using the `Set-Cookie` response header which tells the browser to persist the target cookie.
        setCookies(targetResponse.getCookies(), response);
        return targetResponse;
    }
}
```

>[!TAB TargetRequestUtils.java]

```java {line-numbers="true"}
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Target Only : GetOffer</title>

    <!-- VisitorAPI.js is included in the document header. -->
    <script src="../../js/VisitorAPI.js"></script>
    <script th:inline="javascript">
        // VisitorAPI is initialized with visitorState from the `getOffers` SDK response. This will cause the visitor cookie to be set in the browser so it will be sent to the server on subsequent requests.
        Visitor.getInstance(/*[[${organizationId}]]*/ "", {serverState: /*[[${visitorState}]]*/ {}});
    </script>
</head>
<body>
    <h1>response</h1>
    <pre>[[${targetResponse}]]</pre>
</body>
</html>
```

>[!ENDTABS]

Per ulteriori informazioni su `TargetRequestUtils.java`, vedi [Metodi di utilità (Java)](https://experienceleague.adobe.com/docs/target-dev/developer/server-side/java/utility-methods.html){target=_blank}
