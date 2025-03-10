---
nav_title: Notifications push HTML
article_title: Notifications push HTML pour Android
platform: Android
page_order: 6
description: "Cet article explique comment implémenter des notifications push HTML dans votre application Android."
channel:
  - notification push

---

# Notifications push HTML

Dans le SDK Braze version 3.1.1, du HTML peut être envoyé à un appareil pour afficher du texte multicolore dans les notifications push.

![Un message de notification push Android « Test de notification push multicolore », où les lettres sont de couleurs différentes, en italique et avec une couleur de fond][1]{: style="max-width:40%;"}

Cet exemple est affiché avec le code HTML suivant :

```html
<p><span style="color: #99cc00;">M</span>u<span style="color: #008080;">lti</span>Colo<span style="color: #ff6600;">r</span> <span style="color: #000080;">P</span><span style="color: #00ccff;">u</span><span style="color: #ff0000;">s</span><span style="color: #808080;">h</span></p>
```

```html
<p><em>message</em> <span style="text-decoration: underline; background-color: #ff6600;"><strong>test</strong></span></p>
```

Les systèmes d’exploitation Android limitent les éléments HTML ou balises qui sont valides pour les notifications push. Par exemple, `marquee` n’est pas autorisé.

{% alert important %}
Veuillez noter que le rendu de texte multicolore est spécifique à l’appareil et peut ne pas s’afficher sur un appareil ou une version d’Android.
{% endalert %}

## Implémentation

Pour afficher du texte multicolore dans une notification push, soit :

Ajoutez ce qui suit dans votre `braze.xml` :

```xml
<bool translatable="false" name="com_braze_push_notification_html_rendering_enabled">true</bool>
```

**OU** 

Ajoutez ce qui suit dans votre [`BrazeConfig`][2]:

{% tabs %}
{% tab JAVA %}

```java
BrazeConfig brazeConfig = new BrazeConfig.Builder()
  .setPushHtmlRenderingEnabled(true)
  .build();
Braze.configure(this, brazeConfig);
```
 
{% endtab %}
{% tab KOTLIN %}

```kotlin
val brazeConfig = BrazeConfig.Builder()
    .setPushHtmlRenderingEnabled(true)
    .build()
Braze.configure(this, brazeConfig)
```

{% endtab %}
{% endtabs %}

[1]: {% image_buster /assets/img/multicolor_android_push.png %}
[2]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/advanced_use_cases/runtime_configuration/#runtime-configuration
