# 🎮 Akanoid – CaiWin Edition

[![Language](https://img.shields.io/badge/Language-Java-oracle.svg)]()
[![Framework](https://img.shields.io/badge/Framework-JavaFX-blue.svg)]()
[![Build](https://img.shields.io/badge/Build-Maven-red.svg)]()
[![Java Version](https://img.shields.io/badge/JDK-17%20%2B-orange.svg)]()

**Akanoid – CaiWin Edition** là trò chơi phá gạch xây dựng bằng JavaFX, được truyền cảm hứng từ tựa game huyền thoại *Arkanoid* cổ điển. Dự án được mở rộng mạnh mẽ với hiệu ứng hình ảnh hiện đại, nhạc nền sống động cùng hàng loạt cơ chế nâng cấp lối chơi đầy kịch tính.

---
### ⚡ Lối chơi cốt lõi:
*   **Hiệu năng ổn định:** Vòng lặp game chạy mượt mà ở **70 FPS**, bảo đảm thao tác paddle chính xác và xử lý va chạm không bị trễ.
*   **Khung hình tối ưu:** Các màn chơi được dựng trong khung hình 900x700 nằm cân bằng ở giữa cửa sổ ứng dụng 1200x750.
*   **Hệ thống Trạng thái (`GameState`):** Quản lý chặt chẽ các tiến trình: *LoadingProcess*, *MenuProcess*, *PickLevelProcess*, *PlayingProcess*, *OptionProcess*, *TutorialProcess*, và *GameOverProcess*[cite: 1].
*   **HUD trực quan:** Hiển thị thời gian thực điểm số, số mạng và level hiện tại. Khi dọn sạch gạch, game tự động chuyển màn[cite: 1].

---

## 📐 Kiến trúc & Biểu đồ lớp (Class Diagram)

Hệ thống được thiết kế theo hướng đối tượng chặt chẽ. Dưới đây là kiến trúc tổng thể của trò chơi:

<p align="center">
  <img width="100%" alt="Akanoid Class Diagram" src="https://github.com/user-attachments/assets/d1dfb9c0-474d-4105-b0c1-361754628a17" />
</p>

---

## 🕹️ Cơ chế trò chơi (Gameplay Mechanics)

### Thanh đỡ (Paddle)
*   **Kích thước linh hoạt:** Có 3 mức độ (Ngắn, Chuẩn, Mở rộng)[cite: 1]. Việc co giãn dựa trên tâm paddle giúp loại bỏ hiện tượng giật màn hình[cite: 1].
*   **Vũ khí trang bị:** Nhận năng lượng từ `Shot power-up` để bắn đạn phá gạch trực tiếp[cite: 1].
*   **Giới hạn di chuyển:** Hoạt động hoàn toàn trong biên bản đồ, tốc độ thay đổi linh hoạt tùy theo các bổ trợ nhận được[cite: 1].

### Quả bóng (Ball)
*   **Vật lý phản xạ:** Hướng bóng nảy ra phụ thuộc trực tiếp vào vị trí va chạm trên thanh đỡ, giúp người chơi chủ động điều hướng bóng[cite: 1].
*   **Gia tốc thông minh:** Bóng tăng nhẹ tốc độ sau mỗi lần va chạm để tạo độ kịch tính, nhưng luôn giới hạn ở mức kiểm soát được[cite: 1].
*   **Sửa lỗi kẹt góc:** Tự động hiệu chỉnh các góc đặc biệt (0° hoặc 90°) để tránh tình trạng bóng nảy vô tận[cite: 1].
*   **Xử lý mất bóng:** Khi rơi xuống đáy, hàm `onBallLost` sẽ được kích hoạt; nếu còn bóng phụ từ vật phẩm nhân bản, cuộc chơi vẫn tiếp tục[cite: 1].

<p align="center">
  <img width="599" height="261" alt="image" src="https://github.com/user-attachments/assets/4a14a654-d000-424c-913c-a90a3fb0dcc5" />
</p>

---

## 🧱 Hệ thống gạch & Vật phẩm bổ trợ (Bricks & Power-ups)

### Danh sách các loại gạch đặc biệt

| Loại gạch | Biểu tượng | Đặc điểm hoạt động |
| :--- | :---: | :--- |
| **NormalBrick** | 🧱 | Phá hủy ngay lập tức chỉ sau 1 lần chạm bóng[cite: 1]. |
| **ImmortalBrick**| 🛡️ | Tự hồi phục độ bền nếu không bị tấn công dồn dập liên tục[cite: 1]. |
| **LifeUpBrick** | ❤️ | Sinh ra vật phẩm tăng mạng khi bị phá hủy[cite: 1]. |
| **GoldBrick** | 🪙 | Rơi ra đồng xu vàng mang lại lượng điểm cực lớn[cite: 1]. |
| **FallBombBrick**| 💣 | Rơi bom gây mất mạng trực tiếp nếu người chơi hứng phải[cite: 1]. |
| **AreaBlastBrick**| 💥 | Kích hoạt hiệu ứng nổ lan, dọn sạch các viên gạch lân cận[cite: 1]. |
| **LuckyWheelBrick**| 🎁 | Sinh ngẫu nhiên 1 trong 5 loại Power-up hữu ích[cite: 1]. |
| **PushBrick** | ⬇ | Đẩy toàn bộ các hàng gạch phía trên lùi xuống gần Paddle hơn[cite: 1]. |

> ⚠️ **Đặc biệt:** Từ **Level 7**, gạch sẽ tự động hạ thấp dần theo thời gian[cite: 1]. Từ **Level 10**, hệ thống kích hoạt `SpecialBrickEffect.randomRow` tự sinh hàng mới[cite: 1]. Riêng ở chế độ **Ultimate**, gạch còn có thể di chuyển ngang để thử thách tài ngắm bắn của bạn[cite: 1].

### Hệ thống Power-ups (Vật phẩm bổ trợ)
Vật phẩm rơi tự do với vận tốc cố định (`FALLING_SPEED`) trải qua 4 trạng thái vòng đời: `FALLING` ➔ `APPLYING` ➔ `END` / `FALLOUT`[cite: 1]. Thời gian hiệu lực trung bình là **~6 giây**[cite: 1].

*   **Tương tác Paddle:** `LongerPaddlePowerUp` (Kéo dài) / `ShortenPaddlePowerUp` (Thu ngắn)[cite: 1].
*   **Tương tác Bóng:** `BiggerBallPowerUp` (Bóng khổng lồ) / `DuplicateBallPowerUp` (Nhân ba bóng)[cite: 1].
*   **Tấn công:** `ShotPowerUp` (Trang bị súng laser bắn gạch)[cite: 1].
*   **Hỗ trợ sinh tồn:** `LifeUpPowerUp` (Cộng mạng) / `FallBoomPowerUp` (Bom bẫy)[cite: 1].
*   **Kinh tế:** `GoldPowerUp` / `Gold` (Tăng điểm số)[cite: 1].

*Lưu ý: Để cân bằng game, mỗi màn chơi chỉ giới hạn tối đa 20 power-up thông thường (ngoại trừ LifeUp và FallBoom).*

---

## 🎮 Hướng dẫn điều khiển (Controls)

| Ngữ cảnh | Phím / Chuột | Hành động |
| :--- | :--- | :--- |
| **Menu & Chọn màn** | `Chuột trái` / `Phím mũi tên` + `Enter` | Di chuyển, chọn và tương tác các nút bấm[cite: 1] |
| **Trong trận đấu** | `←` / `→` (Mũi tên Trái / Phải) | Di chuyển thanh đỡ (Paddle)[cite: 1] |
| | `SPACE` (Dấu cách) | Phóng bóng (khi bóng ở trạng thái READY)[cite: 1] |
| | `ESC` | Tạm dừng (Pause) hoặc Tiếp tục game[cite: 1] |
| **Trò chơi kết thúc**| `SPACE` | Quay trở lại Menu chính[cite: 1] |
| | `ESC` | Thoát trò chơi[cite: 1] |
| **Phần hướng dẫn** | `←` / `→` hoặc `SPACE` | Chuyển trang hướng dẫn[cite: 1] |
| | Nút `Back` trên UI | Quay lại mục Option[cite: 1] |

---

## Cấp độ và chế độ
- 13 màn Classic đầu tiên tăng dần độ khó bằng cách thêm gạch đặc biệt, giảm khoảng cách, và cho phép gạch hạ thấp.
- 2 màn Ultimate mở khóa cơ chế sinh hàng ngẫu nhiên và gạch biết di chuyển.
- Bộ bản đồ cơ sở được tạo từ `map/ListOfMap`, kết hợp dữ liệu trong `staticMap/map.txt` và `immortalMap.txt`, sau đó được `CreatSpecialBrick` gán thêm gạch đặc biệt dựa trên level.

---

## Cài đặt & chạy
Yêu cầu:
- JDK 17 trở lên
- Maven 3.9+

Chạy trực tiếp bằng Maven:

```bash
mvn clean javafx:run
```

Tạo JAR phát hành:

```bash
mvn clean package
java -jar target/akanoid-1.0-SNAPSHOT.jar
```

Nếu chạy ngoài Maven, bảo đảm thêm module JavaFX tương ứng vào `--module-path` (hoặc biến môi trường `PATH_TO_FX`).

---

## Cấu trúc dự án
- `src/main/java/main/Main.java` – Entry point JavaFX.
- `gamemanager/GameManager.java` – Bộ điều phối trạng thái và vòng lặp chính.
- `process/` – Tập hợp các màn (Loading, Menu, PickLevel, Playing, Option, Tutorial, GameOver).
- `gameobject/` – Lớp cơ sở, bóng, paddle, gạch, power-up.
- `map/` – Sinh map nền, chèn gạch đặc biệt, tiện ích cận cảnh.
- `src/main/resources/` – Ảnh, âm thanh, video intro và các cấu hình map tĩnh.

---

## Tài nguyên & âm thanh
- `sound/mapsound1.mp3` phát nền vòng lặp; quản lý qua `UI.GlobalSound`.
- `sound/click.wav` dùng cho các nút trong UI.
- Hình ảnh UI và background: `src/main/resources/image`.
- Video intro: `src/main/resources/video/intro.mp4`.

---

## Tình trạng phát triển
- Một số lớp đang refactor (ví dụ PauseMenu) nên tạm thời chưa được gắn vào PlayingProcess.
- Console có thể xuất các thông báo debug; coi như hành vi bình thường trong giai đoạn phát triển.
- Tài nguyên ảnh kích thước lớn, nên tối ưu nếu chuẩn bị phát hành chính thức.
- Có thể sẽ mở rộng thêm sau này để tạo thành sản phẩm ổn hơn.

---

## Thành viên
- Nhóm trưởng: Nguyễn Ngọc Phú.
- Thành viên: Ma Đình Kiên, Phạm Tuấn Phong

---

## Phân công công việc 
<a href="https://docs.google.com/document/d/1OFlPN_8zJcJBUnvsHOD6V-ZPiNAUXaWzxZwewSnSOd8/edit?tab=t.0" target="_blank">Xem tài liệu chi tiết tại Google Docs</a>
## Video demo
<a href="https://drive.google.com/file/d/18CLS8GrBo6uLRoVoOXtPZS19hjr8TbDp/view?usp=sharing" target="_blank">Video demo</a>

## 🖼️ Thư viện hình ảnh Demo 

<table align="center">
  <tr>
    <td><img src="https://github.com/user-attachments/assets/86c0f64a-6101-469f-af31-72b546799557" width="100%" alt="Loading Screen"/><br><p align="center"><i>Màn hình tải trò chơi (Loading)</i></p></td>
    <td><img src="https://github.com/user-attachments/assets/d51b8988-41df-4a85-865d-d96202b4f29b" width="100%" alt="Main Menu"/><br><p align="center"><i>Giao diện chính (Main Menu) sống động</i></p></td>
  </tr>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/895fff7a-e84c-4727-8f63-308d99493744" width="100%" alt="Option Menu"/><br><p align="center"><i>Tùy chỉnh cấu hình âm lượng</i></p></td>
    <td><img src="https://github.com/user-attachments/assets/be8610ad-1667-4107-b85b-6cda6eb2d6c2" width="100%" alt="Tutorial Screen"/><br><p align="center"><i>Bảng hướng dẫn chơi trực quan</i></p></td>
  </tr>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/7e0078cb-626c-48b4-be51-6ba1432e67a0" width="100%" alt="Classic Mode"/><br><p align="center"><i>Lựa chọn màn chơi chế độ Classic</i></p></td>
    <td><img src="https://github.com/user-attachments/assets/b3a18981-9762-4b69-8cae-a099086646d1" width="100%" alt="Ultimate Mode"/><br><p align="center"><i>Chế độ Ultimate đầy thử thách</i></p></td>
  </tr>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/037e4d4e-acec-45c0-803b-9e775759e8e5" width="100%" alt="Pause Menu"/><br><p align="center"><i>Giao diện PauseMenu tiện lợi</i></p></td>
    <td><img src="https://github.com/user-attachments/assets/79748044-5e7a-4396-aeb3-c0c831a1e9e8" width="100%" alt="Gameplay"/><br><p align="center"><i>Trận đấu phá gạch kịch tính với hiệu ứng đẹp mắt</i></p></td>
  </tr>
</table>

<p align="center">
  <img src="https://github.com/user-attachments/assets/1992b51c-4350-4300-974e-831cda4effd5" width="60%" alt="Game Over"/><br>
  <i>Màn hình Game Over kịch tính khi thất bại</i>
</p>



