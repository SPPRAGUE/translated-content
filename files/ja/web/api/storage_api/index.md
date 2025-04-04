---
title: Storage API
slug: Web/API/Storage_API
---

{{securecontext_header}}{{DefaultAPISidebar("Storage")}}

ストレージ標準（Storage Standard）は、個々のウェブサイトのコンテンツがアクセス可能なデータを格納するすべての API およびテクノロジーで使用する、共通の共有ストレージシステムを定義しています。Storage API は、どのくらいのスペースを使用できるかや、すでにどのくらいのスペースを使用しているかを調べたり、{{Glossary("user agent","ユーザーエージェント")}}が他のもののための場所を空けるためにサイトデータを処分する前に、警告する必要があるかどうかを制御したりといった機能を、サイトのコードに提供します。

{{AvailableInWorkers}}

サイトストレージ — ストレージ標準によって管理しているウェブサイト用に格納されたデータ — には、次のものが含まれます。

- [IndexedDB データベース](/ja/docs/Web/API/IndexedDB_API)
- [Cache API のデータ](/ja/docs/Web/API/Cache)
- [サービスワーカーの登録](/ja/docs/Web/API/Service_Worker_API)
- {{domxref("window.localStorage")}} を使用して管理している [Web Storage API のデータ](/ja/docs/Web/API/Web_Storage_API)
- {{domxref("History.pushState()")}} を使用して保存した履歴状態情報
- [アプリケーションキャッシュ](/ja/docs/Web/HTML/Using_the_application_cache)
- [通知データ](/ja/docs/Web/API/Notifications_API)
- 維持される可能性があるその他の種類のサイトでアクセス可能なサイト固有のデータ

## サイトストレージユニット

ストレージ標準によって記述され、Storage API を使用して相互作用するサイトストレージシステムは、各オリジン（{{Glossary("origin")}}）に対して 1 つの**サイトストレージユニット**（site storage unit、サイトの保管単位）で構成されています。基本的に、すべてのウェブサイトまたはウェブアプリには、データを格納するための独自のストレージユニットがあります。以下の図は、内部に 3 つのストレージユニットを持つサイトストレージプールを示しています。これはストレージユニットがどのように異なるデータタイプを格納でき、異なるクォータ（最大ストレージ制限）を持つことができるかを示しています。

![サイトストレージプールが、さまざまな API からのデータとクォータに達する前に残っている可能性のある未使用スペースを含む複数のストレージユニットで構成されている様子を示す図。](StorageUnits.png)

- Origin 1 には、Web Storage のデータと IndexedDB のデータがありますが、空き容量もあります。現在の使用量はまだクォータに達していません。
- Origin 2 にはまだデータが格納されていません。内容を待っているだけの空の箱です。ただし、このオリジンは他の 2 つよりもクォータが少なくなります。アクセス頻度の低いサイト、またはデータストレージ要件が低いことが知られているサイトである可能性があります。
- Origin 3 のストレージユニットは完全にいっぱいです。クォータに達し、既存のデータを削除しない限り、これ以上データを格納できません。

{{Glossary("User agent","ユーザーエージェント")}}は様々なオリジンのクォータを決定するために様々なテクニックを使う可能性があります。最も可能性の高い方法の 1 つ — 実際に仕様が具体的に推奨するもの — は、個々のサイトの人気や使用レベルを考慮して、それらのクォータがどうあるべきかを判断することです。ブラウザーがこれらのクォータをカスタマイズするためのユーザーインターフェイスを提供することも考えられます。

## ボックスモード

各サイトストレージユニット内の実際のデータストレージは、その**ボックス**と呼ばれます。各サイトストレージユニットには、そのすべてのデータを配置するボックスが 1 つだけあり、そのボックスのデータ保持ポリシーを説明する**ボックスモード**があります。次の 2 つのモードがあります。

- `"best-effort"`（最大限の努力）
  - : ユーザーエージェントは可能な限りボックスに含まれているデータを保持しようと試みますが、ストレージスペースが少なくなり、ストレージの圧力を軽減するためにボックスを消去する必要がある場合は*ユーザーに警告しません*。
