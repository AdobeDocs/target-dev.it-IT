---
keywords: google, samesite, cookie, chrome 80, ietf
description: Scopri come  [!DNL Adobe Target]  gestisce lo standard IETF di SameSite introdotto con Google Chrome versione 80 e cosa devi fare per rispettare questi criteri.
title: Come gestisce  [!DNL Target]  i criteri per cookie SameSite di Google?
feature: Privacy & Security
exl-id: 58a83def-9625-4d44-914f-203509c6c434
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '1950'
ht-degree: 70%

---

# Criteri per cookie SameSite di Google Chrome

Google inizierà a imporre nuovi criteri per i cookie per impostazione predefinita per gli utenti a partire da Chrome 80, rilasciato all’inizio del 2020. Questo articolo spiega tutto ciò che devi sapere sui nuovi criteri per i cookie SameSite, come [!DNL Adobe Target] supporta questi criteri e come utilizzare [!DNL Target] per rispettare i nuovi criteri per i cookie SameSite di Google Chrome.

A partire da Chrome 80, gli sviluppatori web devono specificare esplicitamente quali cookie possono funzionare tra siti web diversi. Questo è il primo di molti annunci che Google prevede di fare per migliorare la privacy e la sicurezza sul web.

Dato che Facebook si è ritrovato in una posizione difficile per quanto riguarda la privacy e la sicurezza, altri principali attori come Apple e ora Google hanno approfittato rapidamente dell’opportunità di crearsi nuove identità come campioni in fatto di privacy e sicurezza. Apple ha guidato il gruppo annunciando all’inizio di quest’anno le modifiche ai suoi criteri per i cookie tramite ITP 2.1 e recentemente ITP 2.2. Con ITP 2.1, Apple blocca completamente i cookie di terze parti e conserva i cookie creati sul browser per soli sette giorni. Con ITP 2.2, i cookie vengono conservati per un solo giorno. L’annuncio di Google non è affatto così aggressivo come quello di Apple, ma è il primo passo verso lo stesso obiettivo finale. Per ulteriori informazioni sui criteri di Apple, consulta [Apple Intelligent Tracking Prevention (ITP) 2.x](/help/dev/before-implement/privacy/apple-itp-2x.md).

## Cosa sono i cookie e come vengono utilizzati?

Prima di scoprire le modifiche apportate ai criteri per i cookie di Google, soffermiamoci su cosa sono i cookie e come vengono utilizzati. In breve, i cookie sono piccoli file di testo archiviati nel browser web che vengono utilizzati per memorizzare gli attributi dell’utente.

I cookie sono importanti perché migliorano l’esperienza dell’utente durante la navigazione sul web. Ad esempio, se effettui acquisti su un sito web eCommerce e aggiungi qualcosa al carrello ma non accedi o acquisti durante quella visita, i cookie memorizzano gli articoli e li tengono nel carrello per la tua visita successiva. Oppure immagina di dover reinserire nome utente e password ogni volta che visiti il sito web del tuo social media preferito. I cookie risolvono anche questo problema, perché memorizzano informazioni che aiutano i siti a identificare chi sei. Questi cookie sono chiamati cookie di prima parte perché vengono creati e utilizzati dal sito web visitato.

Esistono anche i cookie di terze parti. Per comprenderli meglio, consideriamo questo esempio:

Supponiamo che un’ipotetica società di social media denominata &quot;Friends&quot; fornisca un pulsante Condividi implementato da altri siti per consentire agli utenti di Friends di condividere il contenuto del sito sul feed di Friends. Ora, un utente legge un articolo su un sito web di notizie che utilizza il pulsante Condividi e fa clic su di esso per pubblicare automaticamente sul proprio account Friends.

Perché ciò accada, il browser recupera il pulsante Condividi di Friends da `platform.friends.com` quando l’articolo viene caricato. All’interno di questo processo, il browser allega i cookie di Friends, che contengono le credenziali di accesso dell’utente, alla richiesta ai server di Friends. Questo consente a Friends di pubblicare l’articolo nel proprio feed per conto dell’utente senza richiedere all’utente di effettuare l’accesso.

Tutto ciò è possibile utilizzando cookie di terze parti. In questo caso, il cookie di terze parti viene salvato nel browser per `platform.friends.com`, in modo che `platform.friends.com` può creare il post nell’app Friends per conto dell’utente.

Prova a immaginare come sarebbe possibile realizzare questo caso d’uso senza cookie di terze parti: l’utente dovrebbe seguire molti passaggi manuali. Prima di tutto, dovrebbe copiare il link all’articolo, quindi dovrebbe accedere all’app Friends separatamente. In seguito, l’utente fa clic sul pulsante Crea post, copia e incolla il collegamento nel campo di testo e infine clicca su Pubblica. Come puoi vedere, i cookie di terze parti aiutano enormemente l’esperienza dell’utente in quanto i passaggi manuali possono essere ridotti drasticamente.

