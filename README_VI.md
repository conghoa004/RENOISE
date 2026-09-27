# Khử Nhiễu Giọng Nói & Tách Tạp Âm với MossFormer2

[English](README.md) | **Tiếng Việt**

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![Model](https://img.shields.io/badge/M%C3%B4%20h%C3%ACnh-MossFormer2__SE__48K-brightgreen.svg)](https://github.com/modelscope/ClearerVoice-Studio)
[![Audio](https://img.shields.io/badge/%C3%82m%20thanh-48kHz%20Full--Band-orange.svg)](#)
[![License](https://img.shields.io/badge/Gi%E1%BA%A5y%20ph%C3%A9p-Apache%202.0-lightgrey.svg)](#)

Quy trình tự động hóa nâng cao chất lượng âm thanh và lọc sạch tạp âm nền giọng nói độ trung thực cao, ứng dụng thư viện **[ClearVoice](https://github.com/modelscope/ClearerVoice-Studio)** cùng mô hình học sâu tiên tiến **MossFormer2_SE_48K**.

Dự án này mang lại chất lượng khử nhiễu giọng hát, podcast và ghi âm đạt tiêu chuẩn phòng thu ở tần số lấy mẫu **48 kHz**, thích hợp cho xử lý hậu kỳ audio, lọc âm podcast, tách tạp âm cho video và tiền xử lý cho các hệ thống AI giọng nói.

---

## Tính Năng Nổi Bật

- **Âm thanh chất lượng cao 48 kHz**: Xử lý toàn dải tần âm thanh (0–24 kHz), giữ trọn vẹn chất giọng tự nhiên, ấm áp mà không bị nghẹt tiếng (muffled) như các mô hình 16kHz truyền thống.
- **Kiến trúc mô hình tân tiến**: Ứng dụng **MossFormer2** — kết hợp cơ chế mạng hồi quy (recurrent) và self-attention đa tầng, tối ưu cho việc tách lọc các thành phần âm học phức tạp.
- **Tự động tải weights (Zero-Config)**: Tự động tải weights mô hình đã được huấn luyện sẵn từ ModelScope/HuggingFace trong lần chạy đầu tiên.
- **Notebook dạng mô-đun (Modular)**: Tách biệt rõ ràng từng ô (cấu hình, kiểm tra file, nạp model, chạy khử nhiễu và nghe thử trực tiếp).
- **Hỗ trợ đa nền tảng**: Tương thích tốt trên cả máy cá nhân (Windows/Linux) và môi trường đám mây như **Google Colab**.

---

## Cấu Trúc Thư Mục

```text
RENOISE/
├── remove_noise.ipynb   # Jupyter Notebook dạng mô-đun để khử nhiễu
├── README.md            # Tài liệu dự án (Tiếng Anh)
└── README_VI.md         # Tài liệu dự án (Tiếng Việt)
```

---

## Cài Đặt Nhanh

### 1. Yêu cầu hệ thống

- Python 3.8 – 3.10
- GPU hỗ trợ CUDA (khuyên dùng để tăng tốc xử lý; vẫn chạy được trên CPU)
- Hệ thống đã cài `ffmpeg` hoặc thư viện `libsndfile`

### 2. Cài đặt thư viện

Cài đặt `clearvoice` cùng các gói xử lý âm thanh cơ bản:

```bash
pip install clearvoice
pip install soundfile librosa numpy
```

---

## Hướng Dẫn Sử Dụng

### Cách 1: Sử dụng qua Jupyter Notebook (`remove_noise.ipynb`)

1. Mở file `remove_noise.ipynb` bằng **VS Code**, **JupyterLab** hoặc tải lên **Google Colab**.
2. Chạy tuần tự các ô lệnh:
   - **Ô 1 (Dependencies)**: Cài đặt các gói thư viện cần thiết.
   - **Ô 2 (Configuration)**: Thiết lập đường dẫn file đầu vào (`INPUT_FILE`), file đầu ra (`OUTPUT_FILE`) và tên mô hình (`MODEL`).
   - **Ô 3 (Validation)**: Kiểm tra file âm thanh đầu vào có tồn tại hay không.
   - **Ô 4 (Load Model)**: Nạp mô hình `MossFormer2_SE_48K` vào bộ nhớ.
   - **Ô 5 (Inference)**: Thực hiện xử lý lọc và khử sạch tạp âm.
   - **Ô 6 (Export Audio)**: Xuất file âm thanh sạch ra đĩa cứng.
   - **Ô 7 (Preview)**: Nghe thử và so sánh âm thanh trước/sau xử lý trực tiếp trên trình duyệt.

### Cách 2: Sử dụng bằng Script Python độc lập

Bạn có thể viết một file Python `.py` độc lập như sau:

```python
import os
import time
from clearvoice import ClearVoice

# Cấu hình đường dẫn
INPUT_FILE = "duong_dan/file_ghi_am.wav"
OUTPUT_FILE = "vocal_clean.wav"
MODEL = "MossFormer2_SE_48K"

# Kiểm tra file đầu vào
if not os.path.isfile(INPUT_FILE):
    raise FileNotFoundError(f"Không tìm thấy file: {INPUT_FILE}")

# Khởi tạo mô hình
enhancer = ClearVoice(
    task="speech_enhancement",
    model_names=[MODEL]
)

# Chạy khử nhiễu
print("Đang khử nhiễu âm thanh...")
output_wav = enhancer(input_path=INPUT_FILE, online_write=False)

# Lưu kết quả
enhancer.write(output_wav, output_path=OUTPUT_FILE)
print(f"Đã lưu file âm thanh sạch tại: {os.path.abspath(OUTPUT_FILE)}")
```

---

## Bảng Tham Số Cấu Hình

| Tham số | Kiểu dữ liệu | Mặc định | Mô tả |
| :--- | :--- | :--- | :--- |
| `INPUT_FILE` | `str` | `"/content/ghiam.wav"` | Đường dẫn file âm thanh có tạp âm cần xử lý (khuyên dùng định dạng `.wav`). |
| `OUTPUT_FILE`| `str` | `"vocal_clean.wav"` | Đường dẫn lưu file âm thanh sau khi đã làm sạch. |
| `MODEL` | `str` | `"MossFormer2_SE_48K"`| Tên checkpoint mô hình khử nhiễu (chuẩn 48 kHz đơn kênh - monophonic). |

---

## Khuyến Nghị Về Hiệu Năng

- **Định dạng âm thanh**: Nên sử dụng file PCM `.wav` chuẩn 16-bit hoặc 24-bit để đạt tốc độ nạp và chất lượng khôi phục tốt nhất.
- **Tần số lấy mẫu (Sample Rate)**: Mô hình tự động nội suy/resample và xuất ra chuẩn âm thanh 48 kHz.
- **Tăng tốc với GPU**: Chạy trên GPU hiện đại (NVIDIA T4, RTX 3060 trở lên) sẽ cho tốc độ nhanh hơn thời gian thực (faster-than-real-time). Khi chạy bằng CPU, hệ thống vẫn hoạt động bình thường nhưng sẽ mất nhiều thời gian hơn với các file âm thanh dài.
- **Lần chạy đầu tiên**: Lần đầu tiên chạy, hệ thống sẽ tự động tải checkpoint mô hình (~vài trăm MB). Các lần chạy tiếp theo sẽ nạp ngay từ bộ nhớ đệm (cache) mà không cần tải lại.

---

## Tham Khảo & Cảm Ơn

- **ClearerVoice-Studio**: [ModelScope ClearerVoice Studio GitHub](https://github.com/modelscope/ClearerVoice-Studio)
- **Công trình nghiên cứu MossFormer2**: *MossFormer2: Combining Recurrent and Self-Attention Mechanisms for Speech Enhancement and Separation.*

---

## Giấy Phép (License)

Dự án áp dụng giấy phép Apache 2.0. Chi tiết về điều khoản bản quyền của trọng số mô hình vui lòng tham khảo kho lưu trữ gốc của ClearVoice.
