# AI Local — MiniMax H3 Film Builder V1.4 Smart Duration

**Ứng dụng HTML chạy cục bộ để chuẩn bị prompt nhiều shot cho MiniMax H3 trong ComfyUI.**

V1.4 bổ sung cơ chế **ước lượng thời lượng theo từng shot** và cho phép chỉnh thời lượng thủ công trước khi tạo prompt. Ứng dụng hỗ trợ soạn kịch bản, mô tả nhân vật/bối cảnh, chọn định hướng đạo diễn, rồi xuất prompt theo định dạng shot-tag cho Auto Shot Runner.

> **Trạng thái hiện tại:** V1.4 Smart Duration là công cụ soạn prompt chạy trong trình duyệt. Ước lượng thời lượng là quy tắc heuristic dựa trên nội dung/hành động và thiết lập của người dùng, không phải AI hiểu phim hoàn chỉnh. Chưa xác nhận end-to-end bằng một lượt render ComfyUI hoàn chỉnh sau cập nhật V1.4.

## Phiên bản và workflow chuẩn

- **Ứng dụng hiện tại:** `AI_Local_MiniMax_H3_Film_Builder_V1_4_Smart_Duration.html`
- **Workflow chuẩn của dự án:** `MINIMAX_H3_AUTO_SHOTS_V4_2.json`
- Không ghi đè workflow chuẩn. Mọi thử nghiệm workflow cần lưu thành file phiên bản riêng.
- App chỉ tạo prompt và file xuất; không tự điều khiển hoặc bấm chạy ComfyUI.
- **Lưu ý quan trọng về xuất workflow JSON:** chức năng xuất JSON dùng template workflow được nhúng trong HTML và chỉ thay trường `master_prompt` của node Auto Shot Runner trong bản sao. Không mặc định xem file JSON xuất từ app là workflow V4.2. Nếu muốn dùng V4.2, hãy mở `MINIMAX_H3_AUTO_SHOTS_V4_2.json` chuẩn và dán prompt vào `master_prompt` của node `MiniMaxAutoShotRunner` (node ID 189), hoặc xác minh template xuất ra đúng phiên bản trước khi chạy.

## Tính năng hiện có

- Giao diện tiếng Việt, mở trực tiếp bằng Chrome hoặc Microsoft Edge.
- Soạn nội dung yêu cầu để sao chép sang ChatGPT; app không tự gọi ChatGPT/API.
- Nhập kịch bản và danh sách hành động theo shot.
- Thiết lập tối đa 3 chủ thể/ảnh tham chiếu, bối cảnh, đạo cụ và quy tắc liên tục.
- Chọn phong cách hình ảnh, tỷ lệ khung hình, tông cảm xúc, ánh sáng, ngôn ngữ máy quay, nhịp dựng, chuyển động, kiểu kết thúc, âm thanh hiện trường và nhạc nền.
- Tạo prompt nhiều shot theo định dạng có nhãn `SHOT` và `DURATION`.
- **Smart Duration:** ước lượng thời lượng từng shot theo quy tắc, thay vì luôn chia tổng thời lượng đều nhau.
- **Chỉnh thời lượng từng shot:** cho phép người dùng rà soát và điều chỉnh thời lượng trước khi cập nhật prompt.
- Cố gắng phân bổ theo tổng thời lượng đã nhập; cần kiểm tra lại tổng thời gian sau khi chỉnh tay.
- Sao chép prompt, tải TXT, xuất/nhập cấu hình dự án JSON.
- Hồ sơ nhân vật/bối cảnh có thể được lưu trong local storage của trình duyệt nếu trình duyệt cho phép.
- Không cần cài Python, Node.js, thư viện hoặc máy chủ để mở app.

## Yêu cầu

- Windows hoặc hệ điều hành có trình duyệt Chrome/Edge hiện đại.
- ComfyUI đã cài đặt riêng.
- Workflow MiniMax H3 Auto Shot Runner, các custom nodes, model, VAE, text encoder và ảnh tham chiếu phải được cấu hình sẵn trong ComfyUI.
- Cấu hình phần cứng của dự án: Intel Core i7-1270P, RAM 32 GB, eGPU SGWZone RTX 4060 8 GB VRAM. Khả năng chạy thực tế còn phụ thuộc vào model, độ phân giải, thời lượng, node và cấu hình workflow.

App không cài model, không cài node và không tự kiểm tra môi trường ComfyUI.

## Bắt đầu nhanh

1. Mở `AI_Local_MiniMax_H3_Film_Builder_V1_4_Smart_Duration.html` bằng Chrome hoặc Edge.
2. Nhập tên dự án và kịch bản. Có thể điền phần phân chia hành động theo shot để kiểm soát tốt hơn.
3. Thiết lập nhân vật, ảnh tham chiếu, bối cảnh, đạo cụ và quy tắc liên tục.
4. Chọn phong cách hình ảnh, máy quay, nhịp dựng, âm thanh và nhạc nền.
5. Nhập tổng thời lượng và số shot mong muốn.
6. Bấm **Tạo prompt** để sinh prompt và bảng ước lượng thời lượng.
7. Kiểm tra từng shot; chỉnh thời lượng thủ công nếu cần. Sau khi chỉnh, cập nhật/tạo lại prompt theo điều khiển của app và đọc lại kết quả.
8. Sao chép prompt hoặc tải file TXT.
9. Mở workflow chuẩn `MINIMAX_H3_AUTO_SHOTS_V4_2.json` trong ComfyUI.
10. Dán prompt vào `master_prompt` của node `MiniMaxAutoShotRunner` (node ID 189).
11. Bấm **PREVIEW SHOTS** và kiểm tra số shot, thứ tự, thời lượng cùng nội dung prompt.
12. Chỉ khi preview đúng mới bấm **START AUTO SHOTS (1 CLICK)**.
13. Kiểm tra các clip MP4 riêng lẻ; ghép clip, chỉnh nhịp và xử lý âm thanh cuối cùng trong phần mềm dựng phim, ví dụ CapCut.

