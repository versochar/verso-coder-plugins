# verso-coder-plugins

Verso Coder **eklenti mağazasının** resmi kayıt deposu. Uygulama içinden
`Eklentiler → Mağaza` bu depodaki `index.json` dosyasını okur, eklentiyi
tek tıkla indirip kurar.

## Yapı

```
index.json                  # kayıt: kim, ne, hangi sürüm, nerede
plugins/<kimlik>/plugin.js  # tek dosyalık eklenti
```

## Eklenti ekleme

1. `plugins/<kimlik>/plugin.js` dosyasını oluştur. Kurallar:
   - Kimlik: küçük harf, rakam, tire (`^[a-z0-9][a-z0-9-]{0,39}$`).
   - Başlık zorunlu: `// @name Görünen Ad` ve `// @version X.Y.Z`.
   - İzinler yalnız bilinenlerden: `fs.read`, `fs.write`, `events`, `ui`, `net`.
     Bilinmeyen izin yazma — uygulama sessizce düşürür, eklentin çalışmaz.
   - `fs.read`/`fs.write` proje köküyle kapsamlıdır; kök dışına çıkılamaz.
2. `index.json` dosyasına girdini ekle (`id`, `name`, `version`,
   `description`, `author`, `permissions`, `file`, `minApp`).
3. Yerelde doğrula: `packaging/plugins/publish.sh --check plugins/<kimlik>/plugin.js`
   (ana depoda) ya da uygulamayı `--portable` açıp dosyayı eklenti klasörüne
   kopyalayarak dene.
4. PR aç ya da (yetkin varsa) `publish.sh` ile gönder.

## İnceleme notları

- En az izin ilkesi: okumayan eklenti `fs.read` istemez.
- `net` izni (dış ağ) gerekçesiz kabul edilmez.
- Kötü niyetli/bozuk eklenti kayıttan çıkarılır; kurulu kopya uygulamada
  karantinaya alınabilir (Eklenti Yöneticisi).
