# Tarayıcı Tabanlı Görüntü Düzenleyici 🎨

Bu proje, görüntü işleme algoritmalarının –özellikle HDR yeniden yapılandırma ve yerel kontrast artırma işlemlerinin– harici kütüphanelere (OpenCV vb.) ihtiyaç duymadan, saf JavaScript ve HTML5 Canvas API kullanılarak tarayıcı üzerinde nasıl uygulanabileceğini gösteren etkileşimli bir web uygulamasıdır.

## 🌟 Özellikler

* **Yerel Kontrast Artırma (HDR):** Görüntüdeki parlaklık (luminance) değerlerini analiz ederek, aydınlık bölgeleri patlatmadan sadece gölgede kalan pikselleri orantılı olarak aydınlatan özel parlaklık maskeleme (luminance masking) algoritması.
* **Temel Ayarlar:** Piksel değerlerini güvenli aralıklarda (clamping) tutarak uygulanan Parlaklık ve Kontrast manipülasyonu.
* **Renk Filtreleri:** Piksel bazlı matematiksel renk uzayı dönüşümleriyle uygulanan Siyah-Beyaz (Luma) ve Sepya filtreleri.
* **İndirme (Export):** İşlenmiş görüntü verilerini (Canvas) cihazınıza PNG formatında kaydetme.

## 🛠️ Kullanılan Teknolojiler

* **HTML5 Canvas API:** Görüntülerin piksel matrislerini (ImageData) okumak ve üzerlerine veri yazmak için.
* **JavaScript File API:** Yerel cihazdan tarayıcıya güvenli bir şekilde görüntü yüklemek için.
* **Vanilla JavaScript:** Çekirdek matematiksel algoritmalar ve DOM (Belge Nesne Modeli) yönetimi için.
* **CSS3:** Modern, duyarlı (responsive) ve koyu tema (dark mode) odaklı kullanıcı arayüzü tasarımı.

## 🚀 Nasıl Çalıştırılır?

Proje tamamen istemci tarafında (client-side) çalışır, herhangi bir sunucu veya bağımlılık kurulumuna ihtiyaç duymaz.

**Google Colab Üzerinde Çalıştırmak İçin:**
1. Boş bir Colab not defteri oluşturun.
2. Yeni bir kod hücresi açın.
3. Birleştirilmiş HTML/JS/CSS kodlarını bu hücreye yapıştırın.
4. Kodun en üst satırında `%%html` sihirli komutunun (magic command) bulunduğundan emin olun.
5. Hücreyi çalıştırın (`Shift + Enter`). Uygulama arayüzü doğrudan hücrenin altında aktif olacaktır.

**Yerel Ortamda Çalıştırmak İçin:**
1. Projenin kodlarını içeren dosyayı `.html` uzantısıyla (örneğin `index.html`) bilgisayarınıza kaydedin.
2. Dosyaya çift tıklayarak favori web tarayıcınızda açın ve kullanmaya başlayın.

## 👨‍💻 Geliştirici
**Ali Eren Balıkçı**
