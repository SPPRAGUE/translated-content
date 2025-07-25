---
title: Age 標頭
slug: Web/HTTP/Reference/Headers/Age
l10n:
  sourceCommit: ee756fd51ccbc4820a4b334aa753648650ad1d51
---

HTTP **`Age`** {{Glossary("response header", "回應標頭")}}表示物件在代理快取中的時間（以秒為單位）。

標頭值通常接近零。如果值為 `0`，則該物件可能是從原始伺服器提取的；否則，值通常是作為代理的當前日期和 HTTP 回應中包含的 {{HTTPHeader("Date")}} 通用標頭之間的差異計算的。

<table class="properties">
  <tbody>
    <tr>
      <th scope="row">標頭類型</th>
      <td>{{Glossary("Response header", "回應標頭")}}</td>
    </tr>
    <tr>
      <th scope="row">{{Glossary("Forbidden request header", "禁止的請求標頭")}}</th>
      <td>否</td>
    </tr>
  </tbody>
</table>

## 語法

```http
Age: <delta-seconds>
```

## 指令

- `<delta-seconds>`
  - : 一個非負整數，表示物件在代理快取中的時間（以秒為單位）。

## 範例

```http
Age: 24
```

## 規範

{{Specifications}}

## 瀏覽器相容性

{{Compat}}

## 參見

- {{HTTPHeader("Cache-Control")}}
- {{HTTPHeader("Expires")}}
