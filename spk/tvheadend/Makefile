SPK_NAME = tvheadend
SPK_SHORT_VERS = 4.3
SPK_GIT_HASH = d885496
SPK_GIT_DATE = 20221128
SPK_VERS = $(SPK_SHORT_VERS).$(SPK_GIT_DATE)
TVH_VERS = $(SPK_SHORT_VERS)~$(SPK_GIT_HASH)
SPK_REV = 34
SPK_ICON = src/tvheadend.png
DSM_UI_DIR = app

# Export version to the build where version
# script gets patched accordingly by:
# 000-fix-version.patch from cross/tvheadend
export TVH_VERS

SPK_DEPENDS = "python310"
WHEELS = src/requirements-pure.txt

DEPENDS = cross/$(SPK_NAME)
DEPENDS += cross/dtv-scan-tables
DEPENDS += cross/zap2epg cross/docker-tvheadend

MAINTAINER = th0ma7
DESCRIPTION = Tvheadend is a TV streaming server and recorder for Linux, FreeBSD and Android supporting DVB-S, DVB-S2, DVB-C, DVB-T, ATSC, ISDB-T, IPTV, SAT IP and HDHomeRun as input sources. Tvheadend offers HTTP, HTSP and SAT IP streaming.
DESCRIPTION_FRE = Tvheadend est un serveur de streaming et enregistreur TV pour Linux, FreeBSD et Android prenant en charge DVB-S, DVB-S2, DVB-C, DVB-T, ATSC, ISDB-T, IPTV, SAT IP et HDHomeRun comme sources d\'entrée. Tvheadend offre le streaming HTTP, HTSP et SAT IP.
DISPLAY_NAME = Tvheadend
STARTABLE = yes
CHANGELOG = "1. Update to latest git version d885496 as of November 28th 2022<br/>2. Update openssl to 1.1.1s<br/>3. Updated to use FFMPEG 4.4.3<br/>4. Updated libhdhomerun to version 20221031<br/>5. iptv_auto: Add support for m3u channel-number tag with HDhomeRun<br/>6. Include enhanced curl<br/>7. Use newer libdvbcsa with CPU optimizations<br/>8. Add OSCam support"
HOMEPAGE = https://tvheadend.org/
LICENSE = GPL v3

# PPC archs except qoriq are not supported
# https://tvheadend.org/issues/5060
UNSUPPORTED_ARCHS = $(OLD_PPC_ARCHS)

CONF_DIR = src/conf/

# 'auto' reserved value grabs SPK_NAME
SERVICE_USER = auto
SERVICE_SETUP = src/service-setup.sh

# Service configuration
SERVICE_PORT = 9981
SERVICE_PORT_TITLE = Tvheadend(HTTP)

# Admin link
ADMIN_PORT = ${SERVICE_PORT}

# Reuse ffmpeg libraries
# Requires to invoke: make [package|publish] ARCH= TCVERSION=
export FFMPEG_DIR = $(shell pwd)/../ffmpeg/work-$(ARCH)-$(TCVERSION)/install/var/packages/ffmpeg/target

ifneq ($(wildcard $(FFMPEG_DIR)),)
PRE_DEPEND_TARGET = tvheadend_pre_depend
SPK_DEPENDS := "$(SPK_DEPENDS):ffmpeg>4.4"
endif

include ../../mk/spksrc.spk.mk

.PHONY: tvheadend_pre_depend tvheadend_extra_install

# Use FFMPEG package shared libraries
FFMPEG_LIBS = libavfilter.pc libpostproc.pc libswresample.pc libavresample.pc libswscale.pc libavutil.pc libavformat.pc libavcodec.pc

# Use other media libraries through FFMPEG package shared libraries
MEDIA_LIBS = fdk-aac.pc numa.pc ogg.pc opus.pc theoradec.pc theoraenc.pc theora.pc vorbisenc.pc vorbisfile.pc vorbis.pc vpx.pc x264.pc x265.pc

# Add Intel VAAPI libraries on Intel platforms
ifeq ($(findstring $(ARCH),$(x64_ARCHS)),$(ARCH))
VAAPI_LIBS = libdrm.pc libdrm_intel.pc libkms.pc libva.pc libva-drm.pc pciaccess.pc
endif

tvheadend_pre_depend:
	mkdir -p $(STAGING_INSTALL_PREFIX)/lib/pkgconfig/
	$(foreach lib,$(FFMPEG_LIBS),ln -sfT $(FFMPEG_DIR)/lib/pkgconfig/$(lib) $(STAGING_INSTALL_PREFIX)/lib/pkgconfig/$(lib) ;)
	$(foreach lib,$(MEDIA_LIBS),ln -sfT $(FFMPEG_DIR)/lib/pkgconfig/$(lib) $(STAGING_INSTALL_PREFIX)/lib/pkgconfig/$(lib) ;)
	$(foreach lib,$(VAAPI_LIBS),ln -sfT $(FFMPEG_DIR)/lib/pkgconfig/$(lib) $(STAGING_INSTALL_PREFIX)/lib/pkgconfig/$(lib) ;)
