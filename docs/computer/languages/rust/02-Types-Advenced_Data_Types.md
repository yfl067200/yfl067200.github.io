## 進階資料型態

與基本資料型態不同之處，進階資料型態的資料是存放在 Heap 而非 Statck。如同 [Stack 與 Heap 一節](./02-Types.html#stack-%E8%88%87-heap)所述，將資料放在 Heap 包含取得與釋放記憶體空間的動作，所有的資料管理都必須由程式自身控制。所以進階資料型態就必須處理包含取得與釋放記憶體空間的動作，我們將釋放記憶體的動作留到第 3 章[擁有權](./03-Ownership.md)說明

### 字串 (String)

在 Rust 中存放字串的方式有兩個，使用 String 類別與字元陣列 (Char Array)。兩者的主要差別在於，要建立字元陣列必須指定陣列大小，建立之後就不能調整。而 String 類別因為將資料存放在 Heap 中，所以長度可以調整。String object 的基本操作如下：

``` rust
//    declaring a new String object ${NAME} [from ${STRING}]
let [mut] ${NAME} = [ String::new() | String::from( "${STRING}" ) ];

//    declaring a new String object ${NAME} from an array of character
let [mut] ${NAME} = ${ARRAY of CHARACTER}.to_string();

//    appending String object with ${STRING} at the end of String object ${NAME}
${NAME}.push_str( "${STRING}" );

//    appending String object with ONE character ${CHARACTER} at the end of String object ${NAME}
${NAME}.push( '${CHARACTER}' );

//    concatenation with + operator
let ${STRING_A} = string::from( "${STRING}" );
let ${STRING_B} = string::from( "${STRING}" );
let ${STRING_C} = ${STRING_A} + &${STRING_B};    //    String object ${STRING_A} give it's ownership to ${STRING_C}, and can NOT access it any more.
```

上述的幾個範例中最值得注意的是將兩個 String objects 合成一個，將導致第一個 String 交出擁有權這件事。

### 結構 (Struct)

struct 是一個供使用者自行組合各種型態，進而建立客製化型態的結構。Struct 雖然類似 Tuple ，但是兩者最大的不同在於，struct 中每一個欄位都需要命名，這讓 struct 比起 Tuple 來說更加好用；因為不像 Tuple 是利用 index 來存取欄位，很可能會因為程式碼的調整導致 index 不同。而 stuct 不管程式碼如何調整，各欄位的名稱不太會改變。struct 的語法如下：

``` rust
//    declaration
struct ${STRUCT_NAME} {
    ${NAME}: ${TYPE},
    [${NAME}: ${TYPE},]
}

//    implementation
let X = ${STRUCT_NAME} {
    ${NAME} = ${VALUE}
};
```

#### Tuple struct

Rust 支援沒有命名欄位的 struct，稱為 tuple struct；這玩意其實就是 tuple。如果兩個 Tuple 內涵的資料型態一樣，如下例，Rust 將視這兩個 Tuple 為同一型態；這可能會導致誤將不同的 Tuple 的 Instance 作為另一個 Tuple 的 copy constructor 參數。為了避免這種問題，Rust 會將不同的 struct tuple 視為不同的資料型態。

``` rust
struct point( i32, i32, i32 );
struct color( i32, i32, i32 );

fn main() {
    let black = color( 0, 0, 0 );
	let current = point( 0, 0, 0 );
}
```


