---
title: Servizio ID Experience Cloud (ECID)
description: Anche se si utilizza [!DNL Target] SDK per il recupero del contenuto da [!DNL Target] può essere potente, il valore aggiunto dell’utilizzo di [!UICONTROL ID EXPERIENCE CLOUD] (ECID) per il tracciamento degli utenti va oltre l’Adobe [!DNL Target]. The ECID enables you to leverage [!DNL Adobe Experience Cloud] prodotti e funzionalità, come la generazione di rapporti A4T e [!DNL Adobe Audience Manager] (AAM).
exl-id: fd7e5c3e-51c1-4965-ab6a-f50a6b0c910b
feature: Implement Server-side
source-git-commit: 09a50aa67ccd5c687244a85caad24df56c0d78f5
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 1%

---

# [!UICONTROL ID EXPERIENCE CLOUD] Servizio (ECID)

## [!UICONTROL ID EXPERIENCE CLOUD] Integrazione (ECID)

Anche se si utilizza [!DNL Target] SDK per il recupero del contenuto da [!DNL Target] può essere potente, il valore aggiunto dell’utilizzo di [!UICONTROL ID EXPERIENCE CLOUD] (ECID) per il tracciamento degli utenti si estende oltre [!DNL Adobe Target]. ECID consente di sfruttare [!DNL Adobe Experience Cloud] prodotti e funzionalità, come la generazione di rapporti A4T e [!DNL Adobe Audience Manager] (AAM).

L’ECID viene generato e gestito da `visitor.js`, che mantiene il proprio stato. Il `visitor.js` crea un cookie denominato `AMCV_{organizationId}`, utilizzato da [!DNL Target] SDK per l’integrazione di ECID. Quando [!DNL Target] viene restituita la risposta, è necessario aggiornare l’istanza Visitatore sul lato client con `thevisitorState` restituito da [!DNL Target] SDK.

```html {line-numbers="true"}
<!doctype html>
<html>
<head>
  <meta charset="UTF-8">
  <title>ECID (Visitor API) Integration Sample</title>
  <script src="VisitorAPI.js"></script>
  <script>
    Visitor.getInstance("${organizationId}", {serverState: ${visitorState}});
  </script>
</head>
<body>
  <pre>Sample content</pre>
</body>
</html>
```

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
const express = require("express");
const cookieParser = require("cookie-parser");
const TargetClient = require("@adobe/target-nodejs-sdk");
const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};
const TEMPLATE = `
<!doctype html>
<html>
<head>
  <meta charset="UTF-8">
  <title>ECID (Visitor API) Integration Sample</title>
  <script src="VisitorAPI.js"></script>
  <script>
    Visitor.getInstance("${organizationId}", {serverState: ${visitorState}});
  </script>
</head>
<body>
  <pre>${content}</pre>
</body>
</html>
`;

const app = express();
const targetClient = TargetClient.create(CONFIG);

app.use(cookieParser());
// We assume that VisitorAPI.js is stored in "public" folder
app.use(express.static(__dirname + "/public"));

function saveCookie(res, cookie) {
  if (!cookie) {
    return;
  }

  res.cookie(cookie.name, cookie.value, {maxAge: cookie.maxAge * 1000});
}

const getResponseHeaders = () => ({
  "Content-Type": "text/html",
  "Expires": new Date().toUTCString()
});

function sendSuccessResponse(res, response) {
  res.set(getResponseHeaders());

  const result = TEMPLATE
  .replace("${organizationId}", CONFIG.organizationId)
  .replace("${visitorState}", JSON.stringify(response.visitorState))
  .replace("${content}", response);

  saveCookie(res, response.targetCookie);

  res.status(200).send(result);
}

function sendErrorResponse(res, error) {
  res.set(getResponseHeaders());
  res.status(500).send(error);
}

