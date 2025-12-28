# AWS_CloudInstanceProject
Bu repo Bulut Bilişim dersi kapsamında gerçekleştirmiş olduğum projenin dokümantasyonunu içermektedir. Proje kapsamında dağıtıma sunulan uygulamaya da profilimdeki Portfolio_App reposundan ulaşılabilir.

Daha kapsamlı bir rapora erişmek için:


## 1. Uygulama Seçimi
Ödev kapsamında, hem modern web teknolojilerini kullanmak hem de kendi çalışmalarımı sunabileceğim bir platform oluşturmak amacıyla Kişisel Portfolyo uygulamamı dağıtmaya karar verdim. Bu uygulama, React ve TypeScript kullanılarak geliştirdiğim statik bir web projesidir.

## 2. Bulut Platformu ve Mimari
Uygulamamı dağıtmak için bulut sağlayıcısı olarak AWS (Amazon Web Services) platformunu seçtim. Bu seçimi yaparken sektördeki yaygın kullanımı ve geniş dokümantasyon desteğini göz önünde bulundurdum. Ayrıca maliyet oluşturmaması adına, ödev yönergesinde belirtildiği gibi sağlayıcının ücretsiz planı (Free Tier) kapsamındaki kaynakları kullandım.

### Kullandığım Teknolojiler ve Servisler:

Servis: AWS EC2 (Elastic Compute Cloud)

Sanal Makine Tipi: t2.micro (veya t3.micro)

İşletim Sistemi: Ubuntu Server 24.04 LTS

Web Sunucusu: Nginx


## 3. Uygulamanın Bulut Platformuna Taşınması (Dağıtım Adımları)
Yerel ortamımdaki projeyi buluta taşımak için şu adımları izledim:


Sanal Makine Oluşturma: AWS Konsolu üzerinden Ubuntu işletim sistemine sahip bir EC2 instance başlattım ve SSH bağlantısı için gerekli anahtar çiftini (.pem dosyası) oluşturdum.

Bağlantı ve Kurulum: Terminal aracılığıyla sunucuya SSH bağlantısı sağladım. Projemi çalıştırmak için gerekli olan Git, Node.js ve NPM paketlerini sunucuya kurdum.

Kodun Taşınması: Projemi GitHub üzerindeki repomdan git clone komutuyla sunucuya çektim.

Derleme (Build): React kodlarını tarayıcının anlayabileceği statik dosyalara dönüştürmek için sunucu içinde npm run build komutunu çalıştırdım.

Sunucu Konfigürasyonu: Uygulamayı dış dünyaya açmak için Nginx web sunucusunu kurdum. Nginx ayar dosyasını (/etc/nginx/sites-available/default), projemin derlenmiş dosyalarının bulunduğu dizini (dist klasörü) işaret edecek şekilde düzenledim.

## 4. Karşılaşılan Zorluklar ve Çözümler
Proje sürecinde karşılaştığım teknik sorunlar ve ürettiğim çözümler şunlardır:

Node.js Sürüm Uyuşmazlığı: Projemi derlemeye çalışırken sunucudaki varsayılan Node.js sürümünün (v18), kullandığım Vite kütüphanesi (v20+) için yetersiz kaldığını gördüm. Resmi NodeSource deposunu ekleyerek Node.js sürümünü v22'ye yükselttim ve sorunu çözdüm.

Erişim Engeli (Timeout): Sunucuyu başlattıktan sonra tarayıcıdan IP adresine girmeye çalıştığımda bağlantı zaman aşımına uğradı. AWS Güvenlik Grubu (Security Group) ayarlarını kontrol ettiğimde HTTP (80) portunun kapalı olduğunu fark ettim. İlgili portu tüm trafiğe (0.0.0.0/0) açarak sorunu giderdim.

500 Internal Server Error: Siteye ulaştığımda sunucu hatası aldım. Log kayıtlarını incelediğimde Nginx'in, proje dosyalarımın bulunduğu /home/ubuntu dizinine erişim izni olmadığını tespit ettim. chmod 755 komutlarını kullanarak gerekli klasör izinlerini düzenledim ve uygulama başarıyla yayına alındı.

## 5. Sonuç
Bu ödev ile birlikte, yerel ortamda geliştirdiğim bir yazılımı bulut tabanlı bir altyapıya (IaaS) taşıma sürecini uçtan uca deneyimledim. Sanal sunucu yönetimi, Linux komut satırı yetkinliği, web sunucusu (Nginx) yapılandırması ve ağ güvenliği konularında pratik bilgiler edindim.


Video Linki:  
