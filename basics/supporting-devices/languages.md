# 适配不同的语言和文化

> 编写:[CrazyPudding](http://github.com/CrazyPudding) - 原文:<http://developer.android.com/training/basics/supporting-devices/languages.html>

在 APP 中，我们可以根据使用环境的不同提供特定的资源文件。例如，当一个 App 中包含不同语言的字符串资源，那它将根据系统当前的语言环境切换使用相应的字符串资源。将特定文化的资源文件与 App 的其他部分进行分离是一种很好的习惯。 Android 会根据系统当前的设置自动切换相应的语言或文化资源文件，我们可以通过 Android 项目中的资源目录为不同的区域设置进行适配。

我们可以根据用户不同的文化习惯提供相应的资源文件。无论哪种[资源类型]，我们都能为用户提供适应他们语言文化的资源文件。例如，下图分别展示了设备在默认的地区设置（*en_US*）和地区设置为西班牙（*es_ES*）时同一个 app 显示的图形和文字：

![languages_01][figure_languages_01]

**图 1.** App 根据当前的地区设置使用不同的资源文件

如果使用 Android SDK Tools 创建了项目（详见[创建 Android 项目]），在项目的根目录会创建一个 `res/` 目录，该目录中可以创建包含各种资源类型的子目录。目前在该目录中有一些默认的文件比如 `res/values/strings.xml`，这个 `strings.xml` 文件用于保存字符串的值。

适配不同的语言不仅仅指使用特定语言的资源文件。部分用户会选择具有从右到左（RTL）的阅读习惯的语言作为 UI 的显示，例如阿拉伯语和希伯来语。而其他用户会选择从左到右（LTR）阅读习惯的语言作为 UI 的显示，例如英语。为了保证适配这两种阅读习惯，我们需要注意以下事项：

* 为 RTL 的语言设计 RTL 的布局
* 检测并声明格式化消息中文本数据的显示方向。通常只要调用一个方法就能决定文本数据的显示方向。

## 创建地区目录和资源文件

为了使 app 支持多个地区的文化习惯，在 `res/` 目录下新增相应的子目录，每一个子目录的命名应遵循以下的格式：

    <resource type>-b+<language code>[+<country code>]
    
例如， `values-b+es/` 目录中可以放置语言代码为 *es* 的字符串资源。同样的， `mipmap-b+es+ES/` 目录中可以放置语言代码为 *es*、城市代码为 *ES* 的图标文件。Android 在运行时会根据当前设备的地区设置加载相应的资源文件。想了解更多信息，请查看[提供可替代资源（Providing Alternative Resources）]。

一旦确定了需要支持的地区，可以创建相应的资源子目录和文件。例如：

```
MyProject/
    res/
       values/
           strings.xml
       values-b+es/
           strings.xml
       mipmap/
           country_flag.png
       mipmap-b+es+ES/
           country_flag.png
```

以下是一些不同语言环境中的资源文件：

 `/values/strings.xml` 表示英语中的字符串（默认的地区设置）：
 
```XML
<resources>
    <string name="hello_world">Hello World!</string>
</resources>
```

`/values-es/strings.xml` 表示西班牙语种的字符串（地区代码为 *es*）：

```XML
<resources>
    <string name="hello_world">¡Hola Mundo!</string>
</resources>
```

`/mipmap/country_flag.png` 表示地区为美国时显示美国国旗（默认的地区设置）

![languages_us_flag][figure_languages_us_flag]

**图 2.** 默认地区设置（en_US）时使用的图标

`/mipmap-b+es+ES/country_flag.png` 表示地区设置为西班牙（es_ES）时显示西班牙国旗

![languages_es_flag][figure_languages_es_flag]

**图 3.** 地区设置为 *es_ES* 是使用的图标

> **注意：** 你可以对任何一种资源类型使用地区限定符或任何配置限定符，例如可以提供 drawable 类型的本地化版本。想了解更多，请参考[本地化（Localization）]。

## 在 App 中使用资源

我们可以在源代码或 XML 文件中使用任何一个资源文件的 *name* 标签来引用相应的资源。

在源代码中，我们可以使用这个语法来引用资源： `R.<resource type>.<resource name>`。有很多方法能以这种方式引用资源。例如：

```java
// 从 app 的资源中获取字符串资源
String hello = getResources().getString(R.string.hello_world);

// 或者将一个字符串资源传入以字符串为参数的方法中
TextView textView = new TextView(this);
textView.setText(R.string.hello_world);
```

在 XML 文件中，只要 XML 属性接受相关的值，我们可以使用这个语法来引用资源： `@<resource type>/<resource name>`。例如：

```XML
<ImageView
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:src="@mipmap/country_flag" />
```


[资源类型]:    //developer.android.com/guide/topics/resources/available-resources.html
[创建 Android 项目]:    ../../basics/firstapp/creating-project.html
[提供可替代资源（Providing Alternative Resources）]:    //developer.android.com/guide/topics/resources/providing-resources.html#AlternativeResources
[本地化（Localization）]:    //developer.android.com/guide/topics/resources/localization.html

[figure_languages_01]:         ./languages_01.png
[figure_languages_us_flag]:    ./languages_us_flag.png
[figure_languages_es_flag]:    ./languages_es_flag.png


## 使用字符资源

我们可以在源代码和其他XML文件中通过`<string>`元素的`name`属性来引用自己的字符串资源。

在源代码中可以通过`R.string.<string_name>`语法来引用一个字符串资源，很多方法都可以通过这种方式来接受字符串。

例如:

```java
// Get a string resource from your app's Resources
String hello = getResources().getString(R.string.hello_world);

// Or supply a string resource to a method that requires a string
TextView textView = new TextView(this);
textView.setText(R.string.hello_world);
```

在其他XML文件中，每当XML属性要接受一个字符串值时，你都可以通过`@string/<string_name>`语法来引用字符串资源。

例如:

```xml
<TextView
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@string/hello_world" />
```
