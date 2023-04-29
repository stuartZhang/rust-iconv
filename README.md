# rust-iconv

`iconv`链接库的`Rust`绑定。

## 向`Cargo.toml`导入依赖

因为已经与`crates.io`上现有的包重名，所以请直接添加`git`依赖：

```toml
iconv-sys = { git = "https://github.com/stuartZhang/rust-iconv", tag = "0.1.2" }
```

## 链接库依赖

简单地讲，需要在操作系统内预置`libiconv`动态链接库。

### `Linux`操作系统

大部分主流`Linux OS`都包含有`libiconv`。若你的`Linux OS`版本比较早或是`compact`版而缺失了`libiconv`也不必慌。按如下方式补装即可：

```shell
wget http://ftp.gnu.org/pub/gnu/libiconv/libiconv-1.9.1.tar.gz
tar -xzvf libiconv-1.9.1.tar.gz
cd libiconv-1.9.1.tar.gz
./configure --prefix=/usr/local
sudo make -j8
sudo make install
sudo ln -s /usr/local/lib/libiconv.so /usr/lib/libiconv.so
sudo ln -s /usr/local/lib/libiconv.so.2 /usr/lib/libiconv.so.2
```

### `Windows`操作系统

要么，从[setup](https://sourceforge.net/projects/gnuwin32/files/libiconv/1.9.2-1/libiconv-1.9.2-1.exe/download?use_mirror=jaist&download=)直接下载安装包，并本地安装之。缺点就是会“污染”你的`PATH`环境变量。

要么，从[binary](https://sourceforge.net/projects/gnuwin32/files/libiconv/1.9.2-1/libiconv-1.9.2-1-bin.zip/download?use_mirror=jaist&download=)下载预编译包。在解压缩之后，将其下的`bin`目录添加到你的编译环境变量`RUST_FLAGS`内。比如，

```shell
set RUST_FLAGS=-L C:\libiconv-1.9.2-1-bin\bin
```

## 吐槽

同一款`libiconv`链接库怎么对`Linux`与`Windows`操作系统提供了**不同名**的导出函数呢？这个“缺德的”命名差异导致我在【编译期·动态链接】环节卡住了好几天。相对于`Linux`版的链接库导出函数名，`Windows`版的每个导出函数都有一个`lib`前缀 —— 故意的吧？真要命。
