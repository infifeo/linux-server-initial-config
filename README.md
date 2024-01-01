# کانفیگ اولیه سرور لینوکس (اوبونتو)

در این مطلب به آموزش کانفیگ اولیه سرور اوبونتو می پردازم. مباحثی مثل آماده سازی سرور برای اعمال هرگونه تغییر و نصب و راه اندازی پکیج های مختلف، امنیت سرور و همچنین یکسری تغییرات جزئی برای افزایش سرعت شبکه سرور.

## نکات اولیه:

- برای اتصال به VPS از پروتکل SSH استفاده می کنیم.
- در ویندوز از نرم افزار [Git Bash](https://git-scm.com/downloads) برای اتصال به سرور استفاده کنید.
- در لینوکس و مک میتونید از ترمینال خود سیستم عامل استفاده کنید.
- پورت SSH به صورت پیشفزض 22 هست.
- برای بررسی سلامت IP سرور از سایت [Check-Host](https://check-host.net/) استفاده کنید.



## بخش اول :: اتصال به سرور

برای اتصال به سرور ابتدا در برنامه مورد نظر دستور زیر را تایپ کنید:

```
ssh root@server_ip
```

اگر پورت پیشفرض SSH برای شما عددی غیر از 22 بود، دستور بالا را به صورت زیر وارد کنید:

```
ssh -p port_number root@server_ip
```

 * توجه کنید که `root` نام کاربر اصلی سرور هست که ممکنه برای شما متفاوت باشه. این نام کاربری در اطلاعات ارسالی توسط جایی که ازش سرور گرفتید موجوده، ولی معمولا `root` هست.



## بخش دوم :: بروزرسانی

با دستور زیر سرور را بروزرسانی کنید:

```
sudo apt update -y && sudo apt upgrade -y
```

 * توجه کنید هرکجا صفحه ای ظاهر شد و ازتون درخواست کرد کاری انجام بدید، فقط `Enter` بزنید.

پس از پایان بروزرسانی، با دستور زیر سرور را ریبوت کنید:

```
sudo reboot
```

یک دقیقه صبر کنید تا سرور دوباره بالا بیاد، بعد دوباره با دستور بخش اول، به سرور وصل بشید و دستور زیرو اجرا کنید تا پکیج های اضافه پاک بشه:

```
sudo apt autoremove
```


## بخش سوم :: تنظیم Hostname

   دستور زیرو وارد کنید تا فایل Hosts براتون باز بشه:
```
sudo nano /etc/hosts
```

در انتهای فایل مقدار زیرو تایپ کنید:

```
server_ip   server
```

* توجه کنید به جای `server_ip` باید IP سرور خودتون و به جای `server` یک نام دلخواه برای سرورتون قرار بدید. با این کار برای سرور یک `Hostname` تعیین می کنید که بعدا به مشکل نخورید. مثلا برای نصب OpenVPN باید ازش استفاده کنید.

برای خروج `ctrl + x` بعد `y` بعد `Enter` بزنید.



## بخش چهارم :: ایجاد کاربر جدید

دستور زیر را وارد کنید تا یک کاربر جدید ایجاد کنید:

```
adduser user_name
```

* توجه کنید به جای `user_name` نام کاربری دلخواه وارد کنید. از این به بعد با این نام کاربری کار می کنیم پس حتما نام کاربری انتخاب کنید که یادتون بمونه!!!

بعد از اینکه `Enter` زدید ازتون میخواد پسورد وارد کنید.

لطفا یک پسورد متشکل از حروف کوچک و بزرگ انگلیسی، عدد و نماد های `(!@$%&*)` انتخاب کنید که حداقل 12 کاراکتر طولش باشه.

در مرحله بعد ازتون میخواد پسورد را دوباره وارد کنید و تایید کنید.

در مرحله بعد ازتون یکسری اطلاعات مثل نام و ایمیل میخواد که اصلا مهم نیست، پس فقط `Enter` بزنید تا پیام بده که کاربر با موفقیت ساخته شد.

دستور زیرو وارد کنید تا به کاربر ایجاد شده دسترسی `Sudo` بدید:

```
adduser user_name sudo
```

* توجه کنید به جای `user_name` نام کاربری که در مرحله قبل ساختید وارد کنید.

حالا با دستور زیر اتصال را قطع کنید:


```
exit
```

با دستور بخش اول به سرور متصل بشید، فقط اینبار به جای نام کاربری اصلی (`root`)، نام کاربری جدیدی که ساختید را وارد کنید:

```
ssh user_name@server_ip
```
یا

```
ssh -p port_number user_name@server_ip
```

## بخش پنجم :: ایجاد SSH Key 

در این بخش ما یک کلید SSH یا SSH Key ایجاد می کنیم که به هیچکس جز کسی که این کلید را داشته باشه اجازه ورود داده نمیشه. برای این کار:

ابتدا یک ترمینال روی سیستم عامل خودتون باز کنید و دستور زیرو تایپ کنید:

```
ssh-keygen
```

بعد از اینکه `Enter` زدید ازتون میخواد یک نام برای کلید انتخاب کنید که به صورت پیشفرض `id_rsa` هست. میتونید بذارید همون باشه یا تغییرش بدید.

در ادامه ازتون یک `Key Phrase` می خواد که میتونید مقدار دلخواه وارد کنید و یا فقط `Enter` کنید تا خالی باشه.

روی `سرور` دستور زیرو تایپ کنید تا پوشه `.ssh` ایجاد بشه:

```
mkdir -p ~/.ssh
```

روی `سیستم خودتون` دستور زیرو تایپ کنید تا کلید SSH به `سرور` منتقل بشه:

```
scp ~/.ssh/id_rsa.pub user_name@server_ip:~/.ssh/authorized_keys
```
یا

```
scp -P port_number ~/.ssh/id_rsa.pub user_name@server_ip:~/.ssh/authorized_keys
```

* در دستور بالا به جای `id_rsa` نام کلید SSH که در همین بخش ساختید را وارد کنید. اگر هم تغییر ی در نام فایل ندادید که هیچ.

* در دستور بالا به جای `user_name` نام کاربری که در بخش چهارم ساختید را وارد کنید.

* در دستور بالا به جای `server_ip` آدرس IP سرور را وارد کنید.

بعد از `Enter` کردن ازتون میخواد پسورد کاربری که در `بحش چهارم` ساختیم را وارد کنید تا فایل منتقل بشه.

حالا روی `سرور` دستورات زیرو وارد کنید تا مجوزهای SSH تنظیم بشه:

```
sudo chmod 700 ~/.ssh/
sudo chmod 600 ~/.ssh/*
```

با دستور زیر از حساب کاربر خارج بشید:

```
logout
```

حالا دوباره با دستور بخش اول به حساب کاربر وارد بشید. در این مرحله اگه درست پیش رفته باشید دیگه ازتون پسورد نمیخواد و مستقیم وارد سرور میشه.


```
ssh user_name@server_ip
```
یا

```
ssh -p port_number user_name@server_ip
```

کانفیگ SSH را باز کنید:

```
sudo nano /etc/ssh/sshd_config
```

مقدار `PermitRootLogin` را پیدا کنید. جلوش هرچی نوشته بود پاک کنید و بنویسید `no`.

با این کار لاگین مستقیم به کاربر اصلی یا `root` غیر ممکن میشه.

در همین فایل چند خط بعد از مقدار قبلی، دنبال مقدار `PasswordAuthentication` بگردید. باید به این صورت باشه:

```
#PasswordAuthentication yes
```

به شکل زیر تغییرش بدید:


```
PasswordAuthentication no
```

با این کار لاگین به سرور با پسورد غیر ممکن میشه و فقط با کلید SSH امکان لاگین وجود داره.

برای خروج `ctrl + x` بعد `y` بعد `Enter` بزنید.

در ادامه دستور زیرو تایپ کنید تا تغییرات اعمال بشه:

```
sudo systemctl restart sshd
```

