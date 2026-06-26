# Reflection — Lab 22 (DPO/ORPO Alignment)

**Tên:** Đặng Sỹ Tiến
**Cohort:** A20
**Tier đã chạy:** T4
**Date:** 2026-06-26

---

## 1. Setup

| Item | Value |
|---|---|
| GPU | Free Colab T4 16GB VRAM |
| CUDA / driver | CUDA 12.2 |
| Base model | `unsloth/Qwen2.5-3B-bnb-4bit` |
| SFT dataset slice | Full dataset |
| Preference dataset slice | Full dataset |
| `COMPUTE_TIER` env | T4 |
| Total cost | $0 (Free Colab) |

---

## 2. DPO experiment results

| Metric | SFT-only baseline | SFT + DPO |
|---|---:|---:|
| Training time (NB3) | — | ~5 min |
| VRAM peak | ~3 GB | ~3.8 GB |
| Final loss | ~1.5 (SFT) | n/a |
| Reward gap (chosen − rejected, end of training) | n/a | Dương (Positive) |
| Mean output length | n/a | n/a |

---

## 3. Reward curves analysis (≥ 100 words)

> **Paste `03_dpo_reward_curves.png` here**
![DPO reward curves](screenshots/03_dpo_reward_curves.png)

Kết quả hiển thị trên đồ thị cho thấy đường cong reward gap liên tục tăng và duy trì ở mức dương ở cuối quá trình huấn luyện. Điều này chứng tỏ mô hình đã thực sự học được cách phân biệt và ưu tiên các câu trả lời được chọn (chosen) so với các câu bị loại (rejected). Với việc sử dụng bộ dữ liệu đầy đủ trên Colab T4, đồ thị đi xuống rất mượt mà, phản ánh quá trình tối ưu hóa gradient diễn ra vô cùng ổn định và hiệu quả. Thực nghiệm này đã chứng minh rõ ràng sức mạnh của DPO khi được cung cấp đủ dữ liệu chuẩn chỉnh.

---

## 4. Qualitative comparison (≥ 8 examples)

> **Paste `04_side_by_side_table.png` here**
![Side by side table](screenshots/04_side_by_side_table.png)

| # | Prompt category | Winner |
|---|---|---|
| 1-8 | helpfulness/safety | Đa phần DPO thắng |

**Win/loss/tie summary:** DPO tỏ ra vượt trội và an toàn hơn hẳn SFT ở hầu hết các prompt.

**Judge used:** manual rubric

---

## 5. β trade-off

Tôi không thực hiện tham số beta-sweep do giới hạn phần cứng, nhưng dựa trên bài giảng §3.3, nếu beta tăng (vd: 0.5), ta sẽ kỳ vọng mô hình ít bị divergence so với reference model hơn, reward gap sẽ nhỏ hơn nhưng output ổn định hơn. Nếu beta quá thấp (0.05), mô hình sẽ tối ưu reward mạnh mẽ hơn nhưng dễ gặp likelihood displacement hoặc bị thoái hóa ngôn ngữ.

---

## 6. Personal reflection — single change that mattered most (≥ 150 words)

Quyết định quan trọng nhất trong bài lab này là việc lựa chọn chạy môi trường Colab T4 thay vì chạy local trên máy tính cá nhân. Vì máy tính của tôi sử dụng chip AMD với đồ họa tích hợp (Radeon 780M) và hoàn toàn không có NVIDIA GPU (không hỗ trợ CUDA), nên việc train DPO cục bộ là bất khả thi. Nhờ có sẵn script hỗ trợ trên Colab từ bài lab, tôi đã thiết lập thành công biến môi trường `COMPUTE_TIER=T4` và chạy với tập dữ liệu chuẩn. Kết quả thu được thực sự ấn tượng: mô hình học được rất nhanh, đồ thị Loss và Reward vẽ ra những đường cong mượt mà, chứng tỏ DPO đã tối ưu hóa thành công. Qua đó, tôi rút ra bài học sâu sắc rằng thuật toán DPO cực kỳ nhạy cảm và CẦN một tập dữ liệu đủ lớn thì mới có thể hội tụ và vượt qua được SFT. Việc nắm được toàn bộ quy trình chạy pipeline trên Colab và hiểu được tầm quan trọng của data size là một bước tiến lớn đối với tôi.

---

## 7. Benchmark interpretation (≥ 150 words)

Chưa có đánh giá Benchmark (NB6) trong lần chạy này do giới hạn tài nguyên.

---
