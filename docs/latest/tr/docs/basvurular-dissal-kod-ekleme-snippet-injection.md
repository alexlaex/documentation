---
id: basvurular-dissal-kod-ekleme-snippet-injection
title: Dışsal Kod Ekleme (Snippet Injection)
sidebar_label: Dışsal Kod Ekleme
---

<a id="aHeaderMenuAnchor" data-header-menu="Docs"></a>

Kaynak kod üreteçleri tarafından üretilen kod içine bazı durumlarda özel kodlar eklemek isteyebilirsiniz. Ya da üreteçler tarafından üretilen bir kod bloğunu etkisiz hale getirerek kendi kod bloğunuzu kaynak koda eklemek isteyebilirsiniz. Bu gibi durumlarda, Dışsal Kod Ekleme (Snippet Injection) yöntemi pratik bir çözüm sunmaktadır.

Pyronome kaynak kod üreteçleri tarafından kaynak kod üretim sürecinin hemen ardından otomatik olarak Dışsal Kod Ekleme (Snippet Injection) işlemi çalıştırılmakta; gerekli yerlere dışsal kodlar eklenmektedir.

Dışsal kod ekleme sistemi iki bölümden oluşur:
1. Dışsal Kod Belirteci
2. `snippets` Dizin Yapısı

Dışsal Kod Belirteci, kalıp şablonları ya da statik dosyalar içine yerleştirilen `{{@snippet:` ile başlayan belirteçlerdir. Bu belirteçlerin bulunduğu alanlara, dizin yapısı göz önünde bulundurularak, kod ekleme işlemi yapılmaktadır. `snippets` Dizin Yapısı, Dışsal Kod Belirteci ile belirtilen bölümlere kaynak kodun nasıl ekleneceğini belirleyen özel isimlendirmeye ve içeriğe sahip dosya ve dizinlerdir.

## Dışsal Kod Belirteci

Dışsal kodun, kaynak kod dosyası içinde tam olarak nereye ekleneceğini dışsal kod belirteçleri belirlemektedir. Üretilen dosyalar ya da statik dosyalar içerisine bu dosyayı oluşturan kullanıcılar tarafından dışsal kod eklenebilecek bölümler belirlenir ve bu bölümlere `{{@snippet:` ile başlayan belirteçler eklenir. Böylelikle kaynak kod üretim işlemi sonrasında çalıştırılan dışsal kod ekleme işlemi hangi kodun nereye ekleneceğini bulup; kod ekleme işlemini gerçekleştirir.

**Örneğin;**

```php
<?php

namespace App\Providers;

use Illuminate\Foundation\Support\Providers\RouteServiceProvider as ServiceProvider;
use Illuminate\Support\Facades\Route;
use App\Http\Middleware\HTMLDBMiddleware;
use App\Http\Middleware\AdminLTEMiddleware;

class RouteServiceProvider extends ServiceProvider
{

    /* {{@snippet:begin_methods}} */

    /**
     * This namespace is applied to your controller routes.
     *
     * In addition, it is set as the URL generator's root namespace.
     *
     * @var string
     */
    protected $namespace = 'App\Http\Controllers';
```

Yukarıdaki kaynak kod içinde, dışsal kod belirteci, `{{@snippet:begin_methods}}` şeklinde kullanılmıştır. Burada `begin_methods` dışsal kod bloğunun adını belirtmektedir.

<div class="panelize-infobox infobox-info">
    <p>
        <strong><i class="fas fa-info-circle"></i> Not:</strong>
    </p>
    <p>Kaynak kod dosyasının türüne göre, dışsal kod belirtecinin başına ve/veya sonuna yorum bloğu açma/kapama karakterleri eklenebilir. Dışsal kaynak kod ekleme sürecinde, dışsal kod belirteci geçen satır TAMAMEN SİLİNİR, dışsal kod bloğu bu satıra yazılır.</p>
</div>

## `snippets` Dizin Yapısı

Pyronome kaynak kod üreteçleri tarafından üretilen kaynak kodun aşağıdakine benzer bir dizin yapısı bulunmaktadır:

<pre>
├── pyronome
├── <span class="yellow-text">snippets</span>
└── <span class="white-text">source</span>
</pre>

Oluşturulan kaynak kod içindeki `snippets` dizininin içinde de `system` ve `user` isimli iki dizin daha bulunmaktadır:

<pre>
├── pyronome
├── snippets
│   ├── <span class="yellow-text">system</span>
│   └── <span class="yellow-text">user</span>
└── source
</pre>