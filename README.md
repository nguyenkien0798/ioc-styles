# ioc-styles

Biến CSS dùng chung: màu, cỡ chữ, bo góc và kích thước. Package không gắn style cho component. App Angular hay React tự viết CSS và đọc các biến này.

Kích thước ghi bằng `px`, không phụ thuộc `font-size` của `html`.

## Cài

```bash
npm install ioc-styles
```

Cần Sass trong app (`sass` với Angular, `sass` với Vite hoặc CRA).

`ioc-styles` và `ioc-styles/theme` nạp cùng một bộ biến. Màu đổi theo sáng/tối nằm ở `ioc-light/theme.css` và `ioc-dark/theme.css`, đúng tên biến app đang đọc (`--primary-color`, `--surface-*`, `--p-primary-color`, …).

## Angular

Trong file style global:

```scss
@use 'ioc-styles/theme';
```

File đó phải nằm trong `styles` của `angular.json` (hoặc `project.json`). App không khai báo lại các biến màu này.

## React

Trong file style global, ví dụ `src/index.scss`, rồi import file đó từ `main.tsx` hoặc `index.tsx`:

```scss
@use 'ioc-styles/theme';
```

```tsx
import './index.scss';
```

Vite và CRA đều xử lý được `@use` khi đã cài `sass`.

## Sáng và tối

Màu sáng gắn trên `:root`. Màu tối gắn trên class `dark` của `<html>`.

```ts
document.documentElement.classList.toggle('dark', isDark);
```

Bỏ class `dark` để về sáng. Không cần đổi file CSS.

`--ioc-control-background`, `--ioc-control-border-color` và `--ioc-control-color` trỏ vào màu theme, nên ô nhập đổi theo sáng/tối.

## App tự style component

Đọc biến trong CSS của app:

```css
.field {
    height: var(--ioc-control-height);
    border-radius: var(--ioc-control-radius);
    font-size: var(--ioc-font-size-control);
    color: var(--ioc-text-color);
    background: var(--ioc-control-background);
    border: 1px solid var(--ioc-control-border-color);
}

.button {
    height: var(--ioc-button-height);
    border-radius: var(--ioc-button-radius);
    font-size: var(--ioc-button-font-size);
}
```

Sửa màu sáng/tối ở `ioc-light/theme.css` và `ioc-dark/theme.css`. App chỉ `@use 'ioc-styles/theme'`.

## Biến nằm ở đâu

| File | Gắn lên | Nội dung |
|---|---|---|
| `styles/theme/shared.css` | `:root` | cỡ chữ, chiều cao, bo góc, màu không đổi theo theme |
| `styles/theme/ioc-light/theme.css` | `:root` | màu sáng |
| `styles/theme/ioc-dark/theme.css` | `.dark` | màu tối, gồm `--primary-color`, `--surface-*`, `--p-*` |

## Biến thường dùng

| Biến | Mặc định | Việc dùng |
|---|---|---|
| `--ioc-font-size-body` | `15px` | chữ nội dung |
| `--ioc-font-size-control` | `14px` | chữ form, nút |
| `--ioc-font-size-helper` | `11.375px` | chữ phụ |
| `--ioc-font-size-dialog-title` | `21px` | tiêu đề dialog |
| `--ioc-control-height` | `35px` | cao input, select, nút |
| `--ioc-control-radius` | `12px` | bo input, select, nút |
| `--ioc-button-sm-height` | `28px` | nút nhỏ |
| `--ioc-button-sm-radius` | `8px` | bo nút nhỏ, nút icon |
| `--ioc-card-radius` | `21px` | card |
| `--ioc-dialog-radius` | `16px` | dialog |
| `--ioc-primary-color` | `#0070ce` / tối `#49aafc` | màu chính |
| `--ioc-text-color` | `#1e293b` / tối `#e1e2e3` | chữ |
| `--ioc-surface-card` | `#ffffff` / tối `#15283c` | nền card, ô nhập |
| `--ioc-surface-border` | `#cecece` / tối `#424b57` | viền |
| `--ioc-invalid-color` | `#ef4444` | lỗi |
| `--ioc-required-color` | `#f65a5a` | dấu bắt buộc |
