<h1 lang="fa" dir="rtl" align="right">راه‌اندازی پراکسی در لینوکس</h1>
<p lang="fa" dir="rtl" align="right">یکی از اولین نیازها برای مهاجرت به لینوکس استفاده از پراکسی هست، متاسفانه آموزش‌های مناسبی هم براش سراغ ندارم در نتیجه تصمیم گرفتم این آموزش رو بنویسم امیدوارم برای تازه کاران مفید باشه.
سعی می کنم به مرور زمان تکمیلش کنم شما هم می‌تونید کمک کنید ایراداش رو بگید روش‌های دیگه رو آموزش بدید منتظر ایشو‌ها یا پول ریکوست‌هاتون هستم.</p>
<p lang="fa" dir="rtl" align="right">توضیح: من سواد لینوکسی، شبکه‌ای، فارسی زیادی ندارم در نتیجه ممکنه هرجا متن اشتباه باشه</p>
<p lang="fa" dir="rtl" align="right">یه توضیح مختصر برای اینکه اطلاعات اولیه داشته باشید
ما برای اینکه بتونیم تحریم‌هارو دور بزنیم لازمه با ip غیر از ip ایران به اینترنت وصل بشیم برای اینکار میشه تونلی به یک سرور خارج ایران زد و بعد ترافیک اینترنت رو از اون تونل انتقال داد. پس آموزش به دو قسمت کلی تقسیم میشه ساخت تونل و رد کردن ترافیک از توش </p>
<p lang="fa" dir="rtl" align="right">چند نکته میرورهای موجود در ایران ممکنه برنامه‌هایی که برای دور زدن استفاده میشه رو نداشته باشند در نتیجه از میرورهای خارج ایران استفاده کنید</p>
<h2 lang="fa" dir="rtl" align="right">ساخت تونل</h2>
<p lang="fa" dir="rtl" align="right">نکته: این تونل‌ها تا زمانی که ترمینال باز هست کار میکنه و با بستن ترمینال بسته میشه. اگر می‌خواید ترمینال رو ببندید میتونید اول دستور screen و یا nohup رو بزنید تا تونل با بستن ترمینال باز بمونه (ممکنه لازم باشه screen و nohup رو نصب کنید) ولی من پیشنهاد میدم یه گوشه‌های باز نگهش دارید تا اگر خطایی داد ببنید مشکل چیه</p>
<h2 lang="fa" dir="rtl" align="right">راه‌اندازی تونل به وسیله shadow socks</h2>
<p lang="fa" dir="rtl" align="right">شدو ساکس یکی از راه‌های ساخت تونل هست لازمه شما یه سرور در خارج از ایران داشته باشید از سیستم خودتون بهش وصل بشید
سمت سرور رو فکر نکنم لازم باشه بگم چون تصورم اینه که اگر سرور داشته باشید می‌تونید شدو ساکس رو هم نصب کنید ولی اگر خواستید اضافش میکنم.</p>
```bash
$ sudo apt install libevent-dev
$ sudo apt install python-pip
$ sudo apt install python-dev
$ sudo apt install python-m2crypto
$ sudo pip install gevent
$ sudo pip install shadowsocks
```
<p lang="fa" dir="rtl" align="right">الان شدو ساکس رو سیستم شما نصب شده و با دستور زیر میتونید یک تونل رو سیستم خودتون ایجاد کنید.</p>
```bash
$ sslocal -s “server” -k password -t 600 -p port  -l 1080 -m encryption
```
<p lang="fa" dir="rtl" align="right">به جای "server" آی‌پی یا آدرس سرور، به جای "password" پسورد و جای "port" پورت، جای "encryption" انکریپشن سرویس رو وارد کنید و به جای "local port" پورت لوکال سیستم که خودتون تایینش میکنید. بعد تونل رو سیستم شما ایجاد میشه و باید به مرحله رد کردن ترافیک از تونل برید.</p>
<p lang="fa" dir="rtl" align="right">میشه دستورات بالا رو تو یه فایل به این شکل ذخیره کرد</p>
```json
{
	"server": "sever addr",
        "server_port": port,
        "local_port": 1080,
        "password": "password",
        "timeout":600,
        "method":"aes-256-cfb"
}
```
<p lang="fa" dir="rtl" align="right">و دستور رو اینجوری زد</p>
```bash
$ sslocal -c path_file
```
<p lang="fa" dir="rtl" align="right">شدو ساکس در پورت‌های مختلف و انکریپشن‌های متفاوت سرعت‌های مختلفی میده فرود در این زمینه اطلاعات دقیق و کاملی داره</p>
<h2 lang="fa" dir="rtl" align="right">تونل با کمک ssh</h2>
<p lang="fa" dir="rtl" align="right">نمی‌دونم اسم این تونل چیه ولی ازش استفاده میکنم
اگر دسترسی ssh به یک سرور دارید می‌تونید به این طریق یک تونل رو سیستم خودتون ایجاد کنید</p>
```bash
$ ssh user@server.address -D 1080
```
<p lang="fa" dir="rtl" align="right">به این طریق یک تونل رو پورت ۱۰۸۰ سیستمتون ایجاد میشه</p>
<h2 lang="fa" dir="rtl" align="right">تونل با کمک tor</h2>
<p lang="fa" dir="rtl" align="right">تور یکی از امن ترین شبکه‌های دنیاست</p>
```bash
$ sudo apt install tor
$ sudo apt install python-pip
$ sudo apt install python-dev gcc
$ sudo pip install obfsproxy
```
<p lang="fa" dir="rtl" align="right">سپس تنظیمات تور رو باید انجام بدید</p>
```bash
$ sudo nano /etc/tor/torrc
```
<p lang="fa" dir="rtl" align="right">موارد زیر رو به انتهای فایل اضافه کنید
با کنترل + c می‌تونید و انتخاب y میتونید تغییرات رو ذخیره کنید</p>
```bash
SocksPort 1080
SocksListenAddress 127.0.0.1
Bridge obfs3 194.132.209.187:39413
Bridge obfs3 194.68.32.131:56006
Bridge obfs3 107.191.58.23:34344
UseBridges 1
ClientTransportPlugin obfs2,obfs3 exec /usr/local/bin/obfsproxy --managed
```
<p lang="fa" dir="rtl" align="right">تعدادی از پل‌های تور هم در این تنظیمات هستند، پل‌ها ممکن است بسته شوند از https://bridges.torproject.org می‌تونید پل‌های جدید بگیرید و با موارد بالا جایگزین کنید</p>
<p lang="fa" dir="rtl" align="right">بعد ذخیره تنظیمات با دستور زیر تور را دوباره راه‌اندازی کنید</p>
```bash
$ sudo service tor restart
```
<p lang="fa" dir="rtl" align="right">حالا تونل در سیستم شما اجرا شده و لازمه ترافیک رو ازش رد کنید</p>

