# Bài 5: Cơ chế In-Context Learning trong phân loại Review khách hàng

## Phần 1: Phân tích cơ chế In-Context Learning

**In-Context Learning (ICL)** là khả năng AI dựa vào hướng dẫn và các ví dụ có trong prompt để thực hiện nhiệm vụ phù hợp hơn, **không cần huấn luyện lại hay cập nhật trọng số của mô hình**.

Trong tình huống TechShop:

- **Zero-shot:** Chỉ đưa review và yêu cầu AI phân loại. AI có thể chú ý nhiều đến các từ tích cực như "đẹp mê ly", "đầm tay", "khỏi cần bật lò sưởi", nên trả lời sai là **TÍCH CỰC**.
- **Few-shot:** Đưa trước một số ví dụ về cách nói mỉa mai. AI dựa vào mẫu để hiểu rằng lời khen bề ngoài có thể đang chê sản phẩm, từ đó phân loại hợp lý hơn.
- **Điểm khác biệt:** AI chỉ sử dụng các ví dụ trong ngữ cảnh của lần yêu cầu hiện tại, chứ không thay đổi kiến thức hoặc trọng số bên trong mô hình.

Với câu *"mùa đông ôm máy chơi game thì khỏi cần bật lò sưởi vì nhiệt độ CPU lên hẳn 98 độ C"*, khách đang **mỉa mai máy quá nóng**, nên nhãn phù hợp là **TIÊU CỰC**. Đây không phải lời khen về tính năng sưởi ấm.

**Lưu ý:** Zero-shot không phải lúc nào cũng chỉ hiểu nghĩa đen; Few-shot cũng không bảo đảm đúng 100%. Nhưng ví dụ mẫu phù hợp sẽ giúp AI nắm rõ cách phân loại mà TechShop mong muốn.

## Phần 2: Prompt Few-shot hoàn chỉnh

```text
Bạn là trợ lý AI của bộ phận Đảm bảo Chất lượng TechShop.

Nhiệm vụ: Đọc review khách hàng và phân loại cảm xúc thành
một trong hai nhãn: TÍCH CỰC hoặc TIÊU CỰC.

Quy tắc:
- Xem xét ý nghĩa toàn câu, không chỉ dựa vào từ khen/chê riêng lẻ.
- Chú ý cách nói mỉa mai, nghĩa bóng, tiếng lóng công nghệ.
- Nếu khách dùng lời khen để phàn nàn về lỗi sản phẩm hoặc dịch vụ,
  hãy chọn TIÊU CỰC.
- Chỉ trả về đúng một nhãn, không giải thích dài dòng.

Dưới đây là 3 ví dụ mẫu (Few-shot):

Ví dụ 1 — Nhiệt độ:
Input: "Laptop xịn thật, chơi game 15 phút mà nóng như bếp điện,
đặt tay lên chắc nướng được bánh!"
Output: TIÊU CỰC

Ví dụ 2 — Pin:
Input: "Pin điện thoại đúng là thần tốc, mới dùng từ sáng đến trưa
đã hết sạch, sạc ngày ba lần cho khỏe người!"
Output: TIÊU CỰC

Ví dụ 3 — Giao hàng:
Input: "Shop giao hàng nhanh ghê, đặt hôm thứ Hai mà đến tận
Chủ nhật mới nhận, chờ muốn thuộc lòng số đơn luôn!"
Output: TIÊU CỰC

Bây giờ hãy phân loại review sau:
Input: "Máy thiết kế đẹp mê ly, cầm rất đầm tay, mùa đông ôm máy
chơi game thì khỏi cần bật lò sưởi vì nhiệt độ CPU lên hẳn 98 độ C!"
Output:
```

### Kết quả chạy thử

**Output:** `TIÊU CỰC`

**Giải thích kết quả:** Dù câu đánh giá có các từ khen về thiết kế, ý chính là phàn nàn về nhiệt độ CPU quá cao khi chơi game. Đây là cách nói mỉa mai nên cần phân loại **TIÊU CỰC**.
