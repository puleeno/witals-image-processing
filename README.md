## 🛠️ 1. Technical Stack (Rust Ecosystem)

Việc chọn thư viện trong Rust rất quan trọng vì nó ảnh hưởng trực tiếp đến tốc độ biên dịch và tài nguyên RAM.

* **Core Library:** `image` crate (Phổ biến, hỗ trợ nhiều định dạng).
* **High Performance:** `photon-rs` (Dùng cho các bộ lọc và hiệu ứng cực nhanh, có hỗ trợ WebAssembly).
* **Turbo Encoders:** `zune-jpeg` (Encoder/Decoder JPEG nhanh nhất hiện nay cho Rust).
* **Concurrency:** `Rayon` (Để xử lý ảnh song song trên nhiều nhân CPU - cực kỳ cần thiết cho Cloud Browser).

---

## 📊 2. Image Processing Pipeline Specs

Hệ thống nên tuân thủ quy trình sau để đảm bảo hiệu suất:

| Tính năng | Đặc tả kỹ thuật (Specs) | Mục đích |
| --- | --- | --- |
| **Định dạng đầu vào** | JPG, PNG, WEBP, AVIF, HEIC, GIF. | Đa dạng nguồn dữ liệu. |
| **Định dạng đầu ra** | **Ưu tiên WebP & AVIF.** | Giảm ~30-50% dung lượng so với JPG/PNG. |
| **Resizing Logic** | Lanczos3 hoặc Catmull-Rom (High Quality). | Đảm bảo ảnh không bị mờ khi thu nhỏ. |
| **Metadata** | **Strip All** (Loại bỏ EXIF, GPS, Profile). | Giảm thêm vài KB cho mỗi ảnh. |
| **Màu sắc** | Chuyển đổi về sRGB (8-bit). | Đảm bảo hiển thị đồng nhất trên mọi trình duyệt. |

---

## ⚡ 3. Performance & Resource Constraints

Vì bạn đang dùng VPS để chạy Panel, xử lý ảnh không được phép "ăn" hết tài nguyên hệ thống.

* **Memory Limit:** Giới hạn tối đa **200MB RAM** cho mỗi task xử lý ảnh (sử dụng `Bounded Channel` trong Rust).
* **Time-to-Process:** Mục tiêu `< 100ms` cho một tấm ảnh 2K xuống Full HD.
* **Parallelism:** Giới hạn số lượng Thread xử lý ảnh bằng **Rayon ThreadPool** (thường bằng `số nhân CPU - 1`).
* **Caching Strategy:** Lưu trữ ảnh đã xử lý vào **LRU Cache** (Local SSD) hoặc chuyển tiếp qua **Smart Cache** của WitalsPanel.

---

## 🎨 4. Smart Transformation Features (Dành cho UI/UX)

Để hỗ trợ tốt cho ngách "Cloud Browser" hoặc "Panel":

1. **Lazy Loading Support:** Tự động tạo ảnh **BlurHash** hoặc **LQIP** (Low-Quality Image Placeholder) - một chuỗi base64 siêu nhỏ để hiển thị ngay lập tức khi ảnh gốc chưa tải xong.
2. **Adaptive Streaming:** Tự động phát hiện màn hình người dùng để trả về kích thước ảnh phù hợp (Responsive Images).
3. **Watermarking:** Chèn watermark dạng vector (SVG) để tiết kiệm tài nguyên hơn so với chèn dạng bitmap.

---

## 🚀 5. Ví dụ cấu trúc Module trong Rust

Bạn có thể tạo một module `image_processor` trong project của mình như sau:

```rust
use photon_rs::native::open_image;
use photon_rs::transform::resize;
use photon_rs::SamplingFilter;

pub fn optimize_for_web(input_path: &str, output_path: &str, width: u32, height: u32) {
    // 1. Load image
    let mut img = open_image(input_path).expect("Failed to open image");

    // 2. Resize với filter chất lượng cao
    img = resize(&img, width, height, SamplingFilter::Lanczos3);

    // 3. Save as WebP (Cần thêm crate hỗ trợ WebP)
    // photon_rs::native::save_image(img, output_path);
}

```
