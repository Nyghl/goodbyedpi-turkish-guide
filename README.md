# GoodbyeDPI Kullanma Rehberi
>
> Bu rehber, Windows için yazılmıştır. Çünkü GoodbyeDPI Windows için olan bir araçtır.<br>Eğer başka işletim sistemindeyseniz, [SpoofDPI](https://github.com/xvzc/SpoofDPI) adlı araca bakabilirsiniz.
>
## Öncelikle GoodbyeDPI Nedir?

> Eğer GoodbyeDPI'ın ne olduğunu önemsemiyorsanız, direkt kurulum kısmına geçebilirsiniz.<br>Kısaca açık kaynak ve güvenilir bir araç.

İnternet sağlayıcıları bizim internete erişimimizi kafalarına göre engellemek için DPI (Deep Package Inspection) adlı bir tekniği uyguluyorlar. Türkçesi ve açılımı Derin Paket İncelemesi olarak çevrilebilir. Burada paketlerden kasıt sizin internet paketleriniz. DPI yöntemiyle paketlerinizi inceleyip, hoşlarına gitmeyen tüm paketleri engelliyorlar.

GoodbyeDPI ise ValdikSS ve topluluk tarafından geliştirilmiş, açık kaynak, senelerdir olan çok popüler bir araç. İçerisinde bu DPI yöntemine karşı koymak ve kafasını karıştırmak için çeşit çeşit teknikler bulunduruyor. Bu teknikleri gönlümüzce, kendi internet ve ISP (internet sağlayıcısı) gereksinimlerimize göre kullanabilir, konfigüre edebiliriz.

Bu VPN gibi bir şey değil. Sizi bir yere bağlamıyor, sadece internet paketlerinizi birazcık değiştirip ISP'nizin kafasını karıştırıyor. Eğer çok yanlış ayar yapmazsanız en ufak yavaşlamaya bile sahip olmazsınız.

## GoodbyeDPI Nasıl Kurulur?
>
> Eğer sıfırdan kendiniz kurmak istemiyorsanız, [Kolay Kurulum](#kolay-kurulum) kısmını okuyabilirsiniz.

GoodbyeDPI'ın varsayılan, yüklediğinizde gelen ayarlar çoğu internet sağlayıcıların kafasını karıştırmaya yeterli değil maalesef. Bu yüzden farklı ayarlarla çalıştırmamız gerekiyor.

<details>
    <summary>GoodbyeDPI Detaylı Kurulum</summary>

### 1. GoodbyeDPI'ı resmi Github sayfasından indirelim

[ValdikSS/GoodbyeDPI/releases](https://github.com/ValdikSS/GoodbyeDPI/releases) sayfasından, 0.2.3rc3 pre-release versiyonunu indirmeniz gerekiyor. Başka bir sürümü indirirseniz rehber çalışmayabilir. İndirmek için de Assets kısmına tıklayıp, goodbyedpi-0.2.3rc3-2.zip adlı dosyayı indirmeniz gerekiyor.

![Örnek Görsel](/github/images/assets.png)

İndikten sonra zipten çıkartıp, istediğiniz bir yere atabilirsiniz.

### 2. Kurulum aşaması

Kurulum aşaması için çalıştırmanız gereken bir setup.exe veya herhangi bir şey yok. Program direkt indirdiğiniz gibi çalışabilecek durumda.
Burada kurulumdan kasıt kendi ülkemizde çalışan ayarlar ile GoodbyeDPI çalıştırmak.

Şahsen deneme yanılmayla benim bulduğum 3 tane ayar var. Sırasıyla hangisi sizin internetinizde çalışıyorsa deneyebilirsiniz.
En hafif ayar:

```cmd
-f 1 -k 1 --auto-ttl
```

> Hafiften kasıt, olabildiğince internet paketlerinizi az değiştiriyor olması. Bu sayede uygulamaların kafası karışmazken, internet sağlayıcının kafası karışıyor. Mesela bazı ayarlar fazla güçlüler ve internet sağlayıcının da kafasını karıştırırken uygulamaların veya sitelerin de kafası karışıyor ve uygulamalar açılmayabiliyor / yavaş çalışıyorlar.

Orta ayar:

```cmd
-f 2 -k 2 --auto-ttl --reverse-frag --max-payload -s
```

En ağır ayar:
> Dikkat: bu ayar ile Twitter ve Instagram biraz patates oluyor. Son çare olarak deneyebilirsiniz.

```cmd
--set-ttl 7
```

> "GoodbyeDPI'ı bu ayarlarla nasıl çalıştıracağız peki?"

Bunun için birden fazla, gönlünüze göre kullanabileceğiniz yol var. Dümdüz goodbyedpi.exe'nin olduğu bir yerde terminal açıp, `goodbyedpi [komutlar]` yazarak çalıştırabilirsiniz.

Fakat bununla sürekli uğraşmak mantıklı olmadığı için genellikle script kaydedip, onla istediğimiz ayarları kullanarak açıyoruz.

Eğer şifreli DNS ayarı yapmadıysanız (ki bir ara yapmanızı tavsiye ederim), ben `2_any_country_dns_redir.cmd` dosyasını kopyalayıp düzenliyorum genellikle.

En hafif komutu kullanan örnek .cmd dosyası:

```cmd
@ECHO OFF
PUSHD "%~dp0"
set _arch=x86
IF "%PROCESSOR_ARCHITECTURE%"=="AMD64" (set _arch=x86_64)
IF DEFINED PROCESSOR_ARCHITEW6432 (set _arch=x86_64)
PUSHD "%_arch%"

start "" goodbyedpi.exe -f 1 -k 1 --auto-ttl --dns-addr 1.1.1.1 --dns-port 53 --dnsv6-addr 2606:4700:4700::1111 --dnsv6-port 53

POPD
POPD
```

Bu .cmd dosyasının yaptıklarına bakacak olursak, dümdüz start goodbyedpi.exe diyip, bizim komutlardan biriyle ve DNS'e bağlanma ayarıyla çalıştırıyor.

Bunu kopyalayıp, goodbyedpi'ın yüklü olduğu klasörde .cmd adlı bir dosya açıp ona yapıştırın. Sonra ister o dosyayı direkt açabilir, isterseniz de o dosyanın kısayolunu oluşturup kısayolu açabilirsiniz. Ama bu CMD dosyasının orjinali sadece goodbyedpi klasöründeyken çalışır, çünkü diğer şekilde goodbyedpi.exe'yi nasıl bulacak :D
</details>

## Kolay Kurulum

Eğer detaylı kurulumla uğraşmak istemezseniz, direkt bu Github projesindeki [goodbyedpi_dnsli_turkiye](goodbyedpi_dnsli_turkiye.cmd) komut dosyasını kullanabilirsiniz.

Bu dosyayı kullanmak için:

1. Opsiyon: Dümdüz tüm bu Github repository'sini yukarıda "Code" -> "Download as Zip" diyerek indirip, zipten çıkartıp çalıştırabilirsiniz.

2. Opsiyon: GoodbyeDPI'ı resmi [Github sayfasından](https://github.com/ValdikSS/GoodbyeDPI/) indirip, [goodbyedpi_dnsli_turkiye](goodbyedpi_dnsli_turkiye.cmd) komut dosyasını, indirdiğiniz GoodbyeDPI'ın içerisine atıp çalıştırabilirsiniz.<br><br>Resmi sayfadan indirmek için [releases](https://github.com/ValdikSS/GoodbyeDPI/releases/) kısmına gidip, Assets kısmına tıklayıp, goodbyedpi-0.2.3rc3-2.zip adlı dosyayı indirmeniz gerekiyor.

![Örnek Görsel](/github/images/assets.png)

Eğer eski bir versiyon indirirseniz bu rehber geçerli olmayabilir.

<details>
    <summary>Sıkça Sorulan Sorular</summary>

### "Kurdum, DNS konfigürasyonu ile de açtım hala giremiyorum çalışmıyor???"

GoodbyeDPI'ın arka planda açık kalması gerekiyor. Eğer kapattıysanız mantıken aktif olarak paketlerinizi ISP'nin kafasını karıştıracak şekilde düzenleyemez veya yönlendiremez.

Eğer açıksa ve yine de çalışmıyorsa, versiyonunu kontrol edin. Bazen indirirken yanlışlıkla 0.2.2 versiyonu indirilinebiliyor. Bu rehber v0.2.3rc3 pre-release sürümü için geçerli.

Sürüm de doğruysa o zaman GoodbyeDPI'ın da yanında şifreli DNS kullanmanız gerekebilir. Ya da daha ağır bir komut denemeniz gerekebilir. [Alternatif Komutlar](alternatif_komutlar/) klasöründen sırasıyla deneyebilirsiniz. Ayriyeten [Alternatif DNS](alternatif_dns/) klasöründen farklı bir DNS de deneyebilirsiniz.

### "Hep altta aşağıda gözükecek mi bu?"

Maalesef evet. Ama üçüncü parti yazılımları kullanarak sistem tepsisine küçültebilirsiniz. Benim şahsen kullandığım yazılım [Traymond](https://github.com/fcFn/traymond).

</details>

> Yasal Uyarı: Bu rehber ve rehberi yazan kişi; yasaklanan, yasal olmayan site ve uygulamaları kullanmanızı teşvik ve tavsiye etmemektedir. Popüler ve güvenilir bir proje olan GoodbyeDPI hakkında bilgilendirme ve eğitim amacıyla, bir programlama topluluğu için yazılmıştır. Bu rehberi lütfen kötü şekillerde kullanmayınız.
