# ioc-styles

Biến CSS dùng chung cho micro-app. Package khai báo màu, cỡ chữ, bo góc và kích thước. Micro-app tự viết CSS component và đọc các biến này. Package không có rule đè `.p-button`, `.p-inputtext`, dialog hay overlay.

Kích thước ghi bằng `px`.

## Cài

```bash
npm install ioc-styles
```

Chưa publish, trỏ thẳng vào repo:

```json
"ioc-styles": "file:../ioc-style"
```

Khai báo trong `angular.json` → `styles`, sau theme PrimeNG của app:

```scss
@use 'ioc-styles/theme';
```

`ioc-styles` và `ioc-styles/theme` cùng nạp một bộ biến.

## Biến nạp ở đâu

| File | Gắn lên | Sửa gì |
|---|---|---|
| `styles/theme/shared.css` | `:root` | cỡ chữ, chiều cao, bo góc, màu không đổi theo theme |
| `styles/theme/ioc-light/theme.css` | `:root` | mã màu sáng |
| `styles/theme/ioc-dark/theme.css` | `.dark` | mã màu tối |

Shell bật tối bằng class `dark` trên `<html>`. `--ioc-control-background`, `--ioc-control-border-color`, `--ioc-control-color` trỏ về màu theme, nên đổi theo sáng/tối.

Tên cũ còn lại: `--border-radius`.

Cỡ chữ micro-app đang đọc: `--app-form-font-size` `14px`, `--app-title-text-size` `21px`, `--app-title-content-text-size` `18px`.

## Micro-app tự đè component

Ví dụ trong stylesheet của app:

```scss
.p-button {
    height: var(--ioc-button-height);
    border-radius: var(--ioc-button-radius);
    font-size: var(--ioc-font-size-control);
}

.p-button-outlined.p-button-secondary {
    border: 1px solid var(--ioc-control-border-color);
    color: var(--ioc-control-color);
    background: var(--ioc-control-background);
}

.p-inputtext,
.p-select,
.p-textarea {
    height: var(--ioc-control-height);
    border-radius: var(--ioc-control-radius);
    font-size: var(--ioc-font-size-control);
}
```

App nào cần token PrimeNG (`--p-*`) ăn theo bộ này thì tự gán, ví dụ `--p-primary-color: var(--ioc-primary-color)`.

## Biến thường dùng

| Biến | Mặc định | |
|---|---|---|
| `--ioc-font-size-body` | `15px` | chữ nội dung |
| `--ioc-font-size-control` | `14px` | chữ form, nút |
| `--ioc-font-size-helper` | `13px` | chữ phụ |
| `--ioc-font-size-dialog-title` | `21px` | tiêu đề dialog |
| `--ioc-control-height` | `40px` | cao input, select, nút |
| `--ioc-control-radius` | `12px` | bo input, select, nút |
| `--ioc-button-sm-height` | `32px` | nút nhỏ |
| `--ioc-button-sm-radius` | `8px` | bo nút nhỏ, nút icon |
| `--ioc-card-radius` | `24px` | card |
| `--ioc-dialog-radius` | `16px` | dialog |
| `--ioc-primary-color` | `#0070ce` / tối `#49aafc` | primary |
| `--ioc-text-color` | `#1e293b` / tối `#e1e2e3` | chữ |
| `--ioc-surface-card` | `#ffffff` / tối `#15283c` | nền |
| `--ioc-surface-border` | `#cecece` / tối `#424b57` | viền |
| `--ioc-invalid-color` | `#ef4444` | lỗi, dùng chung |
| `--ioc-required-color` | `#f65a5a` | dấu bắt buộc, dùng chung |
