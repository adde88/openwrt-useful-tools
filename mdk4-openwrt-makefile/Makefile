#
# Copyright (C) 2009-2014 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=mdk4
PKG_VERSION:=4.1
PKG_RELEASE:=1

PKG_BUILD_DIR:=$(BUILD_DIR)/mdk4-$(PKG_VERSION)
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/adde88/mdk4.git
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
PKG_SOURCE_VERSION:=4f5049bd025a74ca3d73003348ac25e677eacc6e
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)

PKG_MAINTAINER:=Andreas Nilsen <adde88@gmail.com.com>

PKG_BUILD_PARALLEL:=1

MAKE_PATH:=.

PKG_CONFIG_DEPENDS:=\
	CONFIG_MDK4_libnl \
	CONFIG_MDK4_libnl-genl \

include $(INCLUDE_DIR)/package.mk

define Package/mdk4
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=+libpthread +libpcap +libpcre \
		+CONFIG_MDK4_libnl:libnl \
		+CONFIG_MDK4_libnl-genl:libnl-genl +CONFIG_MDK4_libnl-genl:libnl-core
  TITLE:=mdk4
  URL:=https://github.com/aircrack-ng/mdk4
  SUBMENU:=wireless
endef

define Package/mdk4/description
  Tool to exploit wireless vulnerabilities. (DEVELOPER VERSION)
endef

MAKE_FLAGS += \
	$(TARGET_CONFIGURE_OPTS)

TARGET_LDFLAGS+= \
        -static-libstdc++ \
        -Wl,--gc-sections

TARGET_CFLAGS+= \
	-static

#define Build/Compile
#        $(MAKE) -C $(PKG_BUILD_DIR)/ \
#                $(TARGET_CONFIGURE_OPTS) \
#		CFLAGS="$(TARGET_CFLAGS) -static"
#endef

define Package/mdk4/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(CP) $(PKG_BUILD_DIR)/src/mdk4 $(1)/usr/sbin/
endef

$(eval $(call BuildPackage,mdk4))
