#
# Copyright (C) 2017 - 2018 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=asterisk-chan-dongle

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/wdoekes/asterisk-chan-dongle.git
PKG_SOURCE_VERSION:=328b2b7ded3b3c59c2e6c7e7afca6b0ac6f146ce
PKG_SOURCE_DATE=2020-05-28
PKG_RELEASE:=1
PKG_MIRROR_HASH:=b0af444840340722ecee111a7b1f819de441acf290799ae11bc18e16569c6388

PKG_FIXUP:=autoreconf

PKG_LICENSE:=GPL-2.0
PKG_LICENSE_FILES:=COPYRIGHT.txt LICENSE.txt
PKG_MAINTAINER:=Jiri Slachta <jiri@slachta.eu>

MODULES_DIR:=/usr/lib/asterisk/modules

include $(INCLUDE_DIR)/package.mk
# asterisk-chan-dongle needs iconv
include $(INCLUDE_DIR)/nls.mk

define Package/asterisk-chan-dongle/Default
  SUBMENU:=Telephony
  SECTION:=net
  CATEGORY:=Network
  URL:=https://github.com/wdoekes/asterisk-chan-dongle
  DEPENDS:=$(ICONV_DEPENDS) +libsqlite3
  TITLE:=Huawei UMTS 3G dongle support
endef

define Package/asterisk16-chan-dongle
$(call Package/asterisk-chan-dongle/Default)
  DEPENDS+=asterisk16
  VARIANT:=asterisk16
endef

define Package/description/Default
 Asterisk channel driver for Huawei UMTS 3G dongle.
endef

Package/asterisk16-chan-dongle/description = $(Package/description/Default)

ifeq ($(BUILD_VARIANT),asterisk16)
  CHAN_DONGLE_AST_HEADERS:=$(STAGING_DIR)/usr/include/asterisk-16/include
  CONFIGURE_ARGS+= \
	  --with-astversion=16
endif

CONFIGURE_ARGS+= \
	--with-asterisk=$(CHAN_DONGLE_AST_HEADERS) \
	--with-iconv=$(ICONV_PREFIX)/include

TARGET_CFLAGS+= \
	-I$(CHAN_DONGLE_AST_HEADERS)

MAKE_FLAGS+=LD="$(TARGET_CC)"

CONFIGURE_VARS += \
	DESTDIR="$(MODULES_DIR)" \
	ac_cv_type_size_t=yes \
	ac_cv_type_ssize_t=yes

define Package/conffiles/Default
/etc/asterisk/dongle.conf
endef

Package/asterisk16-chan-dongle/conffiles = $(Package/conffiles/Default)

define Package/Install/Default
	$(INSTALL_DIR) $(1)/etc/asterisk
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/etc/dongle.conf $(1)/etc/asterisk
	$(INSTALL_DIR) $(1)$(MODULES_DIR)
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/chan_dongle.so $(1)$(MODULES_DIR)
endef

Package/asterisk16-chan-dongle/install = $(Package/Install/Default)

$(eval $(call BuildPackage,asterisk16-chan-dongle))
