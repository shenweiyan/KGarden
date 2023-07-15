打开 diqus 官网：[http://disqus.com](http://disqus.com/)，点击 "GET STARTED" 进入注册页面：
![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1567129486337-5a6bb0e9-7ad0-400d-9bbb-6362a44d8bba.png#align=left&display=inline&height=544&originHeight=544&originWidth=773&size=87116&status=done&width=773)

选择 "I want to install Disqus on my site"，进入"Create" 创建页面：
![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1567129711741-33a8fb63-56ca-42b3-80ee-a10626e2e344.png#align=left&display=inline&height=544&originHeight=544&originWidth=768&size=174049&status=done&width=768)

在创建页面填写网站信息：
![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1567131538176-6f483e24-15d8-4be0-b102-4a22f5ca758d.png#align=left&display=inline&height=544&originHeight=544&originWidth=768&size=72813&status=done&width=768)

在 "Choose a plan" 页面，拉到最下面，选择免费的 "Basic" 版本，点击 "Subscribe Now"：
![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1567131764891-262017a4-54f7-44c7-9844-38815debc193.png#align=left&display=inline&height=436&originHeight=436&originWidth=768&size=45567&status=done&width=768)

在 "Choose a platform" 选择博客的平台类型，对于 hugo 我们选择页面最下面的 "Universal Code"：
![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1567132017554-2c392abb-bce4-49aa-9ee3-aede20e7e4f3.png#align=left&display=inline&height=436&originHeight=436&originWidth=768&size=56672&status=done&width=768)

把 "Install instructions for Universal Code - Disqus Admin" 页面的代码拷贝到 hugo 博客的  layouts/partials/disqus.html 文件：
![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1567132211441-46b9fe46-e2dc-4d89-8f2b-9f729fc8a6e6.png#align=left&display=inline&height=436&originHeight=436&originWidth=768&size=56370&status=done&width=768)

```html
<div id="disqus_thread"></div>
<script>
  /**
   *  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
   *  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/
  /*
var disqus_config = function () {
this.page.url = "{{ .Permalink }}";  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = "{{ .Permalink }}"; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};
*/
  (function () {
    // DON'T EDIT BELOW THIS LINE
    var d = document,
      s = d.createElement("script");
    s.src = "https://shen-hugo-blog.disqus.com/embed.js";
    s.setAttribute("data-timestamp", +new Date());
    (d.head || d.body).appendChild(s);
  })();
</script>
<noscript
  >Please enable JavaScript to view the
  <a href="https://disqus.com/?ref_noscript"
    >comments powered by Disqus.</a
  ></noscript
>
```

最后，在 "Install instructions for Universal Code - Disqus Admin" 页面拉到最下面，点击 "Configure"：
![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1567133311537-3eac4d8f-8810-4a05-a60e-3137f528a29a.png#align=left&display=inline&height=436&originHeight=436&originWidth=768&size=52227&status=done&width=768)

在 "Configure" 页面填写完善网站的相关信息，点击 "Complete Setup" 完成设置。
![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1567133500623-f9a94853-b118-4afb-b061-0c03b622722c.png#align=left&display=inline&height=728&originHeight=728&originWidth=768&size=86458&status=done&width=768)

在 hugo 博客的  config.toml 配置文件中增加 `disqusShortname`  参数：
![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1567133698291-e3f12bf1-5bb9-4dfd-ab81-2702d843e5b6.png#align=left&display=inline&height=307&originHeight=307&originWidth=851&size=36383&status=done&width=851)

在 hugo 博客  layouts 目录下的模板中引入 `disqus.html`  模板：

```go
{{ partial "disqus.html" . }}
```

最后，执行 hugo，重新渲染生成所有的静态博客文件，push 到 github pages，完成所有操作。
![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1567134788302-d07a70d3-dc3a-4102-a408-f88e2cffff08.png#align=left&display=inline&height=379&originHeight=379&originWidth=682&size=30797&status=done&width=682)
