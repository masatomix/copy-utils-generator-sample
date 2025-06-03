
## MapStructコード生成ツールの使い方。設定ファイル mappingdata.xlsx の書き方。

あらためてUsage

```console
$ npx generate-mapping --help
Options:
  --version    Show version number                                     [boolean]
  --excelPath  Excel file Path          [string] [default: "./mappingdata.xlsx"]
  --output     Output directory                   [string] [default: "./output"]
  --help       Show help                                               [boolean]
$ 
```


### 基本的な使い方

このインプットとなるExcelデータについて。

よく使うカラム

|項目名|必須|説明|
|--|--|--|
|className|必須|マッピング用クラス名を指定。FQCNで指定してください。|
|methodName|必須|マッピング用メソッド名を指定。|
|fromClass|必須|マッピング**元**クラス名を指定(FQCN)|
|toClass|必須|マッピング**先**クラス名を指定(FQCN)|
|fromField|任意|(コピー先のプロパティ名が異なる場合)<br />マッピング元プロパティ名を指定|
|toField|任意|(コピー先のプロパティ名が異なる場合)<br />マッピング先プロパティ名を指定|


例:

| className                                          | methodName | fromClass                            | toClass                                 | fromField | toField      |
| -------------------------------------------------- | ---------- | ------------------------------------ | --------------------------------------- | --------- | ------------ |
| org.example.sample1.mapper.Sample1Dto2DomainMapper | toDomain   | org.example.sample1.model.Sample1Dto | org.example.sample1.model.Sample1Domain | param3    | domainParam3 |

上記のようなExcelを作成し(mapping-sample1.xlsx)実行してみます。


```console
$ npx generate-mapping --excelPath mapping-sample1.xlsx 
```

出力結果は以下の通りです。

```java
package org.example.sample1.mapper;

import org.mapstruct.Mapper;
import org.mapstruct.Mapping;
import org.mapstruct.MappingTarget;
import org.mapstruct.factory.Mappers;

import org.mapstruct.ReportingPolicy;
import org.mapstruct.BeanMapping;
import org.mapstruct.DecoratedWith;
import org.mapstruct.InheritInverseConfiguration;

@Mapper(componentModel = "spring" , unmappedTargetPolicy = ReportingPolicy.WARN  )
public interface Sample1Dto2DomainMapper {

    @Mapping(source = "param3", target = "domainParam3")
    org.example.sample1.model.Sample1Domain toDomain(org.example.sample1.model.Sample1Dto source);

    @Mapping(source = "param3", target = "domainParam3")
    void toDomainUpdate(org.example.sample1.model.Sample1Dto source, @MappingTarget org.example.sample1.model.Sample1Domain target);
}
```


- ``Sample1Dto#param3`` を ``Sample1Domain#domainParam3`` にコピーするメソッド ``toDomain``
- 引数に渡した ``Sample1Domain target``に、``Sample1Dto source`` の各プロパティを更新するメソッド ``toDomainUpdate``

をもつクラス、``Sample1Dto2DomainMapper``が作成されました。
一応ですが、プロパティ名が一致している場合は、それらもコピーされます。

 
![alt text](sample1.png)


名前が異なるプロパティが複数ある場合は、


| className                                          | methodName | fromClass                            | toClass                                 | fromField | toField |
| -------------------------------------------------- | ---------- | ------------------------------------ | --------------------------------------- | --------- | ------- |
| org.example.sample1.mapper.Sample1Dto2DomainMapper | toDomain   | org.example.sample1.model.Sample1Dto | org.example.sample1.model.Sample1Domain | from1     | to1     |
| org.example.sample1.mapper.Sample1Dto2DomainMapper | toDomain   | org.example.sample1.model.Sample1Dto | org.example.sample1.model.Sample1Domain | from2     | to2     |

のように「className,methodName,fromClass,toClass」を繰り返し記述してください。


```java
package org.example.sample1.mapper;

@Mapper(componentModel = "spring" , unmappedTargetPolicy = ReportingPolicy.WARN  )
public interface Sample1Dto2DomainMapper {

    @Mapping(source = "from1", target = "to1")
    @Mapping(source = "from2", target = "to2")
    org.example.sample1.model.Sample1Domain toDomain(org.example.sample1.model.Sample1Dto source);

    ... 省略
}
```

となります。
MapStructの最低限の機能を使う場合は、概ねこれでOKです！



### 応用




その他、ときどき使うカラム


active
ignoreByDefault
ignoreColumn
qualifiedByName
uses
decorator
inverse




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

出力は以下の通り。
 
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