---
title: Come identificare l’ID partner o il sottodominio
description: Scopri come identificare l’ID o il sottodominio del partner durante l’implementazione di alcune funzioni di Experience Cloud e circa due posizioni in cui puoi ottenere questo ID nell’interfaccia utente di Audience Manager.
feature: Implementation Basics
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 2359
role: Developer, Data Engineer
level: Intermediate
exl-id: d3f4a12d-acc5-47b7-a38a-a6a14152bf3a
source-git-commit: 62b43b5627dabf754cf821f974a56c60989ef7ef
workflow-type: tm+mt
source-wordcount: '315'
ht-degree: 0%

---

# Come identificare il sottodominio Audience Manager {#how-to-identify-your-audience-manager-partner-id-or-subdomain}

Quando implementi alcune funzioni di Experience Cloud, è necessario conoscere l’Audience Manager `Subdomain` è (talvolta indicato anche come `client ID` o `Partner ID`). In questo video, ti mostreremo due posizioni in cui puoi ottenere queste informazioni nell’interfaccia utente di Audience Manager.

## Dare via il finale... {#giving-away-the-ending}

Nel caso in cui preferiresti semplicemente saltare dentro e trovarlo senza guardare questo breve video, puoi trovare il tuo `Partner Subdomain` in due posizioni nell’interfaccia utente:

1. Se hai già creato un [!UICONTROL rule-based] caratteristica, fai clic su **[!UICONTROL Get Trait URL]**
   [!UICONTROL Get Trait URL] si trova accanto alla caratteristica nell’elenco delle caratteristiche della cartella e l’URL includerà il sottodominio nell’URL.
1. Se vai nel **[!UICONTROL Tools]** > **[!UICONTROL Tags]** e fai clic su **[!UICONTROL Get code]** per il contenitore, il sottodominio è verso la fine della riga Akamai

Se non riesci a trovarlo rapidamente con questi riferimenti rapidi, il video è un breve impegno. :

>[!VIDEO](https://video.tv.adobe.com/v/25922/?quality=12)

>[!IMPORTANT]
>
>A ciascun cliente di Adobe Experience Cloud è assegnato un ID numerico, spesso denominato &quot;PID&quot; o Partner ID. Questo non è l&#39;ID di cui stiamo parlando in questo articolo e video. Al contrario, il &quot;sottodominio partner&quot;, a volte denominato ID partner, è in genere una versione del nome client ed è il sottodominio del server a cui vengono inviati i dati. Ad esempio, se la tua azienda è &quot;Bob&#39;s Knobs&quot; (tutte le cose che fanno la maniglia, ovviamente, haha), allora è probabile che il tuo sottodominio partner sia &quot;bobsknobs&quot;, mentre il &quot;PID&quot; sarebbe qualcosa di più simile a &quot;12345&quot;. In genere non è necessario conoscere il PID, ma il sottodominio è importante conoscerlo, in modo da poter configurare l’implementazione di Audience Manager.
