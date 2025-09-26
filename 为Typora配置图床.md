# 为 Typora 配置图床

本文使用的是 Typora 自带的 PicGo-Core(command-line)。

## 配置过程

配置项路径：文件-偏好设置-图像

设置“插入图片时…”这个选项为”上传图片“，“上传服务”选择“PicGo-Core(command line)”，选择完成之后点击下载或更新。等待这个过程结束之后点击“打开配置文件”，配置文件如下：

```json
{
  "picBed": {
    "current": "aliyun",
    "aliyun": {
        "accessKeyId": "LTAI5t9XEs9h2Nt39sW9JLyv",
        "accessKeySecret": "m8RTZoYZVhYkce0IC5YSAuhvY0tNSl",
        "bucket": "kat-tc",
        "area": "oss-cn-chengdu"
    }
  },
  "picgoPlugins": {}
}
```

配置完成后点击”验证图片上传选项“判断是否成功。

## 配置项解析

配置项解析：

* `current`：当前使用的图床类型。
