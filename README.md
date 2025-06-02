
## Setup 

```console
$ https://github.com/masatomix/copy-utils-generator-sample.git
$ cd copy-utils-generator-sample
$ npm i 
```

## Usage

```console
$ npx generate-class --help
Options:
  --version    Show version number                                     [boolean]
  --excelPath  Excel file Path            [string] [default: "./classdata.xlsx"]
  --output     Output directory                   [string] [default: "./output"]
  --help       Show help                                               [boolean]
```

```console
$ npx generate-mapping --help
Options:
  --version    Show version number                                     [boolean]
  --excelPath  Excel file Path          [string] [default: "./mappingdata.xlsx"]
  --output     Output directory                   [string] [default: "./output"]
  --help       Show help                                               [boolean]
$ 
```

## Sample

```console
$ npx generate-class --excelPath ./classdata.xlsx --output output
INFO [2025-06-02 15:32:36.527 +0900]: output/org/example/sample1/model/Sample1Dto.java ファイル出力完了
    module: "FileClassRepository"
INFO [2025-06-02 15:32:36.530 +0900]: output/org/example/sample1/model/Sample1Domain.java ファイル出力完了
    module: "FileClassRepository"
INFO [2025-06-02 15:32:36.533 +0900]: output/org/example/sample2/model/Sample2Dto.java ファイル出力完了
    module: "FileClassRepository"
INFO [2025-06-02 15:32:36.535 +0900]: output/org/example/sample2/model/Sample2Domain.java ファイル出力完了
    module: "FileClassRepository"
INFO [2025-06-02 15:32:36.538 +0900]: output/org/example/sample4/model/Sample4Domain.java ファイル出力完了
```

```console
$ npx generate-mapping --excelPath ./mappingdata.xlsx --output output
INFO [2025-06-02 15:31:41.139 +0900]: output/org/example/sample1/mapper/Sample1Dto2DomainMapper.java ファイル出力完了
    module: "FileClassRepository"
INFO [2025-06-02 15:31:41.149 +0900]: output/org/example/sample2/mapper/Sample2Dto2DomainMapper.java ファイル出力完了
    module: "FileClassRepository"
INFO [2025-06-02 15:31:41.157 +0900]: output/org/example/sample3/mapper/Sample3Dto2DomainMapper.java ファイル出力完了
    module: "FileClassRepository"
```

## 詳細

- [クラス生成ツールの詳細](./README_generate-class.md)
- MapStruct生成ツールの詳細
<!-- - [MapStruct生成ツールの詳細](./README_generate-class.md) -->