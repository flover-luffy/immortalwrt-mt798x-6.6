# ImmortalWrt MT798x (Linux 6.6)

本项目是基于 [ImmortalWrt](https://github.com/immortalwrt/immortalwrt) 的 MT798x 平台定制版本，采用最新的 Linux 6.6 内核，专注于提供高性能、稳定的网络体验。

## ✨ 特性

- **内核版本**: Linux 6.6 LTS
- **WiFi 驱动**: 集成 MTK 闭源 WiFi 驱动，提供最佳无线性能
- **插件集成**: 预装 PassWall, SSR-Plus, HomeProxy, TurboACC 等常用插件
- **硬件加速**: 支持 MTK 硬件网络加速 (WED/HNAT)
- **多设备支持**: 覆盖主流 MT7981 (Filogic 820) 和 MT7986 (Filogic 830) 设备

## 📱 支持设备列表

构建配置文件位于 `defconfig/` 目录下。

### MT7981 (Filogic 820) - `defconfig/mt7981-ax3000.config`
- **Xiaomi**: Mi Router AX3000T, Mi Router WR30U
- **CMCC**: RAX3000M (普通版/eMMC版), A10, XR30
- **H3C**: NX30 Pro
- **360**: T7
- **Konka**: Komi A31
- **Imou**: LC-HX3001
- **JCG**: Q30
- **Livinet**: ZR-3020
- **Cetron**: CT3003
- **ABT**: ASR3000

### MT7986 (Filogic 830) - `defconfig/mt7986-ax6000.config`
- **Xiaomi**: Redmi Router AX6000
- **GL.iNet**: GL-MT6000 (Flint 2)
- **TP-Link**: TL-XDR6086, TL-XDR6088
- **JDCloud**: RE-CP-03

### 特殊版
- **BPi-R3 Mini**: `defconfig/mt7986-ax4200-bpir3_mini.config`
- **高功率版 (iPAiLNA)**: `defconfig/mt7975-ipailna-high-power.config`

## ⚙️ 默认配置

- **管理 IP**: `192.168.6.1`
- **用户名**: `root`
- **密码**: *无 (空)*
- **WiFi SSID**: `ImmortalWrt-2.4G` / `ImmortalWrt-5G`
- **WiFi 密码**: *无 (开放)*

## 🛠️ 编译指南

建议使用 Ubuntu 22.04 LTS 或 Debian 11 进行编译。

### 1. 准备环境

```bash
sudo apt update -y
sudo apt full-upgrade -y
sudo apt install -y ack antlr3 asciidoc autoconf automake autopoint binutils bison build-essential \
  bzip2 ccache clang cmake cpio curl device-tree-compiler ecj fastjar flex gawk gettext gcc-multilib \
  g++-multilib git gnutls-dev gperf haveged help2man intltool lib32gcc-s1 libc6-dev-i386 libelf-dev \
  libglib2.0-dev libgmp3-dev libltdl-dev libmpc-dev libmpfr-dev libncurses-dev libpython3-dev \
  libreadline-dev libssl-dev libtool libyaml-dev libz-dev lld llvm lrzsz mkisofs msmtp nano \
  ninja-build p7zip p7zip-full patch pkgconf python3 python3-pip python3-ply python3-docutils \
  python3-pyelftools qemu-utils re2c rsync scons squashfs-tools subversion swig texinfo uglifyjs \
  upx-ucl unzip vim wget xmlto xxd zlib1g-dev zstd
```

### 2. 克隆源码

```bash
git clone https://github.com/flover-luffy/immortalwrt-mt798x-6.6.git
cd immortalwrt-mt798x-6.6
```

### 3. 更新 Feeds

```bash
./scripts/feeds update -a
./scripts/feeds install -a
```

### 4. 加载配置

根据目标设备选择对应的配置文件复制到 `.config`。

**例如编译 Redmi AX6000:**
```bash
cp -f defconfig/mt7986-ax6000.config .config
```

**例如编译 CMCC RAX3000M / Xiaomi AX3000T:**
```bash
cp -f defconfig/mt7981-ax3000.config .config
```

### 5. 定制固件 (可选)

```bash
make menuconfig
```

> [!TIP]
> 如果你需要调整内核设置，可以使用 `make kernel_menuconfig`。

### 6. 下载源码

```bash
make download -j8
```

> [!IMPORTANT]
> 请务必执行此步骤，以确保所有软件包源码完整且哈希校验通过。

### 7. 开始编译

```bash
make -j$(nproc) || make -j1 V=s
```

编译完成后，固件文件将位于 `bin/targets/mediatek/mt7986/` (或 mt7981) 目录下。
