# codestyle
代码风格和细节


# 代码风格要求
- 用4个空格代替tab键
- php的代码对齐
- 使用vim的 = 格式化对齐代码
- 使用 retab 去除tab键
- 使用如下方法配置vim


## vim配置方法

简易安装 vim 

```
wget -qO- https://raw.github.com/ma6174/vim/master/setup.sh | sh -x
```

在 .vimrc 中增加下文，显示tab键和空格
```
set list
set listchars=tab:>-,trail:-
```

将编码改成GBK,与产品机编码一致
```
set langmenu=zh_CN.GBK
```

## gitignore

将不需要跟踪的代码都放入 .gitignore 中，在仓库中已经提供了基本的方法
```
https://github.com/github/gitignore.git
```
对于使用cmake时只要增加build，cmake产生的中间文件都放在build文件中。

## cmake 模板

对于要编译的文件，提供一个cmake文件，可以直接编译使用


## 技术方案和测试

如果项目需要提供技术方案和测试代码，则与代码放在同一个目录中。
技术方案可以放在READMD中

- 在动手之前想好你的技术方案，需要考虑这种方案可不可行。想好再动手，才能真正的节省时间
- 写完技术方案的时候要写好测试示例，你要测试什么东西，你的测试是不是完备的。在开发阶段才不会迷失。


