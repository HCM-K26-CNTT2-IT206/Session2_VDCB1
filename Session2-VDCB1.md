# [Vận dụng cơ bản 1] – Sửa đổi prompt thiếu cấu trúc ràng buộc định dạng

**Sinh viên:** Hà Gia Bách

---

## 0. Tóm tắt đề bài

- **Mục tiêu:** AI trả về kết quả **ngắn gọn, dán thẳng được** vào báo cáo / tài liệu học tập.
- **Prompt lỗi (cũ):**

  > Hãy giải thích ngắn gọn các nội dung của CREATE, Few-shot, Ép định dạng JSON/Table.

- **Triệu chứng:** AI sinh ra lời chào xã giao dông dài, giải thích lòng vòng, không cần thiết.

**Nguyên nhân gốc:** prompt chỉ có **Request** (làm gì) mà thiếu **Type of output** (định dạng) và **Adjustments** (ràng buộc cấm). Chữ "ngắn gọn" là yêu cầu mơ hồ, AI không biết ngắn là bao nhiêu, theo hình thức nào, và không bị cấm viết lời dẫn → AI mặc định "lịch sự" và nói nhiều.

---

## BƯỚC 1: PHÂN TÍCH LỖI

### 1.1. Câu trả lời cũ của AI (minh họa khi chạy prompt lỗi)

> Chào bạn! 😊 Rất vui được hỗ trợ bạn tìm hiểu về chủ đề này. Đây là một chủ đề rất thú vị và quan trọng trong lĩnh vực Prompt Engineering. Dưới đây là phần giải thích của mình nhé!
>
> **CREATE** là một khung viết prompt gồm 6 thành phần: Character, Request, Examples, Adjustments, Type of output và Extras. Khi dùng khung này, bạn sẽ giúp AI hiểu rõ hơn về yêu cầu của mình, từ đó cho ra kết quả tốt hơn rất nhiều.
>
> **Few-shot** là kỹ thuật cung cấp một vài ví dụ mẫu trong prompt để AI học theo cách làm. Kỹ thuật này đặc biệt hữu ích khi bạn muốn AI tuân theo một phong cách hay định dạng nhất định.
>
> **Ép định dạng JSON/Table** là cách yêu cầu AI trả kết quả dưới dạng JSON hoặc bảng để dễ dùng và dễ đọc hơn.
>
> Hy vọng phần giải thích trên giúp ích cho bạn! Nếu bạn cần mình giải thích thêm về bất kỳ phần nào, hoặc muốn xem ví dụ cụ thể, đừng ngần ngại cho mình biết nhé. Chúc bạn học tập thật tốt! 🎓

### 1.2. Ba phần văn bản thừa thãi

| # | Phần thừa | Trích đoạn trong câu trả lời cũ | Vì sao thừa |
|---|---|---|---|
| 1 | **Lời chào + cảm thán mở đầu** | *"Chào bạn! 😊 Rất vui được hỗ trợ bạn... Đây là một chủ đề rất thú vị và quan trọng..."* | Không chứa kiến thức nào; người dùng phải xóa đi trước khi dán vào báo cáo |
| 2 | **Câu dẫn chuyển ý** | *"Dưới đây là phần giải thích của mình nhé!"* | Chỉ báo hiệu nội dung sắp tới, bản thân nó không có thông tin |
| 3 | **Lời kết + mời hỏi thêm + chúc** | *"Hy vọng phần giải thích trên giúp ích cho bạn! Nếu bạn cần mình giải thích thêm... đừng ngần ngại... Chúc bạn học tập thật tốt!"* | Xã giao, không phải nội dung học tập; làm phình độ dài |

**Lỗi phụ (giải thích lòng vòng):** mỗi khái niệm bị kéo dài thành đoạn văn 2 câu, trong đó câu thứ hai chỉ lặp lại lợi ích chung chung (*"giúp AI hiểu rõ hơn"*, *"rất hữu ích"*) và không thêm thông tin mới.