app.get("/abtest", async (req, res) => {
  const visitorCookie = req.cookies[TargetClient.getVisitorCookieName(CONFIG.organizationId)];
  const targetCookie = req.cookies[TargetClient.TargetCookieName];
  const request = {
      execute: {
        mboxes: [{
          address: { url: req.headers.host + req.originalUrl },
          name: "a1-serverside-ab"
        }]
      }};
```

>[!TAB Java]

```java {line-numbers="true"
  console.log("Request", request);

  try {
      const response = await targetClient.getOffers({ request, visitorCookie, targetCookie });
      sendSuccessResponse(res, response);
    } catch (error) {
      sendErrorResponse(res, error);
    }
});

app.listen(3000, function () {
  console.log("Listening on port 3000 and watching!");
});
@Controller
public class TargetControllerSample {

  @Autowired
  private TargetClient targetClient;

  @GetMapping("/")
  public String targetMcid(Model model, HttpServletRequest request, HttpServletResponse response) {
    Context context = getContext(request);
    TargetDeliveryRequest targetDeliveryRequest = TargetDeliveryRequest.builder()
        .context(context)
        .prefetch(getPrefetchRequest())
        .cookies(getTargetCookies(request.getCookies()))
        .build();

    TargetDeliveryResponse targetResponse = targetClient.getOffers(targetDeliveryRequest);
    model.addAttribute("visitorState", targetResponse.getVisitorState());
    model.addAttribute("organizationId", "0DD934B85278256B0A490D44@AdobeOrg");
    setCookies(targetResponse.getCookies(), response);
    return "targetMcid";
  }
}
```

>[!ENDTABS]

## ECID con integrazione ID cliente

Per tenere traccia degli account utente dei visitatori e dei dettagli sullo stato di accesso: `customerIds` può essere trasmesso tramite [!DNL Target] SDK.

```html {line-numbers="true"
<!doctype html>
<html>
<head>
  <meta charset="UTF-8">
  <title>ECID (Visitor API) Integration Sample</title>
  <script src="VisitorAPI.js"></script>
  <script>
    Visitor.getInstance("${organizationId}", {serverState: ${visitorState}});
  </script>
</head>
<body>
  <pre>Sample content</pre>
</body>
</html>
```

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
const express = require("express");
const cookieParser = require("cookie-parser");
const TargetClient = require("@adobe/target-nodejs-sdk");
const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};
const TEMPLATE = `
<!doctype html>
<html>
<head>
  <meta charset="UTF-8">
  <title>ECID (Visitor API) with Customer IDs Integration Sample</title>
  <script src="VisitorAPI.js"></script>
  <script>
    Visitor.getInstance("${organizationId}", {serverState: ${visitorState}});
  </script>
</head>
<body>
  <pre>${content}</pre>
</body>
</html>
`;

const app = express();
const targetClient = TargetClient.create(CONFIG);

app.use(cookieParser());
// We assume that VisitorAPI.js is stored in "public" folder
app.use(express.static(__dirname + "/public"));

function saveCookie(res, cookie) {
  if (!cookie) {
    return;
  }

  res.cookie(cookie.name, cookie.value, {maxAge: cookie.maxAge * 1000});
}

const getResponseHeaders = () => ({
  "Content-Type": "text/html",
  "Expires": new Date().toUTCString()
});

function sendSuccessResponse(res, response) {
  res.set(getResponseHeaders());

  const result = TEMPLATE
  .replace("${organizationId}", CONFIG.organizationId)
  .replace("${visitorState}", JSON.stringify(response.visitorState))
  .replace("${content}", response);

  saveCookie(res, response.targetCookie);

  res.status(200).send(result);
}

function sendErrorResponse(res, error) {
  res.set(getResponseHeaders());
  res.status(500).send(error);
}

app.get("/abtest", async (req, res) => {
  const visitorCookie = req.cookies[TargetClient.getVisitorCookieName(CONFIG.organizationId)];
  const targetCookie = req.cookies[TargetClient.TargetCookieName];
  const customerIds = {
      "userid": {
        "id": "67312378756723456",
        "authState": TargetClient.AuthState.AUTHENTICATED
      }
    };
  const request = {
    execute: {
      mboxes: [{
        address: { url: req.headers.host + req.originalUrl },
        name: "a1-serverside-ab"
      }]
    }};

  try {
    const response = await targetClient.getOffers({ request, visitorCookie, targetCookie, customerIds });
    sendSuccessResponse(res, response);
  } catch (error) {
    sendErrorResponse(res, error);
  }
});

app.listen(3000, function () {
  console.log("Listening on port 3000 and watching!");
});
```

>[!TAB Java]

```java {line-numbers="true"}
@Controller
public class TargetControllerSample {

  @Autowired
  private TargetClient targetJavaClient;

  @GetMapping("/targetMcid")
  public String targetMcid(Model model, HttpServletRequest request, HttpServletResponse response) {
    Context context = getContext(request);
    Map<String, CustomerState> customerIds = new HashMap<>();
    customerIds.put("userid", CustomerState.authenticated("67312378756723456"));
    customerIds.put("puuid", CustomerState.unknown("550e8400-e29b-41d4-a716-446655440000"));
    TargetDeliveryRequest targetDeliveryRequest = TargetDeliveryRequest.builder()
       .context(context)
       .prefetch(getPrefetchRequest())
       .cookies(getTargetCookies(request.getCookies()))
       .customerIds(customerIds)
       .build();

    TargetDeliveryResponse targetResponse = targetJavaClient.getOffers(targetDeliveryRequest);
    model.addAttribute("visitorState", targetResponse.getVisitorState());
    model.addAttribute("organizationId", "0DD934B85278256B0A490D44@AdobeOrg");
    setCookies(targetResponse.getCookies(), response);
    return "targetMcid";
  }
}
```

>[!ENDTABS]

## ECID e [!DNL Analytics] Integrazione

Per ottenere il massimo da [!DNL Target] e di utilizzare le potenti funzionalità di analisi fornite da [!DNL Adobe Analytics], puoi utilizzare le integrazioni in ECID, [!DNL Analytics], e [!DNL Target].

Utilizzando le integrazioni in ECID, [!DNL Analytics], e [!DNL Target] consente di:

* Utilizzare segmenti da Adobe Audience Manager (AAM)
* Personalizzare l’esperienza utente in base al contenuto recuperato da [!DNL Target]
* Assicurati che tutti gli eventi e le metriche di successo siano raccolti in [!DNL Analytics]
* Utilizzare [!DNL Analytics]&quot; query potenti e beneficia delle sue straordinarie visualizzazioni di rapporti

Integrazioni in ECID, [!DNL Analytics], e [!DNL Target] non richiedono alcuna gestione speciale per analytics sul lato server. Al contrario, una volta integrato l’ECID, aggiungi `AppMeasurement.js` ([!DNL Analytics] sul lato client. [!DNL Analytics] quindi utilizza l’istanza Visitatore per eseguire la sincronizzazione con [!DNL Target].

```html {line-numbers="true"}
<!doctype html>
<html>
<head>
  <meta charset="UTF-8">
  <title>ECID and Analytics integration Sample</title>
  <script src="VisitorAPI.js"></script>
  <script>
    Visitor.getInstance("${organizationId}", {serverState: ${visitorState}});
  </script>
</head>
<body>
  <p>Sample content</p>
  <script src="AppMeasurement.js"></script>
  <script>var s_code=s.t();if(s_code)document.write(s_code);</script>
</body>
</html>
```

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
const express = require("express");
const cookieParser = require("cookie-parser");
const TargetClient = require("@adobe/target-nodejs-sdk");
const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};
const TEMPLATE = `
<!doctype html>
<html>
<head>
  <meta charset="UTF-8">
  <title>ECID and Analytics integration Sample</title>
  <script src="VisitorAPI.js"></script>
  <script>
    Visitor.getInstance("${organizationId}", {serverState: ${visitorState}});
  </script>
</head>
<body>
  <p>${content}</p>
  <script src="AppMeasurement.js"></script>
  <script>var s_code=s.t();if(s_code)document.write(s_code);</script>
</body>
</html>
`;

const app = express();
const targetClient = TargetClient.create(CONFIG);

app.use(cookieParser());
// We assume that VisitorAPI.js and AppMeasurement.js are stored in "public" directory
app.use(express.static(__dirname + "/public"));

function saveCookie(res, cookie) {
  if (!cookie) {
    return;
  }

  res.cookie(cookie.name, cookie.value, {maxAge: cookie.maxAge * 1000});
}

const getResponseHeaders = () => ({
  "Content-Type": "text/html",
  "Expires": new Date().toUTCString()
});

function sendSuccessResponse(res, response) {
  res.set(getResponseHeaders());

  const result = TEMPLATE
  .replace("${organizationId}", CONFIG.organizationId)
  .replace("${visitorState}", JSON.stringify(response.visitorState))
  .replace("${content}", response);

  saveCookie(res, response.targetCookie);

  res.status(200).send(result);
}

function sendErrorResponse(res, error) {
  res.set(getResponseHeaders());

  res.status(500).send(error);
}

app.get("/abtest", async (req, res) => {
  const visitorCookie = req.cookies[TargetClient.getVisitorCookieName(CONFIG.organizationId)];
  const targetCookie = req.cookies[TargetClient.TargetCookieName];
  const request = {
      execute: {
        mboxes: [{
          address: { url: req.headers.host + req.originalUrl },
          name: "a1-serverside-ab"
        }]
      }};

    try {
      const response = await targetClient.getOffers({ request, visitorCookie, targetCookie });
      sendSuccessResponse(res, response);
    } catch (error) {
      sendErrorResponse(res, error);
    }
});

app.listen(3000, function () {
  console.log("Listening on port 3000 and watching!");
});
```

>[!TAB Java]

```java {line-numbers="true"}
@Controller
public class TargetControllerSample {

    @Autowired
    private TargetClient targetJavaClient;

    @GetMapping("/targetAnalytics")
    public String targetMcid(Model model, HttpServletRequest request, HttpServletResponse response) {
        Context context = getContext(request);
        Map<String, CustomerState> customerIds = new HashMap<>();
        customerIds.put("userid", CustomerState.authenticated("67312378756723456"));
        customerIds.put("puuid", CustomerState.unknown("550e8400-e29b-41d4-a716-446655440000"));
        TargetDeliveryRequest targetDeliveryRequest = TargetDeliveryRequest.builder()
                .context(context)
                .prefetch(getPrefetchRequest())
                .cookies(getTargetCookies(request.getCookies()))
                .customerIds(customerIds)
                .trackingServer("imsbrims.sc.omtrds.net")
                .build();

        TargetDeliveryResponse targetResponse = targetJavaClient.getOffers(targetDeliveryRequest);
        model.addAttribute("visitorState", targetResponse.getVisitorState());
        model.addAttribute("organizationId", "0DD934B85278256B0A490D44@AdobeOrg");
        setCookies(targetResponse.getCookies(), response);
        return "targetAnalytics";
    }
```

>[!ENDTABS]
