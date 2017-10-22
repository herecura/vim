# vim:set ft=sh:
# Maintainer: BlackEagle < ike DOT devolder AT gmail DOT com >
# thx for the original vim pkg:
# Contributor: Jan "heftig" Steffens <jan.steffens@gmail.com>
# Contributor: tobias [ tobias at archlinux org ]
# Contributor: Daniel J Griffiths <ghost1227@archlinux.us>

pkgbase=vim
#pkgname=('vim-tiny' 'vim-cli' 'vim-gvim-gtk2' 'vim-gvim-gtk3' 'vim-gvim-qt' 'vim-rt' 'vim-gvim-common')
pkgname=('vim-tiny' 'vim-cli' 'vim-gvim-gtk2' 'vim-gvim-gtk3' 'vim-rt' 'vim-gvim-common')
_basever=8.0
_patchlevel=1212
if [ "$_patchlevel" = "0" ]; then
    pkgver=${_basever}
else
    pkgver=${_basever}.${_patchlevel}
fi
_gitcommit=66857f410426ca335f4771a58a32b2d14a7e52b9
pkgrel=1
_versiondir=vim${_basever/./}
arch=('i686' 'x86_64')
license=('custom:vim')
url="http://www.vim.org"
#makedepends=('gpm' 'perl' 'python2' 'python' 'lua' 'desktop-file-utils' 'gtk2' 'gettext' 'pkgconfig' 'sed' 'git' 'qt5-base' 'ruby' 'gtk3' 'libxt')
makedepends=('gpm' 'perl' 'python2' 'python' 'lua' 'desktop-file-utils' 'gtk2' 'gettext' 'pkgconfig' 'sed' 'git' 'ruby' 'gtk3' 'libxt')
#source=(
    #"$pkgbase::git://github.com/vim/vim#commit=$_gitcommit"
    #'license.txt'
    #'vim-qt-src.patch'
    #'qt-icons.tar.gz'
    #'qvim.desktop'
    #'qvim.png'
#)
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
    [ -d gvim-build-gtk2 ] && rm -rf gvim-build-gtk2
    [ -d gvim-build-gtk3 ] && rm -rf gvim-build-gtk3
    #[ -d gvim-build-qt ] && rm -rf gvim-build-qt

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
    cp -a vim-build gvim-build-gtk2
    cp -a vim-build gvim-build-gtk3
    #cp -a vim-build gvim-build-qt

    cd ${srcdir}/vim-build-tn
    (cd src && autoconf)

    cd ${srcdir}/vim-build
    (cd src && autoconf)

    cd ${srcdir}/gvim-build-gtk2
    (cd src && autoconf)

    cd ${srcdir}/gvim-build-gtk3
    (cd src && autoconf)

    #cd ${srcdir}/gvim-build-qt
    #patch -p1 -i ${srcdir}/vim-qt-src.patch
    #(cd src && autoconf)
    #(cd src/qt && tar -zxf ${srcdir}/qt-icons.tar.gz)
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

    msg2 'Building vim-gvim-gtk2'
    cd ${srcdir}/gvim-build-gtk2
    ./configure --prefix=/usr --localstatedir=/var/lib/vim \
        --mandir=/usr/share/man --with-compiledby=BlackEagle \
        --with-features=huge --enable-gpm --enable-acl --with-x=yes \
        --enable-gui=gtk2 --enable-multibyte --enable-cscope \
        --disable-netbeans  --enable-perlinterp=dynamic \
        --enable-pythoninterp=dynamic --enable-python3interp=dynamic \
        --enable-rubyinterp=dynamic --enable-luainterp=dynamic
        #--disable-rubyinterp --enable-luainterp=dynamic
    make

    msg2 'Building vim-gvim-gtk3'
    cd ${srcdir}/gvim-build-gtk3
    ./configure --prefix=/usr --localstatedir=/var/lib/vim \
        --mandir=/usr/share/man --with-compiledby=BlackEagle \
        --with-features=huge --enable-gpm --enable-acl --with-x=yes \
        --enable-gui=gtk3 --enable-multibyte --enable-cscope \
        --disable-netbeans  --enable-perlinterp=dynamic \
        --enable-pythoninterp=dynamic --enable-python3interp=dynamic \
        --enable-rubyinterp=dynamic --enable-luainterp=dynamic
        #--disable-rubyinterp --enable-luainterp=dynamic
    make

    #msg2 'Building vim-gvim-qt'
    #cd ${srcdir}/gvim-build-qt
    #./configure --prefix=/usr --localstatedir=/var/lib/vim \
        #--mandir=/usr/share/man --with-compiledby=BlackEagle \
        #--with-features=huge --enable-gpm --enable-acl --with-x=yes \
        #--enable-gui=qt --with-qt-qmake=/usr/bin/qmake-qt5 \
        #--enable-multibyte --enable-cscope \
        #--disable-netbeans  --enable-perlinterp=dynamic \
        #--enable-pythoninterp=dynamic --enable-python3interp=dynamic \
        #--enable-rubyinterp=dynamic --enable-luainterp=dynamic
        ##--disable-rubyinterp --enable-luainterp=dynamic
    #make
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
    conflicts=('vi' 'vim')
    provides=('vim' 'xxd')

    cd ${srcdir}/vim-build
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
    install -dm755 ${pkgdir}/usr/share/licenses/vim-cli
    install -Dm644 ${srcdir}/license.txt \
        ${pkgdir}/usr/share/licenses/vim-cli/license.txt
}

