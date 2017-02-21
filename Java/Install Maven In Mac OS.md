1. 首先在 [Maven](http://maven.apache.org/download.cgi) 下载

2. 将下载的文件解压至想要存放 Maven 的路径，会看到路径下包含一个名为 *apache-maven-版本号* 的文件夹（以 `/Users/Jet/maven_path/apache-maven-3.3.9` 为例）

3. 打开终端，cd ~

4. 输入 `sublime .bash_profile` 或 `vi .bash_profile` 或使用其他编辑器打开

5. 因为系统原因，我这里并未看到 JAVA_HOME 的相关信息，但实际上在 `/usr/libexec/` 下是有 java_home 设置的

6. 在编辑器中输入以下两条

```bash
export M2_HOME="/Users/Jet/maven_path/apache-maven-3.3.9"
export PATH="$PATH:$M2_HOME/bin"
```

7. 退出编辑器，输入 source .bash_profile 使修改生效

8. 使用 mvn -version 命令测试是否成功

    ​

    ​