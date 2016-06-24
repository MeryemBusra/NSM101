![Bro Eyes](https://www.bro.org/images/bro-eyes.png)
 
# Bro Network Security Monitor 

> * <a href='#giris'>Giriş</a>
>   * <a href='#onbakis'>Ön Bakış</a>
>   * <a href='#ozellikler'>Özellikler</a>
>   * <a href='#mimari'>Mimari</a>
> *  <a href='#kurulum'>Kurulum</a>
>   * <a href='#ongereksinim'>Ön Gereksinimler</a>
>   * <a href='#sourcekurulum'>Kaynaktan Kurulum</a>
> * <a href='#hizlibaslangic'>Hızlı Başlangıç</a>
>   * <a href='#brocontrol'>BroControl ile Bro</a>
>   * <a href='#cmlinebro'>Bro ve Command-Line</a>
> * <a href='#brokullanimi'>Bro Kullanımı</a>
>   


<h1 id='giris'>**Giriş**</h2>



<h4 id='onbakis'>Ön Bakış</h4>
---
  Bro, açık kaynak bir ağ analiz aracıdır. Öncelikli görevi, ağ trafiğinde tehdit 
  olarak algılanan aktiviteleri takip etmektir. Daha geniş kapsamda ise
  performans ölçekleme ve sorun giderme işlemleri yapar. 

  Bro'nun en büyük avantajı, ağdaki aktivitileri log dosyalarında saklayabiliyor
   olması. Bu log dosyaları sadece ağdaki bağlantıları değil, aynı zamanda
oturumunun istek yapılan uri'si, mime typelar ve aynı zamanda serverdan dönen cevaplar; DNS request ve replyları; SSL sertifikaları; SMTP oturumlarının key 
contentleri ve daha da fazlası olabilir. Default olarak Bro tüm bu logları tab-
seperated bir biçimde ayrı log dosyalarına yazar. Kullanıcılar da aynı zamanda, 
kendilerine has log dosyaları oluşturabilirler.

  Log dosyalarına ek olarak Bro, daha farklı analiz ve tespit işleri için yerleşik bir 
  fonksiyonelliğe sahiptir. Bunlara örnek olarak, HTTP oturumlarından dosyaları 
  extract etme işlemleri, SSH brute-forcing tespiti veya SSL sertifikalarının 
  geçerlilik kontrolü gibi işlemler yapılabilir.
  
  Bro her ne kadar klasik bir IDS (Intrusion Detection System) sistem olarak 
  gözükse de, özünde daha çok bir platformdur. Kendine has bir script dili vardır 
  ve kullanıcılar tarafından da bu script dili kullanılarak, kendi ihtiyaçlarını 
  karşılayacak analiz/tespit yöntemleri geliştirilebilir. Halihazırda Bro'nun 
  çözümleri de bu scriptlerin ürünüdür.
  


<h4 id='ozellikler'>Özellikler</h4>
---
* Çalışma ortamı
  * Standart UNIX-style sistemler (Linux, FreeBSD, MacOS gibi).
  * Herhangi bir port veya networkün içindeki akış bazlı izleme/analiz.
  * Paket yakalamak için standart libpcap kütüphanesi.
  * Açık kaynak ve BSD lisanslı.

* Analiz
  * Offline analiz ve forensics için kapsamlı loglama.
  * Uygulama katmanı protokolleri için port-independent analiz.
  * Birçok farklı uygulama katmanı protokol desteği (HTTP, SSH, SSL, DNS gibi).
  * Dosya analizi (MD5/SHA1 fingerprintleri dahil).
  * Kapsamlı IPv6 desteği.

* Script Dili
  * İsteğe bağlı analiz işlemleri oluşturabilmek için Turing-complete dil.
  * Event-based programlama modeli.
  *  IP adresleri, port numaraları ve zamanlayıcılar gibi farklı alanlar için özel data tipleri.

* Arayüz
  * Yapılandırılmış ASCII logları ile default output sağlar.
  * ElasticSearch ve DataSeries için alternatif bir backenddir.
  * Yazılmış scriptler ile farklı eventlere cevap verme.

<h4 id='mimari'>Mimari</h4>
---
![Bro's internal architecture](https://www.bro.org/sphinx/_images/architecture.png)

Mimari olarak, Bro iki katmandan oluşur. Event engine kısmı, devam eden akışı 
'higher-level events'lere dönüştürür. Örnek olarak, ağdaki her bir HTTP isteği 
event engine tarafından http_request adındaki; IP adreslerini, port numaralarını, 
istek yapılan uri gibi özellikleri içeren bir event'e dönüştürülür.
  
İkinci kısım olan script interpreter bölümü ise, kullanılan scriptler aracılığı ile 
kendisine gelen eventler üzerinde gerekli işlemleri yapar.


<h1 id='kurulum'>**Kurulum**</h2>

<h4 id='ongereksinim'>Ön Gereksinimler</h4>
---
Bro'yu yüklemeden önce yüklenilmesi gereken kütüphane ve toollar:

* Libpcap (http://www.tcpdump.org)
* OpenSSL libraries (http://www.openssl.org)
* BIND8 library
* Libz
* Bash (BroControl için)
* Python (BroControl için)

Kaynak dosyadan kurulum yapmak için gerekenler: 

* CMake 2.8 or greater (http://www.cmake.org)
* Make
* C/C++ compiler
* SWIG (http://www.swig.org)
* Bison (GNU Parser Generator)
* Flex (Fast Lexical Analyzer)
* Libpcap headers (http://www.tcpdump.org)
* OpenSSL headers (http://www.openssl.org)
* zlib headers
* Perl

Gerekenlerin kurulması için aşağıdaki DEB/Debian-based Linux komutu 
çalıştırılabilir:

    sudo apt-get install cmake make gcc g++ flex bison libpcap-dev libssl-dev python-dev swig zlib1g-dev


<h4 id='sourcekurulum'>Kaynaktan Kurulum</h4>
---

Kaynak paketler Bro'nun web sayfasında bulunabileceği gibi, son güncellemeler 
ile birlikte üzerinde çalışılan `git.bro.org` sitesinden de indirilebilir. 
Aşağıdaki komutu kullanarak source paket alınır: 

    git clone --recursive git://git.bro.org/bro

Yüklemek için (daha fazla opsiyon -> ./configure --help): 

    ./configure
    make
	make install

Bro default olarak /usr/local/bro dizinindedir. Bu yüzden yükleme yapılırken 
ekstra root yetkileri istenebileceğinden, yükleme işleminin root olarak yapılması 
gerekebilir.

Yükleme tamamlandıktan sonra ise, PATH tanımlaması ile bro fonksiyonlarına 
terminal üzerinden erişim oluşturulması gerekebilir:

    export PATH=/usr/local/bro/bin:$PATH


<h1 id='hizlibaslangic'>**Hızlı Başlangıç**</h4>

<h4 id='brocontrol'>BroControl ile Bro</h4>
---
BroControl interaktif bir shelldir ve basit/çoklu sistemler üzerinde Bro 
kurulumlarını kolayca yönetmeyi sağlar. 

####*Minimal Başlangıç Konfigürasyonu*

    1. İzlenecek interface için: $PREFIX/etc/node.cfg
    2. İzlenecek ağlar için: $PREFIX/etc/networks.cfg
    3. MailTo ve LogRotationInterval ayarları için: $PREFIX/etc/broctl.cfg 

    

> Not: $PREFIX, Bro'nun kurulu olduğu dizini temsil eder.

BroControl shell'ini başlatmak için:

    broctl

İlk kullanım olduğu için, BroControl başlangıç konfigürasyonu kurulumu 
yapılmalıdır:

    [BroControl] > install

Bro instance'ını başlatmak için:

    [BroControl] > start

> Başlarken oluşabilecek yetki hataları için, BroControl'ü root
> yetkileri ile başlatmak gerekebilir.

Mevcut instance'ı durdurmak için:

    [BroControl] > stop

####*Log Dosyalarına Göz Atma*

Default olarak, log dosyaları okunabilir ASCII formatında ve datalara göre organize edilmiş kolonlara yazılır. Loglar $PREFIX/logs/current dizininde tutulur (Eğer Bro çalışmıyorsa dizin boş olacaktır). Örnek bir log dosyası olarak `http.log`:

    # ts          uid          orig_h        orig_p  resp_h         resp_p
    1311627961.8  HSH4uV8KVJg  192.168.1.100 52303   192.150.187.43 80

Görüldüğü üzere ilk 4 sekme Timestamp, sonrasında bir UID, kaynak/hedef ip 
adresleri ve port numaları olarak ayrılmıştır. Kalan sekmelerde de method, host, 
uri gibi kolonlar bulunmaktadır. 

Bazı log dosyalarından da özellikle bahsetmek gerekirse:

> **conn.log**
> Ağda görülen her bir yeni bağlantı için, bağlantının basit (süre, kaynak/hedef 
> ip/port ve servis bilgileri gibi) bilgilerinin tutulduğu log dosyasıdır.
> 
**notice.log**
> Bro tarafından tanımlanmış çeşitli aktiviteler ilgili bilgilendirmenin yapıldığı 
> log dosyasıdır.

Default olarak Bro tüm log dosyalarını `$PREFIX/logs/current` dizininden alır ve tarihlerine göre arşivler, `$PREFIX/logs/2016-10-06` gibi. Ne kadar 
sürede bir arşivleme işlemi yapılacağı `$PREFIX/etc/broctl.cfg` dosyasında
 düzenlenebilir.
 

####*Bro Script*

Bro, önceden yazılmış ve tamamıyla özelleştirilebilir birçok script ile birlikte gelir. 
Default olarak bütün bu scriptler `$PREFIX/share/bro` dizini altında yer alır 
ve scriptadı + `.bro` uzantısı ile ifade edilir. Bu dizindeki dosyaların doğrudan 
değiştirilmemesi gerekir, çünkü Bro update edilirken değişiklikler kaybolacaktır.
Bu kuralın tek istisnası `$PREFIX/share/bro/site` dizini için geçerlidir. 
Otomatik olarak Bro `$PREFIX/share/bro/base` dizini altındaki tüm scriptleri 
başlangıçta otomatik olarak yükler.

Default konfigürasyonlar ile Standalone Bro instance'ını BroControl ile 
yönetmekteki ana nokta, `$PREFIX/share/bro/site/local.bro` scriptidir. 
İlerleyen bölümlerde bahsedilecektir.

####*Script Değerlerini Yeniden Tanımlamak*

Bazı basit konfigürasyonlardaki değerler, Bro `&redef` operatörünü kullanarak 
kendi değerleriniz ile değiştirilmeyi bekliyor olabilir. 

Bu tarz bir değiştirme için kullanılan en tipik yol `+redef` operatörünü `const` 
ile birleştirmektir. Const (sabit) tanımı yapıldıktan sonra redef kullanımlası her ne 
kadar garip gelebilir. Bu değerler çalışma zamanında değiştirilemezler, ancak 
başlangıç değerleri, parse edilme süresinde `&redef` operatörü aracılığı ile 
yeniden düzenlenebilirler.

Örnek olarak 2 SSL notice davranışını düzenleyelim.
 `&PREFIX/share/bro/base/frameworks/notice/main.bro` bakacak olursak:

    module Notice;
	export {
    ...
    ## Ignored notice types.
    const ignored_types: set[Notice::Type] = {} &redef; }

Bu ifade, tam olarak aradığımız şey. `local.bro`'ya aşağıdaki satırı ekleyelim.

    redef Notice::ignored_types += { SSL::Invalid_Server_Cert };

> **Not**
> Notice namespace scope'u burada önemli, çünkü tanımlandığı yer ile referans 
> verildiği modüller farklı. Aynı modülde tanımlanan değerler için bu gerekli 
> değil, fakat dışarıdan erişebilmek için :: kullanılması gerekir.

Şimdi BroControl shell'ine gidip yaptığımız değişikliğin geçerli olup olmadığı 
kontrol edelim: 

    [BroControl] > check
	bro scripts are ok.
	[BroControl] > install
	removing old policies in /usr/local/bro/spool/policy/site ... done.
	removing old policies in /usr/local/bro/spool/policy/auto ... done.
	creating policy directories ... done.
	installing site policies ... done.
	generating standalone-layout.bro ... done.
	generating local-networks.bro ... done.
	generating broctl-config.bro ... done.
	updating nodes ... done.
	[BroControl] > restart
	stopping bro ...
	starting bro ...

Bundan böyle SSL ile ilgili bildirimler görmezden gelinir.


<h4 id='cmlinebro'>Bro ve Command-Line</h4>
---

BroControl'ü tercih etmeyenler için Bro, doğrudan komut satırından live ve offline
 analiz aktiviteleri için kullanılabilir.

####*Canlı Trafik İzlemek*

Herhangi bir interface üzerinde canlı trafiği izlemek için:

    bro -i wlan0 <yüklenecek scriptler listesi>

`wlan0`, izlenmek istenen başka interfaceler ile değiştirilebilir. Bro, log 
dosyalarını üzerinde çalışılan dizine çıkartacaktır. 

 `local.bro` scriptini kullanabilmek için:
 

    bro -i wlan0 local

Bu komut çalıştırıldığında, Bro `Site::local_nets` değerinin konfigüre 
edilmediği ile ilgili hata verecektir. Bu bilgiler elle aşağıdaki gibi sağlanabilir: 

    bro -r mypackets.trace local "Site::local_nets += { 1.2.3.0/24, 5.6.7.0/24 }"

####*Pcap (Packet Capture) Dosyalarını Okumak

Herhangi bir interface üzerinden paket yakalama ve bir dosyaya yazma işlemi 
aşağıdaki gibi yapılabilir: 

    sudo tcpdump -i wlan0 -s 0 -w mypackets.pcap

Bir süre paket yakalama işlemi devam ettikten sonra, CTRL-C sinyali ile 
tcpdump'a son verilir. Bro'nun toplanmış olan paketleri okuması için: 

    bro -r mypackets.pcap

Bro, log dosyalarını üzerinde çalışılan dizine çıkaracaktır.

####*Bro'ya Hangi Scriptlerin Yüklenileceğini Söylemek*

Kullanım genel olarak şöyledir:

    bro <ayarlar> <policyler...>

Son argüman, Bro bir işlem yaparken hangi policy scriptlerini kullanacağını söylemek için kullanılır. Bu argümanlar `.bro` uzantısı içermek zorunda değildir ve eğer bu scriptler `$PREFIX/share/bro` dizini altındaysa, dizin için path belirtmek zorunlu değildir. 

Örnek olarak aşağıdaki komut, tüm temel analizleri yapar ve SSL sertifika onayını ekler:

    bro -r mypackets.pcap protocols/ssl/validate-certs

Kullanılan scriptlerde en üstte kullanılan `@load` ifadesi, diğer scriptlere olan bağımlılığı belirtir. C-C++ dillerindeki `#include` gibi düşünülebilir, tek farkı `"eğer yüklenmediyse yükle"`dir.

####*Bro'yu Kurulum Yapmadan Çalıştırmak*

Bro'yu doğrudan `/build` dizininden (`make install` yapmadan) çalıştırmak isteyen geliştiricilerin tek yapması gereken, başlamadan önce scriptlerin nereden yükleneceğini belirtmek için `BROPATH` ayarlaması yapmaktır. Devamı ise şöyledir:

    ./configure
	make
	source build/bro-path-dev.sh
	bro <options>


<h1 id='brokullanimi'>**Bro Kullanımı**</h2>
