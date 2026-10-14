# AI Local — MiniMax H3 Film Builder V1

**Công cụ tạo prompt nhiều cảnh (multi-shot) cho MiniMax H3 Auto Shot Runner trong ComfyUI.**

Ứng dụng giúp chuẩn bị kịch bản, mô tả nhân vật/bối cảnh, chọn định hướng đạo diễn và xuất prompt theo định dạng shot-tag để đưa vào workflow. Mục tiêu là giảm thao tác lặp lại khi tạo nhiều clip ngắn rồi ghép chúng trong CapCut.

> **Trạng thái:** V1 là công cụ HTML độc lập để soạn prompt. Chưa tích hợp API điều khiển ComfyUI và chưa xác nhận bằng một lượt render hoàn chỉnh trong ComfyUI. Hãy thử bằng kịch bản ngắn trước khi chạy dự án dài.

## Tính năng

- Giao diện tiếng Việt, chạy trực tiếp trong trình duyệt.
- Soạn yêu cầu/kịch bản để gửi ChatGPT.
- Nhập thông tin nhân vật, bối cảnh, đạo cụ và ghi chú hình ảnh.
- Gán tối đa 3 ảnh tham chiếu vào `Picture 1`, `Picture 2`, `Picture 3` tương ứng với `ref_image_0`, `ref_image_1`, `ref_image_2` trong workflow.
- Chọn các định hướng sáng tạo như tỷ lệ khung hình, phong cách hình ảnh, ánh sáng, ngôn ngữ máy quay, nhịp dựng, chuyển động, âm thanh và nhạc nền.
- Tạo prompt theo từng shot, dùng nhãn có thời lượng để Auto Shot Runner có thể phân tích.
- Sao chép prompt, tải prompt dạng TXT, xuất dự án JSON và nhập lại dự án đã lưu.
- Không cần cài Python, Node.js, thư viện hay máy chủ để mở giao diện.

## Yêu cầu

- Trình duyệt Chrome hoặc Microsoft Edge hiện đại.
- Workflow ComfyUI **MiniMax H3 Auto Shot Runner** đã được cài đặt và cấu hình riêng.
- Các model, node tùy chỉnh, ảnh tham chiếu và cấu hình GPU cần thiết cho workflow phải có sẵn trong ComfyUI.

Ứng dụng chỉ tạo nội dung prompt; nó không cài model hoặc node cho ComfyUI.

## Bắt đầu nhanh

1. Tải `AI_Local_MiniMax_H3_Film_Builder_V1.html` về máy.
2. Mở file bằng Chrome hoặc Edge. Không cần chạy lệnh cài đặt.
3. Viết ý tưởng/kịch bản hoặc dùng phần tạo yêu cầu để nhờ ChatGPT phát triển kịch bản.
4. Điền nhân vật, bối cảnh, đạo cụ và ghi chú về những gì bắt buộc phải giữ nhất quán.
5. Chọn phong cách đạo diễn, tỷ lệ khung hình, ánh sáng, máy quay, nhịp dựng, chuyển động, thời lượng và số shot.
6. Bấm tạo prompt, đọc lại từng shot và sửa nội dung nếu cần.
7. Sao chép toàn bộ prompt đã tạo.
8. Mở workflow MiniMax H3 Auto Shot Runner trong ComfyUI.
9. Dán prompt vào trường **`master_prompt` của node `MiniMaxAutoShotRunner` (node ID 189)**.
10. Dùng **PREVIEW SHOTS** để kiểm tra cách workflow tách shot trước khi bấm **START AUTO SHOTS**.
11. Kiểm tra các clip MP4 được tạo ra, sau đó ghép, chỉnh nhịp và xử lý âm thanh trong CapCut.

### Lưu ý về ô prompt

Theo cấu hình workflow hiện tại, prompt nhiều shot cần được dán vào `master_prompt` của node Auto Shot Runner, không phải chỉ dán vào ô prompt một-shot ở node 138. Nếu node hoặc tên trường trong workflow của bạn khác phiên bản được mô tả ở đây, hãy kiểm tra trực tiếp graph trước khi chạy.

## Định dạng shot

Prompt được tạo theo cấu trúc gần như sau:

