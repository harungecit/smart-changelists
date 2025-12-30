# Rules for AI Agents

Bu kurallar tüm AI agent'lar (Claude, Copilot, Cursor, vb.) için geçerlidir.

---

## 1. Git Kuralları

### ASLA Otomatik Git Commit Yapma
- `git commit` komutu ASLA otomatik çalıştırılmayacak
- `git push` komutu ASLA otomatik çalıştırılmayacak
- `git tag` komutu ASLA otomatik çalıştırılmayacak

### Git Komutlarını Çıktı Olarak Ver
- Her görev/süreç tamamlandığında gerekli git komutlarını kullanıcıya göster
- Komutları açıklamalı şekilde listele
- Kullanıcı bu komutları manuel olarak çalıştıracak

### Örnek Format
```
Görev tamamlandı. Git komutları:

# Değişiklikleri stage et
git add .

# Commit at
git commit -m "feat: Add new feature"

# Push et
git push origin master
```

---

## 2. Versiyon Kuralları

### ASLA Otomatik Versiyon Yükseltme
- `package.json` içindeki `version` alanı otomatik değiştirilmeyecek
- Versiyon değişikliği SADECE kullanıcı onayı ile yapılacak

### Versiyon Değişikliği Süreci
1. Kullanıcıya mevcut versiyonu göster
2. Yeni versiyon öner (semantic versioning)
3. Kullanıcıdan onay al
4. ONAY ALINMADAN versiyon değiştirme

### Örnek Dialog
```
Mevcut versiyon: 1.1.3
Önerilen versiyon: 1.1.4 (patch) veya 1.2.0 (minor)

Versiyon yükseltilsin mi? Hangi versiyon?
```

---

## 3. Publish/Release Kuralları

### Yayınlama Öncesi Kontrol Listesi
1. [ ] Kullanıcı versiyon onayı verdi mi?
2. [ ] CHANGELOG.md güncellendi mi?
3. [ ] README.md güncel mi?
4. [ ] Build başarılı mı?
5. [ ] Testler geçti mi?

### Otomatik Publish Yok
- `vsce publish` ASLA otomatik çalıştırılmayacak
- `npm publish` ASLA otomatik çalıştırılmayacak
- Tag push (release tetikler) kullanıcı tarafından yapılacak

---

## 4. Dosya Değişiklik Kuralları

### Onay Gerektiren Değişiklikler
- `package.json` → version alanı
- `.github/workflows/` → workflow dosyaları
- Kritik config dosyaları

### Serbestçe Değiştirilebilir
- Kaynak kod (`src/`)
- Test dosyaları (`src/test/`)
- Dokümantasyon (`.claude/`, `README.md`, `CHANGELOG.md`)
- Style/lint config dosyaları

---

## 5. Süreç Tamamlama Formatı

Her görev tamamlandığında şu formatta çıktı ver:

```
## Tamamlanan İşlemler
- [x] İşlem 1
- [x] İşlem 2
- [x] İşlem 3

## Değiştirilen Dosyalar
- `path/to/file1.ts` - Açıklama
- `path/to/file2.ts` - Açıklama

## Git Komutları
git add path/to/file1.ts path/to/file2.ts
git commit -m "feat: Description"
git push origin master

## Sonraki Adımlar (varsa)
- Adım 1
- Adım 2
```

---

## 6. Özet

| Eylem | İzin |
|-------|------|
| Kod yazma/düzenleme | ✅ Serbest |
| Git add | ❌ Yasak (komut olarak ver) |
| Git commit | ❌ Yasak (komut olarak ver) |
| Git push | ❌ Yasak (komut olarak ver) |
| Git tag | ❌ Yasak (komut olarak ver) |
| Versiyon değiştirme | ❌ Onay gerekli |
| Publish/Release | ❌ Yasak (kullanıcı yapacak) |
| Build/Test çalıştırma | ✅ Serbest |
| Dosya okuma | ✅ Serbest |
| Dokümantasyon güncelleme | ✅ Serbest |
