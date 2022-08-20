# Giới thiệu

## Tham gia đóng góp

Nếu bạn muốn tham gia đóng góp cho quyển sách, bạn có thể tham khảo cẩm nang hướng dẫn
[contribution guidelines](https://github.com/rust-unofficial/patterns/blob/master/CONTRIBUTING.md).

## Design patterns

Khi phát triển phần mềm, chúng ta thường gặp nhiều vấn đề với những điểm tương đồng nhau 
bất kể trong môi trường nào. Ngoài việc cần chú trọng trong các chi tiết của việc thực thi 
để giải quyết vấn đề trước mắt, chúng ta cũng có thể khái niệm hóa các chi tiết này để tìm 
ra các quy chuẩn có thể áp dụng cho nhiều trường hợp.

Design patterns là tập hợp các giải pháp mang tính tái sử dụng và đã được kiểm nghiệm cho 
các vấn đề hay lặp lại trong xây dựng và phát triển phần mềm. Chúng giúp phần mềm trở nên 
chuẩn hóa, đồng thời dễ bảo trì và mở rộng hơn. Ngoài ra, những khuôn mẫu này còn là ngôn ngữ 
chung của các developer, chúng là công cụ hoàn hảo để giao tiếp một cách hiệu quả khi 
cùng giải quyết vấn đề trong team.

## Design patterns trong Rust

Rust không phải là ngôn ngữ thuần hướng đối tượng và sở hữu các đặc tính của lập trình chức năng, 
ngoài ra nó còn có hệ thống strong type và borrow checker. Tất cả phối hợp với nhau khiến Rust 
trở nên độc nhất.

Cũng vì điều này, Rust design patterns rất khác so với các ngôn ngữ hướng đối tượng truyền thống. 
Vậy nên chúng tôi quyết định viết cuốn sách này. Chúng tôi hi vọng bạn sẽ thích nó!
Cuốn sách được chia làm 3 phần chính:

- [Idioms](./idioms/index.md): các nguyên tắc cần tuân theo khi viết code.
  Chúng là các quy tắc chung của cộng đồng lập trình Rust.
  Bạn không nên phá vỡ chúng trừ khi bạn có lý do chính đáng.
- [Design patterns](./patterns/index.md): các phương pháp cho các vấn đề
  chung thường gặp.
- [Anti-patterns](./anti_patterns/index.md): cũng là các phương pháp cho các vấn đề
  chung thường gặp. Nhưng không như patterns, anti-patterns đem lại nhiều vấn đề hơn là lợi ích.
