#本项目是站在前人的肩膀上修改而来

# 音频转码工具

本工具主要用于将微信语音 amr 格式转换为 mp3 格式以便在 html5 的 audio 标签中进行播放。

支持 Linux/Windows/Mac 平台

因为是基于 JAVE 项目的修改，而 JAVE 是依赖 [ffmpeg](http://ffmpeg.org/) 所以可以适用于所有 ffmpeg 所支持的文件格式的转换。具体可以查看 [JAVE 官方文档](http://www.sauronsoftware.it/projects/jave/manual.php)


## 调用 AudioUtils.amrToMp3 方法
```java
public void amrToMp3()  {
    File source = new File("target/test-classes/material/testAudio.amr");
    File target = new File("testAudio.mp3");
    AudioUtils.amrToMp3(source, target);
}
```

# 原理

1. 初始化时判断当前运行环境，将bin目录中对应的 ffmpeg 可执行文件拷贝到临时目录中
2. 根据文件类型及配置通过 Runtime.getRuntime().exec(cmd) 执行 ffmpeg 对应的转码命令

# JAVE 项目的问题

ffmpeg 是依赖运行环境的，JAVE 项目封装了ffmpeg，它通过上述的原理使 java 可以调用ffmpeg而且支持跨平台。

1. 项目老旧没再维护。官网最近版本是2009年发布的，其依赖的ffmpeg早已过时，很多情况下用不了。
2. 转码一直报异常 EncoderException: Stream mapping 
3. 没有发布maven仓库，而且 JAVE 本身也不是一个maven项目
4. 不支持mac

# 本项目特点

本项目为解决上述问题而生。

* 项目依赖的 ffmpeg 可执行文件经过验证可以使用（单元测试中提供了一个简单的检验方法）
* 解决了amr转mp3出现的 EncoderException: Stream mapping 
* 支持 Linux/Windows/Mac 平台

# 扩展

如果程序无法通过拷贝资源文件的方式获取到 ffmpeg 的可执行文件或者内置的 ffmpeg 不支持你所使用的操作系统

你可以通过环境变量或者在 java 中设置 `System.setProperty("ffmpeg.home", "ffmpeg可执行文件所在的目录")` 的方式指定你的系统中安装的可用的 ffmpeg 文件的目录

如 `System.setProperty("ffmpeg.home", "/usr/local/bin/")`
 
# 参考

本工具修改了 https://github.com/dadiyang的源码，在windows上不能成功运行的bug
本工具使用 [JAVE](http://www.sauronsoftware.it/projects/jave/download.php) 源码改造而来



