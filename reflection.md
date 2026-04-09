## 1) Role cụ thể trong nhóm
Owner pipeline **OCR đơn thuốc + đối chiếu tồn kho**, đóng gói thành tool callable cho agent (phối hợp với Kiên để thống nhất input/output và luồng kiểm kho).

## 2) Phần phụ trách cụ thể (2-3 đóng góp có output rõ)
- Xây tool OCR + kiểm kho tại `app/tools/ocr_and_check_pill.py`:
  - Hàm entry ocr_and_check_storage trả về schema {drugs, results, error}` để downstream dùng ổn định.
  - Pipeline theo bước: setup Gemini client (`GEMINI_API_KEY`) -> load CSV tồn kho -> OCR ảnh bằng Gemini Vision -> check tồn kho theo `full_name`.
- Thiết kế prompt OCR và contract JSON:
  - `SYSTEM_PROMPT` ép model trả **JSON array thuần** gồm `ten_thuoc`, `lieu_luong`, `full_name`, bỏ toàn bộ thông tin khác (giá, số lượng mua, hoạt chất).
  - Có bước “làm sạch” response nếu bị bọc ```json``` để tăng tỉ lệ parse JSON.
- Thêm logging vận hành cho pipeline:
  - Log ra console + file `logs/pipeline_<timestamp>.log`, có phân tách BƯỚC 1-4, dễ trace lỗi OCR / thiếu env / thiếu inventory.

## 3) SPEC phần nào mạnh nhất, phần nào yếu nhất? Vì sao?
- Mạnh nhất:
  - Định nghĩa rõ đây là **augmentation** (dược sĩ quyết định cuối), có learning signal (duyệt/từ chối) và failure modes (FDA active ingredient dạng chuỗi, mapping brand/generic).
- Yếu nhất:
  - OCR -> kiểm kho hiện dùng **match chính xác** theo `full_name` với cột `Ten_Thuoc`, nên dễ fail khi OCR sai 1-2 ký tự, khác spacing, hoặc inventory dùng biến thể tên khác.

## 4) Đóng góp cụ thể khác với mục 2
- Debug tính ổn định output của Gemini Vision:
  - Bổ sung guard khi model trả về markdown wrapper, và xử lý trường hợp JSON parse fail bằng cách return `[]` thay vì crash toàn pipeline.
- Chuẩn hóa “điểm rơi” vận hành:
  - Đặt `temperature=0` để giảm biến thiên output OCR, và log “300 ký tự đầu” của raw response để khoanh vùng prompt/format issue nhanh.

## 5) 1 điều học được trong hackathon mà trước đó chưa biết
OCR bằng LLM Vision không “tự nhiên” ra JSON sạch: dù prompt ép format, model vẫn có thể bọc markdown hoặc trả field lệch schema, nên phải coi việc **validate + sanitize output** là yêu cầu bắt buộc của pipeline (không phải tối ưu).

## 6) Nếu làm lại, đổi gì? (cụ thể)
- Thống nhất 1 cơ chế resolve inventory path:
  - Bỏ default `Path("./inventory.csv")` trong OCR tool, thay bằng cơ chế tìm file ổn định giống `app/tools/fda.py:resolve_inventory_path()` để tránh lỗi “File not found” tùy cwd.
- Nâng chất lượng match OCR -> kho:
  - Thêm normalization (lowercase, bỏ liều lượng, bỏ ký tự thừa) và/hoặc fuzzy match (ví dụ Levenshtein) trước khi kết luận “không tìm thấy”.
- Thêm test nhỏ cho phần parse JSON:
  - Unit test cho hàm làm sạch markdown + `json.loads` bằng vài sample response, để refactor prompt không làm vỡ pipeline.

## 7) AI giúp gì? AI sai/mislead ở đâu? (cả hai mặt)
- AI giúp:
  - Draft nhanh `SYSTEM_PROMPT` + schema JSON và khung pipeline theo bước (setup -> OCR -> check kho) để có prototype chạy được trong thời gian ngắn.
  - Gợi ý pattern logging và các guard cơ bản (thiếu `GEMINI_API_KEY`, response bọc ```json```).
- AI sai/mislead:
  - Dễ “đánh lừa” team rằng chỉ cần prompt là sẽ ra JSON chuẩn; thực tế vẫn cần validate/sanitize và thiết kế fallback rõ ràng.
  - Gợi ý match inventory theo chuỗi thô  nhìn đơn giản nhưng gây false negative cao trong dữ liệu thật (OCR noise, biến thể tên thuốc).
