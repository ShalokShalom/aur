# Maintainer: justbispo <aur.fyxy0@slmail.me>
# Maintainer: George Rawlinson <grawlinson@archlinux.org>

pkgname=codeberg-cli
pkgver=0.4.7
pkgrel=1
pkgdesc='CLI Tool for Codeberg similar to gh and glab'
arch=('x86_64')
url='https://codeberg.org/Aviac/codeberg-cli'
license=('AGPL3')
depends=('gcc-libs' 'openssl')
makedepends=('cargo')
options=('!lto')
source=("$pkgname-$pkgver.tar.gz::https://static.crates.io/crates/$pkgname/$pkgname-$pkgver.crate")
sha256sums=('c92502498a4af4839886e4e8d9499fd0c62467bc70de60a7537ae779e78c38c7')

prepare() {
  cd "$srcdir/$pkgname-$pkgver"
  export RUSTUP_TOOLCHAIN=stable
  cargo fetch --locked --target "$(rustc -vV | sed -n 's/host: //p')"
}

build() {
  cd "$srcdir/$pkgname-$pkgver"
  export RUSTUP_TOOLCHAIN=stable
  export CARGO_TARGET_DIR=target
  cargo build --frozen --release --all-features

  for shell in bash fish zsh; do
      ./target/release/berg completion "$shell" > "$shell-completion"
  done
}

check() {
  cd "$srcdir/$pkgname-$pkgver"
  export RUSTUP_TOOLCHAIN=stable
  cargo test --frozen --all-features
}

package() {
  cd "$srcdir/$pkgname-$pkgver"
  install -Dm755 -t "$pkgdir/usr/bin/" "target/release/berg"
  install -Dm644 -t "$pkgdir/usr/share/doc/$pkgname/README.md" "README.md"
  install -Dm644 -t "$pkgdir/usr/share/licenses/$pkgname/LICENSE" "LICENSE"
  install -Dm644 bash-completion "$pkgdir/usr/share/bash-completion/completions/berg"
  install -Dm644 fish-completion "$pkgdir/usr/share/fish/vendor_completions.d/berg.fish"
  install -Dm644 zsh-completion "$pkgdir/usr/share/zsh/site-functions/_berg"
  
}
