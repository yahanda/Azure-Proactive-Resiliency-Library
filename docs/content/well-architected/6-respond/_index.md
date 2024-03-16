+++
title = "6 - Respond"
description = "Microsoft Azure Well-Architected Framework best practices and recommendations for the Reliability Stage - 6 - Respont"
date = "9/18/23"
weight = 6
author = "rodrigosantosms"
msAuthor = "rodrigosantosms"
draft = false
+++

The presented Microsoft Azure Well-Architected Framework recommendations in this guidance include Reliability Stage "6 - Respond (Responding to Failures)" and associated resources and their settings.

This final stage involves having plans and procedures in place to react to incidents affecting reliability. This includes automated failovers, backup restoration, and escalation protocols for manual intervention.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation                                                                                              |  Category          |  Impact   |  State   | ARG Query Available |
| :---------------------------------------------------------------------------------------------------------- | :---------:        | :------:  | :------: | :-----------------: |
| [WARD-1 - Implement proactive Incident Response](#ward-1---implement-proactive-incident-response)           |  Disaster Recovery |   High    | Verified |         No          |
{{< /table >}}

{{< alert style="info" >}}

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### WARD-1 - プロアクティブなインシデント対応を実装します

**Category: Disaster Recovery**

**Impact: High**

**Recommendation/Guidance**

すべての問題を防ぐことは称賛に値しますが、不可能な目標です。物事はうまくいかないので、エンドユーザーへの影響を制限し、できるだけ早く運用を正常に戻すための計画が必要です。

重要なのは、反応するのではなく、緊急性を持って対応することです。反応は、長期的な影響を考慮せずに、より衝動的で、現在の瞬間に基づいている傾向があります。対応は、よく考え抜かれ、整理され、情報に基づいています。

インシデント対応アプローチによって、次の点で有効性が決まります。

何が起こっているのかを理解する(問題を診断する)
問題のトリアージ (緊急度の判断) と優先順位付け
問題を軽減するために適切なリソースを活用する。
問題に関する利害関係者とのコミュニケーション
問題が修復されたら、インシデント後のレビュー プロセスを通じてインシデントから学ぶことができます。これは重要なテーマであり、まったく別のモジュールで議論する必要があります。

**Resources**

- [Importance of incident response](https://learn.microsoft.com/training/modules/improve-reliability-incidents/2-importance)
- [Incident tracking](https://learn.microsoft.com/training/modules/improve-reliability-incidents/5-tracking)

<br><br>
