# Sử dụng borrowed types cho đối số

## Mô tả

Sử dụng tính chất ép kiểu tham chiếu ngược của Rust để làm tăng độ linh hoạt cho code khi bạn 
đang lựa chọn kiểu cho đối số của hàm. Sử dụng cách này sẽ giúp hàm chấp nhận nhiều kiểu 
có thể truyền vào hơn.

Điều này cũng không giới hạn với các kiểu slice-able hay fat pointer. Thực tế thì chúng ta 
nên ưu tiên sử dụng các kiểu được mượn hơn là các kiểu đang mượn chính nó. 
Ví dụ như, `&str` thay vì `&String`, `&[T]` thay vì `&Vec<T>`, hoặc `&T` thay vì `&Box<T>`. 

Sử dụng các kiểu được mượn giúp bạn tránh được các lớp tham chiếu không mong muốn của các 
bản thân các kiểu này. Ví dụ kiểu `String` sẽ có lớp tham chiếu tới giá trị của nó, 
vậy nên `&String` sẽ có 2 lớp tham chiếu. Chúng ta có thể tránh điều này bằng các sử dụng `&str`, và để cho 
tham số kiểu `&String` được ép kiểu về `&str` khi gọi hàm.
## Ví dụ

Trong ví dụ này, chúng ta sẽ thể hiện sự khác biệt trong việc sử dụng `&String` và `&str` để làm đối số của hàm.
Ý tưởng cũng được áp dụng tương tự cho trường hợp `&Vec<T>` với `&[T]` hay là `&Box<T>` với `&T`.

Đoạn code ví dụ sẽ thực thi việc xác định xem 1 từ có chứa 3 nguyên âm liên tiếp hay không.
Chúng ta không cần sở hữu chuỗi ký tự đầu vào, nên chúng ta sẽ nhận vào tham chiếu của nó. 

Đoạn code như sau:

```rust
fn three_vowels(word: &String) -> bool {
    let mut vowel_count = 0;
    for c in word.chars() {
        match c {
            'a' | 'e' | 'i' | 'o' | 'u' => {
                vowel_count += 1;
                if vowel_count >= 3 {
                    return true
                }
            }
            _ => vowel_count = 0
        }
    }
    false
}

fn main() {
    let ferris = "Ferris".to_string();
    let curious = "Curious".to_string();
    println!("{}: {}", ferris, three_vowels(&ferris));
    println!("{}: {}", curious, three_vowels(&curious));

    // This works fine, but the following two lines would fail:
    // println!("Ferris: {}", three_vowels("Ferris"));
    // println!("Curious: {}", three_vowels("Curious"));

}
```

Cách viết này vẫn ổn bởi vì chúng ta truyền vào tham số kiểu `&String`. Nếu ta bỏ
comments 2 dòng cuối, đoạn code sẽ fail. Điều này bởi vì kiểu `&str` sẽ không được ép về
kiểu `&String`. Ta có thể sửa điều này dễ dàng bằng cách đổi kiểu của đối số đầu vào. 

Ví dụ nếu chúng ta đổi khai báo của hàm sang:  

```rust, ignore
fn three_vowels(word: &str) -> bool {
```

thì cả 2 phiên bản code đều được biên dịch và in ra cùng kết quả.

```bash
Ferris: false
Curious: true
```

Nhưng đó không phải là tất cả! Có nhiều điều để bàn luận trong câu chuyện này.
Có lẽ bạn sẽ nói với bản thân rằng: chẳng vấn đề gì cả, tôi chỉ cần không bao giờ
dùng kiểu `&'static str` cho tham số đầu vào (như cách chúng ta dùng `"Ferris"`).
Kể cả khi bỏ qua ví dụ này, bạn vẫn sẽ thấy việc sử dụng `&str` giúp cho code của bạn
linh hoạt hơn là dùng `&String`.

Hãy cùng lấy 1 ví dụ mà đầu vào là 1 câu văn, và ta muốn xác định xem liệu các từ trong
đoạn văn này có chứa 3 nguyên âm liên tiếp không. Chúng ta chắc chắn muốn sẽ dụng lại hàm
trên và đơn giản lặp qua các từ trong đoạn văn, sau đó gọi hàm để kiểm tra.

Đoạn code như sau:

```rust
fn three_vowels(word: &str) -> bool {
    let mut vowel_count = 0;
    for c in word.chars() {
        match c {
            'a' | 'e' | 'i' | 'o' | 'u' => {
                vowel_count += 1;
                if vowel_count >= 3 {
                    return true
                }
            }
            _ => vowel_count = 0
        }
    }
    false
}

fn main() {
    let sentence_string =
        "Once upon a time, there was a friendly curious crab named Ferris".to_string();
    for word in sentence_string.split(' ') {
        if three_vowels(word) {
            println!("{} has three consecutive vowels!", word);
        }
    }
}
```

Chạy đoạn code trên với hàm được khai báo đối số đầu vào kiểu `&str`
sẽ in ra

```bash
curious has three consecutive vowels!
```

Đoạn code này sẽ không chạy nếu hàm của chúng ta được khai báo với đối số đầu vào kiểu `&String`. 
Bởi vì những đoạn kí tự sẽ có kiểu `&str` chứ không phải kiểu `&String` 
và nó sẽ cần biến đầu vào có kiểu `&String`, việc chuyển đổi này không được thực hiện tự động,
trong khi việc ép kiểu từ `String` sang `&str` thì ít tốn tài nguyên và tự động.

## Các nguồn tham khảo

- [Rust Language Reference on Type Coercions](https://doc.rust-lang.org/reference/type-coercions.html)
- Để hiểu sâu hơn về `String` và `&str`, hãy xem qua
  [chuỗi bài viết này (2015)](https://web.archive.org/web/20201112023149/https://hermanradtke.com/2015/05/03/string-vs-str-in-rust-functions.html)
  by Herman J. Radtke III
