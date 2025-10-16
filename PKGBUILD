pkgname=ffmpeg
pkgver=8.0
pkgrel=3
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
    'gsm'
    'harfbuzz'
    'lame'
    'libaom'
    'libass'
    'libavc1394'
    'libbluray'
    'libbs2b'
    'libdrm'
    'libdvdnav'
    'libdvdread'
    'libglvnd'
    'libiec61883'
    'libjxl'
    'libmodplug'
    'libopenmpt'
    'libplacebo'
    'libpulse'
    'libraw1394'
    'librsvg'
    'libsoxr'
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
    'pipewire-jack'
    'rav1e'
    'rubberband'
    'snappy'
    'speex'
    'srt'
    'svt-av1'
    'v4l-utils'
    'vapoursynth'
    'vid.stab'
    'vmaf'
    'vulkan-loader'
    'x264'
    'x265'
    'xvidcore'
    'xz'
    'zeromq'
    'zimg'
    'zlib'
)
makedepends=(
    'amf-headers'
    'avisynthplus'
    'clang'
    'ffnvcodec-headers'
    'frei0r-plugins'
    'git'
    'ladspa'
    'mesa'
    'nasm'
    'opencl-headers'
    'vulkan-headers'
)
source=(git+https://git.ffmpeg.org/ffmpeg.git#tag=a4044e04486d1136022498891088a90baf5b2775
    0001-Add-av_stream_get_first_dts-for-Chromium.patch
    0001-unbreak-glslang-build.patch)
sha256sums=(3c7a424271b300ba626ca1d75f3bf203a39ed3b9003f0faaf80067e1278fc758
    f865d677f8ad39c79dde69186629cb6468c2b289c4156dbb8dec8e68b0131b40
    77ea85385f8eafd1f91206a8a1e0523c75186d45f9565ab1ac1921a23397bc91)

pkgver() {
    cd ${pkgname}

    git describe --tags | sed 's/^n//'
}

prepare() {
    cd ${pkgname}

    # https://crbug.com/1251779
    git apply -3 ${srcdir}/0001-Add-av_stream_get_first_dts-for-Chromium.patch

    # https://github.com/FFmpeg/FFmpeg/commit/f1e9032a2000b8b885cffd6fed8eacd47b37673f
    git apply -3 ${srcdir}/0001-unbreak-glslang-build.patch

}

build() {
    cd ${pkgname}

    local configure_args=(
        --prefix=/usr
        --libdir=/usr/lib64
        --disable-debug
        --disable-static
        --disable-stripping
        --enable-amf
        --enable-avisynth
        --enable-cuda-llvm
        --enable-lto
        --enable-fontconfig
        --enable-frei0r
        --enable-gmp
        --enable-gnutls
        --enable-gpl
        --enable-ladspa
        --enable-libaom
        --enable-libass
        --enable-libbluray
        --enable-libbs2b
        --enable-libdav1d
        --enable-libdrm
        --enable-libdvdnav
        --enable-libdvdread
        --enable-libfreetype
        --enable-libfribidi
        --enable-libglslang
        --enable-libgsm
        --enable-libharfbuzz
        --enable-libiec61883
        --enable-libjack
        --enable-libjxl
        --enable-libmodplug
        --enable-libmp3lame
        --enable-libopencore_amrnb
        --enable-libopencore_amrwb
        --enable-libopenjpeg
        --enable-libopenmpt
        --enable-libopus
        --enable-libplacebo
        --enable-libpulse
        --enable-librav1e
        --enable-librsvg
        --enable-librubberband
        --enable-libsnappy
        --enable-libsoxr
        --enable-libspeex
        --enable-libsrt
        --enable-libssh
        --enable-libsvtav1
        --enable-libtheora
        --enable-libv4l2
        --enable-libvidstab
        --enable-libvmaf
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
        --enable-libzmq
        --enable-nvdec
        --enable-nvenc
        --enable-opencl
        --enable-opengl
        --enable-shared
        --enable-vapoursynth
        --enable-version3
        --enable-vulkan
        --docdir=/usr/share/doc/${pkgname}-${pkgver}
        --cc="${CHOST}-gcc"
        --cxx="${CHOST}-g++"
    )

    export PKG_CONFIG_PATH='/usr/lib64/mbedtls2/pkgconfig'

    ./configure "${configure_args[@]}"

    make
    make tools/qt-faststart
    make doc/ff{mpeg,play}.1
}

package() {
    cd ${pkgname}

    make DESTDIR=${pkgdir} install install-man

    install -Dm 755 tools/qt-faststart ${pkgdir}/usr/bin/

    install -vdm755 ${pkgdir}/usr/share/doc/${pkgname}-${pkgver}
    install -vm644 doc/*.txt ${pkgdir}/usr/share/doc/${pkgname}-${pkgver}
}
