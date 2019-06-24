#
# Copyright (C) 2007-2014 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=pcapy
PKG_VERSION:=0.10.9
PKG_RELEASE:=1
PKG_LICENSE:=Apache Modified

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://pypi.python.org/packages/source/p/pcapy
PKG_MD5SUM:=b91b423b0efbcffbe516d369f5d77ada

PKG_MAINTAINER:=Andreas Nilsen <adde88@gmail.com>

PKG_BUILD_DEPENDS:=python python-setuptools libpcap

include $(INCLUDE_DIR)/package.mk
$(call include_mk, python-package.mk)

define Package/python-pcapy
  SUBMENU:=Python
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=Python interface to the libpcap packet capture library
  URL:=https://pypi.python.org/pypi/pcapy
  MAINTAINER:=Dmitry Kireev<dmitry@kireev.co>
  DEPENDS:=+python +libpcap +libstdcpp
endef

define Package/python-pcapy/description
 Pcapy is a Python extension module that interfaces with the libpcap packet capture library.
endef

define Build/Compile
	$(call Build/Compile/PyMod,., \
		install --prefix="/usr" --root="$(PKG_INSTALL_DIR)", \
	)
endef

define Package/python-pcapy/install
	$(INSTALL_DIR) $(1)$(PYTHON_PKG_DIR)
	$(CP) $(PKG_INSTALL_DIR)$(PYTHON_PKG_DIR)/* $(1)$(PYTHON_PKG_DIR)
endef

$(eval $(call BuildPackage,python-pcapy))
