---
title: "HtmlEncode, HtmlEncodeDoc, HtmlToPlainText"
topic: "spxml-functions/html"
source_files:
  - "raw/2026-06-17/spxml-functions/spxml_html_functions.md"
status: "draft"
---

# HtmlEncode, HtmlEncodeDoc, HtmlToPlainText

`HtmlEncode(str)` кодирует текст для HTML: заменяет `&` и `<`, а переводы строк преобразует в `<br/>`.

`HtmlEncodeDoc(str)` делает то же, но формирует полный HTML-документ с `<html>` и `<body>`.

`HtmlToPlainText(html)` преобразует HTML-строку в простой текст.
