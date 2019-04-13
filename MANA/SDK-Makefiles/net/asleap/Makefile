#
# Copyright (C) 2006-2014 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=asleap
PKG_VERSION:=2.2
PKG_RELEASE:=1

PKG_SOURCE:=asleap-$(PKG_VERSION).tgz
PKG_SOURCE_URL:=http://www.willhackforsushi.com/code/asleap/2.2/

PKG_MD5SUM:=a1d06729fb2addcc5b09bfc14f9b3173
PKG_CAT:=zcat

PKG_BUILD_DIR:=$(BUILD_DIR)/asleap-$(PKG_VERSION)

PKG_MAINTAINER:=Andreas Nilsen <adde88@gmail.com.com>, Original asleap package: Josh <jwright@hasborg.com>

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

define Package/asleap
	SECTION:=base
	CATEGORY:=Network
	TITLE:=Generic MS-CHAPv2 cracking tool, with pcap support.
	URL:=http://www.willhackforsushi.com
	DEPENDS:=+libpthread +libopenssl +libpcap
endef

define Package/asleap/description
	A generic MS-CHAPv2 cracking tool.
	Can be applied anytime you have a MS-CHAPv2 packet capture available.
endef

define Build/RunMake
	CFLAGS="$(TARGET_CPPFLAGS) $(TARGET_CFLAGS)" \
	$(MAKE) $(PKG_JOBS) -C $(PKG_BUILD_DIR)/$(1) \
		$(TARGET_CONFIGURE_OPTS) \
		LIBS="$(TARGET_LDFLAGS)" \
		LIBS_c="$(TARGET_LDFLAGS_C)" \
		BCHECK= \
		$(2)
endef

define Package/asleap/install
		$(INSTALL_DIR) $(1)/usr/sbin
		$(INSTALL_BIN) $(PKG_BUILD_DIR)/asleap $(1)/usr/sbin/
endef

$(eval $(call BuildPackage,asleap))