- `"persistent"`（永続的）
  - : ユーザーエージェントはデータを可能な限り長く保持し、`"persistent"` とマークされたボックスの消去を検討する前にすべての `"best-effort"` ボックスを消去します。persistent ボックスの消去を検討する必要がある場合、ユーザーエージェントはユーザーに通知し、必要に応じて 1 つ以上の persistent ボックスを消去する方法を提供します。

オリジンのボックスモードを変更するには、`"persistent-storage"`（永続的ストレージ）機能を使うパーミッションが必要です。

## データの永続化と消去

サイトやアプリに **`"persistent-storage"`** 機能のパーミッションがある場合は、{{domxref("StorageManager.persist()")}} メソッドを使用して、そのボックスを永続的にすることを要求できます。また利用特性や他の測定基準により、ユーザーエージェントがサイトのストレージユニットを永続的にすることを決定することも可能です。`"persistent-storage"` 機能のパーミッション関連のフラグ、アルゴリズム、およびタイプはすべてパーミッションの標準デフォルトに設定されています。ただし、**パーミッションの状態**（permission state）はオリジン全体で同じでなければならず、パーミッションの状態が `"granted"` （付与）されていない場合（何らかの理由で永続的ストレージ機能へのアクセスが拒否された場合）、そのオリジンのサイトストレージユニットのボックスモードは常に `"best-effort"` になります。

> [!NOTE]
> パーミッションの取得と管理の詳細については、[Permissions API の使用](/ja/docs/Web/API/Permissions_API/Using_the_Permissions_API)を参照してください。

サイトストレージユニットを消去するとき、オリジンのボックスは単一の実体として扱われます。ユーザーエージェントがそれを消去する必要があり、ユーザーが承認した場合、個々の API からデータのみを消去する手段を提供するのではなく、データストア全体を消去します。

ボックスが `"persistent"` とマークされている場合、その内容は、データのオリジン自身やユーザーが特別に消すことなしには、ユーザーエージェントによって消去されません。これには、ユーザーが「キャッシュを消去」や「最近の履歴を消去」のオプションを選択した場合などのシナリオが含まれます。ユーザーは、persistent サイトストレージユニットを削除するための許可を特に求められます。

## クォータと使用量の推定

ユーザーエージェントは、選択した任意のメカニズムを使用して、特定のサイトが使用できるストレージの最大量を決定します。この最大値がオリジンの**クォータ**（quota）です。サイトが使用しているスペースの量は、その**使用量**（usage）と呼ばれます。これらの値は両方とも推定値です。正確でない理由は次のようにいくつかあります。

- ユーザーエージェントは、これらの値がフィンガープリント（個人の推定）の目的で使用されるのを防ぐために、特定のオリジンが使用するデータの正確なサイズを不明瞭にしておくことが奨励されています。
- 格納されたデータの物理的サイズを縮小するために重複排除、圧縮、およびその他の方法が使用されることがあります。
- クォータは、オリジンが使えるスペースの控えめな概算値であり、オーバーランを防ぐためにデバイスで利用可能なスペースより小さくなければなりません。

ユーザーエージェントは、オリジンのクォータのサイズを決定するために選択した任意の方法を使用することができ、そして仕様では人気があるか頻繁に使用されるサイトに追加スペースを提供することを奨励しています。

特定のオリジンのクォータと使用量の推定値を決定するには、{{domxref("StorageManager.estimate", "navigator.storage.estimate()")}} メソッドを使用します。このメソッドは、解決すると、これらの数値を含む {{domxref("StorageEstimate")}} を受け取るという Promise を返します。例えば次のようにです。

```js
navigator.storage.estimate().then((estimate) => {
  // estimate.quota は見積もりクォータです
  // estimate.usage は見積もり使用バイト数です
});
```

## 仕様書

{{Specifications}}

## ブラウザーの互換性

### `StorageManager`

{{Compat}}

## 関連情報

- {{domxref("NavigatorStorage.storage", "navigator.storage")}}
- {{domxref("StorageManager")}}（`navigator.storage` から返されたオブジェクト）
- [Permissions API の使用](/ja/docs/Web/API/Permissions_API/Using_the_Permissions_API)
