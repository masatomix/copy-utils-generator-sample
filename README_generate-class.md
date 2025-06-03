
## クラス生成ツールの使い方。設定ファイル classdata.xlsx の書き方。

あらためてUsage

```console
$ npx generate-class --help
Options:
  --version    Show version number                                     [boolean]
  --excelPath  Excel file Path            [string] [default: "./classdata.xlsx"]
  --output     Output directory                   [string] [default: "./output"]
  --help       Show help                                               [boolean]
```

このインプットとなるExcelデータについて。

|項目名|必須|説明|指定の単位|
|---|---|---|---|
|className|必須|クラス名を指定します。パッケージ付きのFQCNで指定をしてください。|フィールド単位|
|type|必須|フィールドの型を指定します。パッケージ付きのFQCNで指定をしてください。|フィールド単位|
|fieldName|必須|フィールド名を指定します。|フィールド単位|
|active|任意|falseの行は読み飛ばし、無視されます。<br />未指定時のデフォルトはtrue(有効)です。|フィールド単位|
|immutable|任意|trueの行はImmutableのクラスを出力します。<br />未指定時のデフォルトはfalseです。|クラス単位<br />(クラスごとの先頭行を参照します)|


例:

| className                               | type              | fieldName    |
| --------------------------------------- | ----------------- | ------------ |
| org.example.sample1.model.Sample1Dto    | java.lang.String  | param1       |
| org.example.sample1.model.Sample1Dto    | java.lang.String  | param2       |
| org.example.sample1.model.Sample1Dto    | java.lang.String  | param3       |
| org.example.sample1.model.Sample1Domain | java.lang.String  | param1       |
| org.example.sample1.model.Sample1Domain | java.lang.Integer | param2       |
| org.example.sample1.model.Sample1Domain | java.lang.String  | domainParam3 |

上記のようなExcelを作成し(sample1.xlsxとします)、実行してみます。

```console
$ npx generate-class --excelPath sample1.xlsx
```

> ときおり、Excelファイルにゴミ行が混入してエラーになったりします。その場合は適宜
> データ行以下の行を、行削除してみてください。

出力結果は以下の通りです。
 
```java
package org.example.sample1.model;

import lombok.Data;
import lombok.*;

@Data @NoArgsConstructor
@AllArgsConstructor
@Builder
public class Sample1Dto {
  private  java.lang.String param1;
  private  java.lang.String param2;
  private  java.lang.String param3;
}
```

```java
package org.example.sample1.model;

import lombok.Data;
import lombok.*;

@Data @NoArgsConstructor
@AllArgsConstructor
@Builder
public class Sample1Domain {
  private  java.lang.String param1;
  private  java.lang.Integer param2;
  private  java.lang.String domainParam3;
}
```

上記のコードが生成されました。


immutableもやってみます。

| className                               | type                             | fieldName    | immutable |
| --------------------------------------- | -------------------------------- | ------------ | --------- |
| org.example.sample1.model.Sample1Dto    | java.util.List<java.lang.String> | param1s      |           |
| org.example.sample1.model.Sample1Dto    | java.lang.String                 | param2       |           |
| org.example.sample1.model.Sample1Dto    | java.lang.String                 | param3       |           |
| org.example.sample1.model.Sample1Domain | java.lang.String                 | param1       | TRUE      |
| org.example.sample1.model.Sample1Domain | java.lang.Integer                | param2       |           |
| org.example.sample1.model.Sample1Domain | java.lang.String                 | domainParam3 |           |



Sample1Domain だけ immutable に指定しました。
上記のようなExcelを作成し(sample2.xlsxとします)、実行してみます。


```console
$ npx generate-class --excelPath sample2.xlsx
```


```java
package org.example.sample1.model;

import lombok.Data;
import lombok.*;

@Data @NoArgsConstructor
@AllArgsConstructor
@Builder
public class Sample1Dto {
  private  java.util.List<java.lang.String> param1s;
  private  java.lang.String param2;
  private  java.lang.String param3;
}
```


```java
package org.example.sample1.model;


import lombok.Value;
import lombok.*;

@Value
@AllArgsConstructor
@Builder
public class Sample1Domain {
  private final java.lang.String param1;
  private final java.lang.Integer param2;
  private final java.lang.String domainParam3;
}

```

Sample1Domain だけ、Immutableなクラスとなりました！


activeのプロパティも、列を追加してtrue/falseを設定してください。

以上
