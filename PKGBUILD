# Maintainer: Kimiblock Moe
# Contributor: wszqkzqk

pkgname=wechat-bin
pkgver=4.1.13.26
# 用 epoch=1 壓在 AUR 那顆之上（AUR 版 depends 仍寫 libvlc）。之前靠 pkgrel=1001 硬壓，
# 上游一 bump pkgver 就失效（實測 4.1.13.25 之前那版就是這樣被蓋回來的）；epoch 才是
# 版本比較先看的欄位，pkgrel 回到 1 跟著上游走。
pkgrel=1
epoch=1

pkgdesc="微信是一种生活方式. This is a repackage of WeChat."
arch=('x86_64' 'aarch64' 'loong64')
url="https://weixin.qq.com/"
license=('LicenseRef-proprietary')
groups=()
options=(!debug !strip)

makedepends+=()

replaces+=()

depends=(
	"nss"
	"xcb-util-renderutil"
	"xcb-util-keysyms"
	"xcb-util-image"
	"xcb-util-wm"
	"libxkbcommon-x11"
	"libxkbcommon"
	"libxcb"
	"gcc-libs"
	"nspr"
	"glibc"
	"zlib"
	"libxcomposite"
	"glib2"
	"libxrender"
	"libxext"
	"alsa-lib"
	"dbus"
	"libxrandr"
	"fontconfig"
	"pango"
	"freetype2"
	"libxfixes"
	"cairo"
	"libx11"
	"expat"
	# libvlc 已移除：微信自帶自己的 VLC 於 /opt/wechat/（libvlc.so.5.6.1、libvlccore.so.9.0.1
	# 與整套 vlc_plugins/），且唯一使用者 /opt/wechat/wxplayer 用 DT_RPATH=$ORIGIN 只会載自帶那份。
	# 實測證據見 /tmp/wechat-novlc/evidence.txt（ldd、readelf -d、私有 mount namespace 遮掉
	# /usr/lib/libvlc* 後解析不變、ld.so --verify 全回 0），上游 Tencent .deb 的 control
	# 也從未宣告 libvlc 依賴（只有 fonts-noto-cjk）。裝了系統 libvlc 也不會被用到，故不進 optdepends。
	"libxdamage"
	"libdrm"
	"mesa"
	"libglvnd"
	"libpulse"
	"hicolor-icon-theme"
)

optdepends=(
	"wechat: Sandbox support and enhancements"
	'ttf-twemoji: An emoji font that will work with WeChat'
	'libjack.so: libjack_plugin.so'
	'at-spi2-core: Accessibility in the browser part'
)

makedepends+=(
	"libarchive"
)

checkdepends=()

source=()

# The official site does not have versioned download, so checksums are skipped

source_x86_64=(
	wechat-x86-${pkgver}.deb::"https://dldir1v6.qq.com/weixin/Universal/Linux/WeChatLinux_x86_64.deb"
)

source_aarch64=(
	wechat-arm-${pkgver}.deb::"https://dldir1v6.qq.com/weixin/Universal/Linux/WeChatLinux_arm64.deb"
)

source_loong64=(
	wechat-loong64-${pkgver}.deb::"https://dldir1v6.qq.com/weixin/Universal/Linux/WeChatLinux_LoongArch.deb"
)


md5sums_x86_64=('SKIP')
md5sums_aarch64=('SKIP')
md5sums_loong64=('SKIP')

function pkgver() {
	tar -xf control.tar.xz ./control
	cat control | grep 'Version: ' | cut -c '10-'
}

function package() {
	tar -xf data.tar.xz ./opt
	cp -r opt \
		"${pkgdir}/"
	tar -xf data.tar.xz ./usr
	cp -r usr \
		"${pkgdir}/"
	install -d "${pkgdir}/usr/share/licenses/${pkgname}"
	echo "https://www.wechat.com/us/service_terms.html" \
		>"${pkgdir}/usr/share/licenses/${pkgname}/ToS.txt"
	sed -i 's|/usr/bin/wechat|/opt/wechat/wechat|g' \
		"${pkgdir}/usr/share/applications"/*.desktop
	ln -sf "/opt/wechat/wechat" \
		"${pkgdir}/usr/bin/wechat"
}