package_vim-gvim-gtk2() {
    pkgdesc='Vi Improved, gtk2 gui'
    depends=('vim-cli' 'vim-gvim-common' 'desktop-file-utils' 'gtk2' 'hicolor-icon-theme' 'shared-mime-info')
    provides=('gvim')
    replaces=('vim-gvim-gtk')
    conflicts=('vim-gvim-gtk3')

    # allow install of icons and desktopfiles
    install -dm755 "${pkgdir}/usr/share/icons/hicolor/48x48/apps"
    install -dm755 "${pkgdir}/usr/share/icons/locolor/32x32/apps"
    install -dm755 "${pkgdir}/usr/share/icons/locolor/16x16/apps"
    install -dm755 "${pkgdir}/usr/share/applications"

    cd ${srcdir}/gvim-build-gtk2
    make -j1 VIMRCLOC=/etc DESTDIR=${pkgdir} install

    # move vim to gvim
    rm -f ${pkgdir}/usr/bin/gvim
    mv ${pkgdir}/usr/bin/{vim,gvim}
    # remove files provided by vim-cli
    rm -f ${pkgdir}/usr/bin/{vimtutor,xxd,rview,rvim,view,vimdiff,ex}
    rm -f ${pkgdir}/usr/share/man/*{,/*}/{vim*,vimtutor*,xxd*,rview*,rvim*,view*,vimdiff*,ex*}
    # recreate gvim symlinks
    (
    cd ${pkgdir}/usr/bin
    for link in eview evim gview gvimdiff rgview rgvim; do
        rm -f ${link}
        ln -s gvim ${link}
    done
    )

    # Runtime provided by runtime package
    rm -r ${pkgdir}/usr/share/vim

    # Move the man pages for common packaging
    mv ${pkgdir}/usr/share/man ${srcdir}/gvim-man-install

    # remove vim desktop file
    rm ${pkgdir}/usr/share/applications/vim.desktop

    # license
    install -dm755 ${pkgdir}/usr/share/licenses/vim-gvim-gtk2
    install -Dm644 ${srcdir}/license.txt \
        ${pkgdir}/usr/share/licenses/vim-gvim-gtk2/license.txt
}

package_vim-gvim-gtk3() {
    pkgdesc='Vi Improved, gtk3 gui'
    depends=('vim-cli' 'vim-gvim-common' 'desktop-file-utils' 'gtk3' 'hicolor-icon-theme' 'shared-mime-info')
    provides=('gvim')
    replaces=('vim-gvim-gtk')
    conflicts=('vim-gvim-gtk2')

    # allow install of icons and desktopfiles
    install -dm755 "${pkgdir}/usr/share/icons/hicolor/48x48/apps"
    install -dm755 "${pkgdir}/usr/share/icons/locolor/32x32/apps"
    install -dm755 "${pkgdir}/usr/share/icons/locolor/16x16/apps"
    install -dm755 "${pkgdir}/usr/share/applications"

    cd ${srcdir}/gvim-build-gtk3
    make -j1 VIMRCLOC=/etc DESTDIR=${pkgdir} install

    # move vim to gvim
    rm -f ${pkgdir}/usr/bin/gvim
    mv ${pkgdir}/usr/bin/{vim,gvim}
    # remove files provided by vim-cli
    rm -f ${pkgdir}/usr/bin/{vimtutor,xxd,rview,rvim,view,vimdiff,ex}
    rm -f ${pkgdir}/usr/share/man/*{,/*}/{vim*,vimtutor*,xxd*,rview*,rvim*,view*,vimdiff*,ex*}
    # recreate gvim symlinks
    (
    cd ${pkgdir}/usr/bin
    for link in eview evim gview gvimdiff rgview rgvim; do
        rm -f ${link}
        ln -s gvim ${link}
    done
    )

    ## Runtime provided by runtime package
    #rm -r ${pkgdir}/usr/share/vim
    # Move the runtime for later packaging
    mv ${pkgdir}/usr/share/vim ${srcdir}/runtime-install

    # Move the man pages for common packaging
    mv ${pkgdir}/usr/share/man ${srcdir}/gvim-man-install

    # remove vim desktop file
    rm ${pkgdir}/usr/share/applications/vim.desktop

    # license
    install -dm755 ${pkgdir}/usr/share/licenses/vim-gvim-gtk3
    install -Dm644 ${srcdir}/license.txt \
        ${pkgdir}/usr/share/licenses/vim-gvim-gtk3/license.txt
}

#package_vim-gvim-qt() {
    #pkgdesc='Vi Improved, qt gui'
    #depends=('vim-cli' 'vim-gvim-common' 'desktop-file-utils' 'qt5-base' 'hicolor-icon-theme' 'shared-mime-info')
    #provides=('gvim')

    #cd ${srcdir}/gvim-build-qt
    #make -j1 VIMRCLOC=/etc DESTDIR=${pkgdir} install

    ## move vim to gvim
    #rm -f ${pkgdir}/usr/bin/gvim
    #mv ${pkgdir}/usr/bin/{vim,qvim}
    ## remove files provided by vim-cli
    #rm -f ${pkgdir}/usr/bin/{vimtutor,xxd,rview,rvim,view,vimdiff,ex}
    #rm -f ${pkgdir}/usr/share/man/*{,/*}/{vim*,vimtutor*,xxd*,rview*,rvim*,view*,vimdiff*,ex*}
    ## move gvimtutor to qvimtutor
    #mv ${pkgdir}/usr/bin/{gvimtutor,qvimtutor}
    ## recreate gvim symlinks
    #(
    #cd ${pkgdir}/usr/bin
    #for link in qview qvimdiff rqview rqvim; do
        #rm -f ${link}
        #ln -s qvim ${link}
    #done
    #)

    ## Move the runtime for later packaging
    #mv ${pkgdir}/usr/share/vim ${srcdir}/runtime-install

    ## remove the man pages for common packaging
    #rm -r ${pkgdir}/usr/share/man

    ## remove vim desktop file
    #rm ${pkgdir}/usr/share/applications/vim.desktop

    ## no need for gvim references
    #find "$pkgdir" -name "gvim.*" -delete

    ## freedesktop links
    #install -Dm644 ${srcdir}/qvim.desktop \
        #${pkgdir}/usr/share/applications/qvim.desktop
    #install -Dm644 ${srcdir}/qvim.png ${pkgdir}/usr/share/pixmaps/qvim.png

    ## license
    #install -dm755 ${pkgdir}/usr/share/licenses/vim-gvim-qt
    #install -Dm644 ${srcdir}/license.txt \
        #${pkgdir}/usr/share/licenses/vim-gvim-x11/license.txt
#}

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

package_vim-gvim-common() {
    pkgdesc='common files for gvim/qvim'

    # Install the common split from gvim/qvim
    install -dm755 ${pkgdir}/usr/share
    mv ${srcdir}/gvim-man-install ${pkgdir}/usr/share/man

    # license
    install -dm755 ${pkgdir}/usr/share/licenses/vim-gvim-common
    install -Dm644 ${srcdir}/license.txt \
        ${pkgdir}/usr/share/licenses/vim-gvim-common/license.txt
}

