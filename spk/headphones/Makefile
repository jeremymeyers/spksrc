SPK_NAME = headphones
SPK_VERS = $(shell date +%Y%m%d)
SPK_REV = 11
SPK_ICON = src/headphones.png

DEPENDS =
SPK_DEPENDS = "python310:git"

MAINTAINER = Diaoul
DESCRIPTION = Headphones is an automatic NZB and torrent downloader for music. Mark albums as wanted and have them downloaded as soon as they are available. Different quality settings are possible, including lossless. With Headphones you can also manage your music library automatically: move, tag, rename and index for your DiskStation.
DESCRIPTION_FRE = Headphones permet le téléchargement automatique de musique via NZB ou torrent. Indiquez les albums que vous désirez et ils seront téléchargés dès que possible. Pous pouvez choisir entre différentes qualités, dont le lossless. Avec Headphones, vous pouvez aussi gérer votre bibliothèque musicale automatiquement: déplacement, tag, renommage et indexation pour votre DiskStation.
DESCRIPTION_SPN = Headphones permite descargar automáticamente música mediante NZB o torrent. Puedes marcar albumes y estos serán descargados en cuanto estén disponibles. Es posible utilizar diferentes calidades, incluido lossless \(sin pedida\). Con Headphones tambien puedes administrar tu libreria: mover, taguear, renombrar e indexar en el DiskStation
DISPLAY_NAME = Headphones
CHANGELOG = "1. Update to Python 3.10.<br/>2. Add DSM 7 compatibility."

HOMEPAGE   = https://github.com/rembo10/headphones
LICENSE    = GPLv3
STARTABLE  = yes

SERVICE_USER = auto
SERVICE_SETUP = src/service-setup.sh
SERVICE_PORT = 8181
SERVICE_PORT_TITLE = $(DISPLAY_NAME)

# Admin link for in DSM UI
ADMIN_PORT = $(SERVICE_PORT)

WIZARDS_DIR = src/wizard/

COPY_TARGET = nop
POST_STRIP_TARGET = headphones_extra_install

# Pure Python package, make sure ARCH is not defined
override ARCH=

include ../../mk/spksrc.spk.mk

.PHONY: headphones_extra_install
headphones_extra_install: $(STAGING_DIR)/share/Headphones
	install -m 755 -d $(STAGING_DIR)/var
	install -m 600 src/config.ini $(STAGING_DIR)/var/config.ini

$(STAGING_DIR)/share/Headphones:
	install -m 755 -d $(STAGING_DIR)/share
	cd $(STAGING_DIR)/share && git clone --depth=1 https://github.com/rembo10/headphones.git Headphones
