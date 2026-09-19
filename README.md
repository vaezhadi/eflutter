# Eflutter

این ریپو فایل‌های منتشرشده‌ی `vaezhadi/eflutter-dev` را بدون انتقال تاریخچه،
اشخاص یا commitهای ریپوی منبع همگام می‌کند. هر همگام‌سازی یک commit جدید با
هویت `vaezhadi` در این ریپو ایجاد می‌کند.

## فعال‌سازی همگام‌سازی فوری

فایل `.github/workflows/sync-from-dev.yml` با زمان‌بندی پنج‌دقیقه‌ای و اجرای
دستی آماده است. برای اجرای فوری، در ریپوی `vaezhadi/eflutter-dev` یک secret
با نام `SYNC_DESTINATION_TOKEN` بسازید. این توکن باید دسترسی `Contents: Read and
write` روی همین ریپو داشته باشد، سپس workflow زیر را در ریپوی منبع با نام
`.github/workflows/notify-destination.yml` اضافه کنید:

```yaml
name: Notify file mirror

on:
  push:
    branches: [main]

jobs:
  notify:
    runs-on: ubuntu-latest
    steps:
      - name: Trigger destination sync
        env:
          GH_TOKEN: ${{ secrets.SYNC_DESTINATION_TOKEN }}
        run: |
          gh api \
            --method POST \
            -H "Accept: application/vnd.github+json" \
            "/repos/vaezhadi/Eflutter/dispatches" \
            -f event_type=sync-from-dev
```

اگر می‌خواهید commitها با توکن حساب شخصی شما push شوند، secret اختیاری
`SYNC_TOKEN` را در همین ریپو تنظیم کنید. در غیر این صورت از `GITHUB_TOKEN` خود
این ریپو استفاده می‌شود.