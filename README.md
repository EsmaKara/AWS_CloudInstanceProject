# AWS_CloudInstanceProject
Bu repo Bulut Bilişim dersi kapsamında gerçekleştirmiş olduğum projenin dokümantasyonunu içermektedir. Proje kapsamında dağıtıma sunulan uygulamaya da profilimdeki Portfolio_App reposundan ulaşılabilir.

Daha kapsamlı bir rapora erişmek için:
[AWS_Projesi_Raporu.pdf](https://github.com/user-attachments/files/24361892/AWS_Projesi_Raporu.pdf)


## 1. Uygulama Seçimi
Ödev kapsamında, kişisel yetkinliklerimi sergilemek amacıyla React ve TypeScript kullanılarak geliştirilmiş statik bir "Kişisel Portfolyo" web uygulamamı dağıtmaya karar verdim. Böylece, staj veya iş görüşmelerinde teknik becerilerimi kanıtlamak için sunabileceğim, her yerden ve her an erişilebilir çevrimiçi bir referans noktasına sahip olmayı hedefledim.
Repo:   https://github.com/EsmaKara/Portfolio_App

## 2. Bulut Platformu ve Mimari
Uygulamamı dağıtmak için bulut sağlayıcısı olarak AWS (Amazon Web Services) platformunu seçtim. Bu seçimi yaparken sektördeki yaygın kullanımı ve geniş dokümantasyon desteğini göz önünde bulundurdum. Ayrıca maliyet oluşturmaması adına, ödev yönergesinde belirtildiği gibi sağlayıcının ücretsiz planı (Free Tier) kapsamındaki kaynakları kullandım.

### Kullandığım Teknolojiler ve Servisler:

**Servis:** AWS EC2 (Elastic Compute Cloud)

**Sanal Makine Tipi:** t3.micro

**İşletim Sistemi:** Ubuntu Server 24.04 LTS

**Web Sunucusu:** Nginx


**Uygulama Mimarisi:** Sistemin mimarisi, Kaynak Kod Yönetimi ve Kullanıcı Erişimi olmak üzere iki temel akıştan oluşmaktadır. Geliştirilen React projesinin kaynak kodları GitHub üzerinde barındırılmakta ve sunucuya buradan çekilerek (pull) derlenmektedir. Çalışma zamanında ise; tarayıcıdan gelen son kullanıcı istekleri, AWS ağ geçidi üzerinden geçerek yapılandırdığım Güvenlik Grubu (Security Group) kurallarına tabi tutulmakta ve EC2 sunucumdaki Nginx servisine iletilmektedir. Nginx, bu istekleri karşılayarak derlenmiş statik dosyaları kullanıcıya sunar.


## 3. Uygulamanın Bulut Platformuna Taşınması (Dağıtım Adımları)
Yerelden GitHub ortamına taşıdığım uygulamamı bulutta dağıtmak amacıyla şu adımları izledim:


**Sanal Makine Oluşturma:** AWS Konsolu üzerinden Ubuntu işletim sistemine sahip bir EC2 instance başlattım ve SSH bağlantısı için gerekli anahtar çiftini (.pem dosyası) oluşturdum.

**Bağlantı ve Kurulum:** Terminal aracılığıyla sunucuya SSH bağlantısı sağladım. Uygulamamı çalıştırmak için gerekli olan Git, Node.js ve NPM paketlerini sunucuya kurdum.

**Kodun Taşınması:** Uygulamamı GitHub üzerindeki repomdan git clone komutuyla sunucuya çektim.

**Derleme (Build):** React kodlarını tarayıcının anlayabileceği statik dosyalara dönüştürmek için sunucu içinde npm run build komutunu çalıştırdım.

**Sunucu Konfigürasyonu:** Uygulamayı dış dünyaya açmak için Nginx web sunucusunu kurdum. Nginx ayar dosyasını (/etc/nginx/sites-available/default), projemin derlenmiş dosyalarının bulunduğu dizini (dist klasörü) işaret edecek şekilde düzenledim.

## 4. Karşılaşılan Zorluklar ve Çözümler
Proje sürecinde karşılaştığım teknik sorunlar ve ürettiğim çözümler şunlardır:

**Node.js Sürüm Uyuşmazlığı:** Projemi derlemeye çalışırken sunucudaki varsayılan Node.js sürümünün (v18), kullandığım Vite kütüphanesi (v20+) için yetersiz kaldığını gördüm. Resmi NodeSource deposunu ekleyerek Node.js sürümünü v22'ye yükselttim ve sorunu çözdüm.

**Derleme (Build) Hataları ve Kod Yönetimi:** Uygulamanın sunucu üzerindeki derleme aşamasında (build), kod kaynaklı bazı hatalar nedeniyle işlemin başarısız olduğunu tespit ettim. Sunucu üzerinde kod düzenlemenin verimsiz olması nedeniyle, ilgili hataları yerel geliştirme ortamımda düzelttim ve değişiklikleri GitHub reposuna gönderdim (push). Ardından sunucu terminali üzerinden git pull komutuyla güncellemeleri çekip projeyi yeniden derleyerek sorunu giderdim. 

**Erişim Engeli (Timeout):** Sunucuyu başlattıktan sonra tarayıcıdan IP adresine girmeye çalıştığımda bağlantı zaman aşımına uğradı ve "Bu siteye ulaşılamıyor" hatası aldım. AWS Güvenlik Grubu (Security Group) ayarlarını kontrol ettiğimde HTTP (80) portunun kapalı olduğunu, böyle bir kuralın bulunmadığını fark ettim. Yeni bir kural ekleyerek, ilgili portu tüm trafiğe (0.0.0.0/0) açarak sorunu giderdim.

**500 Internal Server Error:** Siteye ulaştığımda sunucu hatası aldım. Log kayıtlarını incelediğimde Nginx'in, proje dosyalarımın bulunduğu /home/ubuntu dizinine erişim izni olmadığını tespit ettim. chmod 755 komutlarını kullanarak gerekli klasör izinlerini düzenledim ve uygulama başarıyla yayına alındı.

## 5. Sonuç
Bu ödev ile birlikte, yerel ortamda geliştirdiğim bir yazılımı bulut tabanlı bir altyapıya (IaaS) taşıma sürecini uçtan uca deneyimledim. Sanal sunucu yönetimi, Linux komut satırı yetkinliği, web sunucusu (Nginx) yapılandırması ve ağ güvenliği konularında pratik bilgiler edindim.


**Proje Anlatımı YouTube Video Linki:**  https://youtu.be/6CKWeQqLcCU
