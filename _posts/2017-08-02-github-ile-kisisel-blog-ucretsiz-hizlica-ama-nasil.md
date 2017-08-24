---
layout: post
date:   2017-08-02
title:  "Github ile kişisel blog; ücretsiz, hızlıca. Ama nasıl?"
tags: [yazılım]
---
# Nasıl mı?

Mesela [http://dogancan.net](http://dogancan.net)
Bu arkadaş her nekadar kendi domaininde yapmış olsada aslında bu adres
[https://dgncan.github.io](https://dgncan.github.io) adresinden yönlendiriliyor. 

Buradaki dgncan github taki hesap ismi

mesela senin github'taki sitenin adresi https://githubismin.github.io gibi olacak

Bu arkadaşınki [https://github.com/dgncan/dgncan.github.io](https://github.com/dgncan/dgncan.github.io)

Burda hazır bir sistem kullanmış. Temaları da var bi sürü ücretsiz. 
Bu sistem ruby de yazılmı [Jekyll](https://jekyllrb.com/) isminde bir sistem.
Temel özelliği statik web siteleri üretmek.
Zaten github sana statik içerik koyabileceğin bir alan veriyor.
Jekyll'ın en güzel özelliği _post klasörünün içine düz metin (markdown formatında) olarak blogun için içeriğini yazıyorsun, jekyll da onu güzelce parse ediyor.

Örnek olarak nasıl bir şey yazıldığını [şurdan](https://raw.githubusercontent.com/dgncan/dgncan.github.io/master/_posts/2017-08-02-github-ile-kisisel-blog-ucretsiz-hizlica-ama-nasil.md) bakabilirsin.

[Şurdan](http://dogancan.net/github-ile-kisisel-blog-ucretsiz-hizlica-ama-nasil) da nasıl göründüğüne bakabilirsin.   

Mesela yazdıklarıma yorum da yazsın birileri istiyorsan. Kendine özel bir şey yapmana gerek yok.
[Disqus](https://disqus.com/) ile entegre edebilirsin. 

Bunun için Disqus tan bir hesap açman gerek. Akabinde jekyll'da bir ayar yapman yeterli.

## Disqus entegrasyonu
Başlık afilli ama pek bi numarası yok.

_includes klasöründe disqus.html diye bir dosya oluştur.
içine şunları yaz:

{% highlight html %}
<div class="comments">
	<div id="disqus_thread"></div>
	<script type="text/javascript">
	    var disqus_shortname = '{{ site.disqus }}';
	    /* ensure that pages with query string get the same discussion */
            var url_parts = window.location.href.split("?");
            var disqus_url = url_parts[0];	    
	    (function() {
	        var dsq = document.createElement('script'); dsq.type = 'text/javascript'; dsq.async = true;
	        dsq.src = '//' + disqus_shortname + '.disqus.com/embed.js';
	        (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(dsq);
	    })();
	</script>
	<noscript>Please enable JavaScript to view the <a href="http://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
</div>
{% endhighlight %}


_config.yml de

disqus: "dogancanblog"

Not: "dogancanblog" yazan yerlere kendi disqus hesabını yazmalısın.

## Google Analytics entegrasyonu


_includes klasörüne google_analytics.html adında bir dosya oluştur.
içine:
{% highlight html %}
<script>
	(function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
	(i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
	m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
	})(window,document,'script','//www.google-analytics.com/analytics.js','ga');
	ga('create', '{{ site.google_analytics }}', 'auto');
	ga('send', 'pageview');
</script>
{% endhighlight %}

_config.yml de

google_analytics: UA-49928671-1

Not: "UA-49928671-1" yazan yere kendi analytics kodunu yazmalısın.


