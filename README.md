
## setup 

```
$ https://github.com/masatomix/copy-utils-generator-sample.git
$ cd copy-utils-generator-sample
$ npm i 
```

## Usage

```
$ npx generate-class --help
Options:
  --version    Show version number                                     [boolean]
  --excelPath  Excel file Path            [string] [default: "./classdata.xlsx"]
  --output     Output directory                   [string] [default: "./output"]
  --help       Show help                                               [boolean]
```

```
$ npx generate-mapping --help
Options:
  --version    Show version number                                     [boolean]
  --excelPath  Excel file Path          [string] [default: "./mappingdata.xlsx"]
  --output     Output directory                   [string] [default: "./output"]
  --help       Show help                                               [boolean]
$ 
```

## Sample

```
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
    module: "FileClassRepository"
INFO [2025-06-02 15:32:36.542 +0900]: output/org/example/sample4/model/Sample4Dto.java ファイル出力完了
    module: "FileClassRepository"
INFO [2025-06-02 15:32:36.545 +0900]: output/org/example/sample4/model/Order.java ファイル出力完了
    module: "FileClassRepository"
INFO [2025-06-02 15:32:36.548 +0900]: output/org/example/sample5/model/Sample5Dto1.java ファイル出力完了
    module: "FileClassRepository"
INFO [2025-06-02 15:32:36.553 +0900]: output/org/example/sample5/model/Sample5Dto2.java ファイル出力完了
    module: "FileClassRepository"
INFO [2025-06-02 15:32:36.557 +0900]: output/org/example/sample5/model/Sample5Dto3.java ファイル出力完了
    module: "FileClassRepository"
INFO [2025-06-02 15:32:36.558 +0900]: output/org/example/sample5/model/Sample5Domain.java ファイル出力完了
    module: "FileClassRepository"
INFO [2025-06-02 15:32:36.560 +0900]: output/org/example/sample6/model/Sample6Dto.java ファイル出力完了
    module: "FileClassRepository"
INFO [2025-06-02 15:32:36.562 +0900]: output/org/example/sample6/model/Sample6Domain1.java ファイル出力完了
    module: "FileClassRepository"
INFO [2025-06-02 15:32:36.564 +0900]: output/org/example/sample6/model/Sample6Domain2.java ファイル出力完了
    module: "FileClassRepository"
INFO [2025-06-02 15:32:36.565 +0900]: output/org/example/sample6/model/Sample6Domain3.java ファイル出力完了
    module: "FileClassRepository"
```

 
```
$ npx generate-mapping --excelPath ./mappingdata.xlsx --output output
INFO [2025-06-02 15:31:41.139 +0900]: output/org/example/sample1/mapper/Sample1Dto2DomainMapper.java ファイル出力完了
    module: "FileClassRepository"
INFO [2025-06-02 15:31:41.149 +0900]: output/org/example/sample2/mapper/Sample2Dto2DomainMapper.java ファイル出力完了
    module: "FileClassRepository"
INFO [2025-06-02 15:31:41.157 +0900]: output/org/example/sample3/mapper/Sample3Dto2DomainMapper.java ファイル出力完了
    module: "FileClassRepository"
INFO [2025-06-02 15:31:41.163 +0900]: output/org/example/sample4/mapper/Sample4Dto2DomainMapper.java ファイル出力完了
    module: "FileClassRepository"
INFO [2025-06-02 15:31:41.169 +0900]: output/org/example/sample5/mapper/Sample5Dto2DomainMapper.java ファイル出力完了
    module: "FileClassRepository"
INFO [2025-06-02 15:31:41.172 +0900]: output/org/example/sample6/mapper/Sample6Dto2DomainMapper.java ファイル出力完了
    module: "FileClassRepository"
```