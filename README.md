# Pacman

Game **Pac-Man** cổ điển viết bằng Python và [Pygame](https://www.pygame.org/), mô phỏng lối chơi, bản đồ và hành vi ma (Blinky, Pinky, Inky, Clyde) gần với bản arcade gốc.

## Tính năng

- Điều khiển Pac-Man trên lưới 8×8 pixel, va chạm tường và đường hầm hai bên màn hình
- Thu thập **điểm nhỏ** (10 điểm) và **điểm lớn** (100 điểm) để kích hoạt chế độ ma sợ
- Bốn con ma với AI riêng: đuổi theo, phân tán, sợ hãi, bị ăn và quay về nhà ma
- Âm thanh hiệu ứng (ăn, chết, thắng, v.v.)
- Ba mạng, thua khi hết mạng, thắng khi ăn hết thức ăn trên bản đồ

## Yêu cầu

- Python 3.9 trở lên (đã thử với 3.9 / 3.12)
- Thư viện **pygame**

## Cài đặt

```bash
git clone https://github.com/ThanhVuong200/Pacman.git
cd Pacman
pip install pygame
```

## Chạy game

```bash
python main.py
```

Cửa sổ game: **448×535** pixel, **60 FPS**.

## Điều khiển

| Phím | Hành động |
|------|-----------|
| ↑ ↓ ← → | Di chuyển Pac-Man |
| Space | Chơi lại sau khi thua hoặc thắng |
| Đóng cửa sổ | Thoát game |

Pac-Man chỉ đổi hướng khi đang nằm đúng ô lưới (bội số của 8 pixel), giống cơ chế bản gốc.

## Luật chơi (tóm tắt)

| Sự kiện | Điểm |
|---------|------|
| Ăn điểm nhỏ | +10 |
| Ăn điểm lớn | +100, ma chuyển sang chế độ sợ (~7 giây) |
| Ăn ma khi đang sợ | +300 |
| Va chạm ma (không sợ / không bị ăn) | Mất 1 mạng; hết 3 mạng → Game Over |
| Ăn hết thức ăn | Thắng |

Sau màn **READY** (~1 giây), ma bắt đầu di chuyển theo chu kỳ **đuổi** (20 giây) và **phân tán** (10 giây).

## Cấu trúc thư mục

```
Pacman/
├── main.py              # Logic game, entity, ma, vòng lặp chính
├── Key.py               # Trạng thái phím (giữ / thả)
├── res/
│   ├── level/
│   │   └── level.csv    # Bản đồ (định dạng bên dưới)
│   ├── img/             # Sprite Pac-Man, ma, nền, icon
│   ├── sound/           # File .wav
│   └── font/
│       └── font.ttf     # Font hiển thị điểm / READY
└── BaoCaoDoAnPython.docx  # Báo cáo đồ án (nếu có)
```

## Định dạng bản đồ (`level.csv`)

File CSV dùng dấu **`;`** làm phân tách. Mỗi ô là một ký tự:

| Ký tự | Ý nghĩa |
|-------|---------|
| `x` | Tường |
| `-` | Cửa nhà ma (ma có thể đi qua khi về nhà) |
| `.` | Điểm nhỏ |
| `o` | Điểm lớn |
| ` ` (khoảng trắng) | Ô trống |
| `P` | Vị trí xuất phát Pac-Man |
| `b` | Blinky (đuổi trực tiếp vị trí Pac-Man) |
| `p` | Pinky (nhắm trước hướng Pac-Man) |
| `i` | Inky (dựa trên Blinky và hướng Pac-Man) |
| `c` | Clyde (đuổi khi xa, phân tán khi gần) |

Chỉnh sửa `res/level/level.csv` rồi chạy lại game để thử bản đồ mới.

## Kiến trúc mã (ngắn gọn)

- **`Entity`** → **`MovingEntity`** (Pac-Man, ma) và **`StaticEntity`** (tường, điểm)
- Mỗi **`Ghost`** dùng state pattern: `HouseMode`, `ChaseMode`, `ScatterMode`, `FrightenedMode`, `EatenMode`
- Va chạm: `checkWallCollision`, `checkFood`, `checkGhostCollisionFrighten`
- Khởi tạo bản đồ: hàm `init()` đọc CSV và tạo danh sách `entities`, `wall`, `ghosts`

## Tắt nhạc (tùy chọn)

Trong `main.py`, bỏ comment hoặc thêm `return` sớm trong `playMusic()` / `forcePlayMusic()` (có gợi ý trong code) để tắt âm thanh khi debug.

## Ghi chú

- Repo gốc: [ThanhVuong200/Pacman](https://github.com/ThanhVuong200/Pacman)
- Dự án phù hợp học Pygame, OOP và mô phỏng AI đơn giản theo lưới

## Giấy phép

Chưa khai báo giấy phép trong repository. Liên hệ tác giả repository nếu bạn muốn sử dụng hoặc phân phối lại mã nguồn.
