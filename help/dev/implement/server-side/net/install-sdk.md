---
title: Installare .NET SDK
description: Scopri come installare SDK  [!DNL Adobe Target] .NET.
feature: APIs/SDKs
exl-id: 3cc84775-4692-4d14-9e82-db2873140835
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '51'
ht-degree: 0%

---

# Installare .NET SDK

.NET SDK è distribuito da [NuGet](https://www.nuget.org/packages/Adobe.Target.Client). Per iniziare, aggiungerlo come dipendenza installandolo tramite `Package Manage` o `.NET CLI`:

## Gestione pacchetti

>[!BEGINTABS]

>[!TAB Gestione pacchetti]

```csharp {line-numbers="true"}
Install-Package Adobe.Target.Client
```

>[!TAB .NET CLI]

```csharp {line-numbers="true"}
dotnet add package Adobe.Target.Client
```

>[!ENDTABS]

Il codice open source è disponibile all&#39;indirizzo [https://github.com/adobe/target-dotnet-sdk](https://github.com/adobe/target-dotnet-sdk).
