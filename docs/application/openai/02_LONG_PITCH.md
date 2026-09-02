# 長い説明

Agent tool の local Runtime には扱いやすい操作面が必要だが、その操作面が無言で権限境界になることを許してはならない。

GUI-Shell は UI state、Adapter metadata、memory、diagnostics、LLM output を非権限入力として扱う。Shell Core contract が統治状態を定義し、Rust broker が native helper と IPC の境界を担う。local validation は Schema、conformance、manifest、release gate、Windows evidence の意味を分離して検査する。

公開 repository は、墨消し済み Windows proof asset と外部レビュー資料を含む。ただし公開 copy は canonical release evidence ではなく、完成製品 release、owner GO、OpenAI endorsement を意味しない。

## 外部公開用英語射影（非正本）

```text
GUI-Shell separates usable operator surfaces from authority. UI state, adapter metadata, diagnostics, memory, and LLM output remain non-authoritative; contracts and a Rust broker preserve the governed boundary. Public proof copies support review but do not establish release readiness.
```
