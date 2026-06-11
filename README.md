# EndWorth - Hệ Thống Bán Đồ & Cấp Độ Nghề Nghiệp

**EndWorth** là một plugin Spigot/Paper mạnh mẽ, được thiết kế để tạo ra một hệ thống kinh tế ảo hoàn chỉnh cho máy chủ Minecraft của bạn. Plugin không chỉ hiển thị giá trị mua/bán trực tiếp trên vật phẩm mà còn tích hợp một hệ thống cấp độ nghề nghiệp sâu sắc, khuyến khích người chơi tham gia vào các hoạt động kinh tế đa dạng.

## Tính Năng Nổi Bật

*   **Hiển Thị Giá Trị Vật Phẩm (Lore):** Tự động thêm dòng thông tin "Worth" vào mọi vật phẩm, hiển thị giá mua và giá bán. Người chơi có thể bật/tắt tính năng này.
*   **Hệ Thống Bán Đồ Qua GUI:** Cung cấp một giao diện đồ họa (`/sell`) trực quan để người chơi có thể bán nhanh các vật phẩm trong túi đồ của mình.
*   **Cấp Độ Ngành Nghề:**
    *   Phân loại vật phẩm vào các ngành nghề khác nhau (ví dụ: `ores`, `wood`, `food`).
    *   Người chơi sẽ tăng cấp và nhận được ưu đãi tốt hơn khi bán vật phẩm thuộc một ngành nghề nhất định.
    *   Giao diện (`/selllevel`) để theo dõi tiến trình và lộ trình thăng cấp của từng ngành.
*   **Lịch Sử Giao Dịch:** Người chơi có thể xem lại lịch sử các lần bán đồ của mình (`/sellhistory`), và quản trị viên có thể xem lịch sử của người khác.
*   **Tùy Chỉnh Linh Hoạt:** Hầu hết mọi khía cạnh của plugin đều có thể được cấu hình, từ giá cả, giao diện, cho đến các cấp độ và phần thưởng.
*   **Hỗ Trợ PlaceholderAPI:** Tích hợp sẵn các placeholder để hiển thị thông tin kinh tế của người chơi trên scoreboard, tab, hoặc các plugin khác.
*   **Hiệu Suất Cao:** Sử dụng PacketEvents để sửa đổi lore vật phẩm một cách "ảo", không làm thay đổi dữ liệu thực của vật phẩm, đảm bảo hiệu suất và tránh các vấn đề tương thích.

## Lệnh & Quyền Hạn

| Lệnh | Bí danh | Mô tả | Quyền Hạn |
| :--- | :--- | :--- | :--- |
| `/endworth sell` | `/sell` | Mở giao diện bán đồ. | `endworth.sell` |
| `/endworth selllevel` | `/selllevel` | Mở bảng cấp độ các ngành nghề. | `endworth.selllevel` |
| `/endworth history [tên]` | `/sellhistory [tên]` | Xem lịch sử bán đồ của bạn hoặc của người khác. | `endworth.sellhistory` (cho bản thân), `endworth.admin` (cho người khác) |
| `/endworth toggle` | `/worth on`, `/worth off` | Bật/tắt hiển thị giá trị trên vật phẩm. | `endworth.toggle` |
| `/endworth reload` | | Tải lại toàn bộ cấu hình của plugin. | `endworth.admin` |

## Cài Đặt

1.  Tải phiên bản mới nhất của plugin từ trang phát hành.
2.  **Yêu cầu PacketEvents.** Tải và cài đặt PacketEvents vào thư mục `plugins` của bạn. Đây là một thư viện bắt buộc.
3.  (Tùy chọn) Cài đặt PlaceholderAPI để sử dụng các placeholder.
4.  Đặt file `EndWorth.jar` vào thư mục `plugins` của máy chủ.
5.  Khởi động lại máy chủ. Plugin sẽ tự động tạo các file cấu hình mặc định.

## Cấu Hình

Plugin cung cấp các file cấu hình sau trong thư mục `/plugins/EndWorth/`:

### `config.yml`

File cấu hình chính cho các thiết lập chung.

```yaml
settings:
  # Thế giới mà người chơi không thể sử dụng lệnh /sell
  disabled_worlds:
    - "world_the_end"
  # Tính giá dựa trên độ bền còn lại của vật phẩm
  durability_check: true
  # Thưởng thêm tiền dựa trên tổng cấp độ của các enchantment
  enchant_bonus: 0.05 # (5% mỗi cấp)
  # Tính cả giá trị của các vật phẩm bên trong Shulker Box
  shulker_check: true
```

### `prices.yml`

Nơi định giá và phân loại cho tất cả các vật phẩm.

```yaml
# Tên vật phẩm theo chuẩn Bukkit Material
DIAMOND:
  # Giá plugin sẽ mua từ người chơi (giá hiển thị màu xanh)
  unit_sell: 100.0
  # Giá người chơi sẽ mua từ plugin (giá hiển thị màu đỏ)
  unit_buy: 120.0
  # Ngành nghề của vật phẩm, phải khớp với tên trong levelsell.yml
  category: "ores"

WHEAT:
  unit_sell: 0.5
  unit_buy: 1.0
  category: "food"
```

### `levelsell.yml`

Định nghĩa các cấp độ, yêu cầu và phần thưởng cho mỗi ngành nghề.

```yaml
enabled: true # Bật/tắt toàn bộ hệ thống level

categories:
  # Tên ngành nghề, phải khớp với 'category' trong prices.yml
  ores:
    name: "&f&lNgành Khai Khoáng"
    icon: DIAMOND_PICKAXE
    # Danh sách các cấp độ
    levels:
      - # Cấp 1
        money_required: 1000 # Cần đạt 1000$ doanh thu để lên cấp
        sell_reduce: 0.5 # Giá bán = 50% giá mua
        # Các tùy chọn khác như enchant_multiplier, durability_affect...
      - # Cấp 2
        money_required: 5000
        sell_reduce: 0.6 # Giá bán = 60% giá mua
```

### `lvgui.yml`

Tùy chỉnh giao diện của bảng cấp độ nghề nghiệp (`/selllevel`).

```yaml
gui:
  title: "&6&lBảng Cấp Độ Nghề Nghiệp"
  size: 54
  fill_material: GRAY_STAINED_GLASS_PANE

# Đặt vị trí hiển thị cho từng ngành (slot 0-53)
slots:
  ores: 10
  food: 12
  wood: 14
```

### `sell.yml`

Tùy chỉnh giao diện bán đồ (`/sell`).

## Hỗ Trợ PlaceholderAPI

Nếu bạn đã cài đặt PlaceholderAPI, EndWorth sẽ tự động đăng ký các placeholder sau, cho phép bạn hiển thị dữ liệu kinh tế của người chơi trên các plugin khác như scoreboard, tab, hoặc голограми.

| Placeholder | Mô tả |
| :--- | :--- |
| `%endworth_player_sell%` | Hiển thị tổng số tiền người chơi đã kiếm được từ việc bán đồ. |
| `%endworth_player_sell_<category>%` | Hiển thị tổng số tiền người chơi đã kiếm được từ một ngành nghề cụ thể. Thay thế `<category>` bằng tên ngành nghề (ví dụ: `ores`, `food`, `wood`). |

**Ví dụ sử dụng:**

*   `%endworth_player_sell%` -> `1.5M`
*   `%endworth_player_sell_ores%` -> `750.2K`
*   `%endworth_player_sell_wood%` -> `12.3K`

Lưu ý: Tên ngành nghề (`<category>`) phải được viết bằng chữ thường và khớp với tên đã được định nghĩa trong file `prices.yml` và `levelsell.yml`.