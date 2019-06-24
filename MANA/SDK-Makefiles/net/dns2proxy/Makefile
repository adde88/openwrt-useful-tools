
#
# Copyright (C) 2011 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=dns2proxy
PKG_VERSION:=1.0
PKG_RELEASE:=3

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/LeonardoNve/dns2proxy.git
PKG_SOURCE_VERSION:=5d20f2cdf569956e0a9ccae07c53333acbfb4b65
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk
$(call include_mk, python-package.mk)

define Package/dns2proxy
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=+python +python-dns +python-pcapy
  TITLE:=Offensive DNS server
  URL:=https://github.com/LeonardoNve/dns2proxy
  MAINTAINER:=Andreas Nilsen <adde88@gmail.com>
endef

define Package/dns2proxy/description
  This tools offer a different features for post-explotation once you change
  the DNS server to a Victim. <Referer to help (-h) to new params options>
endef

define Build/Compile
endef

define Package/dns2proxy/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_DIR) $(1)/etc/dns2proxy/
	$(CP) $(PKG_BUILD_DIR)/dns2proxy.py	$(1)/usr/bin/dns2proxy
	$(CP) $(PKG_BUILD_DIR)/*.cfg		$(1)/etc/dns2proxy/
	$(CP) $(PKG_BUILD_DIR)/handler_msg.sh	$(1)/etc/dns2proxy/
	$(CP) $(PKG_BUILD_DIR)/resolv.conf	$(1)/etc/dns2proxy/
endef

$(eval $(call BuildPackage,dns2proxy))
