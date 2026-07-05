---
title: 'SQLiteのSQL文メモ'
category: '技術メモ'
published_at: 2022-07-18 21:43:38
tags: ['SQLite', 'SQL']
---

## タイムスタンプのカラムがあるテーブルを作成

```sql
CREATE TABLE message (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  body TEXT,
  created_at TIMESTAMP DEFAULT (datetime(CURRENT_TIMESTAMP,'localtime'))
)
```

タイムスタンプのカラム名には`created_at`という名前をつけるという命名規約があるらしい。

- [データベースオブジェクトの命名規約](https://qiita.com/genzouw/items/35022fa96c120e67c637)


## カラムの一覧を表示

```sql
PRAGMA table_info('テーブル名')
```


## テーブルの一覧を表示

```sql
SELECT * FROM sqlite_master WHERE type='table'
```


