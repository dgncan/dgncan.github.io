---
layout: post
title:  "Bir nesnenin fonksiyon gibi çağırılmasının gereği nedir?"
date:   2017-06-20 00:00:00 +0300
tags:  [development,php]
---
# PHP'de __invoke() magic metoduna neden ihtiyaç var?

php 5.3 ten bu yana magic metotlara eklenen __invoke() nesneyi fonksiyonmuş gibi çağırmamızı sağlıyor.
Bir nevi default fonksiyon. 
Hemen basit bir örnek yapıldığında gördüğümüz şu

{% highlight ruby %}

class Siparis {
    public function __invoke() {
        echo "sipariş geçilecek";
    }
    public function run() {
        echo "run oldu";
    }
}

$siparis = new Siparis();
$siparis();

OUTPUT
#sipariş geçilecek

{% endhighlight %}


Ee ne anladık? Büyük bir sihir yok sanki. Şöyle de yapabilirdik
{% highlight ruby %}
class Siparis {
    public function gec() {
        echo "sipariş geçilecek";
    }
}

$siparis = new Siparis();
$siparis->gec();
{% endhighlight %}

__construct() metodunnda yapabilir miydik? Evet ama new eder etmez (instance olur olmaz) sipariş geçilmesini istemeyebiliriz. Çok münasip bir yer değil. Sipariş geçmek Sipariş classının bir eylemi olmalı. Sipariş nesnesinin kurulumu ayrı bir olay.

__invoke metodu olmayan nesneyi yukarıdaki gibi örneği $siparis() şeklinde çalıştırırsak ne olur?
{% highlight ruby %}
class Siparis {
  public function gec() {
  	echo "sipariş geçilecek";
  }
}
$siparis = new Siparis();
$siparis();

OUTPUT:
PHP Fatal error:  Function name must be a string
{% endhighlight %}


Neden nesnemizi $siparis() şeklinde kullanırıza gelmeden önce bu hatayı almamak için 
ya da bir nesnenin __invoke() metodu olup olmadığını denetlemek için 
php nin [is_callable()](http://php.net/manual/en/function.is-callable.php) fonksiyonu yardımımıza koşuyor.

{% highlight ruby %}
class Siparis {
  public function gec() {
  	echo "sipariş geçilecek";
  }
}
$siparis = new Siparis();
if (is_callable($siparis)) {
	$siparis();
} else {
	echo "callable değil";
}
# ya da böyle de kontrol edebiliriz. eğer direkt ilgili metodu çağıracaksak
if (is_callable([$siparis, 'gec']) {
    $siparis->gec();
}
{% endhighlight %}

Eveet, şimdi gelelim bu __invoke ne işe yarayacak/yarıyor? konusuna
invoke metoduna sahip bir Siparis classımız var yine. Buna ek olarak Container isimli bir classımız daha var. Bunu görevi gere

{% highlight ruby %}
class Siparis {
    public function __invoke() {
        echo "sipariş geçilecek\n";
    }
    public function gec() {
        echo "geç bakalım oldu\n";
    }
}

class Container {
    public function __invoke($service)
    {
        return new $service;
    }
}

$container = new Container();
$siparis = $container('Siparis');
{% endhighlight %}


iki temel dependecy injection türü bulunur. __construct olarak bilinen yapılandırıcı fonksiyona parametre geçmek odaklı ve methot çağırımı olarak. (getter , setter)




