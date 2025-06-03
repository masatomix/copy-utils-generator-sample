
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
|fromField|任意|(コピー先のプロパティ名が異なる場合)<br />マッピング元プロパティ名を指定<br />同名のプロパティだけコピーすればよい場合は空でOK|
|toField|任意|(コピー先のプロパティ名が異なる場合)<br />マッピング先プロパティ名を指定<br />同名のプロパティだけコピーすればよい場合は空でOK|
|active|任意|trueの行は読み飛ばし、無視されます。<br/>デフォルトはfalseです。|



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

 クラス図にすると以下のとおりです。
 
![alt text](sample1.png)


名前が異なるプロパティが複数ある場合は、


| className                                          | methodName | fromClass                            | toClass                                 | fromField | toField |
| -------------------------------------------------- | ---------- | ------------------------------------ | --------------------------------------- | --------- | ------- |
| org.example.sample1.mapper.Sample1Dto2DomainMapper | toDomain   | org.example.sample1.model.Sample1Dto | org.example.sample1.model.Sample1Domain | from1     | to1     |
| org.example.sample1.mapper.Sample1Dto2DomainMapper | toDomain   | org.example.sample1.model.Sample1Dto | org.example.sample1.model.Sample1Domain | from2     | to2     |

のように「``className,methodName,fromClass,toClass``」を繰り返し記述してください。


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

#### 除外カラム

| 項目名(すべて任意指定) | 説明                                   |
| ---------------------- | -------------------------------------- |
| ignoreColumn           | コピーから除外したいプロパティ名を指定 |


使用例:


| className                                          | methodName | fromClass                             | toClass                                 | fromField | toField | ignoreColumn |
| -------------------------------------------------- | ---------- | ------------------------------------- | --------------------------------------- | --------- | ------- | ------------ |
| org.example.sample5.mapper.Sample5Dto2DomainMapper | toDomain   | org.example.sample5.model.Sample5Dto3 | org.example.sample5.model.Sample5Domain |           |         | id           |
| org.example.sample5.mapper.Sample5Dto2DomainMapper | toDomain   | org.example.sample5.model.Sample5Dto3 | org.example.sample5.model.Sample5Domain |           |         | v1           |
| org.example.sample5.mapper.Sample5Dto2DomainMapper | toDomain   | org.example.sample5.model.Sample5Dto3 | org.example.sample5.model.Sample5Domain |           |         | v2           |
| org.example.sample5.mapper.Sample5Dto2DomainMapper | toDomain   | org.example.sample5.model.Sample5Dto3 | org.example.sample5.model.Sample5Domain | v3        | v3      |              |
    
実行結果(適宜整形しています)


```java

package org.example.sample5.mapper;


@Mapper(componentModel = "spring" , unmappedTargetPolicy = ReportingPolicy.WARN  )
public interface Sample5Dto2DomainMapper {

    @Mapping(target = "id" , ignore = true)
    @Mapping(target = "v1" , ignore = true)
    @Mapping(target = "v2" , ignore = true)
    @Mapping(source = "v3", target = "v3")
    Sample5Domain toDomain(Sample5Dto3 source);


    @Mapping(target = "id" , ignore = true)
    @Mapping(target = "v1" , ignore = true)
    @Mapping(target = "v2" , ignore = true)
    @Mapping(source = "v3", target = "v3")
    void toDomainUpdate(Sample5Dto3 source, @MappingTarget Sample5Domain target);

}
```

参考: [Sample5: 複数のオブジェクトから一つのオブジェクトを作成する](https://github.com/masatomix/MapStructSample?tab=readme-ov-file#sample5-%E8%A4%87%E6%95%B0%E3%81%AE%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E3%81%8B%E3%82%89%E4%B8%80%E3%81%A4%E3%81%AE%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E3%82%92%E4%BD%9C%E6%88%90%E3%81%99%E3%82%8B)

![alt text](sample5.png)



#### 

------

その他、ときどき使うカラム




以上