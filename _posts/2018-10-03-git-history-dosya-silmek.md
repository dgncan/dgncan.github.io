---
layout: post
date:   2018-10-03
title:  "Git History - Dosya Silmek"
tags: [yazılım]
---
# Git Reponuzun Commit Geçmişinden Dosya Silmek

Bazen hassas bilgilerin ya da büyük boyutlu dosyaların git commit history den silinmesini isteyebiliriz.
Bunun için Git'in filter-branch komutu var. Fakat bu işi daha pratik, hızlı ve raporlayarak çözen bir araç var.

BFG Repo-Cleaner
https://rtyley.github.io/bfg-repo-cleaner/

Jar dosyasını indir. İlgili repoya git.

# İki Örnek
* Dosya silmek:

    java -jar ~/Downloads/bfg-1.13.0.jar --delete-files gizli-sifrelerim.txt
    
* 100M den Büyük dosyaları tespit edip silmek:

    java -jar ~/Downloads/bfg-1.13.0.jar --strip-blobs-bigger-than 100M

* Bu komutlardan sonra ufak birkaç işimiz daha var
    * git reflog expire --expire=now --all && git gc --prune=now --aggressive
    * git push -f

* Bu repoyu çekenlerde de bu işlemin etkili olması için:
    * git fetch --all
    * git reset --hard origin/master    
    
# Uyarı
   
   * Tüm history yeniden yazılıyor. Tüm commit numaraları değişiyor. Buna göre birşeyin varsa dikkat.
   * reflog temizliği ve force push etmezsen kendi kendine bir iş yapmış olursun.
   * Repoyu daha önce pull etmiş kişiler sadece git pull diyerek yeni durumu alamaz. Fetch ve reset adımlarını yapmalılar.
   * Master dışında uzak dalların varsa (remote branch, genellikle origin dediğimiz) onlarda da hard reset ve force işlemi yapmalısın.
   * Uzak Tag ler için de aynı şey geçerli. Uzak tagleri silip tekrar push etmek gerekir. 
        * git push origin :refs/tags/0.2.5
        * git push --tags
   
# Linkler
   * https://help.github.com/articles/removing-sensitive-data-from-a-repository/  
   * https://stackoverflow.com/questions/1125968/how-do-i-force-git-pull-to-overwrite-local-files
