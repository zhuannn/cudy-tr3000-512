# 如何在本地编译固件或uboot
可参考以下命令


## ImmortalWrt固件
1. 安装依赖（参考[immortalwrt-requirements](https://github.com/immortalwrt/immortalwrt?tab=readme-ov-file#requirements)）
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
2. 克隆源码
```bash
mkdir ~/workdir && cd ~/workdir
git clone --depth 1 https://github.com/immortalwrt/immortalwrt -b v24.10.4
git clone --depth 1 https://github.com/zhuannn/cudy-tr3000-512 mod512
```
3. 修改512MB配置
```bash
cd ~/workdir
cat mod512/openwrt-mod/cudy-tr3000-512.mk >> immortalwrt/target/linux/mediatek/image/filogic.mk
cp mod512/openwrt-mod/mt7981*.dts immortalwrt/target/linux/mediatek/dts/
```
4. 安装feeds，进行编译配置
```bash
cd ~/workdir/immortalwrt
./scripts/feeds update -a
./scripts/feeds install -a
make menuconfig
```
5. 编译
```bash
cd ~/workdir/immortalwrt
make -j$(nproc)
```

## uboot
```bash
# 安装依赖
sudo apt update
sudo apt install -y gcc-aarch64-linux-gnu build-essential flex bison libssl-dev device-tree-compiler python-is-python3
# 克隆仓库
mkdir workdir && cd workdir
git clone --depth 1 https://github.com/zhuannn/cudy-tr3000-512 mod512
git clone --depth 1 https://github.com/hanwckf/bl-mt798x bl-mt798x
# 修改512MB配置
cd bl-mt798x
cp ../mod512/uboot-mod/mt7981_cudy_tr3000-v1_multi_layout_defconfig ./uboot-mtk-20230718-09eda825/configs/
cp ../mod512/uboot-mod/mt7981-cudy-tr3000-v1.dts ./uboot-mtk-20230718-09eda825/arch/arm/dts/
cp ../mod512/uboot-mod/index.html ./uboot-mtk-20230718-09eda825/failsafe/fsdata/index.html
# 编译uboot
SOC=mt7981 BOARD=cudy_tr3000-v1 MULTI_LAYOUT=1 ./build.sh
# 计算MD5
cd output
md5sum *.bin
```