```text
[SHOT 01 | DURATION: 3s | PRIMARY CAMERA SETUP: Medium shot, locked camera]
Mô tả nhân vật, bối cảnh, hành động, chuyển động máy quay và trạng thái cuối shot.

[SHOT 02 | DURATION: 3s | PRIMARY CAMERA SETUP: Close-up, slow push-in]
Mô tả shot tiếp theo...
```

Mỗi shot nên mô tả rõ:

1. **Thời gian và cỡ cảnh:** shot dài bao lâu, toàn/trung/cận hay góc máy nào.
2. **Trạng thái đầu:** nhân vật đang ở đâu, nhìn về hướng nào, tư thế và đạo cụ đang ở trạng thái nào.
3. **Hành động:** một hành động chính, cụ thể và có thể nhìn thấy được.
4. **Máy quay:** đứng yên, pan, tilt, tracking, push-in hoặc chuyển động phù hợp.
5. **Trạng thái cuối:** vị trí, hướng nhìn và hành động kết thúc để shot sau có thể nối tiếp.
6. **Tính liên tục:** giữ nhận dạng, trang phục, tóc, đạo cụ, ánh sáng và bố cục không thay đổi vô cớ.

## Ảnh tham chiếu

- `Picture 1` → `ref_image_0`
- `Picture 2` → `ref_image_1`
- `Picture 3` → `ref_image_2`

Hãy xác nhận đúng ảnh trong workflow trước khi render. Việc chọn nhãn ảnh trong ứng dụng giúp prompt nói rõ ảnh nào dùng cho nhân vật hoặc bối cảnh; ứng dụng không tự tải ảnh lên ComfyUI hay tự gắn file ảnh vào node.

## Lưu và chuyển dự án

- **TXT:** lưu prompt đã tạo để dùng lại hoặc đưa vào workflow.
- **JSON:** lưu các lựa chọn và dữ liệu dự án để nhập lại trong ứng dụng.
- **Dữ liệu cục bộ:** ứng dụng không cần tài khoản hay API key. Hãy tự quản lý các file JSON/TXT đã xuất và không chia sẻ chúng nếu có nội dung riêng tư.

## Giới hạn đã biết

- Không gọi ChatGPT tự động; phần yêu cầu cho ChatGPT cần được sao chép sang ChatGPT rồi mang kết quả trở lại ứng dụng.
- Không kết nối trực tiếp hoặc bấm chạy ComfyUI thay người dùng.
- Việc chia kịch bản thành shot dựa trên quy tắc xử lý văn bản đơn giản, không phải mô hình AI hiểu phim. Luôn kiểm tra số shot, thời lượng, hành động và tính liên tục trước khi chạy.
- Không đảm bảo mọi prompt sẽ được mọi phiên bản Auto Shot Runner phân tích giống nhau. Hãy kiểm tra bằng **PREVIEW SHOTS**.
- Chất lượng, độ dài, tên file và việc tiếp tục/resume phụ thuộc vào workflow Auto Shot Runner, model, node tùy chỉnh và cấu hình ComfyUI đang dùng.
- Chưa có kiểm thử end-to-end xác nhận toàn bộ quy trình render; V1 nên được xem là bản hỗ trợ soạn prompt.

## Cấu trúc repository gợi ý

```text
AI-Local-MiniMax-H3-Film-Builder/
├── README.md
└── AI_Local_MiniMax_H3_Film_Builder_V1.html
```

## Lộ trình dự kiến

- [ ] Kiểm thử bằng kịch bản 3–4 shot trong workflow thực tế.
- [ ] Xác nhận quy tắc phân tách shot, thời lượng và đầu ra MP4.
- [ ] Cải thiện cách chia kịch bản và kiểm tra tính liên tục.
- [ ] Nghiên cứu tích hợp ComfyUI API để giảm thao tác sao chép thủ công.

Các mục trên là kế hoạch dự kiến, không phải tính năng đã có trong V1.

## Đóng góp và báo lỗi

Khi báo lỗi, vui lòng ghi phiên bản HTML, phiên bản workflow Auto Shot Runner, thông báo lỗi đầy đủ và một prompt thử nghiệm ngắn. Không gửi API key, mật khẩu hoặc thông tin nhạy cảm.

## Giấy phép

Chưa chỉ định giấy phép cho repository. Nếu muốn người khác sử dụng, sửa đổi hoặc phân phối lại mã nguồn, hãy chọn và thêm một tệp giấy phép phù hợp trước khi công bố.
