+++
title = "IoT Hub"
description = "Best practices and resiliency recommendations for IoT Hub and associated resources and settings."
date = "10/25/23"
author = "ReneHezser"
msAuthor = "rehezser"
draft = false
+++

The presented resiliency recommendations in this guidance include IoT Hub and associated resources and settings. General guidance are available in the Well-Architected Framework for IoT [Reliability in your IoT workload](https://learn.microsoft.com/ja-jp/azure/well-architected/iot/iot-reliability).

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                    |  Category                                                               |  Impact         |  State            | ARG Query Available |
| :------------------------------------------------ | :---------------------------------------------------------------------: | :------:        | :------:          | :-----------------: |
| [IOTH-1 - Device Identities are exported to a secondary region](#ioth-1---device-identities-are-exported-to-a-secondary-region) | Disaster Recovery | High | Preview  |         No         |
| [IOTH-2 - Do not use free tier](#ioth-2---do-not-use-free-tier) | Availability | High | Preview  |         Yes          |
| [IOTH-3 - Use Availability Zones](#ioth-3---use-availability-zones) | Availability | High | Preview  |         No          |
| [IOTH-4 - Use Device Provisioning Service](#ioth-4---use-device-provisioning-service) | Scalability | Critical | Preview  |         Yes          |
| [IOTH-5 - Define Failover Guidelines](#ioth-5---define-failover-guidelines) | Availability | High | Preview  |         No          |
| [IOTH-6 - Disabled Fallback Route](#ioth-6---disabled-fallback-route) | Monitoring | Low | Preview  |         Yes          |

{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### IOTH-1 - デバイス ID をセカンダリ リージョンにエクスポートします

**Category: Disaster Recovery**

**Impact: High**

**Recommendation**

デバイス ID は、別の IoT Hub へのフェールオーバーが発生した場合にすべての IoT デバイスが接続できるように、フェールオーバー リージョン IoT-Hub にコピーする必要があります。

別のリージョンへの IoT Hub の手動フェールオーバーは高速 (RTO) であり、ミッション クリティカルなワークロードに使用できます。

**Resources**

- [Import and export IoT Hub device identities in bulk](https://learn.microsoft.com/ja-jp/azure/iot-hub/iot-hub-bulk-identity-mgmt)
- [IoT Hub high availability and disaster recovery](https://learn.microsoft.com/ja-jp/azure/iot-hub/iot-hub-ha-dr#manual-failover)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/ioth-1/ioth-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### IOTH-2 - 無料利用枠を使用しないでください

**Category: Availability**

**Impact: High**

**Recommendation**

運用シナリオでは、Free レベルでは必要な SLA が提供されないため、IoT Hub レベルを Free にしないでください。

**Resources**

- [Choose the right IoT Hub tier and size for your solution](https://learn.microsoft.com/ja-jp/azure/iot-hub/iot-hub-scaling)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/ioth-2/ioth-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### IOTH-3 - 可用性ゾーンを使用します

**Category: Availability**

**Impact: High**

**Recommendation**

IoT Hub の Availability Zones をサポートするリージョンでは、これらのゾーンを使用して可用性を高める必要があります。Availability Zones は、サポートされているリージョン内の新しい IoT Hub に対して自動的にアクティブ化されます。

**Resources**

- [Azure IoT Hub high availability and disaster recovery](https://learn.microsoft.com/ja-jp/azure/iot-hub/iot-hub-ha-dr#availability-zones)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/ioth-3/ioth-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### IOTH-4 - Device Provisioning Service を使用します

**Category: System Efficiency**

**Impact: High**

**Recommendation**

Device Provisioning Service (DPS) を使用すると、スケーリングと可用性のために IoT デバイスを簡単に再配布できます。デバイスは特定の IoT Hub インスタンスにバインドされませんが、ルールを使用して再割り当てできます。

Device Provisioning Service に関連付けられている IoT Hub でも、デバイスで使用されているかどうかを確認する必要があります。

**Resources**

- [IoT Hub Device Provisioning Service (DPS) terminology](https://learn.microsoft.com/ja-jp/azure/iot-dps/concepts-service)
- [Best practices for large-scale IoT device deployments](https://learn.microsoft.com/ja-jp/azure/iot-dps/concepts-deploy-at-scale)
- [IoT Hub Device Provisioning Service high availability and disaster recovery](https://learn.microsoft.com/ja-jp/azure/iot-dps/iot-dps-ha-dr)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/ioth-4/ioth-4.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### IOTH-5 - フェイルオーバーガイドラインを定義します

**Category: Availability**

**Impact: High**

**Recommendation**

リージョンで障害が発生した場合、IoT Hub は 2 番目のリージョンにフェールオーバーできます。このフェールオーバーは、自動または手動で開始できます。どちらの場合も、アプリケーションが動作し続けるには、特定の要件が必要です。フェールオーバーのガイダンスを確認ください。

- 自動フェイルオーバーの場合にRTOが一致しているかどうかを確認します
- デバイスが IoT ハブに接続するために IP アドレスは使用されません

**Resources**

- [IoT Hub high availability and disaster recovery](https://learn.microsoft.com/ja-jp/azure/iot-hub/iot-hub-ha-dr)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/ioth-5/ioth-5.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### IOTH-6 - フォールバックルートを無効化します

**Category: Monitoring**

**Impact: Low**

**Recommendation**

メッセージ ルーティングを使用してメッセージをカスタム エンドポイントにルーティングする場合、条件がミートでないと、メッセージがカスタム ルートに配信されないことがあります。既定のルートでは、常にすべてのメッセージが受信されます。無効にすると、メッセージが配信されない可能性があります。

**Resources**

- [Use message routing - Fallback route](https://learn.microsoft.com/ja-jp/azure/iot-hub/iot-hub-devguide-messages-d2c#fallback-route)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/ioth-6/ioth-6.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
