# Giriş Sistemi Kurulumu

Panel artık 4 haneli PIN yerine **kullanıcı adı + şifre** ile açılıyor. Veritabanı da sadece
yetkili kullanıcılara açık olacak. Aşağıdaki adımları **bu sırayla** yapın.

Firebase paneli: https://console.firebase.google.com → **raz-plastik** projesi

## 1. Şifreli girişi açın
1. Soldaki menüden **Authentication**'a (Kimlik doğrulama) girin. İlk kez giriyorsanız **Get started**'a basın.
2. **Sign-in method** sekmesinde **Email/Password**'u seçin, ilk anahtarı **açın**, **Save**.

## 2. Kullanıcıları oluşturun
1. **Users** sekmesinde **Add user**'a basın.
2. **Email** kutusuna: `kullaniciadi@razpanel.local` yazın.
   Örnek: kullanıcı adı `oraz` olacaksa → `oraz@razpanel.local`
   (Türkçe harf kullanmayın; panelde "Şükrü" yazılırsa "sukru" olarak aranır.)
3. **Password** kutusuna en az 8 karakterli, tahmin edilmesi zor bir şifre yazın → **Add user**.
4. Paneli kullanacak her kişi için tekrarlayın.

Panelde giriş yaparken sadece kullanıcı adı (`oraz`) ve şifre yazılır.

## 3. Yeni paneli yayına alın ve giriş yapın
Yeni kodu ana dala birleştirin, sayfayı açıp oluşturduğunuz kullanıcıyla **giriş yapabildiğinizi
kontrol edin**. Giriş çalışmadan 4. adıma geçmeyin.

## 4. Veritabanını kilitleyin
1. Soldaki menüden **Firestore Database** → **Rules** sekmesi.
2. Oradaki her şeyi silin, bu depodaki `firestore.rules` dosyasının içeriğini yapıştırın.
3. Listeye 2. adımda oluşturduğunuz **bütün** kullanıcıları ekleyin, örneğin:
   ```
   'oraz@razpanel.local',
   'hasan@razpanel.local'
   ```
4. **Publish**'e basın.

Bundan sonra listede olmayan hiç kimse — sayfa adresini bilse bile — verileri okuyamaz ve değiştiremez.

## Notlar
- Oturum her cihazda hatırlanır; şifre sadece ilk girişte (ve **Çıkış**'tan sonra) sorulur.
- Şifre unutulursa: Authentication → Users → kişinin satırındaki menü → **Reset password** yerine
  hesabı silip aynı adla yeniden oluşturmak en kolayıdır (sahte e-postaya mail gitmez).
- Bir kişinin erişimini kapatmak için: Users'tan hesabını silin ve kurallardaki listeden çıkarın.

## Güncelleme: yeni veri düzeni (kayıtlar ayrı ayrı, geçmiş, silinenler)
Panelin bu sürümü verileri ilk açılışta otomatik olarak yeni düzene taşır (eski veri silinmez,
`raz/data` belgesinde yedek olarak kalır). Sonra:
1. Paneli açık olan **bütün cihazlarda** sayfayı yenileyin (eski sürüm açık kalmasın).
2. Firestore Database → Rules: bu depodaki güncel `firestore.rules` içeriğini yapıştırın,
   **kullanıcı listesini kendi listenizle tekrar doldurun**, Publish.
   Bu kurallarla işlem geçmişi kimse tarafından silinemez/değiştirilemez ve eski veri belgesine
   artık yazılamaz.
