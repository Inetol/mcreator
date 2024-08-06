# Maintainer: Ivan Gabaldon <aur[at]inetol.net>

pkgname=mcreator
_pkgvermajor=2024.2
_pkgverbuild=32117
pkgver="$_pkgvermajor.$_pkgverbuild"
pkgrel=1
pkgdesc='Make Minecraft Java Edition mods, Bedrock Edition Add-Ons, and data packs using visual graphical programming or integrated IDE'
arch=('x86_64')
url='https://mcreator.net'
license=('GPL-3.0-or-later WITH GPL-3.0-interface-exception')
noextract=("$pkgname-$pkgver.tar.gz")
source=("$pkgname-$pkgver.tar.gz::https://github.com/$pkgname/$pkgname/releases/download/$pkgver/MCreator.$_pkgvermajor.Linux.64bit.tar.gz"
        "$pkgname.desktop")
b2sums=('83e65b352f0ec271a7c0bb36578c3500b0b3478ccdcd94e1b46d49df1098775a3e5b7f4129d6c409523a98c18721cb94bea29d1efe307f94870aac3a3e3ac281'
        'ac5a53421869276596ebbc7cccfa7d20aeb63ea3a556d0b77a0b9b936176bcbabe7771be60fc5b034ec79a49da5f4aa538b79e1c6edca478a5614f49ec478d6f')

prepare() {
    PKGBUILD_JAVA='/usr/lib/jvm/$(archlinux-java status | awk '\''/java-21/{print $NF}'\'')/bin/java'

    mkdir -p "$pkgname-$pkgver/"
    bsdtar -xpf "$pkgname-$pkgver.tar.gz" --strip-components=1 -C "$pkgname-$pkgver/"

    # Convert
    cd "$pkgname-$pkgver/"

    mv LICENSE.txt LICENSE

    cat >"$pkgname"<<EOF
#!/usr/bin/env bash
export CLASSPATH="./$pkgname/lib/mcreator.jar:./lib/*"

cd "/opt/$pkgname/"
$PKGBUILD_JAVA --add-opens=java.base/java.lang=ALL-UNNAMED net.mcreator.Launcher "\$1"
EOF

    cp "$srcdir/$pkgname.desktop" "$pkgname.desktop"

    mv icon.png "$pkgname.png"

    rm -rf {jdk/,"$pkgname.sh"}
}

package() {
    depends=('bash'
             'glibc'
             'java-runtime=21')

    install -d "$pkgdir/opt/$pkgname/"
    cp -a "$pkgname-$pkgver/." "$pkgdir/opt/$pkgname/"

    chmod 755 "$pkgdir/opt/$pkgname/$pkgname"

    install -d "$pkgdir/usr/bin/"
    ln -s "/opt/$pkgname/$pkgname" "$pkgdir/usr/bin/$pkgname"

    install -d "$pkgdir/usr/share/applications/"
    ln -s "/opt/$pkgname/$pkgname.desktop" "$pkgdir/usr/share/applications/$pkgname.desktop"

    install -d "$pkgdir/usr/share/icons/"
    ln -s "/opt/$pkgname/$pkgname.png" "$pkgdir/usr/share/icons/$pkgname.png"

    install -d "$pkgdir/usr/share/licenses/$pkgname/"
    ln -s "/opt/$pkgname/LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