## Smart Duration hoạt động như thế nào?

- V1.4 sử dụng các quy tắc heuristic để ước lượng thời lượng dựa trên hành động/mô tả shot và cấu hình tổng thời lượng.
- Các shot không bắt buộc phải có thời lượng bằng nhau.
- Người dùng có thể điều chỉnh từng shot; thời lượng đề xuất không phải quyết định đạo diễn tuyệt đối.
- Kết quả phụ thuộc vào cách mô tả hành động. Nếu một dòng chứa nhiều hành động phức tạp, hãy cân nhắc tách thành nhiều shot hoặc tăng thời lượng.
- Đây không phải mô hình AI phân tích video và không bảo đảm thời lượng tối ưu cho mọi tình huống.
- Sau khi chỉnh thời lượng, hãy kiểm tra lại tổng thời lượng và prompt đầu ra. Sau đó luôn kiểm tra **PREVIEW SHOTS** trong Auto Shot Runner.

Ví dụ minh họa định dạng:

```text
[SHOT 01 | DURATION: 5s | PRIMARY CAMERA SETUP: wide establishing shot]
Thiết lập bối cảnh và vị trí ban đầu của nhân vật.

[SHOT 02 | DURATION: 3s | PRIMARY CAMERA SETUP: close-up]
Một hành động đơn giản, rõ ràng.

[SHOT 03 | DURATION: 6s | PRIMARY CAMERA SETUP: medium shot]
Một hành động phức tạp hơn cần thời gian để diễn ra.
```

Các con số trên chỉ là ví dụ định dạng, không phải thời lượng bắt buộc.

## Ảnh tham chiếu

- `Picture 1` → `ref_image_0`
- `Picture 2` → `ref_image_1`
- `Picture 3` → `ref_image_2`

Chọn đúng ảnh trong workflow và xác minh ảnh đã được nạp vào các cổng tham chiếu tương ứng. Việc ghi tên ảnh trong prompt không tự tải ảnh lên ComfyUI hoặc tự kết nối file ảnh.

## Lưu và chuyển dự án

- **TXT:** lưu prompt đã tạo.
- **Cấu hình dự án JSON:** lưu các thiết lập để nhập lại trong app.
- **Workflow JSON:** tạo một bản sao từ template nhúng trong app và thay `master_prompt`; cần xác minh phiên bản workflow trước khi dùng. Workflow chuẩn hiện tại của dự án vẫn là `MINIMAX_H3_AUTO_SHOTS_V4_2.json`.
- **Hồ sơ cục bộ:** hồ sơ có thể được lưu trong local storage của trình duyệt. Dữ liệu có thể không được chia sẻ giữa các trình duyệt hoặc giữa các cách mở file khác nhau.
- App không gửi nội dung ra API theo thiết kế hiện tại; vẫn nên tránh lưu thông tin nhạy cảm vào prompt hoặc file dự án nếu không cần thiết.

## Giới hạn và trạng thái kiểm thử

- App không tự gọi ChatGPT và không điều khiển ComfyUI.
- Tự chia shot từ kịch bản là hỗ trợ bằng quy tắc văn bản; nên nhập outline từng shot để kiểm soát chính xác hơn.
- Smart Duration là ước lượng heuristic, không phải AI hiểu ngữ nghĩa hoàn chỉnh.
- App không xác nhận model, custom node, ảnh tham chiếu hoặc GPU đã sẵn sàng.
- Prompt được tạo ra không bảo đảm workflow sẽ phân tích chính xác; cần kiểm tra bằng **PREVIEW SHOTS** trước khi chạy.
- Chưa xác nhận end-to-end rằng mọi thao tác của V1.4 và toàn bộ lượt render đều hoạt động hoàn hảo trong môi trường ComfyUI thực tế.
- Không được xem việc xuất JSON thành công là bằng chứng workflow đã render thành công.

## Cấu trúc file gợi ý

```text
AI-Local-MiniMax-H3-Film-Builder/
├── README.md
├── AI_Local_MiniMax_H3_Film_Builder_V1_4_Smart_Duration.html
└── MINIMAX_H3_AUTO_SHOTS_V4_2.json
```

## Việc nên kiểm thử tiếp theo

- [ ] Mở app V1.4 bằng Chrome/Edge và xác nhận giao diện hiển thị bình thường.
- [ ] Tạo prompt thử với 3–4 shot có hành động khác nhau.
- [ ] Chỉnh thời lượng một shot và xác nhận prompt được cập nhật đúng.
- [ ] Kiểm tra tổng thời lượng sau khi chỉnh từng shot.
- [ ] Dán prompt vào workflow V4.2 chuẩn và kiểm tra **PREVIEW SHOTS**.
- [ ] Render một số shot ngắn trước khi chạy cả phim.
- [ ] Xác minh riêng chức năng xuất workflow JSON để tránh nhầm template nhúng với workflow V4.2 chuẩn.

Các mục này là checklist kiểm thử còn lại, không phải tính năng đã được xác nhận hoàn tất.

## Báo lỗi

Khi báo lỗi, hãy cung cấp phiên bản app, tên file workflow đang dùng, thông báo lỗi đầy đủ và một prompt thử nghiệm ngắn. Không gửi mật khẩu, API key hoặc thông tin nhạy cảm.

## Giấy phép

Chưa chỉ định giấy phép cho repository. Trước khi công bố hoặc cho phép phân phối lại, hãy chọn giấy phép phù hợp.
