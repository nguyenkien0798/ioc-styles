# ioc-styles

Style dùng chung cho mọi micro-app PrimeNG theo component của **iocv3-fe**: input/select/textarea cao `2.5rem`, bo `0.75rem`, nút cùng kích thước (sm `2rem` / `0.5rem`), focus ring `1px` màu primary, card bo `1.5rem`, dialog bo `1rem`.

Package chỉ có SCSS, không phụ thuộc Angular, nên dùng được cả Angular 19 (PrimeNG 19) và Angular 21 (PrimeNG 21).

Màu chỉnh ở hai file:

- `styles/theme/ioc-light/theme.css` — gắn lên `:root`
- `styles/theme/ioc-dark/theme.css` — gắn lên `.dark` (class trên `<html>`)

`styles/theme/variable.css` là bảng token PrimeNG gốc. Hai file trên nạp sau nên giá trị ở đó thắng. Shell bật tối bằng class `dark`, không đổi file CSS.

## Cài

```bash
npm install ioc-styles
```

Local, trước khi publish:

```json
"ioc-styles": "file:../ioc-style"
```

## Dùng

Gọi mixin **bên trong selector host** của micro-app. Stylesheet được shell nhét vào `document.head`; selector trần sẽ đè giao diện shell.

`$prefix` là prefix biến CSS của PrimeNG trong app đó. Mặc định `p` (token `--p-*`). App đặt prefix riêng thì truyền đúng prefix đó, để token của app tách khỏi `--p-*` của shell.

`$important` bật khi theme của shell đè stylesheet của micro-app.

```scss
@use 'ioc-styles/theme';
@use 'ioc-styles' as ioc;

app-root {
    @include ioc.apply($prefix: p);
}
```

Khi stylesheet bị shell đè:

```scss
app-root {
    @include ioc.apply($prefix: p, $important: true);
}
```

Dialog/popover gắn `appendTo="body"` không nằm trong host, nên gọi lại mixin trên đúng class overlay nếu control trong đó cũng cần cùng style.

## Token có thể ghi đè

Đặt trên host, trước hoặc sau mixin (biến khai báo sau thắng nếu cùng selector):

| Biến | Mặc định | Nguồn iocv3-fe |
|---|---|---|
| `--ioc-control-height` | `2.5rem` | input, select, button |
| `--ioc-control-radius` | `0.75rem` | input, select, button |
| `--ioc-button-sm-height` | `2rem` | `.btn-sm` |
| `--ioc-button-sm-radius` | `0.5rem` | `.btn-sm`, nút icon |
| `--ioc-card-radius` | `1.5rem` | `card-layout` |
| `--ioc-dialog-radius` | `1rem` | `_dialog.scss` |
| `--ioc-control-font-size` | `--app-form-font-size`, fallback `14px` | `_variables.scss` |
| `--ioc-primary-color` | `#0070ce` / dark `#49aafc` | `ioc-light/theme.css`, `ioc-dark/theme.css` |
| `--ioc-surface-card` | `#ffffff` / dark `#15283c` | `ioc-light/theme.css`, `ioc-dark/theme.css` |
| `--ioc-surface-border` | `#cecece` / dark `#424b57` | `ioc-light/theme.css`, `ioc-dark/theme.css` |
| `--ioc-text-color` | `#1e293b` / dark `#e1e2e3` | `ioc-light/theme.css`, `ioc-dark/theme.css` |

Shell đặt `html { font-size: 14px }`. App chạy riêng cần cùng root size thì rem mới ra đúng pixel của shell.