Più in generale, i cookie di terze parti consentono di memorizzare i dati sul browser di un utente senza richiedere all’utente di visitare esplicitamente un sito web.

## Problemi di sicurezza

Nonostante i cookie migliorino le esperienze degli utenti e alimentino la pubblicità, possono anche introdurre vulnerabilità a livello di sicurezza come attacchi CSRF (Cross-Site Request Forgery). Ad esempio, se un utente accede a un sito bancario per pagare le fatture delle carte di credito, lascia il sito senza disconnettersi e poi accede a un sito dannoso nella stessa sessione, può verificarsi un attacco CSRF. Il sito dannoso potrebbe includere del codice che invia una richiesta al sito bancario e viene eseguito al caricamento della pagina. Poiché l’utente è ancora autenticato sul sito bancario, il cookie di sessione può essere utilizzato per avviare un attacco CSRF per avviare un evento di trasferimento di fondi dal conto bancario dell’utente. Ciò accade perché ogni volta che visiti un sito, tutti i cookie vengono allegati alla richiesta HTTP. Dati questi problemi di sicurezza, Google sta cercando di mitigarli.

## In che modo [!DNL Target] utilizza i cookie?

Detto questo, vediamo come [!DNL Target] utilizza i cookie. Per utilizzare [!DNL Target], devi innanzitutto installare la libreria JavaScript di [!DNL Target] sul tuo sito. Questo consente di inserire un cookie di prima parte nel browser dell’utente che visita il sito. Quando l’utente interagisce con il sito web, puoi trasmettere i dati di comportamento e di interesse dell’utente a [!DNL Target] tramite la libreria JavaScript. Il [!DNL Target] La libreria JavaScript di utilizza cookie di prime parti per estrarre informazioni di identificazione sull’utente da mappare ai dati di comportamento e di interesse dell’utente. Questi dati vengono quindi utilizzati da [!DNL Target] per potenziare le attività di personalizzazione.

Target utilizza (a volte) anche cookie di terze parti. Se possiedi più siti web esistenti su domini diversi e desideri monitorare il percorso utente su tali siti web, puoi utilizzare i cookie di terze parti sfruttando il tracciamento tra domini diversi. Attivando il tracciamento tra domini diversi nella libreria JavaScript di [!DNL Target], il tuo account inizierà a utilizzare cookie di terze parti. Quando un utente passa da un dominio all’altro, il browser comunica con il server backend di Target e durante questo processo viene creato e inserito un cookie di terze parti nel browser dell’utente. Tramite il cookie di terze parti presente nel browser dell’utente, [!DNL Target] è in grado di fornire un’esperienza coerente su domini diversi per un singolo utente.

## Nuova ricetta dei cookie di Google

Per garantire la protezione degli utenti quando i cookie vengono inviati tra siti diversi, Google prevede di aggiungere il supporto per uno standard IETF denominato SameSite, che richiede agli sviluppatori web di gestire i cookie con il componente di attributo SameSite nell’intestazione Set-Cookie.

Tre valori diversi possono essere passati nell’attributo SameSite: Strict, Lax o None (Rigoroso, Tollerante, Nessuno).

| Valore | Descrizione |
| --- | --- |
| Strict (Rigoroso) | I cookie con questa impostazione sono accessibili solo quando si visita il dominio in cui sono stati inizialmente impostati. In altre parole, Strict impedisce completamente che il cookie possa essere utilizzato tra siti diversi. Questa opzione è indicata per le applicazioni che richiedono una protezione elevata, ad esempio per i siti delle banche. |
| Lax (Tollerante) | I cookie con questa impostazione vengono inviati solo per le richieste dello stesso sito o nella navigazione di livello superiore con richieste HTTP non idempotenti, come `HTTP GET`. Pertanto questa opzione va utilizzata se il cookie può essere usato da terze parti, pur proteggendo gli utenti da attacchi CSRF (Cross-Site Request Forgery). |
| None (Nessuno) | I cookie con questa impostazione continuano a funzionare così come fanno attualmente. |

Tenendo conto di quanto sopra, Chrome 80 introduce due impostazioni indipendenti per gli utenti: “SameSite by default cookies” (Cookie SameSite per impostazione predefinita) e “Cookies without SameSite must be secure” (Cookie senza SameSite devono essere protetti). Queste impostazioni saranno abilitate per impostazione predefinita in Chrome 80.

![Finestra di dialogo SameSite](../assets/samesite.png)

* **SameSite per cookie predefiniti**: con questa impostazione, tutti i cookie privi dell’attributo SameSite verranno automaticamente costretti a utilizzare `SameSite = Lax`.
* **Cookies without SameSite must be secure** (Cookie senza SameSite devono essere protetti): con questa impostazione, i cookie privi dell’attributo SameSite o con `SameSite = None` devono essere protetti. In questo contesto, protetti significa che tutte le richieste del browser devono seguire il protocollo HTTPS. I cookie che non aderiscono a questo requisito vengono rifiutati. Per soddisfare questo requisito, tutti i siti web devono utilizzare HTTPS.

