YAML 是繼 JSON 之後，被大量使用的另一套 Markup 語言。相較於 JSON，YAML 提供以下

[Collection]() - YAML 資料集的型態，包含 List 型態 (類似 JSON 的 Array) 與 Mapping 型態 (即 Dictionary)

資料結構話












# 基本語法

- 大小寫不同
- 資料不可分行
    - 每一行為單一一筆資料
- 使用縮排表現層級
    - 縮排只能用空格，不能用 tab
    - 同一層級的縮排必須對齊
		- 推薦子層比父層多使用 2 格空格
    - 
- 支援註解
    - 在符號 "#" 之後的字串都會被視為註解。


# 資料類型

## 基本資料型態

YAML 支援的基本資料型態包括

| 型態       | 說明與範例                                                                           |
| :------- | :------------------------------------------------------------------------------ |
| Boolean  | true (或是 True 與 TRUE) <br> false (或是 False 與 FALSE)                             |
| Null     | 使用符號 "~" 表示                                                                     |
| Integer  | 除了一般的十進制之外，也支援二進制 0b1010_0111_0100_1010_1110                                    |
| Float    |                                                                                 |
| Date     | 必須是 ISO 8601 格式，即 yyyy-MM-dd                                                    |
| Datetime | 必須是 ISO 8601 格式，日期與時間之間要使用 'T' 分隔，最後使用 '+' 表示時區 <br>如 2018-02-17T15:02:31+08:00 |
| String   | 不需使用單引號 (或是雙引號) <br>唯有特殊字符、非 ASCII 字串、或是空格，才需要使用引號                              |

## 資料集合型態

目前支援的集合型態包含 List 與 Dictionary 兩種。YAML 除了自己定義的格式之外，也支援 JSON 的表現方式

### List

YAML 稱為 Sequence，使用 "- " (符號 "-" 之後加上一個空格) 除一列出個元素
``` YAML
# JSON 的表現方式
[ 1, 2, 3, 4, 5]

# YAML 原生的表現方式
- 1
- 2
- 3
- 4
- 5
```

### Dictionary

YAML 稱為 Mapping。使用 ": " (符號 ":" 之後加上一個空格) 分隔 key 與 value
``` YAML
# JSON 的表現方式
key_A: { Key_1: Value_1, Key_2: Value_2 }

# YAML 原生的表現方式
Key_A: 
  Key_1: Value_1
  Key_2: Value_2
```

若 Key 為複合值，YAML 也支援使用符號 "? " 表示多個 key 的組合，再搭配 ": " 列出對應的值。要注意符號 "?" 與 ":" 之後要接空格
``` YAML
? 
  - Key_1
  - Key_2
: 
  - Value_1
  - Value_2
```

### 混用

``` YAML
# 範例一
Companies:
  - 
    id: 1
    name: company_1
    price: 100w
  - 
    id: 2
    name: company_2
    price: 200w

# 範例二
language:
  - Ruby
  - Perl
  - Python
website:
  YAML: yaml.org
  Ruby: ruby-lang.org
  Python: python.org
  Perl: use.perl.org
```

上面的範例等同於以下 JSON 
``` JSON
{
  "Companies": [
    {
      "id": 1,
      "name": "company_1",
      "price": "100w"
    },
    {
      "id": 2,
      "name": "company_2",
      "price": "200w"
    }
  ]
}

{
  "language": [ "Ruby", "Perl", "Python" ],
  "websute": {
    "YAML": "yaml.org",
    "Ruby": "ruby-lang.org",
    "Python": "python.org",
    "Perl": "use.perl.org"
  }
}
```

## 類型轉換



# 錨點與引用

YAML 有類似 JSON Reference 的功能，稱為錨點 ("&") 與引用 ("\*")。透過錨點建立 Reference，再透過引用去參考錨點的值：
``` YAML
default: &default
  adapter: postgres
  host: localhost

development:
  database: myapp_development
  <<: *default

test:
  database: myapp_test
  <<: *default
```

在 YAML 中可以使用 "<<" 表示合併到此，故上述的資料等同於下面的 JSON：
``` JSON
{
    "default": {
      "adapter": "postgres",
      "host": "localhost"
    },
    "development": {
      "database": "myapp_development",
      "adapter": "postgres",
      "host": "localhost"
    },
    "test": {
      "database": "myapp_test",
      "adapter": "postgres",
      "host": "localhost"
    }
}
```

