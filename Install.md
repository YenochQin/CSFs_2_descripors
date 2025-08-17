# CSF Descriptor 安装与使用指南

## 📦 包管理器安装

### Ubuntu/Debian
```bash
# 下载 .deb 包
wget https://github.com/YenochQin/CSFs_2_descripors/releases/download/v1.0.0/csf-descriptor_1.0.0_amd64.deb

# 安装
sudo dpkg -i csf-descriptor_1.0.0_amd64.deb
sudo apt-get install -f  # 自动修复依赖
```

### CentOS/RHEL/Fedora
```bash
# CentOS/RHEL
sudo yum install https://github.com/YenochQin/CSFs_2_descripors/releases/download/v1.0.0/csf-descriptor-1.0.0-1.x86_64.rpm

# Fedora
sudo dnf install https://github.com/YenochQin/CSFs_2_descripors/releases/download/v1.0.0/csf-descriptor-1.0.0-1.x86_64.rpm
```

## 📁 预编译压缩包安装

### Linux
```bash
# 下载压缩包
wget https://github.com/YenochQin/CSFs_2_descripors/releases/download/v1.0.0/csf-descriptor-1.0.0-Linux.tar.gz

# 解压
tar -xzf csf-descriptor-1.0.0-Linux.tar.gz
cd csf-descriptor-1.0.0-Linux

# 安装到系统
sudo ./install.sh

# 或者手动安装到用户目录
mkdir -p ~/.local/bin ~/.local/lib ~/.local/include
cp bin/csf_descriptor ~/.local/bin/
cp lib/libcsf.a ~/.local/lib/
cp -r include/csf ~/.local/include/
```


## 🚀 使用说明

### 基本使用
```bash
# 处理 CSF 文件
csf_descriptor input.csf

# 指定输出文件
csf_descriptor input.csf -o output.h5

# 使用扩展描述符格式
csf_descriptor -e input.csf
```

### 性能优化
```bash
# 使用多线程（自动检测 CPU 核心数）
csf_descriptor -t auto large.csf

# 指定线程数
csf_descriptor -t 16 huge.csf

# 静默模式（减少 I/O 开销）
csf_descriptor -q -t 32 massive.csf
```

## 📊 python调用描述符

```python
import h5py
import numpy as np

# 读取 HDF5 文件
def load_descriptors(h5_file):
    with h5py.File(h5_file, 'r') as f:
        descriptors = f['/descriptors'][:]
        labels = f['/labels'][:] if '/labels' in f else None
    return descriptors, labels
```

## 🛠️ 故障排除

### 常见问题

#### 1. 找不到命令
```bash
# 检查是否安装成功
csf_descriptor --version

# 检查 PATH
echo $PATH | grep -o '[^:]*bin[^:]*'

# 添加用户目录到 PATH
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc | ~/.zshrc
source ~/.bashrc | ~/.zshrc
```

#### 2. HDF5 错误
```bash
# 检查 HDF5 库
ldd $(which csf_descriptor) | grep hdf5

# 更新库缓存
sudo ldconfig

# 设置库路径
export LD_LIBRARY_PATH="/usr/local/lib:$LD_LIBRARY_PATH"
```

#### 3. 权限问题
```bash
# 使用用户目录安装
cmake .. -DCMAKE_INSTALL_PREFIX=$HOME/.local
make install

# 或者使用 sudo
sudo make install
```

### 验证安装
```bash
# 检查版本
csf_descriptor --version

# 测试功能
wget https://raw.githubusercontent.com/YenochQin/CSFs_2_descripors/main/data/test.csf
csf_descriptor test.csf

# 检查输出文件
h5dump -n test_descriptors.h5
```