## [!DNL Target] segue le best practice di sicurezza di Google

Ad Adobe, desideriamo sempre supportare le più recenti best practice del settore in materia di sicurezza e privacy. Siamo lieti di annunciare che [!DNL Target] supporta le nuove impostazioni di sicurezza introdotte da Google.

Per l’impostazione “SameSite by default cookies” (Cookie SameSite per impostazione predefinita), [!DNL Target] continuerà a distribuire la personalizzazione senza alcun impatto e intervento da parte tua. [!DNL Target] utilizza i cookie di prima parte e continuerà a funzionare correttamente in quanto Google Chrome applica il flag `SameSite = Lax`.

Per l’opzione &quot;Cookies without SameSite must be secure&quot; (Cookie senza SameSite devono essere protetti), se non scegli di usare la funzione di tracciamento tra domini diversi in Target, i cookie di prime parti in [!DNL Target] continuerà a funzionare.

Tuttavia, se scegli di utilizzare il tracciamento tra domini diversi per sfruttare tali funzioni di [!DNL Target], Chrome richiede che vengano usati i flag `SameSite = None` e Secure per i cookie di terze parti. Devi quindi assicurarti che i tuoi siti utilizzino il protocollo HTTPS. Le librerie lato client in [!DNL Target] utilizzeranno automaticamente il protocollo HTTPS e allegheranno i flag `SameSite = None` e Secure ai cookie di terze parti in [!DNL Target] per garantire che tutte le attività continuino a essere eseguite.

## Cosa devi fare?

Per capire cosa devi fare perché [!DNL Target] continui a funzionare per gli utenti di Google Chrome 80+, consulta la tabella sottostante, che include le seguenti colonne:

* **Libreria JavaScript di Target**: utilizzo di at.js 1.*x* o di at.js 2.*x* nei tuoi siti.
* **“SameSite by default cookies” = Abilitato**: impatto se l’opzione “SameSite by default cookies” è abilitata e attività da eseguire affinché [!DNL Target] continui a funzionare.
* **“Cookies without SameSite must be secure” = Abilitato**: impatto se l’opzione “Cookies without SameSite must be secure” è abilitata in e attività da eseguire affinché [!DNL Target] continui a funzionare.

| Libreria JavaScript di Target | “SameSite by default cookies” = Abilitato | “Cookies without SameSite must be secure” = Abilitato |
| --- | --- | --- |
| at.js 1.*x* con cookie di prime parti. | Nessun impatto. | Nessun impatto, se non utilizzi il tracciamento tra domini diversi. |
| at.js 1.*x* con tracciamento tra domini diversi abilitato. | Nessun impatto. | Devi abilitare il protocollo HTTPS nel tuo sito.<br />Target utilizza un cookie di terze parti per monitorare gli utenti e Google richiede che i cookie di terze parti abbiano `SameSite = None` e flag Secure. Il flag Secure richiede l’utilizzo del protocollo HTTPS da parte dei siti. |
| at.js 2.*x* | Nessun impatto. | Nessun impatto. |

## Cosa deve fare [!DNL Target]?

Cosa abbiamo dovuto fare nella nostra piattaforma per aiutarti a rispettare i nuovi criteri per i cookie SameSite di Google Chrome 80+?

| Libreria JavaScript di Target | “SameSite by default cookies” = Abilitato | “Cookies without SameSite must be secure” = Abilitato |
| --- | --- | --- |
| at.js 1.*x* con cookie di prime parti. | Nessun impatto. | Nessun impatto, se non utilizzi il tracciamento tra domini diversi. |
| at.js 1.*x* con tracciamento tra domini diversi abilitato. | Nessun impatto. | at.js 1.*x* con tracciamento tra domini diversi abilitato. |
| at.js 2.*x* | Nessun impatto. | Nessun impatto. |

## Qual è l’impatto se non si passa all’utilizzo del protocollo HTTPS?

L’unico caso d’uso che avrà un impatto su di te è l’utilizzo della funzione di tracciamento tra domini diversi in [!DNL Target] attraverso at.js 1.*x*. Se non passi a HTTPS, un requisito di Google, vedrai un picco di visitatori univoci nei tuoi domini perché il cookie di terze parti utilizzato verrà rimosso da Google. Poiché il cookie di terze parti verrà rimosso, [!DNL Target] non sarà in grado di fornire un’esperienza coerente e personalizzata per l’utente mentre si sposta da un dominio all’altro. Il cookie di terze parti viene utilizzato principalmente per identificare un singolo utente che naviga tra i domini che possiedi.

## Conclusione

Nel settore si fanno grandi progressi nella creazione di un web più sicuro per i consumatori, e Adobe si impegna totalmente ad aiutare i propri clienti a fornire esperienze personalizzate che garantiscano sicurezza e privacy agli utenti finali. Tutto ciò che devi fare è seguire le best practice di cui sopra e sfruttare [!DNL Target] per rispettare i nuovi criteri per i cookie SameSite di Google Chrome.
