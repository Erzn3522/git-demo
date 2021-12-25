# Notlar 

## Table of contents
* [Giriş](#giriş)
* [Eksiklerim](#eksiklerim)
* [C Sharp](#c-sharp)
* [Halcon](#halcon)
* [Sql](#sql)


## Giriş
Bu dosyada karşılaştığım hataları, bilmem gereken bilgileri vs depolayacağım.

## Eksiklerim

- Uygulamaları önce kafanda ve defterinde tasarladıktan sonra kod yazmaya başla!!
- Uygulamaların readmelerine o an kullandığın programların versiyonlarını ekle
- Kameranın açısı, pozisyonları ile ilgili görsel ve kamerayı test ettiğin istasyonun detaylı açıklamalarını ekle
- Kodun içinde daha fazla yorum satırı kullan ve onları olabildiğince sade yap
- Sadece İngilizce ve akılda kalıcı değişken isimleri tanımla
- Multithreading'de bol bol pratik yap

	
## C Sharp
### hwindowcontrol strech problemi
Görüntüyü hwindowcontrol ekranında gösterirken strecth işlemi yapamadığım için ekranda görüntünün sadece 1/4'ünü görebiliyordum. bu kod parçacığı ile görüntüyü ekrana sığdırdım
```c#
            HTuple hWindow = hWindowControl1.HalconID;
            HOperatorSet.GetImageSize(ho_Image, out hv_Width, out hv_Height);
            HOperatorSet.SetPart(hWindow, 0, 0, hv_Height - 1, hv_Width - 1);
```

### Halcon compenentlerinin toolbox'ta görünmemesi
halcon componentlerinin toolbox'ta görünmesi için Toolbox-->options--> browse--> halcon'un local dosyasındaki bin/dotnet-35 yolundaki dll'leri ekle.

### System.BadImageFormatException hatası
System.BadImageFormatException: Geçersiz biçimdeki bir program yüklenmek istendi. (HRESULT özel durum döndürdü: 0x8007000B) hatasının sebebi aktif çözüm platformunun "Any Cpu" seçeneğinde olmasıdır. onu x64'e çekerek bu sorunu çözdüm

### Halcon HWindowControl tanımlanması
HWindowControl componentini halcondan export ettiğim kodda belirtilen ```public HTuple hv_ExpDefaultWinHandle;``` tanımlaması ile çalışmıyor yani bu şekilde HWindowControl elementine erişemiyosun. bunun için de  ```HTuple hWindow = hWindowControl1.HalconID; ```  yapmak gerekiyor.
	
## Halcon
### Edge Extraction (Subpixel and Pixel approximation)
subpixel daha avantajlı çünkü kullanımda senden tek parametre ister, İmajı ver, egde'leri al. Daha hızlı ve yüksek doğrulukla çalışır.
### Matching types
- **The correlation-based matching (Korelasyona dayalı eşleştirme):**
Gri değerlere ve normalleştirilmiş bir çapraz korelasyona dayanır. Burada lineer aydınlatma değişiklikleri ile başa çıkılabilir. Operatörler, şekil tabanlı eşleştirme(shape-based matching) operatörlerine benzer şekilde kullanılır. Şekil tabanlı eşleştirmeye göre avantajı, dokunun da işlenebilmesidir. \
Öte yandan, nesne yalnızca döndürülebilir ancak ölçeklendirilemez ve yaklaşım gri değerli görüntülerle sınırlıdır. Ek olarak, yaklaşım, tıkanıklık, dağınıklık ve doğrusal olmayan aydınlatma değişikliklerine karşı hassastır.
- **The shape-based matching (Şekil tabanlı eşleştirme):** Şekil tabanlı eşleştirme, makine görüşünde en son teknolojiyi temsil eder Birden çok örnek bulunabilir ve birden çok model kullanılabilir
aynı zamanda. \
Yöntem, nesnelerin döndürülmesine ve ölçeklenmesine izin verir ve çok kanallı görüntüler için de uygulanabilir.

- **The component-based matching(Bileşen tabanlı eşleştirme):** Bileşenleri ayrı ayrı matching yapmaya izin verir örneğin makası shape based yaparsan tek bir contur model oluştururken bununla yaparsan makasın sağ ve sol tarafını 
iki ayrı model gibi inceler.\
 shape based'a göre kötü tarafı görüntüde bulanıklık, kontrast değişimi ve ya deformasyon olursa shape based gibi iyi sonuç vermez.

##String kısımda indisleme yapma
stringin içine dinamik olarak indislemede kullanacağım yapı. Arama kısmına tuple_string yazarak ulaşabilirsin
```
  for Index := 1 to 5 by 1
      write_image (Image, 'hobj', 0,'D:/AbdullahWorkspace/Halcon Examples/PencilAngle/images/Image'+Index$'02'+'.hobj')
  endfor

```
## Matchin'e Regionla yardımcı olma
matching yaparken regionla kısımları bulup boundary yaparak xld şekli region bulmuş oluyoz. 
```c#
        binary_threshold (Image, Region, 'smooth_histo', 'light', UsedThreshold)
        connection (Region, Holes)
        select_shape (Holes, SmallHoles, 'area', 'and', 5000, 10000)
        boundary (SmallHoles, RegionBorder, 'inner_filled')
```

## SQL
### Sql Connection String
sql bağlantısını basit düzeyde yaptığım uygulama. sql'e bağlantı genelde sorun yapıyo iş yeri bilgisayarında o yüzden bu connection stringi kullanarak ulaşıyorum. 
```
public SqlConnection sqlConnection = new SqlConnection(@"Data Source=gensysbox\sqlexpress2014; Initial Catalog=""AbdullahTestDataBase"";Trusted_Connection=Yes;Integrated Security=True;User ID=**;Password = *******;");
```
