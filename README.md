# GEP1316-odev4-kayit-defteri
<img width="616" height="1130" alt="Ekran görüntüsü 2026-11-25 175950" src="https://github.com/user-attachments/assets/170ac517-afcb-47c4-83ec-edc07deab06b" />
def kayit_ekle(kayitlar, ad, etkinlik, tarih, katki_payi):
    """Listeye yeni bir katılımcı kaydı (sözlük) ekler ve eklenen kaydı döndürür."""
    yeni_kayit = {
        "ad": ad,
        "etkinlik": etkinlik,
        "tarih": tarih,
        "katki_payi": katki_payi
    }
    kayitlar.append(yeni_kayit)
    return yeni_kayit


def kayitlari_listele(kayitlar):
    """Tüm kayıtları numaralandırarak ekrana yazdırır; liste boşsa False döndürür."""
    if not kayitlar:
        print("\nHenüz kayıt bulunmamaktadır.")
        return False

    print("\n--- Tüm Katılımcı Kayıtları ---")
    for indeks, kayit in enumerate(kayitlar, start=1):
        print(
            f"{indeks}. Ad: {kayit['ad']} | Etkinlik: {kayit['etkinlik']} | "
            f"Tarih: {kayit['tarih']} | Katkı Payı: {kayit['katki_payi']:.2f} TL"
        )
    return True
def tarihe_gore_filtrele(kayitlar, baslangic_tarihi):
    """Belirtilen tarihte veya sonrasında (YYYY-AA-GG) yapılan kayıtları döndürür."""
    filtrelenenler = []
    for kayit in kayitlar:
        if kayit["tarih"] >= baslangic_tarihi:
            filtrelenenler.append(kayit)
    return filtrelenenler


def ozet_hesapla(kayitlar):
    """Kayıtların toplam, ortalama ve en yüksek katkı payı istatistiklerini döndürür."""
    if not kayitlar:
        return None

    tutarlar = [kayit["katki_payi"] for kayit in kayitlar]
    return {
        "toplam_kayit": len(kayitlar),
        "toplam_tutar": sum(tutarlar),
        "ortalama_tutar": sum(tutarlar) / len(tutarlar),
        "en_yuksek_tutar": max(tutarlar)
    }
def menu_goster():
    """Ana menüyü ekrana yazdırır ve kullanıcının seçimini döndürür."""
    print("\n=== Kulüp Etkinliği Kayıt Defteri ===")
    print("1. Yeni Kayıt Ekle")
    print("2. Tüm Kayıtları Listele")
    print("3. Tarihe Göre Filtrele")
    print("4. Özet İstatistikleri Göster")
    print("5. Çıkış")
    return input("Seçiminiz (1-5): ").strip()


def main():
    kayitlar = [
        {
            "ad": "Elif Kaya",
            "etkinlik": "Karakter Tasarımı Atölyesi",
            "tarih": "2026-11-10",
            "katki_payi": 150.0
        },
        {
            "ad": "Mert Demir",
            "etkinlik": "Storyboard ve Kurgu Semineri",
            "tarih": "2026-11-18",
            "katki_payi": 200.0
        }
    ]

    while True:
        secim = menu_goster()

        if secim == "1":
            ad = input("Katılımcı Adı Soyadı: ").strip()
            etkinlik = input("Etkinlik Adı: ").strip()
            tarih = input("Tarih (YYYY-AA-GG): ").strip()
            try:
                katki_payi = float(input("Katkı Payı (TL): ").strip())
                eklenen = kayit_ekle(kayitlar, ad, etkinlik, tarih, katki_payi)
                print(f"Kayıt eklendi: {eklenen['ad']} ({eklenen['etkinlik']})")
            except ValueError:
                print("Hata: Katkı payı sayısal bir değer olmalıdır. Kayıt iptal edildi.")

        elif secim == "2":
            kayitlari_listele(kayitlar)

        elif secim == "3":
            sinir_tarih = input("Başlangıç tarihi (YYYY-AA-GG): ").strip()
            sonuclar = tarihe_gore_filtrele(kayitlar, sinir_tarih)
            if not sonuclar:
                print(f"{sinir_tarih} ve sonrası için kayıt bulunamadı.")
            else:
                print(f"\n--- {sinir_tarih} ve Sonrası Kayıtlar ---")
                kayitlari_listele(sonuclar)

        elif secim == "4":
            ozet = ozet_hesapla(kayitlar)
            if ozet is None:
                print("Özet hesaplamak için önce kayıt eklemelisiniz.")
            else:
                print("\n--- Kayıt Defteri Özeti ---")
                print(f"Toplam Katılımcı : {ozet['toplam_kayit']}")
                print(f"Toplam Katkı     : {ozet['toplam_tutar']:.2f} TL")
                print(f"Ortalama Katkı   : {ozet['ortalama_tutar']:.2f} TL")
                print(f"En Yüksek Katkı  : {ozet['en_yuksek_tutar']:.2f} TL")

        elif secim == "5":
            print("Programdan çıkılıyor. İyi günler!")
            break

        else:
            print("Geçersiz seçim! Lütfen 1 ile 5 arasında bir sayı girin.")


if __name__ == "__main__":
    main()
