# Samed Panel - Kiosk Uygulaması

Bu uygulama, belirli bir web panelini (Digital Signage) tam ekran (WebView) olarak çalıştıran bir Android projesidir.

## Özellikler
* **Kiosk Modu:** Tam ekran çalışma ve geri tuşu engelleme.
* **Otomatik Başlatma:** Cihaz açıldığında veya güç bağlandığında uygulama otomatik açılır.
* **Bağlantı Kontrolü:** İnternet kesilip geldiğinde sayfayı otomatik yeniler.
* **Bakım:** Günde bir kez (gece 04:00) donma riskine karşı sayfayı tazeler.
* **Canlı İzleme:** 5 dakikada bir otomatik reload.

## APK Nasıl Alınır?
Bu repoda her güncelleme yapıldığında **GitHub Actions** otomatik olarak bir APK derleyen bir iş akışı (workflow) var. `main` dalına (branch) yapılan push’lar ya da **Actions** sekmesinden el ile tetikleme (`Run workflow`) bu işlemi başlatır.

APK'yı indirmek için:
1. GitHub sayfanızda projeye gidin.
2. Üstteki **Actions** sekmesine tıklayın.
3. Son başarıyla tamamlanmış çalışmaya tıklayın.
4. Sayfanın altındaki **Artifacts** bölümünden `app-debug` öğesini indirip açın.


---

## 🧰 Yerel Derleme İpuçları

Eğer projeyi kendi bilgisayarınızda manuel olarak derlemek isterseniz, aşağıdaki adımları takip edebilirsiniz.

### Gereksinimler

1. **Android SDK**
   - Android Studio ile kurulum yapabilirsiniz veya [komut satırı araçları](https://developer.android.com/studio#cmdline-tools) ile `sdkmanager` aracını kullanabilirsiniz.
   - En az bir `platform` (ör. `platforms;android-34`), `platform-tools` ve `build-tools` paketlerini yükleyin.
   - SDK yolunu bilmeniz gerekir; örneğin `/home/username/Android/Sdk`.

2. **Java JDK** (8 veya üstü).
3. **Gradle wrapper** (projede zaten `gradlew` mevcut, çalıştırılabilir yapın).
4. **ADB** (cihaz veya emulatöre yükleme için; `platform-tools` içinde gelir).

### Ortam Ayarları

Proje kökünde bir `local.properties` dosyası oluşturup gerçek SDK yolunu yazın:

```properties
sdk.dir=/home/username/Android/Sdk
```

Alternatif olarak terminalde şu environment değişkenlerini ayarlayabilirsiniz:

```bash
export ANDROID_HOME="/home/username/Android/Sdk"
export ANDROID_SDK_ROOT="$ANDROID_HOME"
```

`gradlew`’ın çalıştırılabilir olduğundan emin olun:

```bash
chmod +x gradlew
```

### APK Derleme

Projeyi derlemek için:

```bash
cd /workspaces/androidtv
./gradlew clean assembleDebug
```

Oluşan debug APK `app/build/outputs/apk/debug/app-debug.apk` yolunda bulunacaktır.

#### Release sürümü (mağazaya gönderme)

1. Bir keystore oluşturun:
   ```bash
   keytool -genkey -v -keystore my-release-key.jks -alias my_alias -keyalg RSA -keysize 2048 -validity 10000
   ```
2. `app/build.gradle` içindeki `signingConfigs` ve `buildTypes.release` bölümlerine keystore bilgilerinizi ekleyin.
3. `./gradlew assembleRelease` komutunu çalıştırın; sonuç `app/build/outputs/apk/release/app-release.apk` olacaktır.

### Cihaza Yükleme

```bash
adb install -r app/build/outputs/apk/debug/app-debug.apk
```

> 📝 **Not:** Bu container’da Android SDK yüklü olmayabilir; bu adımları yerel makinenizde tekrarlamak genellikle daha sorunsuz olur. `local.properties` içindeki yolu mutlaka belirtin; aksi takdirde derleme hata verir.

---
