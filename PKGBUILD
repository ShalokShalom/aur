# Maintainer: justbispo <aur.fyxy0@slmail.me>
# Maintainer: George Rawlinson <grawlinson@archlinux.org>

pkgname=codeberg-cli
pkgver=0.4.9
pkgrel=1
pkgdesc='CLI Tool for Codeberg similar to gh and glab'
arch=('x86_64')
url='https://codeberg.org/Aviac/codeberg-cli'
license=('AGPL3')
depends=('gcc-libs' 'openssl')
makedepends=('cargo')
options=('!lto')
source=("$pkgname-$pkgver.tar.gz::https://static.crates.io/crates/$pkgname/$pkgname-$pkgver.crate")
sha256sums=('fc7de05901b7b1fe8699e32669f5e837d83b250bfc2da7d77dec2268cb97062c')

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
