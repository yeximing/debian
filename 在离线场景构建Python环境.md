`pip`有一个很好用的命令，`pip download`，可以下载Python包及其依赖项，而不安装它们。这在需要离线安装包或准备部署环境时非常有用。

```shell
pip download <package_name>==<version>
```
这个命令会从 Python Package Index (PyPI) 下载指定的包以及它的依赖项，并将它们存储在当前目录中。下载的文件通常是 .whl（wheel 文件）或 .tar.gz（源代码压缩包）格式。

可以使用`-d`选项指定文件夹，不需要依赖的话，可以用`--no-deps`选项。

也能指定与特定Python版本兼容的包。
```shell
pip download <package_name>==<version> --python-version <python_version>
```
例如，要下载Python 3.11版本的pytest包，可以使用以下命令：
```shell
pip download <package_name> --python-version <version>
```

还能指定特定平台。
```shell
pip download <package_name>==<version> --platform <platform>
```

使用 --only-binary 下载只提供二进制文件的包（whl 文件），或者使用 --no-binary 下载源码包：

```shell
pip download <package_name> --only-binary=:all:
```

或者：

```shell
pip download <package_name> --no-binary=:all:
```

---

有时本地安装了所需的依赖包，但是安装时依然提示缺少依赖包。
这可能是因为：默认情况下，`pip`在构建和安装过程中会使用一个隔离的环境。构建隔离确保构建过程中的所有依赖项都是干净的、不受当前环境影响的。但是，这也可能导致找不到已经安装的依赖包。

解决方法：
增加`--no-build-isolation`选项，这样`pip`就不会使用隔离环境，而是使用当前环境。
即：
```shell
pip install --no-build-isolationa .
```