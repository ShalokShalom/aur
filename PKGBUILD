# Maintainer: justbispo <aur.fyxy0@slmail.me>
# Maintainer: George Rawlinson <grawlinson@archlinux.org>

pkgname=codeberg-cli
pkgver=0.4.0
pkgrel=1
pkgdesc='CLI Tool for Codeberg similar to gh and glab'
arch=('x86_64')
url='https://codeberg.org/RobWalt/codeberg-cli'
license=('AGPL3')
depends=('gcc-libs' 'openssl')
makedepends=('git' 'cargo')
source=("$pkgname-$pkgver.tar.gz::https://codeberg.org/RobWalt/$pkgname/archive/v$pkgver.tar.gz")
sha512sums=('8913d407ec8618984f8373553ab917fd78c73d1a202fc10eda14ba5cb13153f7a4bd1f615074fa52b229af71fcf6a3c12af8ec8d4de58fda5d50a9082c4e3580')

prepare() {
  cd "$pkgname"
  export RUSTUP_TOOLCHAIN=stable
  cargo fetch --locked --target "$(rustc -vV | sed -n 's/host: //p')"
}

build() {
  cd "$pkgname"
  export RUSTUP_TOOLCHAIN=stable
  export CARGO_TARGET_DIR=target
  cargo build --frozen --release --all-features

  for shell in bash fish zsh; do
      ./target/release/berg completion "$shell" > "$shell-completion"
  done
}

check() {
  cd "$pkgname"
  export RUSTUP_TOOLCHAIN=stable
  cargo test --frozen --all-features
}

package() {
  cd "$pkgname"
  install -Dm755 -t "$pkgdir/usr/bin/" "target/release/berg"
  install -Dm644 -t "$pkgdir/usr/share/doc/${pkgname}/README.md" "README.md"
  install -Dm644 -t "$pkgdir/usr/share/licenses/${pkgname}/LICENSE" "LICENSE"
  mkdir -p "${pkgdir}/usr/share/bash-completion/completions/"
  mkdir -p "${pkgdir}/usr/share/zsh/site-functions/"
  mkdir -p "${pkgdir}/usr/share/fish/vendor_completions.d/"
  install -vDm644 bash-completion "$pkgdir/usr/share/bash-completion/completions/berg"
  install -vDm644 fish-completion "$pkgdir/usr/share/fish/vendor_completions.d/berg.fish"
  install -vDm644 zsh-completion "$pkgdir/usr/share/zsh/site-functions/_berg"
  
}
