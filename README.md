# 🌟 RawJS Image Processing Engine (Tarayıcı Tabanlı Görüntü İşleme Motoru)

Bu proje, genellikle Python ve OpenCV gibi harici kütüphanelerle gerçekleştirilen dijital görüntü işleme (digital image processing) algoritmalarının, tarayıcı üzerinde saf JavaScript (Vanilla JS) ve HTML5 Canvas API kullanılarak nasıl sıfırdan inşa edilebileceğini gösteren etkileşimli bir mühendislik çalışmasıdır. 

Uygulama, yüklenen görüntülerin piksel matrislerine doğrudan müdahale ederek yerel kontrast artırma (HDR yapılandırması), renk uzayı dönüşümleri ve doğrusal parlaklık manipülasyonlarını tamamen istemci tarafında (client-side) gerçekleştirir.

## 🚀 Öne Çıkan Özellikler ve Algoritmalar

### 1. Yerel Kontrast Artırma ve Parlaklık Maskeleme (HDR)
Standart parlaklık artışının aksine, bu algoritma görüntünün aşırı pozlanmış (patlamış) bölgelerini korurken yalnızca gölgede kalan karanlık alanları aydınlatır.
* **Nasıl Çalışır?** Her pikselin Luma (parlaklık) değeri `0.299R + 0.587G + 0.114B` formülü ile hesaplanır. Elde edilen değere göre `1 - (Luma / 255)` formülüyle bir **Gölge Maskesi (Shadow Mask)** oluşturulur. Bu maske, karanlık piksellerde 1'e yaklaşırken aydınlık piksellerde 0'a yaklaşır ve aydınlatma şiddeti sadece gerekli alanlara orantılı olarak uygulanır.

### 2. İnsan Gözü Duyarlılığına Göre Grayscale (Luma)
Standart `(R+G+B)/3` ortalaması yerine, insan gözünün yeşil renge olan yüksek duyarlılığını temel alan algısal parlaklık formülü kullanılmıştır:
* `Gray = 0.3 * R + 0.59 * G + 0.11 * B`

### 3. Renk Uzayı Dönüşümü (Sepya Filtresi)
Her bir RGB kanalı, belirli katsayılardan oluşan standart Sepya matrisi ile çarpılarak retro renk tonu elde edilir.

### 4. Güvenli Değer Sınırlandırma (Clamping)
Matematiksel manipülasyonlar sonucu piksel değerlerinin 8-bitlik renk aralığı (0-255) dışına çıkmasını ve veri bozulmasını (overflow/underflow) engellemek için tüm sonuçlar `Math.min(255, Math.max(0, value))` işleminden geçirilir.

## 🛠️ Teknik Mimari ve Teknolojiler

* **HTML5 Canvas 2D API:** Görüntünün render edilmesi ve `getImageData` / `putImageData` metotlarıyla piksellerin `Uint8ClampedArray` formatında tek boyutlu bir matris (RGBA) olarak okunup yazılması.
* **JavaScript File API:** Kullanıcı cihazından alınan görüntü verilerinin asenkron `FileReader` ile güvenli bir şekilde `Base64` formatına dönüştürülmesi.
* **Vanilla DOM Manipulation:** Harici bir framework (React/Vue vb.) kullanılmadan sağlanan dinamik durum (state) yönetimi ve olay dinleyicileri (event listeners).
* **CSS3:** Flexbox tabanlı, duyarlı (responsive) ve modern 'Dark Mode' arayüz tasarımı.

**Yerel Ortamda Çalıştırmak İçin:**
1. Projenin kodlarını içeren dosyayı `.html` uzantısıyla (örneğin `index.html`) bilgisayarınıza kaydedin.
2. Dosyaya çift tıklayarak favori web tarayıcınızda açın ve kullanmaya başlayın.

## 👨‍💻 Geliştirici
**Ali Eren Balıkçı**
  
   
