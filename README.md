# ioc-styles

Gói SCSS dùng chung cho micro-app PrimeNG. Nó kéo màu, kích thước form, nút, card, dialog và overlay về cùng một bộ token. Package không phụ thuộc Angular, dùng được với Angular 19 (PrimeNG 19) và Angular 21 (PrimeNG 21).

Số đo mặc định (root `14px` của shell):

- input, select, textarea, nút: cao `2.5rem`, bo `0.75rem`
- nút nhỏ và nút icon: cao `2rem`, bo `0.5rem`
- focus ring: `1px` màu primary
- card: bo `1.5rem`
- dialog: bo `1rem`

## Cài

```bash
npm install ioc-styles
```

Chưa publish, trỏ thẳng vào repo:

```json
"ioc-styles": "file:../ioc-style"
```

Sass của app phải resolve được package trong `node_modules` (Angular CLI làm việc này mặc định). Entry public:

| Import | Nội dung |
|---|---|
| `ioc-styles` | mixin `apply` và token kích thước |
| `ioc-styles/theme` | màu sáng trên `:root`, màu tối trên `.dark` |

## Gắn vào micro-app

Tạo một file SCSS của app (ví dụ `src/styles/ioc.scss`) và khai báo nó trong `angular.json` → `styles`, sau theme PrimeNG của app.

```scss
@use 'ioc-styles/theme';
@use 'ioc-styles' as ioc;

app-root,
.p-dialog,
.p-confirmdialog,
.p-drawer,
.p-popover,
.p-menu,
.p-toast,
.p-select-overlay,
.p-multiselect-overlay,
.p-treeselect-overlay,
.p-autocomplete-overlay,
.p-datepicker-panel,
.p-colorpicker-panel {
    @include ioc.apply($prefix: p);
}
```

Thay `app-root` bằng selector host thật của app (`app-sample`, `app-operations-map-root`, …).

Gọi mixin **bên trong** selector đó. Stylesheet của micro-app thường được shell chèn vào `document.head`. Selector trần (không bọc host) sẽ đè luôn giao diện shell.

### Overlay

`appendTo="body"` đưa dialog, menu, toast, panel select ra ngoài host. Vì vậy các class overlay phải đứng cùng host trong danh sách selector. Mixin vẫn ăn khi selector chính là overlay (`&.p-dialog`).

App nào gắn `styleClass` riêng cho dialog thì thay `.p-dialog` bằng class đó, để không đụng dialog của shell.

### `$prefix`

Prefix biến CSS của PrimeNG trong app đó.

- Mặc định `p` → token `--p-*`.
- App đặt prefix riêng (ví dụ `omap`) thì truyền đúng prefix đó, để token của app tách khỏi `--p-*` của shell.

```scss
@include ioc.apply($prefix: omap);
```

### `$important`

Bật khi theme của shell được nạp sau và đè stylesheet của micro-app. Mọi khai báo kích thước và màu then chốt sẽ có `!important`.

```scss
@include ioc.apply($prefix: omap, $important: true);
```

## Theme sáng / tối

`ioc-styles/theme` nạp lần lượt:

1. `styles/theme/variable.css` — bảng token PrimeNG gốc
2. `styles/theme/ioc-light/theme.css` — gắn lên `:root`
3. `styles/theme/ioc-dark/theme.css` — gắn lên `.dark`

Hai file màu nạp sau nên thắng `variable.css`. Shell bật tối bằng class `dark` trên `<html>`, không đổi file CSS.

Shell đặt `html { font-size: 14px }`. App chạy riêng cần cùng root size thì `rem` mới ra đúng pixel của shell.

## Ghi đè trong một app

Đặt biến trên cùng selector host. Biến khai báo sau thắng nếu cùng selector.

```scss
app-root {
    --ioc-control-height: 2.25rem;
    @include ioc.apply($prefix: p);
}
```

| Biến | Mặc định | Ảnh hưởng |
|---|---|---|
| `--ioc-control-height` | `2.5rem` | input, select, button |
| `--ioc-control-radius` | `0.75rem` | input, select, button |
| `--ioc-button-sm-height` | `2rem` | nút nhỏ |
| `--ioc-button-sm-radius` | `0.5rem` | nút nhỏ, nút icon |
| `--ioc-card-radius` | `1.5rem` | card |
| `--ioc-dialog-radius` | `1rem` | dialog |
| `--ioc-control-font-size` | `1rem` | chữ control |
| `--ioc-primary-color` | `#0070ce` / tối `#49aafc` | primary, focus |
| `--ioc-surface-card` | `#ffffff` / tối `#15283c` | nền control, card |
| `--ioc-surface-border` | `#cecece` / tối `#424b57` | viền |
| `--ioc-text-color` | `#1e293b` / tối `#e1e2e3` | chữ |

Màu dùng chung cho mọi app sửa trong `ioc-light/theme.css` và `ioc-dark/theme.css` của package, rồi cài version mới. Đổi `--ioc-*` hoặc rule trong package xong, component nằm trong selector trên ăn theo, không cần sửa CSS từng app.

Gỡ stylesheet local đang đánh cùng class (`.p-inputtext`, `.p-button`, …). Rule cũ vẫn thắng nếu specificity hoặc thứ tự nạp cao hơn package.
