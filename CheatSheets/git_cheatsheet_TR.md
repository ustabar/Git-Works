# Git Komutları - Kapsamlı Cheat Sheet

## 📋 İçindekiler
- [Kurulum ve Konfigürasyon](#kurulum-ve-konfigürasyon)
- [Repository Oluşturma](#repository-oluşturma)
- [Temel Komutlar](#temel-komutlar)
- [Branch İşlemleri](#branch-işlemleri)
- [Commit İşlemleri](#commit-işlemleri)
- [Remote İşlemleri](#remote-işlemleri)
- [Merge ve Rebase](#merge-ve-rebase)
- [Değişiklikleri Geri Alma](#değişiklikleri-geri-alma)
- [Stash İşlemleri](#stash-işlemleri)
- [Log ve Geçmiş](#log-ve-geçmiş)
- [Tag İşlemleri](#tag-işlemleri)
- [Gelişmiş Komutlar](#gelişmiş-komutlar)

---

## Kurulum ve Konfigürasyon

### İlk Kurulum
```bash
# Kullanıcı adı ayarla
git config --global user.name "Adınız Soyadınız"

# Email ayarla
git config --global user.email "email@example.com"

# Varsayılan editör ayarla
git config --global core.editor "code --wait"  # VS Code
git config --global core.editor "vim"           # Vim

# Varsayılan branch adını ayarla (main)
git config --global init.defaultBranch main

# Konfigürasyonu görüntüle
git config --list
git config user.name
git config --global --list

# Konfigürasyonu düzenle
git config --global --edit
```

### Faydalı Config Ayarları
```bash
# Renkli çıktı
git config --global color.ui auto

# Satır sonu karakterleri (Windows)
git config --global core.autocrlf true

# Satır sonu karakterleri (Mac/Linux)
git config --global core.autocrlf input

# Alias'lar oluştur
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.visual 'log --oneline --graph --all'
```

---

## Repository Oluşturma

### Yeni Repo Oluşturma
```bash
# Mevcut dizini Git repo yap
git init

# Yeni dizin oluştur ve Git repo yap
git init proje-adi

# Bare repository oluştur (sunucu için)
git init --bare
```

### Mevcut Repo'yu Klonlama
```bash
# HTTPS ile klonla
git clone https://github.com/kullanici/repo.git

# SSH ile klonla
git clone git@github.com:kullanici/repo.git

# Farklı klasör adıyla klonla
git clone https://github.com/kullanici/repo.git yeni-klasor

# Belirli bir branch'i klonla
git clone -b develop https://github.com/kullanici/repo.git

# Shallow clone (sadece son commit)
git clone --depth 1 https://github.com/kullanici/repo.git

# Tek branch klonla
git clone --single-branch --branch main https://github.com/kullanici/repo.git
```

---

## Temel Komutlar

### Durum Kontrolü
```bash
# Çalışma dizini durumu
git status

# Kısa format
git status -s
git status --short

# Branch bilgisi ile
git status -sb
```

### Dosya Ekleme (Staging)
```bash
# Tek dosya ekle
git add dosya.txt

# Birden fazla dosya ekle
git add dosya1.txt dosya2.txt

# Tüm dosyaları ekle
git add .
git add -A
git add --all

# Belirli uzantıdaki dosyaları ekle
git add *.js

# İnteraktif ekleme
git add -i

# Patch mode (dosyanın bir kısmını ekle)
git add -p dosya.txt

# Yeni ve değişen dosyaları ekle (silinen hariç)
git add .

# Tüm değişiklikleri ekle (silinen dahil)
git add -A
```

### Staging'den Çıkarma
```bash
# Dosyayı staging'den çıkar (değişikliği koru)
git reset HEAD dosya.txt
git restore --staged dosya.txt

# Tüm dosyaları staging'den çıkar
git reset HEAD
```

### Dosya Silme
```bash
# Dosyayı sil ve staging'e ekle
git rm dosya.txt

# Sadece Git'ten sil (dosyayı koruyarak)
git rm --cached dosya.txt

# Klasörü sil
git rm -r klasor/

# Zorla sil
git rm -f dosya.txt
```

### Dosya Taşıma/Yeniden Adlandırma
```bash
# Dosyayı taşı veya yeniden adlandır
git mv eski-ad.txt yeni-ad.txt
git mv dosya.txt klasor/dosya.txt
```

### Değişiklikleri Görüntüleme
```bash
# Working directory vs Staging area
git diff

# Staging area vs Son commit
git diff --staged
git diff --cached

# Working directory vs Son commit
git diff HEAD

# İki commit arası fark
git diff commit1 commit2

# Belirli dosya için fark
git diff dosya.txt

# Branch'ler arası fark
git diff main..feature-branch

# Sadece değişen dosya isimlerini göster
git diff --name-only

# İstatistiksel özet
git diff --stat
```

---

## Branch İşlemleri

### Branch Oluşturma
```bash
# Yeni branch oluştur (geçiş yapma)
git branch feature-login

# Yeni branch oluştur ve geç
git checkout -b feature-login
git switch -c feature-login      # Yeni syntax

# Belirli commit'ten branch oluştur
git branch bugfix abc1234
git checkout -b bugfix abc1234

# Remote branch'ten local branch oluştur
git checkout -b feature origin/feature
git checkout --track origin/feature

# Başka branch'ten branch oluştur
git checkout -b new-feature develop
```

### Branch'ler Arası Geçiş
```bash
# Branch'e geç (eski)
git checkout main

# Branch'e geç (yeni)
git switch main

# Önceki branch'e dön
git checkout -
git switch -

# Detached HEAD modunda belirli commit'e geç
git checkout abc1234
```

### Branch Listeleme
```bash
# Local branch'leri listele
git branch

# Remote branch'leri listele
git branch -r

# Tüm branch'leri listele
git branch -a

# Son commit ile listele
git branch -v
git branch -vv  # Upstream bilgisi ile

# Merge edilmiş branch'ler
git branch --merged
git branch --merged main

# Merge edilmemiş branch'ler
git branch --no-merged

# Pattern ile filtrele
git branch --list "feature/*"
```

### Branch Silme
```bash
# Local branch sil (güvenli)
git branch -d feature-login

# Local branch sil (zorla)
git branch -D feature-login

# Remote branch sil
git push origin --delete feature-login
git push origin :feature-login     # Eski syntax

# Birden fazla branch sil
git branch -d branch1 branch2 branch3

# Merge edilmiş tüm branch'leri sil
git branch --merged | grep -v "\*" | grep -v "main" | xargs -n 1 git branch -d
```

### Branch Yeniden Adlandırma
```bash
# Mevcut branch'i yeniden adlandır
git branch -m yeni-isim

# Başka branch'i yeniden adlandır
git branch -m eski-isim yeni-isim

# Remote'u da güncelle
git push origin --delete eski-isim
git push origin -u yeni-isim
```

---

## Commit İşlemleri

### Commit Oluşturma
```bash
# Basit commit
git commit -m "Commit mesajı"

# Detaylı commit mesajı (editör açılır)
git commit

# Add ve commit birlikte (tracked dosyalar için)
git commit -am "Commit mesajı"
git commit -a -m "Commit mesajı"

# Boş commit (değişiklik olmadan)
git commit --allow-empty -m "Empty commit"

# Son commit'i değiştir (mesaj değiştir)
git commit --amend -m "Yeni mesaj"

# Son commit'e dosya ekle (mesaj değişmeden)
git commit --amend --no-edit

# Commit tarihini değiştir
git commit --date="2024-01-01 12:00:00" -m "Mesaj"

# Farklı author ile commit
git commit --author="İsim <email@example.com>" -m "Mesaj"
```

### Commit Geçmişi
```bash
# Commit geçmişi
git log

# Tek satırda
git log --oneline

# Grafik ile
git log --graph
git log --oneline --graph --all

# Son N commit
git log -n 5
git log -5

# Belirli tarih aralığı
git log --since="2024-01-01"
git log --after="2 weeks ago"
git log --until="2024-12-31"
git log --before="yesterday"

# Belirli author'un commit'leri
git log --author="İsim"

# Commit mesajında ara
git log --grep="bugfix"

# Belirli dosyanın geçmişi
git log -- dosya.txt
git log -p dosya.txt  # Değişikliklerle birlikte

# İstatistiklerle
git log --stat

# Kısa özet
git log --oneline --graph --decorate --all

# Pretty format
git log --pretty=format:"%h - %an, %ar : %s"
```

### Commit Detayları
```bash
# Son commit detayları
git show

# Belirli commit detayları
git show abc1234

# Belirli dosyanın belirli commit'teki hali
git show abc1234:dosya.txt

# Commit'teki değişiklikleri göster
git show --stat abc1234
```

---

## Remote İşlemleri

### Remote Repository Yönetimi
```bash
# Remote'ları listele
git remote
git remote -v        # URL'lerle birlikte

# Remote ekle
git remote add origin https://github.com/kullanici/repo.git

# Remote URL'ini değiştir
git remote set-url origin https://github.com/kullanici/yeni-repo.git

# Remote'u yeniden adlandır
git remote rename origin upstream

# Remote'u sil
git remote remove origin
git remote rm origin

# Remote detaylarını göster
git remote show origin
```

### Push (Gönderme)
```bash
# Mevcut branch'i push et
git push

# İlk kez push et ve upstream ayarla
git push -u origin main
git push --set-upstream origin main

# Belirli branch'i push et
git push origin feature-login

# Tüm branch'leri push et
git push --all

# Tag'leri push et
git push --tags

# Zorla push (DİKKATLİ!)
git push -f
git push --force

# Güvenli zorla push
git push --force-with-lease

# Remote branch'i sil
git push origin --delete feature-login

# Dry run (ne olacağını göster, push etme)
git push --dry-run
```

### Fetch (Çekme - Merge Etmeden)
```bash
# Tüm remote değişikliklerini çek
git fetch

# Belirli remote'tan çek
git fetch origin

# Belirli branch'i çek
git fetch origin main

# Tüm remote'ları çek
git fetch --all

# Silinen remote branch'leri temizle
git fetch --prune
git fetch -p

# Tag'leri de çek
git fetch --tags
```

### Pull (Çekme ve Merge)
```bash
# Fetch + Merge
git pull

# Belirli remote ve branch'ten pull
git pull origin main

# Rebase ile pull
git pull --rebase
git pull -r

# Otomatik commit oluşturma (fast-forward değilse)
git pull --no-commit

# Sadece fast-forward ise pull et
git pull --ff-only

# Tüm remote'lardan pull
git pull --all
```

---

## Merge ve Rebase

### Merge
```bash
# Branch'i mevcut branch'e merge et
git merge feature-branch

# Fast-forward merge (mümkünse)
git merge feature-branch

# Her zaman merge commit oluştur
git merge --no-ff feature-branch

# Sadece fast-forward yapılabilirse merge et
git merge --ff-only feature-branch

# Merge ama commit etme
git merge --no-commit feature-branch

# Merge'i iptal et
git merge --abort

# Squash merge (tek commit'e sıkıştır)
git merge --squash feature-branch

# Merge stratejisi belirt
git merge -s recursive feature-branch
git merge -X theirs feature-branch    # Conflict'te onların versiyonunu al
git merge -X ours feature-branch      # Conflict'te bizim versiyonumuzu al
```

### Rebase
```bash
# Mevcut branch'i başka branch üzerine rebase et
git rebase main

# Interactive rebase (son 3 commit)
git rebase -i HEAD~3

# Belirli commit'ten itibaren rebase
git rebase -i abc1234

# Rebase'i devam ettir (conflict sonrası)
git rebase --continue

# Rebase'i iptal et
git rebase --abort

# Mevcut commit'i atla
git rebase --skip

# Onto kullanarak rebase
git rebase --onto main develop feature
```

### Interactive Rebase Komutları
```bash
# Editörde kullanılan komutlar:
# pick   = commit'i kullan
# reword = commit'i kullan ama mesajı değiştir
# edit   = commit'i kullan, düzenleme için dur
# squash = commit'i öncekiyle birleştir
# fixup  = squash gibi ama commit mesajını at
# drop   = commit'i at
# exec   = shell komutu çalıştır

# Örnek:
git rebase -i HEAD~3
# pick abc123 First commit
# squash def456 Second commit
# reword ghi789 Third commit
```

### Cherry-Pick
```bash
# Belirli commit'i mevcut branch'e kopyala
git cherry-pick abc1234

# Birden fazla commit
git cherry-pick abc1234 def5678

# Commit aralığı
git cherry-pick abc1234..def5678

# Cherry-pick ama commit etme
git cherry-pick -n abc1234
git cherry-pick --no-commit abc1234

# İptal et
git cherry-pick --abort

# Devam ettir
git cherry-pick --continue
```

---

## Değişiklikleri Geri Alma

### Working Directory'deki Değişiklikleri Geri Al
```bash
# Dosyadaki değişiklikleri geri al
git checkout -- dosya.txt
git restore dosya.txt        # Yeni syntax

# Tüm değişiklikleri geri al
git checkout -- .
git restore .

# Belirli commit'teki haline getir
git checkout abc1234 -- dosya.txt
git restore --source=abc1234 dosya.txt
```

### Staging'deki Değişiklikleri Geri Al
```bash
# Dosyayı staging'den çıkar
git reset HEAD dosya.txt
git restore --staged dosya.txt

# Tüm dosyaları staging'den çıkar
git reset HEAD
```

### Commit'leri Geri Alma

#### Reset (Geçmişi Değiştirir)
```bash
# Soft reset (commit'i geri al, değişiklikleri staging'de tut)
git reset --soft HEAD~1

# Mixed reset (varsayılan - commit'i geri al, değişiklikleri working dir'de tut)
git reset HEAD~1
git reset --mixed HEAD~1

# Hard reset (commit ve değişiklikleri tamamen sil)
git reset --hard HEAD~1

# Belirli commit'e reset
git reset --hard abc1234

# Remote'taki haliyle senkronize et
git reset --hard origin/main
```

#### Revert (Yeni Commit Oluşturur)
```bash
# Commit'i geri alan yeni commit oluştur
git revert abc1234

# Birden fazla commit'i revert et
git revert abc1234 def5678

# Merge commit'i revert et
git revert -m 1 abc1234

# Revert ama commit etme
git revert -n abc1234
git revert --no-commit abc1234
```

### Clean (İzlenmeyen Dosyaları Sil)
```bash
# İzlenmeyen dosyaları göster
git clean -n
git clean --dry-run

# İzlenmeyen dosyaları sil
git clean -f
git clean --force

# İzlenmeyen dosya ve klasörleri sil
git clean -fd

# .gitignore'daki dosyaları da sil
git clean -fx

# İnteraktif clean
git clean -i
```

---

## Stash İşlemleri

### Stash Oluşturma
```bash
# Değişiklikleri stash'le
git stash
git stash save

# İsimli stash
git stash save "Çalışma devam ediyor"

# Untracked dosyaları da dahil et
git stash -u
git stash --include-untracked

# Tüm dosyaları stash'le (ignored dahil)
git stash -a
git stash --all

# Sadece staged değişiklikleri stash'le
git stash --staged
```

### Stash Listeleme ve Görüntüleme
```bash
# Stash listesi
git stash list

# Stash detayları
git stash show
git stash show -p           # Patch format
git stash show stash@{0}

# Belirli stash'in içeriği
git stash show -p stash@{2}
```

### Stash Uygulama
```bash
# Son stash'i uygula (stash'te kalır)
git stash apply

# Belirli stash'i uygula
git stash apply stash@{2}

# Son stash'i uygula ve stash'ten sil
git stash pop

# Belirli stash'i uygula ve sil
git stash pop stash@{2}

# Branch oluştur ve stash uygula
git stash branch yeni-branch
git stash branch yeni-branch stash@{1}
```

### Stash Silme
```bash
# Son stash'i sil
git stash drop

# Belirli stash'i sil
git stash drop stash@{2}

# Tüm stash'leri sil
git stash clear
```

---

## Log ve Geçmiş

### Log Görüntüleme
```bash
# Standart log
git log

# Tek satır
git log --oneline

# Grafik görünümü
git log --graph
git log --oneline --graph --all --decorate

# Son N commit
git log -5
git log -n 5

# Detaylı farklar ile
git log -p
git log --patch

# İstatistikler ile
git log --stat

# Kısa özet
git log --shortstat

# Tarih aralığı
git log --since="2 weeks ago"
git log --until="2024-12-31"
git log --after="2024-01-01" --before="2024-06-01"

# Author filtrele
git log --author="İsim"
git log --author="email@example.com"

# Commit mesajında ara
git log --grep="bugfix"
git log --grep="feature" --grep="hotfix" --all-match

# Dosya değişikliklerinde ara
git log -S"function_name"
git log -G"regex_pattern"

# Belirli dosyanın geçmişi
git log -- dosya.txt
git log --follow -- dosya.txt  # Yeniden adlandırmaları takip et

# Branch karşılaştırması
git log main..feature           # feature'da olup main'de olmayan
git log feature..main           # main'de olup feature'da olmayan
git log main...feature          # her ikisinde de olmayan

# Format özelleştirme
git log --pretty=format:"%h %an %ar %s"
git log --pretty=format:"%C(yellow)%h%Creset %s %C(cyan)(%cr)%Creset"
```

### Reflog (Referans Geçmişi)
```bash
# Tüm HEAD hareketlerini göster
git reflog

# Son 10 hareketi göster
git reflog -10

# Branch'e özel reflog
git reflog show feature-branch

# Kaybolmuş commit'i geri getir
git reflog
git checkout abc1234  # reflog'dan bulduğunuz commit
```

### Blame (Satır Bazında Geçmiş)
```bash
# Dosyanın her satırı için son değişikliği göster
git blame dosya.txt

# Belirli satır aralığı
git blame -L 10,20 dosya.txt

# Email ile göster
git blame -e dosya.txt

# Daha kısa format
git blame -s dosya.txt
```

### Bisect (İkili Arama ile Hata Bulma)
```bash
# Bisect başlat
git bisect start

# Mevcut commit'i kötü olarak işaretle
git bisect bad

# İyi çalışan commit'i işaretle
git bisect good abc1234

# Git otomatik olarak ortadaki commit'e geçer
# Test et ve işaretle:
git bisect good   # veya
git bisect bad

# Bitir
git bisect reset
```

---

## Tag İşlemleri

### Tag Oluşturma
```bash
# Lightweight tag
git tag v1.0.0

# Annotated tag (önerilen)
git tag -a v1.0.0 -m "Version 1.0.0 release"

# Belirli commit'e tag
git tag -a v0.9.0 abc1234 -m "Version 0.9.0"

# İmzalı tag (GPG ile)
git tag -s v1.0.0 -m "Signed version 1.0.0"
```

### Tag Listeleme
```bash
# Tüm tag'leri listele
git tag

# Pattern ile filtrele
git tag -l "v1.*"
git tag --list "v1.*"

# Tag detaylarını göster
git show v1.0.0
```

### Tag'leri Push Etme
```bash
# Tek tag push et
git push origin v1.0.0

# Tüm tag'leri push et
git push origin --tags
git push --tags

# Annotated tag'leri push et
git push --follow-tags
```

### Tag Silme
```bash
# Local tag sil
git tag -d v1.0.0
git tag --delete v1.0.0

# Remote tag sil
git push origin --delete v1.0.0
git push origin :refs/tags/v1.0.0
```

### Tag'e Checkout
```bash
# Tag'e geç (detached HEAD)
git checkout v1.0.0

# Tag'ten branch oluştur
git checkout -b version-1.0 v1.0.0
```

---

## Gelişmiş Komutlar

### Submodule
```bash
# Submodule ekle
git submodule add https://github.com/user/repo.git path/to/submodule

# Submodule'leri başlat ve güncelle
git submodule init
git submodule update

# Veya tek komutta
git submodule update --init --recursive

# Tüm submodule'leri güncelle
git submodule update --remote

# Submodule durumunu göster
git submodule status

# Submodule'ü sil
git submodule deinit path/to/submodule
git rm path/to/submodule
```

### Worktree (Çoklu Çalışma Alanları)
```bash
# Yeni worktree oluştur
git worktree add ../yeni-klasor feature-branch

# Worktree'leri listele
git worktree list

# Worktree sil
git worktree remove ../yeni-klasor

# Worktree'yi temizle
git worktree prune
```

### Archive (Arşiv Oluşturma)
```bash
# ZIP arşivi oluştur
git archive --format=zip --output=project.zip HEAD

# TAR arşivi oluştur
git archive --format=tar HEAD | gzip > project.tar.gz

# Belirli branch'ten arşiv
git archive --format=zip --output=project.zip feature-branch

# Belirli klasörü arşivle
git archive --format=zip --output=docs.zip HEAD:docs/
```

### Patch (Yama Dosyaları)
```bash
# Son commit'i patch dosyası olarak kaydet
git format-patch -1 HEAD

# Son 3 commit'i patch olarak kaydet
git format-patch -3

# İki commit arası patch'ler
git format-patch abc1234..def5678

# Patch uygula
git apply patch-dosyasi.patch

# Patch'i commit olarak uygula
git am patch-dosyasi.patch
```

### Bundle (Offline Transfer)
```bash
# Repository bundle'ı oluştur
git bundle create repo.bundle --all

# Bundle'dan klonla
git clone repo.bundle yeni-klasor

# Bundle içeriğini doğrula
git bundle verify repo.bundle

# Bundle'dan pull
git pull repo.bundle main
```

### Filter-Branch / Filter-Repo (Geçmiş Temizleme)
```bash
# Dosyayı tüm geçmişten sil (eski yöntem - kullanımı önerilmez)
git filter-branch --tree-filter 'rm -f hassas-dosya.txt' HEAD

# Daha iyi alternatif: git-filter-repo (harici araç)
git filter-repo --path dosya.txt --invert-paths

# Author email değiştir
git filter-repo --email-callback 'return email.replace(b"eski@email.com", b"yeni@email.com")'
```

### Grep (Kod Arama)
```bash
# Tüm dosyalarda ara
git grep "search_term"

# Satır numarası ile
git grep -n "search_term"

# Kaç kez geçiyor say
git grep -c "search_term"

# Belirli dosya tipinde ara
git grep "search_term" -- "*.js"

# Regex ile ara
git grep -E "pattern[0-9]+"

# Case-insensitive arama
git grep -i "search_term"
```

### Sparse Checkout (Kısmi Checkout)
```bash
# Sparse checkout aktif et
git sparse-checkout init --cone

# Belirli klasörleri seç
git sparse-checkout set src/ docs/

# Listeyi göster
git sparse-checkout list

# Devre dışı bırak
git sparse-checkout disable
```

### Notes (Commit Notları)
```bash
# Commit'e not ekle
git notes add -m "Ek bilgi" abc1234

# Notları göster
git log --show-notes

# Notu düzenle
git notes edit abc1234

# Notu sil
git notes remove abc1234
```

---

## Faydalı Kombinasyonlar

### Hızlı Workflow
```bash
# Hızlı add-commit
git commit -am "Mesaj"

# Son commit'i düzelt ve push et
git commit --amend --no-edit && git push -f

# Yeni branch oluştur, değişiklikleri taşı
git stash
git checkout -b new-feature
git stash pop

# Merge ve branch'i sil
git checkout main
git merge feature-branch
git branch -d feature-branch
git push origin --delete feature-branch

# Remote branch'i track et
git checkout --track origin/feature-branch
```

### Temizlik ve Bakım
```bash
# Tüm merge edilmiş branch'leri temizle
git branch --merged main | grep -v "^\*\|main" | xargs git branch -d

# Remote'ta silinmiş branch'leri temizle
git fetch --prune

# Git garbage collection
git gc

# Aggressive GC (daha fazla optimizasyon)
git gc --aggressive --prune=now

# Repository boyutunu kontrol et
git count-objects -vH

# Reflog expire
git reflog expire --expire=now --all
```

### Karşılaştırma ve Analiz
```bash
# İki branch arasındaki farkları göster
git diff main..feature-branch

# Hangi commit'ler merge edildi
git log --merges

# Hangi commit'ler merge edilmedi
git log --no-merges

# Contributor istatistikleri
git shortlog -sn

# Commit sayısı branch'e göre
git rev-list --count main
```

---

## .gitignore Şablonları

```bash
# Node.js
node_modules/
npm-debug.log
.env

# Python
__pycache__/
*.py[cod]
venv/
.env

# Java
*.class
target/
*.jar

# IDE
.vscode/
.idea/
*.swp
*.swo
.DS_Store

# Logs
*.log
logs/

# Geçici dosyalar
*.tmp
*.bak
*~

# OS
Thumbs.db
.DS_Store
```

---

## Git Alias Örnekleri

### Konfigürasyona Eklenecek Alias'lar
```bash
# Kısa komutlar
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.unstage 'reset HEAD --'

# Log alias'ları
git config --global alias.lg "log --oneline --graph --all --decorate"
git config --global alias.last 'log -1 HEAD'
git config --global alias.hist "log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short"
git config --global alias.today "log --since='midnight' --author='$(git config user.name)' --oneline"

# Diff alias'ları
git config --global alias.df diff
git config --global alias.dc 'diff --cached'

# Branch yönetimi
git config --global alias.branches 'branch -a'
git config --global alias.remotes 'remote -v'
git config --global alias.tags 'tag -l'

# Stash alias'ları
git config --global alias.sl 'stash list'
git config --global alias.sa 'stash apply'
git config --global alias.ss 'stash save'

# Temizlik
git config --global alias.cleanup "!git branch --merged | grep -v '\\*\\|main\\|develop' | xargs -n 1 git branch -d"
git config --global alias.undo 'reset HEAD~1 --mixed'

# Gelişmiş
git config --global alias.sync '!git fetch --all && git pull --rebase'
git config --global alias.amend 'commit --amend --no-edit'
git config --global alias.contributors 'shortlog -sn --all'
```

---

## Güvenlik ve En İyi Pratikler

### Hassas Bilgileri Koruma
```bash
# Bir dosyayı geçmişten tamamen sil
git filter-repo --path hassas-dosya.txt --invert-paths

# .env dosyasını ignore et
echo ".env" >> .gitignore
git rm --cached .env
git commit -m "Remove .env from tracking"

# Git secrets kurulumu (AWS keys vb. için)
git secrets --install
git secrets --register-aws
```

### GPG İmzalama
```bash
# GPG key oluştur
gpg --gen-key

# Git'e GPG key'i tanıt
git config --global user.signingkey YOUR_GPG_KEY_ID

# Otomatik imzalama
git config --global commit.gpgsign true

# İmzalı commit
git commit -S -m "Signed commit"

# İmzalı tag
git tag -s v1.0.0 -m "Signed tag"

# İmzaları doğrula
git verify-commit abc1234
git verify-tag v1.0.0
```

### SSH Kurulumu
```bash
# SSH key oluştur
ssh-keygen -t ed25519 -C "email@example.com"

# SSH agent'a ekle
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# GitHub'a public key'i ekle
cat ~/.ssh/id_ed25519.pub

# SSH bağlantısını test et
ssh -T git@github.com
```

---

## Sorun Giderme

### Yaygın Problemler ve Çözümler

#### Merge Conflict Çözme
```bash
# Conflict olan dosyaları göster
git status

# Conflict marker'ları düzenle
# <<<<<<< HEAD
# Sizin versiyonunuz
# =======
# Onların versiyonu
# >>>>>>> branch-name

# Düzeltilmiş dosyayı stage'e al
git add cakisan-dosya.txt

# Merge'i tamamla