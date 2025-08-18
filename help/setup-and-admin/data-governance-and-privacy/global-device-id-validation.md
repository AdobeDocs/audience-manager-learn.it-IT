---
title: Convalida ID dispositivo globale
description: Scopri come convalidare gli ID dispositivo inviati alle origini dati globali per verificare la formattazione corretta e i messaggi di errore quando gli ID non vengono formattati correttamente.
feature: Data Governance & Privacy
topics: mobile
activity: implement
doc-type: article
team: Technical Marketing
kt: 2977
role: Developer, Data Engineer, Architect
level: Experienced
exl-id: 0ff3f123-efb3-4124-bdf9-deac523ef8c9
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '616'
ht-degree: 1%

---

# Convalida ID dispositivo globale {#global-device-id-validation}

Gli identificatori di Device Advertising (ad esempio, iDFA, GAID, Roku ID) dispongono di standard di formattazione che devono essere soddisfatti per essere utilizzabili nell’ecosistema della pubblicità digitale. Oggi, clienti e partner possono caricare gli ID sulle nostre origini dati globali in qualsiasi formato senza essere avvisati della corretta formattazione dell’ID. Questa funzione introdurrà la convalida degli ID dispositivo inviati alle origini dati globali per la formattazione corretta e fornirà messaggi di errore quando gli ID non sono formattati correttamente. Al momento dell&#39;avvio, supporteremo la convalida per [!DNL iDFA], [!DNL Google Advertising] e [!DNL Roku IDs].

## Panoramica degli standard di formato {#overview-of-format-standards}

Di seguito sono riportati i pool ID globali di Device Advertising attualmente riconosciuti e supportati da AAM. Questi sono implementati come [!UICONTROL Data Sources] condivisi e possono essere utilizzati da qualsiasi cliente o partner di dati che lavora con dati legati agli utenti di queste piattaforme.

<table>
  <tr>
   <td>Piattaforma </td>
   <td>ID AAM Data Source </td>
   <td>Formato ID </td>
   <td>PID AAM </td>
   <td>Note </td>
  </tr>
  <tr>
   <td>Google Android (GAID)</td>
   <td>20914</td>
   <td>32 numeri esadecimali, generalmente presentati come 8-4-4-4-12<em>esempio, 97987bca-ae59-4c7d-94ba-ee4f19ab8c21<br/> </em> </td>
   <td>1352</td>
   <td>Questo ID deve essere raccolto in un riferimento modulo non elaborato/senza hash/non modificato - <a href="https://play.google.com/about/monetization-ads/ads/ad-id/">https://play.google.com/about/monetization-ads/ads/ad-id/</a></td>
  </tr>
  <tr>
   <td>Apple iOS (IDFA)</td>
   <td>20915</td>
   <td>32 numeri esadecimali, generalmente presentati come 8-4-4-4-12 <em>esempio, 6D92078A-8246-4BA4-AE5B-76104861E7DC<br /> </em> </td>
   <td>3560</td>
   <td>Questo ID deve essere raccolto in un riferimento modulo non elaborato/senza hash/non modificato - <a href="https://support.apple.com/en-us/HT205223">https://support.apple.com/en-us/HT205223</a></td>
  </tr>
  <tr>
   <td>Roku (RIDA)</td>
   <td>121963</td>
   <td>32 numeri esadecimali, generalmente presentati come 8-4-4-4-12 <em>esempio,</em> <em>fcb2a29c-315a-5e6b-bcfd-d889ba19aada</em></td>
   <td>11536</td>
   <td>Questo ID deve essere raccolto in un riferimento modulo non elaborato/senza hash/non modificato - <a href="https://sdkdocs.roku.com/display/sdkdoc/Roku+Advertising+Framework">https://sdkdocs.roku.com/display/sdkdoc/Roku+Advertising+Framework</a> </td>
  </tr>
  <tr>
   <td>Microsoft Advertising ID (MAID)</td>
   <td>389146</td>
   <td>Stringa numerica Alpha</td>
   <td>14593</td>
   <td>Questo ID deve essere raccolto in un riferimento modulo non elaborato/senza hash/non modificato - <a href="https://docs.microsoft.com/en-us/uwp/api/windows.system.userprofile.advertisingmanager.advertisingid">https://docs.microsoft.com/en-us/uwp/api/windows.system.userprofile.advertisingmanager.advertisingid</a><br/><a href="https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.userprofile.advertisingmanager.advertisingid.aspx">https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.userprofile.advertisingmanager.advertisingid.aspx</a></td>
  </tr>
  <tr>
   <td>Samsung DUID</td>
   <td>404660</td>
   <td>Esempio di stringa numerica Alpha, 7XCBNROQJQPYW</td>
   <td>15950</td>
   <td>Questo ID deve essere raccolto in un riferimento modulo non elaborato/senza hash/non modificato - <a href="https://developer.samsung.com/tv/develop/api-references/samsung-product-api-references/productinfo-api">https://developer.samsung.com/tv/develop/api-references/samsung-product-api-references/productinfo-api</a> </td>
  </tr>
