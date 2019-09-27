# vim:set ft=sh:
# Maintainer: BlackEagle < ike DOT devolder AT gmail DOT com >
# thx for the original vim pkg:
# Contributor: Jan "heftig" Steffens <jan.steffens@gmail.com>
# Contributor: tobias [ tobias at archlinux org ]
# Contributor: Daniel J Griffiths <ghost1227@archlinux.us>

pkgbase=vim
pkgname=('vim-tiny' 'vim-cli-nox' 'vim-cli' 'vim-rt')
_basever=8.1
_patchlevel=2084
if [ "$_patchlevel" = "0" ]; then
    pkgver=${_basever}
else
    pkgver=${_basever}.${_patchlevel}
fi
_gitcommit=5e8e967f25085de78d7826fe5a6bebbace1c6823
pkgrel=1
_versiondir=vim${_basever/./}
arch=('x86_64')
license=('custom:vim')
url="http://www.vim.org"
makedepends=('gpm' 'perl' 'python2' 'python' 'lua' 'desktop-file-utils' 'gettext' 'pkgconfig' 'sed' 'git' 'ruby' 'libxt')
source=(
    "$pkgbase::git://github.com/vim/vim#commit=$_gitcommit"
    'license.txt'
)
sha256sums=('SKIP'
            'bb4744930a0030085d382356e9fdd4f2049b6298147aee2470c7fca7ec82fd55')

prepare() {
    # remove old build dirs if exist
    [ -d vim-build ] && rm -rf vim-build
    [ -d vim-build-tn ] && rm -rf vim-build-tn
    [ -d vim-build-nox ] && rm -rf vim-build-nox

    cp -a ${pkgbase} vim-build
    (
        cd vim-build && rm -rf ./.git*
    )

    # define the place for the global (g)vimrc file (set to /etc/vimrc)
    sed -i 's|^.*\(#define SYS_.*VIMRC_FILE.*"\) .*$|\1|' \
        vim-build/src/feature.h
    sed -i 's|^.*\(#define VIMRC_FILE.*"\) .*$|\1|' \
        vim-build/src/feature.h

    cp -a vim-build vim-build-tn
    cp -a vim-build vim-build-nox

    cd ${srcdir}/vim-build
    (cd src && autoconf)

    cd ${srcdir}/vim-build-tn
    (cd src && autoconf)

    cd ${srcdir}/vim-build-nox
    (cd src && autoconf)
}

build() {
    msg2 'Building vim-tiny'
    cd ${srcdir}/vim-build-tn
    ./configure --prefix=/usr --localstatedir=/var/lib/vim \
        --mandir=/usr/share/man --with-compiledby=BlackEagle \
        --with-features=tiny --disable-gpm --enable-acl --with-x=no \
        --disable-gui --enable-multibyte --disable-cscope \
        --disable-netbeans --disable-perlinterp --disable-pythoninterp \
        --disable-rubyinterp --enable-luainterp=no
    make

    msg2 'Building vim-cli-nox'
    cd ${srcdir}/vim-build-nox
    ./configure --prefix=/usr --localstatedir=/var/lib/vim \
        --mandir=/usr/share/man --with-compiledby=BlackEagle \
        --with-features=huge --enable-gpm --enable-acl --with-x=no \
        --disable-gui --enable-multibyte --enable-cscope \
        --disable-netbeans --enable-perlinterp=dynamic \
        --enable-pythoninterp=dynamic --enable-python3interp=dynamic \
        --enable-rubyinterp=dynamic --enable-luainterp=dynamic
        #--disable-rubyinterp --enable-luainterp=dynamic
    make

    msg2 'Building vim-cli'
    cd ${srcdir}/vim-build
    ./configure --prefix=/usr --localstatedir=/var/lib/vim \
        --mandir=/usr/share/man --with-compiledby=BlackEagle \
        --with-features=huge --enable-gpm --enable-acl --with-x=yes \
        --disable-gui --enable-multibyte --enable-cscope \
        --disable-netbeans --enable-perlinterp=dynamic \
        --enable-pythoninterp=dynamic --enable-python3interp=dynamic \
        --enable-rubyinterp=dynamic --enable-luainterp=dynamic
        #--disable-rubyinterp --enable-luainterp=dynamic
    make
}

