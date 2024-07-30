# v2board 增加第三方客服代码的方法

默认 v2board 是支持第三方客服系统的。支持的是 crisp .但是 我不是用的这个客服系统。

我用的是tawk.to ,如果你没有特别的需求可以考虑继续用 crisp 也挺好用的。我用tawk.to是因为我其他的系统也要用到这个系统。不想挂好几个客服。

![img](https://dalao-1251452305.cos.ap-tokyo.myqcloud.com/2022/07/3345079989.png)

默认不支持。而且后台也不能直接填代码，那就只能自己修改主题了。

找到 public/theme/v2board/dashboard.blade.php 这个文件

在最下面的 body 前面插入 代码即可！！！

非常简单。 但是缺点就是。如果系统更新可能需要重新添加代码。

[![img](https://dalao-1251452305.cos.ap-tokyo.myqcloud.com/2022/07/3170228427.png)](https://dalao-1251452305.cos.ap-tokyo.myqcloud.com/2022/07/3170228427.png)

注意 上面的方法 仅适用于V1.6之前的版本。

V1.6之后的版本 在后台新增了自定义代码的位置。

在后台 主题配置→ 主题设置→ 自定义脚本HTML 这里直接添加代码即可！

[![img](https://dalao-1251452305.cos.ap-tokyo.myqcloud.com/2022/07/998555669.png)](https://dalao-1251452305.cos.ap-tokyo.myqcloud.com/2022/07/998555669.png)

