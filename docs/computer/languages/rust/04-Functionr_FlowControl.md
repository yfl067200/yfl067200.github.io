# 函式與流程控制

Rust 的語法可以分成兩種：

- statements
    - 進行某些動作，但是不回傳任何值
	- 使用分號 (;) 作為結束符號
- Expressions
    - 進行某些動作，但是會回傳值
    - 回傳的值必須符合型態
	- 為 statement 的子集合
	    - 不包含分號 (;)

Expressions 的最後一個定義說明 Expressions 不包含分號 (;)，這是一個非常奇怪的規定，很難確定為什麼 Rust 要這麼規定。只能記住，在 assign 動作 (= 符號) 的右側，必須是 expression。不論是函式、或是新的 scope (使用新的大括號 ({}) 收集的程式碼)，最後一行都必須 **不** 加分號 (;)，以確保為 Expression。

## 函式

與其他語言一樣，Rust 程式可以被切割成多個函式。在 Rust 中建立函式的方式如下，包含 main 函式：

``` rust
fn ${NAME}( [${ARG}: {TYPE}[, ${ARG}: {TYPE}]] ) [-> ${TYPE}] {
}
```

與其他語言不同的地方在於，Rust 不需要在呼叫函式之前先宣告函式。雖然參數是函式的 signature 的一部分，但是在同一個 scope 中不能有一個以上相同名稱的函式。不同於變數宣告，Rust 要求在函式中的參數與返回值，必須明確的指定資料型態。

如果函式有返回值，就必須將返回值的程式碼變成 expression。在函式中回傳值的部分，可以透過 return statement 回傳；因為這是 statement，所以可以在後面加上分號 (;) 作為 statement 的結束。但是，在回傳的部分不使用 return statement，直接使用 express。以下為範例，不論是透過 return statement，或是 expression 都可以正常讓函式傳出回覆值。

``` rust
fn main() {
    let a = over_zero( -3 );
	let b = over_zero( 3 );
	println!( "a = {}, and b = {}", a, b );

    let A = alter_over_zero( -5 );
    let B = alter_over_zero( 5 );
	println!( "A = {}, and B = {}", A, B );
}

fn over_zero( x: i8 ) -> bool {
    if 0 > x {
	    false
	} else {
	    true
	}
}

fn alter_over_zero( x: i8 ) -> bool {
    if 0 > x {
	    return false;
	} else {
	    return true;
	}
}
```


## 流程控制

在 Rust 中支援的流程控制不多，包含 if expression。而迴圈部分，Rust 支援 loop、while、與 for 3 種迴圈

### if expression

Rust 的 if expression 跟其他語言一樣，但是 Rust 並沒有類似 switch case 這種語法 candy。另外一件事是，在 Rust 中要求 if statement 必須使用大括弧 ({}) 包含 statements，不論是否只有一行 statement。在 if 之後的 ${CONDITION} 必須是 expression，且返回值必須為布林型態。

``` rust
    if ${CONDITION} {
	[} else if ${CONDITION} {]
	[} else {]
	}
```

#### let 搭配 if expression

類似 C 的語法 candy，A == B ? C = 1 : C = ，Rust 也有同樣的語法 candy。如之前所說，使用大迴圈 ({}) 組成的 scope 最後一行程式碼如果不加分號結尾，就可以作為 exrepeesion 返回值；在這我們就是利用這個特色，在 if statement 後的 scope 使用 expression 返回

``` rust
fn blow_zero( x: i8 ) -> bool {
    let result = if 0 > x { true } else { false };
	return result;
}
```

### 迴圈

Rust 支援 3 種迴圈，分別是

- loop
    - 沒有設定結束條件，將不斷地進行迴圈
- while
    - 只要 while statement 中的 condition 符合，將會不斷的進行迴圈
- for
    - 在 for statement 中明確的指出要跑幾圈

#### loop 迴圈

Rust 也支援 break 與 continue 語法，可以控制迴圈；在 Rust 中，Break statement 除了離開迴圈之外，還有兩種額外功能，分別是返回值，與跳出多重迴圈的功能 (需搭配 loop label)。以下是範例：

``` rust
fn main () {
    let mut counter = 0;

    //    Setting outer loop with lable 'outer
    let result = 'outer: loop {

        loop {
            counter += 1;
		    if 10 < counter {
			    //    It will break out loop with label 'outer
			    break 'outer counter * 2;
		    }
		}    //    Normal loop statement doesn't need semicolon to finish

        //    There is noway to reach these code
    	if 100 < counter {
	        break counter * 2;
        }
    };    //    The scope end with semicolon due to it is a let statement

	println!( "Result value from loop is {result}" );
}
```

loop 與 break 的語法如下。可以在 loop 建立 label，而 break 可以同時跳脫多重 loop，並返回值。注意上述範例，loop 迴圈最後不需要分號 (;) 做為結束符號，而 outer 這個 loop 最後必須使用分號 (;) 是因為他是 let statement，需要分號 (;) 做為結束符號。

``` rust
['{LABEL}:] loop {
}

break [ ${LABLE} ][ ${VALUE} ];
```

#### while 與 for 迴圈

與 loop 迴圈不同之處在於，while 與 for 迴圈都是有條件的迴圈。一旦 while 或是 for statement 中的 condition 不符合，迴圈就會結束；而 condition 必須是 expression，且返回布林值。對於 while 與 for 迴圈兩者的差異是，while 迴圈的 condition 是比對某些條件是否成立，而 for 迴圈是逐一跑過某個 set 中符合條件的值。

``` rust
//    while loop
while ${CONDITION} {
}

for ${CONDITION} {
}
```
