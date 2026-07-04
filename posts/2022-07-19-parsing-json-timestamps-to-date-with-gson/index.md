---
title: GsonでjsonのタイムスタンプをDate型に変換
categories:
  - 開発
date: 2022-07-19 17:32:19
tags:
  - Java
---

以下のようなResultsetの`getTimestamp()`で取得したようなタイムスタンプを含むjsonを、GsonでDate型のフィールドを持つオブジェクトに変換する。


```json
[
  {
    "id": "1",
    "date": "2022-07-18 20:01:54.0"
  }
  {
    "id": "2",
    "date": "2022-07-18 21:31:15.0"
  }
]
```


## GsonBuilder

```java
import java.util.Date;

public class SampleDTO {
	private int id;
	private Date date;
	
	public int getId() {
		return id;
	}
	public void setId(int id) {
		this.id = id;
	}
	public Date getDate() {
		return date;
	}
	public void setDate(Date date) {
		this.date = date;
	}
	public SampleDTO(int id, Date date) {
		this.id = id;
		this.date = date;
	}
}
```

```java
InputStream in = conn.getInputStream();
BufferedReader br = new BufferedReader(new InputStreamReader(in, "UTF-8"));

Gson gson = new GsonBuilder().setDateFormat("yyyy-MM-dd HH:mm:ss.S").create();
JsonReader jr = new JsonReader(br);

Type listType = new TypeToken<List<SampleDTO>>() {
}.getType();

List<SampleDTO> messages = gson.fromJson(jr, listType);
```

`Gson()`の代わりに`GsonBuilder()`でインスタンスを生成し、`.setDateFormat()`でjsonのタイムフォーマットにあわせた指定をすることで、タイムスタンプをオブジェクトに変換できる。

## 参考

- [SimpleDateFormat (Java Platform SE 8 )](https://docs.oracle.com/javase/jp/8/docs/api/java/text/SimpleDateFormat.html)
  - 日付パターンが記載されている
- [GsonBuilder (Gson 2.9.0 API)](https://www.javadoc.io/doc/com.google.code.gson/gson/latest/com.google.gson/com/google/gson/GsonBuilder.html)