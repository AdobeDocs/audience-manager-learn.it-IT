---
title: Supporto IAB TCF 2.2
description: Scopri il plug-in di Audience Manager in IAB TCF e come funziona con l’oggetto opt-in di Adobe e il tuo provider di gestione dei consensi (CMP).
feature: Data Governance & Privacy
thumbnail: 26434.jpg
kt: 5027
role: Developer, Data Engineer, Architect
level: Experienced
exl-id: 04b4e786-0457-4dcc-bcf9-a79eda67bb2e
source-git-commit: f9708e705d95b43084ff11e342dc54ff11d6326c
workflow-type: tm+mt
source-wordcount: '1059'
ht-degree: 0%

---

# Supporto IAB TCF 2.2 in Audience Manager {#iab-tcf-support-in-audience-manager}

Adobe consente di gestire e comunicare le scelte degli utenti in materia di privacy tramite la funzionalità Opt-in e il supporto di Audience Manager Plug-in per IAB Transparency and Consent Framework 2.2 (TCF 2.2). Questo articolo funziona insieme alla documentazione per aiutarti a comprendere il plug-in di Audience Manager in IAB TCF e come funziona con l’oggetto Opt-in di Adobe e il tuo provider di gestione dei consensi (CMP). Per ulteriori informazioni su IAB, visitare il sito Web all&#39;indirizzo [https://www.iabeurope.eu/](https://www.iabeurope.eu/).

## Primo passaggio: comprendere il consenso di Experience Cloud ID {#first-step-understand-ecid-s-opt-in}

