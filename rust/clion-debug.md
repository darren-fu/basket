## 安装Clion
- 安装intellij-rust插件

## 下载msys2   http://www.msys2.org/
---
### 设置 msys2 镜像
#### pacman 的配置
- 编辑 /etc/pacman.d/mirrorlist.mingw32 ，在文件开头添加：
Server = https://mirrors.tuna.tsinghua.edu.cn/msys2/mingw/i686
- 编辑 /etc/pacman.d/mirrorlist.mingw64 ，在文件开头添加：

Server = https://mirrors.tuna.tsinghua.edu.cn/msys2/mingw/x86_64
- 编辑 /etc/pacman.d/mirrorlist.msys ，在文件开头添加：

Server = https://mirrors.tuna.tsinghua.edu.cn/msys2/msys/$arch
- 然后执行 pacman -Sy 刷新软件包数据即可。
---

### 安装GNU too chain
运行mingw64.exe
运行pacman -S --noconfirm base-devel mingw-w64-x86_64-toolchain git

---

### 配置 GNU

#### 添加配置到C:\Users\YourName\.cargo\config
``` 
[target.x86_64-pc-windows-gnu]
linker = "D:\\ide\\msys64\\mingw64\\bin\\gcc.exe"
ar = "D:\\ide\\msys64\\mingw64\\bin\\ar.exe"
```

添加C:\msys64\usr\bin（如果报冲突，这个就不添加）和C:\msys64\mingw64\bin到Path环境变量

#### 并添加MinGW工具链到CLion，路径为C:\msys64\mingw64

---

### 启用GNU

- rustup toolchain list
- rust安装 gnu toolchain   
rustup toolchain install stable-gnu
- rustup set default-host x86_64-pc-windows-gnu
