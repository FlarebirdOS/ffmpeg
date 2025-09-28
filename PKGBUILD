pkgname=ffmpeg
pkgver=8.0
pkgrel=1
pkgdesc="Complete solution to record, convert and stream audio and video"
arch=('x86_64')
url="https://ffmpeg.org"
license=('GPL-3.0-only')
depends=(
    'alsa-lib'
    'bzip2'
    'cairo'
    'dav1d'
    'fontconfig'
    'freetype2'
    'fribidi'
    'glib2'
    'glibc'
    'glslang'
    'gmp'
    'gnutls'
    'harfbuzz'
    'lame'
    'libaom'
    'libass'
    'libbluray'
    'libdrm'
    'libglvnd'
    'libjxl'
    'libplacebo'
    'libpulse'
    'librsvg'
    'libssh'
    'libtheora'
    'libva'
    'libvdpau'
    'libvorbis'
    'libvpl'
    'libvpx'
    'libwebp'
    'libx11'
    'libxcb'
    'libxext'
    'libxml2'
    'libxv'
    'ocl-icd'
    'opencore-amr'
    'openjpeg'
    'opus'
    'rav1e'
    'sdl2'
    'speex'
    'srt'
    'svt-av1'
    'v4l-utils'
    'vapoursynth'
    'vulkan-loader'
    'x264'
    'x265'
    'xvidcore'
    'xz'
    'zimg'
    'zlib'
)
makedepends=(
    'ffnvcodec-headers'
    'mesa'
    'nasm'
    'opencl-headers'
    'vulkan-headers'
)
source=(https://ffmpeg.org/releases/${pkgname}-${pkgver}.tar.xz
    0001-Add-av_stream_get_first_dts-for-Chromium.patch)
sha256sums=(b2751fccb6cc4c77708113cd78b561059b6fa904b24162fa0be2d60273d27b8e
    f865d677f8ad39c79dde69186629cb6468c2b289c4156dbb8dec8e68b0131b40)

prepare() {
    cd ${pkgname}-${pkgver}

    patch -Np1 -i ${srcdir}/0001-Add-av_stream_get_first_dts-for-Chromium.patch
}

build() {
    cd ${pkgname}-${pkgver}

    local configure_args=(
        --prefix=/usr
        --libdir=/usr/lib64
        --disable-debug
        --disable-static
        --disable-stripping
        --enable-gpl
        --enable-version3
        --enable-shared
        --enable-lto
        --enable-fontconfig
        --enable-gmp
        --enable-gnutls
        --enable-libaom
        --enable-libass
        --enable-libbluray
        --enable-libdav1d
        --enable-libdrm
        --enable-libfreetype
        --enable-libfribidi
        --enable-libglslang
        --enable-libharfbuzz
        --enable-libjxl
        --enable-libmp3lame
        --enable-libopencore_amrnb
        --enable-libopencore_amrwb
        --enable-libopenjpeg
        --enable-libopus
        --enable-libplacebo
        --enable-libpulse
        --enable-librav1e
        --enable-librsvg
        --enable-libspeex
        --enable-libsrt
        --enable-libssh
        --enable-libsvtav1
        --enable-libtheora
        --enable-libv4l2
        --enable-libvorbis
        --enable-libvpl
        --enable-libvpx
        --enable-libwebp
        --enable-libx264
        --enable-libx265
        --enable-libxcb
        --enable-libxml2
        --enable-libxvid
        --enable-libzimg
        --enable-nvdec
        --enable-nvenc
        --enable-opencl
        --enable-opengl
        --enable-vapoursynth
        --enable-vulkan
        --docdir=/usr/share/doc/${pkgname}-${pkgver}
    )

    ./configure "${configure_args[@]}"

    make
}

package() {
    cd ${pkgname}-${pkgver}

    make DESTDIR=${pkgdir} install install-man

    install -vdm755 ${pkgdir}/usr/share/doc/${pkgname}-${pkgver}
    install -vm644 doc/*.txt ${pkgdir}/usr/share/doc/${pkgname}-${pkgver}
}