</table>

## Impostazione di un identificatore Advertising nell’app {#setting-an-advertising-identifier-in-the-app}

L&#39;impostazione dell&#39;ID inserzionista nell&#39;app è in realtà un processo in due fasi: prima recupera l&#39;ID inserzionista e quindi lo invia ad Experience Cloud. Di seguito sono riportati i collegamenti per l’esecuzione di questi passaggi.

1. Recuperare l’ID
   1. [!DNL Apple] informazioni su [!DNL advertising ID] sono disponibili [QUI](https://developer.apple.com/documentation/adsupport/asidentifiermanager).
   1. Alcune informazioni sull&#39;impostazione di [!DNL advertiser ID] per gli sviluppatori di [!DNL Android] sono disponibili [QUI](http://android.cn-mirrors.com/google/play-services/id.html).
1. Inviarla ad Experience Cloud utilizzando il metodo [!DNL setAdvertisingIdentifier] in SDK
   1. Le informazioni per l&#39;utilizzo di `setAdvertisingIdentifier` si trovano nella [documentazione](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/mobile-core/identity/identity-api-reference#set-an-advertising-identifier) sia per [!DNL iOS] che per [!DNL Android].

`// iOS (Swift) example for using setAdvertisingIdentifier:`
`ACPCore.setAdvertisingIdentifier([AdvertisingId]) // ...where [AdvertisingId] is replaced by the actual advertising ID`

## Messaggi di errore DCS per ID non corretti  {#dcs-error-messaging-for-incorrect-ids}

Quando un ID dispositivo globale errato (IDFA, GAID, ecc.) viene inviato in tempo reale ad Audience Manager, viene restituito un codice di errore nell’hit. Di seguito è riportato un esempio di errore restituito perché l&#39;ID viene inviato come [!DNL Apple IDFA], che deve contenere solo lettere maiuscole, ma l&#39;ID contiene una &quot;x&quot; minuscola.

![immagine di errore](assets/image_4_.png)

Per un elenco dei codici di errore, consulta la [documentazione](https://experienceleague.adobe.com/docs/audience-manager/user-guide/api-and-sdk-code/dcs/dcs-api-reference/dcs-error-codes.html?lang=it#api-and-sdk-code).

## Onboarding degli ID dispositivo globali {#onboarding-global-device-ids}

Oltre all&#39;invio in tempo reale degli ID dispositivo globali, puoi anche &quot;[!DNL onboard]&quot; (caricare) dati rispetto agli ID. Questo processo è uguale a quello seguito per l’onboarding dei dati in base agli ID cliente (in genere tramite coppie chiave/valore), ma è sufficiente utilizzare gli ID Data Source appropriati, in modo che i dati vengano assegnati all’ID dispositivo globale. La documentazione sul processo di onboarding è disponibile nella [documentazione](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/sending-audience-data/batch-data-transfer-process/batch-data-transfer-overview.html?lang=it#implementation-integration-guides). Ricorda di utilizzare l’ID sorgente dati globale, a seconda della piattaforma in uso.

Se tramite il processo di onboarding vengono inviati ID dispositivo globali errati, gli errori verranno visualizzati in [[!DNL Onboarding Status Report]](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reporting/onboarding-status-report.html?lang=it#reporting).

Di seguito è riportato un esempio di errore che verrebbe generato da tale rapporto:

![immagine di errore](assets/image_5_.png)