<h2 lang="fa" dir="rtl" align="right">ساخت تونل با yourfreedom</h2>
<p lang="fa" dir="rtl" align="right">باید قبلش برید تو سایتش ثبت‌نام کنید از قسمت دانلود نسخه جاوا دانلود کنید از حالت فرشده خارج کنید
با cd به محل اکسترکت شده برید و دستور زیر رو وارد کنید</p>
```bash
java -jar freedom.jar
```
<p lang="fa" dir="rtl" align="right">حواستون باشه که لازمه قبلش جاوا رو نصب کرده باشید. اگر برای نصب جاوا به مشکل خوردید بگید شاید برای اونم یه چیزی ردیف کردیم.
خوب فریدام باید اجرا شه در اولین دفعه خودش پنجره کانفیگ رو باز میکنه اگر بعد از مدتی دیدید کنده یا کار نمیکنه دوباره از همین قسمت دنبال سرور‌های جدید بگردید
در پنجره کانفیگ چندین بار نکست رو بزنید تا شروع کنه دنبال سرورهاش بگره (طول میکشه)
بعد از پیدا شدن سرورها یکیش رو انتخاب کنید و نکست
یوزر پسورد اکانتتون رو وارد کنید دخیره کنید و بیاید بیرون
استارت کانکشن رو بزنید بعد از چند ثانیه ارتباط برقرار میشه</p>

<h2 lang="fa" dir="rtl" align="right">openvpn</h2>
<p lang="fa" dir="rtl" align="right">از openvpn هم میشه در لینوکس استفاده کرد اول نصبش کنید</p>
```bash
$ sudo apt install openvpn
```
<p lang="fa" dir="rtl" align="right">فایل کانفیگ openvpn رو دانلود کنید و به این طریق وصل شید</p>
```bash
$ sudo openvpn filename
```
<p lang="fa" dir="rtl" align="right">با اجرا این دستور کل ترافیک شما میره سمت سرور و لازم نیست پراکسی جایی ست کنید در نتیجه تمام تنظیمات پراسی‌هاتون رو مستقیم کنید.</p>