**Kết luận:** tỉ lệ nội dung hữu ích thấp; người dùng phải tự lọc và chỉnh sửa thủ công → mất mục tiêu "dán nhanh".

---

## BƯỚC 2: VIẾT PROMPT TỐI ƯU

### 2.1. Chiến lược sửa

| Vấn đề | Cách sửa | Từ khóa bắt buộc đưa vào prompt |
|---|---|---|
| Không có định dạng | Ép **gạch đầu dòng** | "Chỉ trình bày bằng gạch đầu dòng" |
| Không có giới hạn độ dài | Đặt **số lượng cụ thể** | "Mỗi mục đúng 1 gạch đầu dòng, tối đa 20 từ" |
| Hay chào hỏi mở đầu | **Cấm** lời chào ở đầu | "KHÔNG viết lời chào, lời dẫn mở đầu" |
| Hay viết lời kết | **Cấm** lời kết | "KHÔNG viết lời kết, lời chúc, lời mời hỏi thêm" |
| Nội dung lan man | Quy định **cấu trúc mỗi ý** | "Theo mẫu: **Tên:** định nghĩa + công dụng chính" |
| Nội dung đầu tiên | Yêu cầu **bắt đầu ngay** | "Dòng đầu tiên phải là nội dung chính" |

### 2.2. Prompt tối ưu (có thể copy dùng)

````text
Vai trò: Bạn là trợ giảng Prompt Engineering, viết súc tích như ghi chú ôn thi.

Nhiệm vụ: Giải thích 3 nội dung: CREATE, Few-shot, Ép định dạng JSON/Table.

Định dạng bắt buộc:
- CHỈ trình bày bằng gạch đầu dòng, đúng 3 gạch đầu dòng chính (mỗi nội dung 1 gạch).
- Mỗi gạch theo mẫu: **Tên nội dung:** định nghĩa (1 câu) + công dụng chính (1 câu).
- Mỗi gạch tối đa 30 từ. Tổng cộng không quá 90 từ.
- Viết tiếng Việt, giữ nguyên thuật ngữ tiếng Anh.

Ràng buộc cấm:
- KHÔNG viết lời chào, lời dẫn hay câu mở đầu. Dòng đầu tiên phải là gạch đầu dòng thứ nhất.
- KHÔNG viết lời kết, lời chúc hay lời mời hỏi thêm.
- KHÔNG dùng emoji, KHÔNG giải thích thêm ngoài 3 gạch đầu dòng.
````

### 2.3. Kết quả khi chạy prompt tối ưu

- **CREATE:** Khung viết prompt 6 khối (Character, Request, Examples, Adjustments, Type of output, Extras); giúp prompt đầy đủ, ít mơ hồ.
- **Few-shot:** Kỹ thuật đưa vài cặp ví dụ input → output vào prompt; giúp AI bắt chước đúng cách làm và văn phong.
- **Ép định dạng JSON/Table:** Quy định rõ kết quả trả về dạng JSON hoặc bảng; giúp dữ liệu có cấu trúc, dễ đọc và dễ xử lý tự động.

---

## 3. Đối chiếu trước – sau

| Tiêu chí | Prompt cũ | Prompt mới |
|---|---|---|
| Lời chào mở đầu | Có | **Không** |
| Câu dẫn chuyển ý | Có | **Không** |
| Lời kết / mời hỏi thêm | Có | **Không** |
| Hình thức | Đoạn văn | **3 gạch đầu dòng** |
| Độ dài | ~150 từ | **≤ 90 từ** |
| Dán thẳng vào báo cáo | Phải xóa chỉnh | **Dán được ngay** |

## 4. Bài học rút ra

1. "Ngắn gọn" là yêu cầu mơ hồ → phải **lượng hóa** (số gạch, số từ).
2. Chỉ nói AI *nên làm gì* là chưa đủ → cần thêm **câu lệnh cấm** (KHÔNG chào, KHÔNG kết) vì AI mặc định lịch sự và nói nhiều.
3. Luôn chỉ định **định dạng** (gạch đầu dòng, bảng, JSON) ở khối *Type of output* trong khung CREATE.
