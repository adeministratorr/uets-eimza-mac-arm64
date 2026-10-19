# PTT UETS E-İmza (e-Tebligat) — macOS Apple Silicon (M1/M2/M3/M4) Kurulumu (arm64)

UETS e-Tebligat e-imza uygulamasını **MacBook, MacBook Pro, MacBook Air, Mac mini ve iMac**'de native çalıştırın. Apple Silicon (M serisi, ARM tabanlı işlemci) için **arm64** `.app`; gömülü Java 11 ile gelir — Rosetta gerekmez, ayrı Java kurulumu gerekmez.

**Kimler için:** e-tebligat alan herkes — avukatlar, arabulucular ve bilirkişiler (1 Ocak 2019'dan beri UETS'e dahil olma ve KEP alma zorunluluğu vardır), anonim/limited şirketler (Tebligat Kanunu 7/A), mali müşavirler, doktorlar, mühendis ve mimarlar, sigorta ve finans profesyonelleri ile KEP adresi bulunan tüm kullanıcılar.

## Kimler için?
- **Avukatlar:** UETS üzerinden gelen e-tebligatları açan ve imzalayanlar — Mac'te "e-tebligat açılmıyor / imza atamıyorum" sorununu çözer.
- **Arabulucular ve bilirkişiler:** Elektronik Tebligat Yönetmeliği gereği KEP/UETS kullanan zorunlu gruplar.
- **Şirket yetkilileri:** Anonim, limited ve sermayesi paylara bölünmüş komandit şirketlerin zorunlu e-tebligat kullanıcıları.
- **Mali müşavirler, doktorlar, mühendis-mimarlar:** KEP adresine e-tebligat gelen tüm profesyoneller.
- **Hukuk büroları:** MacBook (M1/M2/M3/M4) ve iMac ile e-imza kurulumu.

## "Mac'te e-imza çalışmıyor" arayanlar için kısa cevaplar
- **Neden Mac'te açılmıyor?** Uygulama Java Web Start (JNLP) tabanlı; Java Web Start Java 11 ve sonrasında kaldırıldı. Bu paket uygulamayı gömülü Java 11 ile native `.app`'e çevirir.
- **MacBook / MacBook Pro / MacBook Air / iMac / Mac mini'de çalışır mı?** Evet — Apple Silicon (M1/M2/M3/M4) tüm modeller destekli. Intel Mac desteklenmez.
- **Rosetta veya Java kurmam gerekir mi?** Hayır; ARM native (arm64), Java gömülü.
- **İmza için ek olarak ne gerekir?** e-imza kartı + kart okuyucu + **arm64 AKİS sürücüsü** (aşağıda).

## Neden Mac'te çalışmıyordu? Nasıl çözdük?

- **Sorun:** UETS e-tebligat uygulaması modern Mac'lerde açılmıyordu.
- **Neden:** Uygulama Java Web Start (JNLP) ile dağıtılıyordu; Java Web Start Java 11 ile kaldırıldı. Eski Java 8 ise Retina ekranda bulanık çiziyordu.
- **Çözüm:** `jpackage` + gömülü **arm64 Java 11** ile native `.app`; `jnlp.config` (sunucu adresleri) ve dil ayarları uygulamaya gömüldü. PTT KEP ile çakışmaması için ayrı bundle kimliği ve ayrı uygulama adıyla paketlendi.
- **Kart erişimi:** Kart okuma TÜBİTAK ESYA API + `javax.smartcardio` üzerinden; sistemde **arm64 AKİS sürücüsü** kurulu olmalıdır.
- **Doğrulama (dürüst not):** Uygulamanın açılışı ve sunucu bağlantısı test edildi; kart takılı olmadığından yalnızca "Akıllı Kart Bulunamadı" durumu görüldü. Gerçek kartla imzalama, aynı yöntemin kullanıldığı kardeş projede uçtan uca doğrulandı.

## Kurulum (3 yol)
1. **Tek satır (kaynaktan derler):** aşağıdaki "Kolay kurulum" bölümü.
2. **Homebrew cask (hazır DMG, derleme beklemez):**

```bash
brew install --cask adeministratorr/tap/uets-eimza
```

3. **Hazır DMG:** [Releases](../../releases) → `UetsEimza-arm64-*.dmg`