package_vim-tiny() {
    pkgdesc='Vi Improved, tiny edition'
    depends=('acl')
    conflicts=('vi' 'vim')
    provides=('vim')

    cd ${srcdir}/vim-build-tn
    make -j1 VIMRCLOC=/etc DESTDIR=${pkgdir} install

    # Runtime not needed
    rm -r ${pkgdir}/usr/share/vim

    # vi symlink
    ln -sf /usr/bin/vim ${pkgdir}/usr/bin/vi

    # license
    install -dm755 ${pkgdir}/usr/share/licenses/vim-tiny
    install -Dm644 ${srcdir}/license.txt \
        ${pkgdir}/usr/share/licenses/vim-tiny/license.txt
}

package_vim-cli-nox() {
    pkgdesc='Vi Improved, cli'
    depends=("vim-rt=${pkgver}-${pkgrel}" 'gpm')
    optdepends=(
        'perl: vim perl binding'
        'python2: vim python2 binding'
        'python: vim python3 binding'
        'lua: vim lua binding'
        'ruby: vim ruby binding'
    )
    conflicts=('vi' 'vim' 'vim-cli')
    provides=('vim' 'xxd' 'vi')

    cd ${srcdir}/vim-build-nox
    make -j1 VIMRCLOC=/etc DESTDIR=${pkgdir} install

    # remove evim manpages
    rm -f ${pkgdir}/usr/share/man/*{,/*}/evim*

    # Runtime provided by runtime package
    rm -r ${pkgdir}/usr/share/vim

    # vi symlink
    ln -sf /usr/bin/vim ${pkgdir}/usr/bin/vi

    # no need for gvim references
    find "$pkgdir" -name "gvim.*" -delete

    # license
    install -dm755 ${pkgdir}/usr/share/licenses/vim-cli-nox
    install -Dm644 ${srcdir}/license.txt \
        ${pkgdir}/usr/share/licenses/vim-cli-nox/license.txt
}

package_vim-cli() {
    pkgdesc='Vi Improved, cli'
    depends=("vim-rt=${pkgver}-${pkgrel}" 'gpm' 'libxt')
    optdepends=(
        'perl: vim perl binding'
        'python2: vim python2 binding'
        'python: vim python3 binding'
        'lua: vim lua binding'
        'ruby: vim ruby binding'
    )
    conflicts=('vi' 'vim' 'vim-cli-nox')
    provides=('vim' 'xxd' 'vi')

    cd ${srcdir}/vim-build
    make -j1 VIMRCLOC=/etc DESTDIR=${pkgdir} install

    # remove evim manpages
    rm -f ${pkgdir}/usr/share/man/*{,/*}/evim*

    ## Runtime provided by runtime package
    #rm -r ${pkgdir}/usr/share/vim
    # Move the runtime for later packaging
    mv ${pkgdir}/usr/share/vim ${srcdir}/runtime-install

    # vi symlink
    ln -sf /usr/bin/vim ${pkgdir}/usr/bin/vi

    # no need for gvim references
    find "$pkgdir" -name "gvim.*" -delete

    # license
    install -dm755 ${pkgdir}/usr/share/licenses/vim-cli
    install -Dm644 ${srcdir}/license.txt \
        ${pkgdir}/usr/share/licenses/vim-cli/license.txt
}

package_vim-rt() {
    pkgdesc='Runtime for vim and gvim'
    conflicts=('vim-runtime')
    provides=('vim-runtime')
    optdepends=(
        'python: tools'
        'gawk: tools'
    )

    # Install the runtime split from gvim
    install -dm755 ${pkgdir}/usr/share
    mv ${srcdir}/runtime-install ${pkgdir}/usr/share/vim

    # fix FS#22661 add rgb.txt
    install -Dm644 ${srcdir}/vim/runtime/rgb.txt \
        ${pkgdir}/usr/share/vim/${_versiondir}/rgb.txt

    # fix FS#17216
    sed -i 's|messages,/var|messages,/var/log/messages.log,/var|' \
        ${pkgdir}/usr/share/vim/${_versiondir}/filetype.vim

    # patch filetype.vim for better handling of pacman related files
    sed -i "s/rpmsave/pacsave/;s/rpmnew/pacnew/;s/,\*\.ebuild/\0,PKGBUILD*,*.install/" \
        ${pkgdir}/usr/share/vim/${_versiondir}/filetype.vim
    sed -i "/find the end/,+3{s/changelog_date_entry_search/changelog_date_end_entry_search/}" \
        ${pkgdir}/usr/share/vim/${_versiondir}/ftplugin/changelog.vim

    # license
    install -dm755 ${pkgdir}/usr/share/licenses/vim-rt
    install -Dm644 ${srcdir}/license.txt \
        ${pkgdir}/usr/share/licenses/vim-rt/license.txt
}
