<div align="center">

# 🏹 Hunter DNS

**اسمارت DNS برای ایران — نصب با یک دستور، پنل مدیریت و پنل مشتری**

[فارسی](#فارسی) · [English](#english)

![Version](https://img.shields.io/badge/version-0.0.8-orange)
![Platform](https://img.shields.io/badge/Ubuntu%20%2F%20Debian-supported-orange)
![Shell](https://img.shields.io/badge/installer-bash-orange)

</div>

---

<a name="فارسی"></a>

<div dir="rtl">

## فارسی

Hunter DNS یک نصب‌کننده یک‌فایلی برای راه‌اندازی **اسمارت DNS** است؛ یعنی DNS و پراکسی‌ای که سایت‌های محدودشده را برای کاربران داخل ایران در دسترس می‌کند. همراهش یک پنل وب برای مدیریت و یک پنل برای مشتری‌ها نصب می‌شود.

### چطور کار می‌کند؟

سیستم از دو بخش ساخته شده است:

| بخش | کجاست | کارش چیست |
|---|---|---|
| **relay (رله)** | سرور داخل ایران | کاربرها DNS خودشان را روی این سرور می‌گذارند. دامنه‌های محدودشده را به همین سرور جواب می‌دهد و اتصال را به سرور خارج می‌فرستد |
| **exit (خروجی)** | سرور خارج از ایران | نام سایت (SNI) را می‌خواند و به سایت اصلی وصل می‌شود |
| **single (تک‌سرور)** | یک سرور خارج از ایران | هر دو بخش روی یک سرور، بدون رله در ایران |
| **node (نود)** | سرور خارج دیگر | یک خروجی اضافه که به پنل اصلی وصل می‌شود |

### امکانات

- نصب و آپدیت با یک دستور؛ اجرای دوباره‌اش بی‌خطر است و از تنظیمات قبلی نسخه پشتیبان می‌گیرد
- پنل مدیریت و پنل مشتری با رابط فارسی و انگلیسی
- مدیریت مشتری‌ها، پلن‌ها، سهمیه حجم و محدودیت سرعت
- کنترل دسترسی و شمارش ترافیک هر کاربر
- DNS روی HTTPS و TLS
- گواهی HTTPS خودکار (Let's Encrypt) با تمدید خودکار
- ربات تلگرام
- حذف کامل و تمیز: دقیقاً همان چیزهایی که نصب کرده را برمی‌گرداند

### پیش‌نیازها

- سرور **Ubuntu** یا **Debian** با دسترسی root
- سیستم‌عامل با systemd
- پورت‌های **53**، **80** و **443** آزاد (یا در اختیار nginx)
- *اختیاری:* یک دامنه یا زیردامنه که رکورد **A** آن به IP سرور اشاره کند (برای HTTPS و پنل مشتری)

> اگر دامنه را روی Cloudflare می‌گذارید، ابر کنار رکورد باید **خاکستری (DNS only)** باشد.

### نصب

روی سرور، به‌عنوان root اجرا کنید:

```bash
curl -fsSLO https://raw.githubusercontent.com/AlisanX17/Hunter-dns/main/hunter-dns.sh && sudo bash hunter-dns.sh
```

نصب‌کننده می‌پرسد این سرور کدام نقش را دارد:

```
1) relay  - سرور داخل ایران
2) exit   - سرور خارج که به سایت‌های محدود وصل می‌شود
3) single - هر دو روی یک سرور خارج، بدون رله
4) node   - خروجی اضافه، متصل به پنل اصلی
```

**اگر دو سرور دارید:** اول سرور **خارج** (`exit`) را نصب کنید، بعد سرور **ایران** (`relay`). دستور بالا را روی هر دو اجرا کنید.
**اگر یک سرور خارج دارید:** گزینه `3` را بزنید.

بعد از آن می‌پرسد `domain:`. اگر دامنه دارید و رکوردش را ساخته‌اید بنویسید؛ اگر نه، Enter بزنید تا HTTPS رد شود. توجه: بدون دامنه، **پنل مشتری** راه نمی‌افتد.

### نصب بدون سؤال

```bash
sudo env PANEL_DOMAIN=panel.example.com ASSUME_YES=1 bash hunter-dns.sh
```

### دستورها

```bash
sudo bash hunter-dns.sh --check       # بررسی سرور بدون تغییر دادن چیزی
sudo bash hunter-dns.sh --version     # نمایش نسخه
sudo bash hunter-dns.sh --tunnel      # انتخاب دوباره تونل بین رله و خروجی
sudo bash hunter-dns.sh --help        # راهنما
sudo bash hunter-dns.sh               # نصب یا آپدیت
```

### آپدیت

همان دستور نصب را دوباره بزنید. مشتری‌ها، تنظیمات، گواهی‌ها و لیست آدرس‌های مجاز نگه داشته می‌شوند.

### حذف

```bash
sudo bash hunter-dns.sh --uninstall
```

این دستور فقط چیزهایی را که خودش نصب کرده برمی‌گرداند. گواهی‌های HTTPS در `/etc/letsencrypt` نگه داشته می‌شوند.

### نکته مهم درباره HTTPS

Let's Encrypt برای هر اسم دامنه فقط **۵ گواهی در هفته** می‌دهد. پشت‌سرهم نصب و حذف نکنید. اگر سقف پر شد، یک زیردامنه دیگر بسازید یا تا پایان مهلت صبر کنید.

### مشکل دارید؟

- روی سرور اول `sudo bash hunter-dns.sh --check` را اجرا کنید تا ببینید چه چیزی نصب را مسدود می‌کند.
- متن کامل خطا را در بخش [Issues](../../issues) بگذارید.

</div>

---

<a name="english"></a>

## English

Hunter DNS is a single-file installer for a **smart DNS** service for users in Iran: a DNS and proxy pair that makes blocked sites reachable, with an admin panel and a customer panel.

### How it works

| Role | Where | What it does |
|---|---|---|
| **relay** | Server inside Iran | Clients point their DNS here. Answers blocked domains with its own address and carries the connection abroad |
| **exit** | Server outside Iran | Reads the SNI and connects to the real site |
| **single** | One server abroad | Both roles on one machine, no relay in Iran |
| **node** | Another server abroad | An extra exit joined to the main panel |

### Requirements

- Ubuntu or Debian with systemd, root access
- Ports 53, 80 and 443 free (or held by nginx)
- Optional: a domain whose **A** record points at the server (needed for HTTPS and the customer panel)

### Install

```bash
curl -fsSLO https://raw.githubusercontent.com/AlisanX17/Hunter-dns/main/hunter-dns.sh && sudo bash hunter-dns.sh
```

With two servers, install the **exit** first, then the **relay**. With one server abroad, choose **single**.

### Options

```bash
sudo bash hunter-dns.sh --check       # inspect only, change nothing
sudo bash hunter-dns.sh --version     # print version
sudo bash hunter-dns.sh --tunnel      # choose the relay-exit tunnel again
sudo bash hunter-dns.sh --uninstall   # put the machine back as it was
```

Environment variables go after `sudo`:

```bash
sudo env PANEL_DOMAIN=panel.example.com ASSUME_YES=1 bash hunter-dns.sh
```

### Uninstall

```bash
sudo bash hunter-dns.sh --uninstall
```

### Let's Encrypt rate limit

Let's Encrypt issues at most 5 certificates per exact domain name per week. Avoid repeated install and uninstall cycles; use another subdomain if you hit the limit.

---

## Credits

- Tunnel: [BackPack](https://github.com/AminMGMT/BackPack) by Amin Mohammadi (AGPL-3.0)