Per capire come utilizzare IAB TCF, devi prima comprendere la funzionalità [!DNL Opt-in], che fa parte della libreria del servizio Experience Cloud ID (ECID). Se non sai come funziona il servizio Opt-in, consulta prima [questo utile articolo](https://experienceleague.adobe.com/docs/core-services-learn/tutorials/id-service/use-opt-in-to-control-experience-cloud-activities-based-on-user-consent.html?lang=it). È inoltre necessario rivedere la [documentazione](https://experienceleague.adobe.com/docs/id-service/using/implementation/opt-in-service/optin-overview.html?lang=it) di Opt-in. Dopo aver esaminato tali risorse, torna a questa pagina e continua.

## Il plug-in Audience Manager per IAB TCF {#the-audience-manager-plug-in-for-iab-tcf}

Ora che hai almeno una conoscenza di base del funzionamento del servizio Opt-in, Audience Manager può intervenire sul supporto di [!DNL IAB Transparency and Consent Framework (TCF)] tramite un plug-in nell&#39;oggetto Opt-in.

Il plug-in Audience Manager per IAB TCF estende le funzionalità di Opt-in e consente ai clienti di AAM di valutare, rispettare e inoltrare le scelte sulla privacy degli utenti ai partner a valle in conformità con IAB TCF. Fornisce uno standard che i titolari del trattamento dei dati (ossia i clienti di Adobe) e i fornitori (DMP, DSP, SSP, Ad Server, ecc.) possono utilizzare per comprendere il consenso nell’intera area del consenso.

## Abilita IAB TCF {#enabling-iab-tcf}

Abilitare il plug-in Audience Manager per IAB TCF è facile se si utilizza Adobe Experience Platform Launch, in quanto si tratta di una semplice casella di controllo, come mostrato nel breve video seguente:

>[!VIDEO](https://video.tv.adobe.com/v/38260/?quality=12&captions=ita)

In alternativa, se non utilizzi Launch, puoi utilizzare `isIabContext=true` per abilitarlo quando crei un&#39;istanza del visitatore di Experience Cloud. Questo avvia il flusso IAB TCF, ovvero aggiunge un altro passaggio alla raccolta dei consensi, utilizzando IAB TCF per eseguire query sulla stringa IAB TC e la restituisce a Opt-in, che a sua volta comunica con le soluzioni Experience Cloud.

## Stringa TC IAB {#iab-tcf-consent-string}

Uno degli standard che IAB fornisce è una &quot;stringa di consenso&quot; (nota anche come &quot;DaisyBit&quot;), che in realtà è due elenchi messi insieme:

1. Scopi: **Che cosa** viene concesso per eseguire l&#39;operazione?
1. Fornitori: **A chi** viene dato il consenso?

### Finalità {#purposes}

Con IAB TCF 2.2, ci sono dieci &quot;scopi&quot; per i quali raccogliere il consenso (quello che i fornitori possono fare con i dati del visitatore). Adobe Audience Manager non richiede tutti i dieci, ma richiede il consenso solo per i seguenti scopi, oltre al consenso del fornitore:

* **Scopo 1:** Archiviare e/o accedere a informazioni su un dispositivo;
* **Scopo 10:** Sviluppare e migliorare i prodotti;
* **Scopo speciale 1:** Garantire la sicurezza, prevenire le frodi ed eseguire il debug.

Questa è la prima parte della stringa TC IAB ed è appena registrata come 1 e 0, che determina se lo scopo/attività viene approvata o meno.

>[!NOTE]
>
>In base alle normative IAB, la funzione speciale 1 (garantire la sicurezza, prevenire le frodi ed eseguire il debug) è sempre autorizzata e gli utenti non possono opporsi.

### Fornitori {#vendors}

Un’altra parte della stringa TC IAB è costituita da un lungo elenco di diverse centinaia di fornitori, in modo che ai visitatori possa essere presentato un elenco dei fornitori applicabili che dispongono di tag sul sito e possono scegliere quali fornitori utilizzare. I fornitori mantengono il proprio posto nell&#39;elenco. Ad esempio, il numero del fornitore Adobe Audience Manager in questo elenco è 565. Se quel numero nell’elenco ha &quot;1&quot;, allora Audience Manager può fare le finalità approvate dalla parte anteriore dell’elenco. Se il punto di AAM ha &quot;0&quot;, non può fare nulla con i dati.

**Affinché Audience Manager fornisca ai clienti un&#39;interfaccia utente che consenta loro di utilizzare IAB TCF per scegliere questi scopi e fornitori o per approvare/disapprovare tutte le attività, è necessario utilizzare un partner CMP registrato con IAB TCF o crearne uno che supporti IAB TCF e sia registrato con IAB TCF.**

## Opt-in: traduzione tra applicazioni IAB e Adobe {#opt-in-translating-between-iab-and-adobe-solutions}

Uno dei vantaggi dell’utilizzo di IAB TCF è che gli scopi standard elencati sopra probabilmente forniscono all’utente finale più un’idea di ciò che sta approvando rispetto a un elenco di soluzioni Adobe. Gli utenti finali potrebbero non sapere cosa significa &quot;approvare&quot; Audience Manager o [!DNL Target], ma &quot;archiviare e/o accedere alle informazioni su un dispositivo&quot; o &quot;sviluppare e migliorare i prodotti&quot; è probabilmente più facile per loro da capire e acconsentire.

Affinché Audience Manager possa essere approvato (ad es. Affinché la traduzione delle finalità IAB di Opt-in dia ad AAM il &quot;sì&quot;), le finalità 1 e 10, come sopra elencate, devono ottenere il consenso dell’utente finale. Se uno di questi non è approvato, o se un fornitore non è approvato, AAM non eseguirà incendi di pixel o non imposterà cookie. È inoltre utile sapere che molti clienti scelgono semplicemente di fornire all’utente finale un’interfaccia utente &quot;tutto o niente&quot;, che consentirebbe o meno l’utilizzo di Audience Manager (e delle altre soluzioni Experience Cloud).

Nella [documentazione](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html?lang=it) sono disponibili informazioni utili sulle modalità con cui il flusso del plug-in di Audience Manager per IAB TCF si applica sia ai casi di utilizzo di Publisher che a quelli degli inserzionisti.

## IAB: invio del consenso a valle {#iab-sending-consent-downstream}

Quando si utilizza il plug-in di Audience Manager per IAB TCF, le scelte di consenso dell’utente vengono inviate anche alle sincronizzazioni ID a livello di piattaforma (terze parti) per i partner presenti nell’elenco dei fornitori globali, in modo che il partner disponga delle informazioni di consenso dell’utente e possa agire di conseguenza. Queste informazioni vengono inviate in due variabili:

* rgpd = 1
* gdpr_consent = [stringa di consenso codificata]

Bisogna però prestare attenzione: se l’utente si trova nel contesto IAB e non fornisce il consenso (o fornisce un consenso negativo), Audience Manager non raccoglie affatto la stringa TC IAB e, di conseguenza, interrompe le chiamate. Quindi, in quel caso... non si passa il consenso a valle.

## Demo {#demo}

Nel video seguente, scopri in che modo le scelte degli utenti IAB influenzano i cookie e i beacon di ECID e delle soluzioni.

>[!VIDEO](https://video.tv.adobe.com/v/38243/?quality=12&captions=ita)

Per informazioni più dettagliate sul plug-in Audience Manager per IAB TCF 2.2, tra cui come implementare e testare, casi d&#39;uso e flusso di lavoro, consulta la [documentazione](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html?lang=it).
