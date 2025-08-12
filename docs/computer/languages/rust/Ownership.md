# 擁有權

擁有權是 Rust 最獨特的核心特色，這個核心特色影響了整個 Rust 語言的實作方式。Rust 藉由擁有權，在不使用 Garbage Collector 技術的情況下保證記憶體的安全性。在 Rust 中擁有權在實作上應該類似 C++ 的 RAII 機制，以下為 Rust 的擁有權規範：

- 所有的值都有一個 owner
    - 所有的值在同一時間都只有一個 owner
- 當 owner 消滅，值也將一併消滅

下面使用 String 作為範例。當 String object "s" 離開了 scope ，他的生命週期就到底；Rust 將會呼叫一個叫做 "drop" 的特殊函式將字串 "s" 消滅。將記憶體釋放的程式碼，就必須放在這個特殊的函式。

``` rust
{
    let s = String::from( "Hello" );    //    值 "Hello" 的 Owner 為變數 s
}                                       //    離開此 scope 之後，變數 s 就會被消滅；連帶的值 "Hello" 也會被消滅

fn dump( s: String ) {                  //    Copy by value，將會觸發 copy constructor 導致擁有權交換
    println!( "", s );
}                                       //    離開此 scope 之後，變數 s 就會被消滅；連帶的值 "Hello" 也會被消滅

fn main() {
    let s = "Hello".to_string();
    dump( s );                          //    因為函式的參數不是 reference，所以這邊呼叫 copy constructor 導致擁有權交換
    println!( "s = {}", s );            //    因為擁有權交換，無法 access "s"。這邊會發生錯誤
}
```

在 Stack 上的資料其大小在編譯時已知，所以可以直接在 = operator 中使用複製的方式；但是在 Heap 的資料大小在編譯時不確定，故不會用複製的方式。如果要用複製的方式處理 Heap 上的資料，不能使用 = operator，而是應該呼叫 clone() 函式。Rust 也開放透過設定 Copy trait 的方式，讓 = operator 中使用複製而非轉移擁有權的方式；但是，有實作出 drop() 函式的類別是不允許設定 Copy trait 的。


## RAII

在 C++ 中，使用了被稱為 RAII (Resource Acquisition Is Initialization) 的規範來進行資源的管理。以下為 C++ RAII 的規範：

- 所有需要的資源都包裝在同一個類別 (class) 中
    - 類別的 Constructor 必須初始化類別中所需的資源
        - 若是失敗，則應該丟出一個 Exception
    - 類別的 Destrcutor 必須釋放所有的資源
        - 永遠不會丟出 Exception
- 類別中的資源，包含
    - 暫時性的資源，其生命週期必須明確
        - 建立與釋放必須成對，不會有建立而沒有釋放的狀況
    - 生命週期與其他的資源綁定
- 使用 std::unique_ptr、std::shared_ptr、std::lock_guard、std::unique_lock、或是 std::shared_lock 管理資源



