#
# Copyright (C) 2019 Andreas Nilsen <adde88@gmail.com>
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=kismet-remote
PKG_VERSION:=2019
PKG_RELEASE:=6

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://www.kismetwireless.net/git/kismet.git
PKG_SOURCE_VERSION:=70bd0c0464ce26b6bce6f1be103747c56ce6af88
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz

PKG_MAINTAINER:=Andreas Nilsen <adde88@gmail.com>

PKG_USE_MIPS16:=0

include $(INCLUDE_DIR)/package.mk

PKG_BUILD_DEPENDS:=+libpthread +libpcap +libnl-tiny +protobuf-c

define Package/kismet-remote
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Kismet Remote Capture (2019)
  URL:=https://www.kismetwireless.net/
  DEPENDS:=+libpthread +libpcap +libnl-tiny +libprotobuf-c
endef

define Package/kismet-remote/description
  Kismet remote capture binary for 2019 Kismet
endef

define Build/Configure
  $(call Build/Configure/Default,--with-linux-headers=$(LINUX_DIR))
endef

CONFIGURE_ARGS += \
	--bindir=/usr/bin \
	--enable-capture-tools-only \
	--disable-python-tools \
	--disable-libusb \
	--with-netlink-tiny \
	--with-protocc=$(STAGING_DIR_HOST)/bin/protoc-c

# Override libs to force lpthread because we don't detect it properly on openwrt
CONFIGURE_VARS += \
	CXXFLAGS="$$$$CXXFLAGS" \
	CFLAGS+="$$$$CFLAGS" \
	LIBS="-lpthread"

TARGET_CFLAGS+=-I$(STAGING_DIR)/usr/include/libnl-tiny
TARGET_CPPFLAGS+=-I$(STAGING_DIR)/usr/include/libnl-tiny

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) \
		LD="$(TARGET_CXX)" \
		datasources
endef

define Package/kismet-remote/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/kismet_cap_pcapfile $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/capture_linux_wifi/kismet_cap_linux_wifi $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/capture_linux_bluetooth/kismet_cap_linux_bluetooth $(1)/usr/bin/
endef

$(eval $(call BuildPackage,kismet-remote))