Diğer araçlar (aynı tap): [e-Devlet E-İmza](https://github.com/adeministratorr/edevlet-eimza-mac-arm64) · [PTT KEP Webmail](https://github.com/adeministratorr/pttkep-eimza-mac-arm64) · [TNB Teknoloji İmza](https://github.com/adeministratorr/tnb-eimza-mac-arm64) · [UHAP İmzalama](https://github.com/adeministratorr/uhap-eimza-mac-arm64) · [UDF QuickLook](https://github.com/adeministratorr/udf-quicklook-extension) · [UYAP Doküman Editörü](https://github.com/adeministratorr/ude-mac-arm64-1)

> ⚠️ **Önemli:** UETS (e-tebligat) ile PTT KEP Webmail **farklı** uygulamalardır; işlem kodları karşılıklı geçmez. Doğru uygulamayı seçtiğinizden emin olun (aşağıda karşılaştırma var).

---

# PTT UETS E-İmza — Native Apple Silicon (arm64) .app

[UETS e-Tebligat](https://etebligat.gov.tr) (Ulusal Elektronik Tebligat Sistemi)
E-İmza Uygulaması, bir Java Web Start (JNLP) uygulamasıdır. Java Web Start,
Java 11+ ile kaldırıldığından uygulama modern Mac'lerde kolayca açılamıyor. Bu
depo, uygulamayı **gömülü arm64 Java 11 runtime'ı** ile **çift tıklayıp
açabileceğiniz native bir `.app`'e** paketler — Rosetta gerektirmez, ayrıca Java
kurmanıza gerek kalmaz.

> ⚠️ **Bu depo UETS E-İmza uygulamasının kaynak kodunu içermez.** Tamamen
> bağımsız, **gayriresmî** bir Mac **paketleyicisidir**: hiçbir kamu kurumu
> tarafından geliştirilmemiş/onaylanmamıştır. Burada bulunan yalnızca paketleme
> ve build betikleridir; resmî `uets-eimza.jar`'ı build sırasında
> api.etebligat.gov.tr'den **siz** indirir ve native `.app`'i **siz** üretirsiniz.
> "Olduğu gibi" sunulur.

> ✅ Uygulamanın Apple Silicon'da native açıldığı, `config.properties`'i
> sunucudan okuduğu ve arayüzün Retina'da keskin çalıştığı doğrulandı.
> **Gerçek kartla imzalama akışı bu depoda test edilmedi** — kart takılı
> olmadığı için yalnızca "Akıllı Kart Bulunamadı" durumu görüldü. (Aynı
> paketleme yöntemi, kardeş depo
> [pttkep-eimza-mac-arm64](https://github.com/saidsurucu/pttkep-eimza-mac-arm64)'da
> gerçek kartla uçtan uca doğrulanmıştı.)

---

## ⚠️ UETS ≠ PTT KEP Webmail — doğru uygulamayı kullanın

PTT'nin **iki ayrı** e-imza sistemi var ve **işlem kodları karşılıklı geçmez**:

| Sistem | Arayüz | Uygulama |
|--------|--------|----------|
| **UETS / e-Tebligat** | etebligat.gov.tr | **bu depo** |
| PTT KEP Webmail | ptt.hs01.kep.tr | [pttkep-eimza-mac-arm64](https://github.com/saidsurucu/pttkep-eimza-mac-arm64) |

Yanlış uygulamaya işlem kodu girerseniz sunucu şu hatayı döner:

> "İmzalama işlemi için E-İmza uygulamasını işlem yapmak istediğiniz arayüzden
> indirerek…"

İki uygulama farklı bundle-id ve farklı ikon kullanır; aynı Mac'te yan yana
kurulabilirler.

---

# 👩‍💼 Kolay kurulum — tek satır

Programcı olmanıza gerek yok. **Terminal** uygulamasını açın (klavyede
`Command (⌘) + Boşluk`'a basıp açılan kutuya **Terminal** yazın ve **Enter**'a
basın), ardından aşağıdaki **tek satırı** kopyalayıp yapıştırın ve **Enter**'a
basın:

```bash
curl -fsSL https://raw.githubusercontent.com/saidsurucu/uets-eimza-mac-arm64/main/kur.sh | bash
```

> Terminal'iniz Rosetta (x86_64) modunda açılmışsa betik bunu algılayıp kendini
> otomatik olarak arm64'e geçirir — `arch -arm64` yazmanıza gerek yoktur.

Hepsi bu kadar. Manuel indirme, klasöre girme, Java kurma gibi adımlar **yok**. Bu
komut gerisini sizin için yapar:

- Gerekiyorsa **geliştirici araçlarını** (Xcode komut satırı araçları) kurar — bir
  pencere açılırsa yalnızca **"Yükle"**ye basıp bitmesini bekleyin, betik
  kendiliğinden devam eder.
- **Kaynak kodu** `~/uets-eimza-mac-arm64` klasörüne indirir (zaten varsa en
  güncel sürüme günceller).
- Gereken **Java** sürümlerini otomatik indirir.
- Resmî `uets-eimza.jar`'ı api.etebligat.gov.tr'den indirir, uygulamayı **derler +
  imzalar** ve doğrudan **/Applications** klasörüne kurar.

İlk derleme internet hızınıza göre birkaç dakika sürebilir (jar ~22 MB, üretilen
`.app` ~193 MB — gömülü Java runtime dahil).

Bittiğinde uygulama **Launchpad** ve **Applications** klasöründe hazırdır; çift
tıklayarak açabilirsiniz. (Kendiniz derleyip imzaladığınız için macOS "geliştirici
doğrulanamadı" uyarısı **çıkmaz**; `xattr` ile uğraşmanıza gerek yoktur.)

**Yeni sürüm çıktığında yukarıdaki tek satırı yeniden çalıştırmanız yeterli. En
güncel sürüm otomatik inecek ve paketlenecek.**

> İsterseniz sürükle-bırak yerleşimli bir `.dmg` de üretebilirsiniz:
> `brew install create-dmg` sonrası `make dmg`.

> ⚠️ **E-imza için arm64 AKİS sürücüsü ŞART** (aşağıdaki bölüm). Uygulama native
> arm64'tür; kart sürücünüz de arm64 olmalıdır.

---

## ⚠️ Apple Silicon (arm64) AKİS sürücüsü kurulumu (zorunlu)

Bu uygulama native arm64 çalışır. **Bir arm64 uygulama, yalnızca Intel (x86_64)
derlenmiş bir sürücüyü yükleyemez** (mimari uyuşmazlığı). TÜBİTAK AKİS'in macOS
için **ayrı Intel ve Apple Silicon paketleri** vardır; çoğu kullanıcıda
eski/Intel sürüm kuruludur.

**Çözüm:** Apple Silicon AKİS paketini kurun:

1. [TÜBİTAK BİLGEM AKİS — Destek/İndirme](https://akiskart.bilgem.tubitak.gov.tr/tr/destek/)
   sayfasından **"Mac OS Arm (Apple Silicon)"** başlığı altındaki güncel paketi indirin
   (ör. `Akia_macos_arm_6_8_9.pkg`). **"Mac OS Intel" paketini değil**, Arm paketini seçin.
2. İndirilen `.pkg`'a çift tıklayıp kurulumu tamamlayın (yönetici şifresi ister).
3. **PTT UETS E-İmza** uygulamasını kapatıp yeniden açın; kartı takıp deneyin.

**Doğru sürümü kurduğunuzu teyit:** Terminal'de şu komut **`x86_64 arm64`** (veya
en azından `arm64`) yazmalı — sadece `x86_64` yazıyorsa hâlâ Intel sürüm kuruludur:

```
lipo -archs /usr/local/lib/libakisp11.dylib
```

---

# 🛠️ Mühendisler için — Teknik ayrıntı

Yukarıdaki adımlar derlemek için yeterlidir. Bu bölüm, tek tek build hedeflerini
ve dönüşümün **neyi nasıl** çözdüğünü açıklar. Gereksinimler kendiliğinden kurulur
(Azul Zulu 11 + 21). Apple Silicon Mac'te:

```bash
make all          # download → icns → package → sign
make run          # üretilen .app'i aç
make dmg          # sürükle-bırak yerleşimli .dmg üret (brew install create-dmg gerekir)
```

DMG arka planı `assets/dmg-background.svg`'den üretilir; düzenleyip `make assets`
ile yeniden render edebilirsiniz (`brew install librsvg`). Tek tek hedefler için `make help`.

### Nasıl çalışır

- `jpackage --type app-image` + `--runtime-image <tam arm64 Zulu 11>` ile
  uygulama ve **gömülü Java 11 runtime** tek bir native `.app`'e paketlenir.
  Tam JRE şarttır (jlink-strip değil): smartcardio/crypto provider'ları gerekir.
- **Neden Java 11:** otomatik HiDPI (JEP 263) → Retina'da keskin metin
  (arm64 Java 8 Swing bulanık render ediyor). `uets-eimza.jar` Java 8
  bytecode'dur (major 52) ve Java 11'de sorunsuz çalışır.
- **`-Djnlp.config` (kritik):** JNLP, uygulamaya
  `jnlp.config=https://api.etebligat.gov.tr/v1/auth/_eimza/config.properties`
  property'sini geçirir; sunucu adresleri (`getdata_url`/`setdata_url`) bu
  dosyadan okunur. jpackage `--java-options` ile bu property `.app`'e gömülür —
  yoksa uygulama sunucuyu bulamaz. JNLP'nin ayrıca geçirdiği
  `jnlp.user.language=tr` / `jnlp.user.country=TR` de aynı yolla gömülür.
- **Kart erişimi:** jar, IAIK PKCS#11 wrapper DEĞİL, TÜBİTAK ESYA API
  (`APDUSmartCard`) + `javax.smartcardio` kullanır; jar'da kart erişimi için
  native kütüphane taşınmaz. Bu yüzden e-Devlet paketleyicisindeki Javassist
  connect-fix patch'i burada gerekmez. AKİS middleware'inin arm64 sürümü
  sistemde kurulu olmalıdır.
- **İkon (KEP'ten fark):** codebase'de `logo.png` **mevcut** — resmî UETS
  amblemi, ama 250×173 (kare değil). `.icns` üretilirken en uzun kenar 480'e
  ölçeklenip **şeffaf dolguyla** 512×512 kareye ortalanır (`sips -p`, dolguyu
  RGBA 0,0,0,0 yapar). Codebase'e erişilemezse jar içindeki `images/logo.png`'ye
  düşülür — fakat o, PTT KEP damgasıdır; o durumda ikon kardeş uygulamayla aynı
  görünür.
- **codesign + Türkçe karakter:** `.app` adındaki `İ` gibi karakterler imzayı
  bozuyor; bu yüzden executable ASCII tutulur (`PTTUETSEImza`), görünen ad
  sonradan `CFBundleName`/`CFBundleDisplayName` ile Türkçe yapılır. Ad-hoc imza
  (`codesign -s -`) uygulanır.
- **Ayrı bundle-id:** `tr.gov.ptt.uets.digitalsignature` — PTT KEP paketleyicisi
  `tr.gov.ptt.kep.digitalsignature` kullandığı için iki uygulamanın aynı Mac'te
  çakışmadan durabilmesi adına farklı tutulur. (Main-Class ikisinde de aynıdır:
  `…kep.digitalsignature.swing.FrmApplet`.)

### JNLP gerçekleri (kaynak)

| Alan | Değer |
|------|-------|
| codebase | `https://api.etebligat.gov.tr/v1/auth/_eimza` |
| ana jar | `uets-eimza.jar` (~22 MB, self-contained) |
| main-class | `tr.gov.ptt.kep.digitalsignature.swing.FrmApplet` |
| j2se | 1.7+ · all-permissions |
| jnlp.config | `…/_eimza/config.properties` (`getdata_url`/`setdata_url`; KEP'teki `getvers_url` **yok**) |
| sürüm | `1.1.60` (JNLP `<jar version=…>`) — pencere başlığı ise `UETS v1.1.50` yazar, bkz. aşağıdaki not |
| ikon | codebase'deki `logo.png` (250×173 UETS amblemi) → kareye ortalanıp `.icns` |

**Sürüm notu:** `uets-eimza.jar`, `?version-id=1.1.60` ve `uets-eimza__V1.1.60.jar`
adreslerinin üçü de **bayt bayt aynı** dosyayı döndürür; yani tek bir jar var.
JNLP `1.1.60` derken uygulamanın kendi pencere başlığı `v1.1.50` yazıyor — jar
içindeki sürüm sabiti güncellenmemiş. PTT jar'ı yenilediğinde değişen değer
JNLP'deki olduğu için `APP_VERSION` (ve dolayısıyla `CFBundleVersion` + yayın
etiketi) JNLP sürümüne bağlanmıştır.

**Bilinen zararsız log'lar:** uygulama açılışta
`ClassNotFoundException: com.sun.java.swing.plaf.nimbus.NimbusLookAndFeel` ve
`MAC PSCS pathi bulunamadı, varsayılan atandı.` basar. İkisi de zararsızdır;
uygulama varsayılanlara düşüp normal çalışır.

---

## CI build (isteğe bağlı)

`.github/workflows/release.yml` elle tetiklenir (`workflow_dispatch`):
macos-14 (arm64) runner'da `.app` üretir, mimariyi+imzayı doğrular ve
`<APP_VERSION>_<N>` etiketli bir yayın oluşturur.
