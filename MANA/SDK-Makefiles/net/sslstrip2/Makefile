#
# Copyright (C) 2013 CZ.NIC z.s.p.o. (http://www.nic.cz/)
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=sslstrip2
PKG_VERSION:=1.0
PKG_RELEASE:=1

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/byt3bl33d3r/sslstrip2.git
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
PKG_SOURCE_VERSION:=f228f302826cf7b4f18823ba2f8a4b1cca89404e
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)

PKG_MAINTAINER:=Andreas Nilsen <adde88@gmail.com>

PKG_BUILD_PARALLEL:=1

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
PKG_BUILD_DEPENDS:=python

PKG_MAINTAINER:=Andreas Nilsen <adde88@gmail.com.com>

include $(INCLUDE_DIR)/package.mk
$(call include_mk, python-package.mk)

define Package/sslstrip2
	SECTION:=net
	CATEGORY:=Network
	TITLE:=sslstrip2
	URL:=https://github.com/LeonardoNve/sslstrip2
	DEPENDS:=+python +twisted +twisted-web +pyopenssl +dns2proxy
endef

define Package/sslstrip2/description
	With the new feature to avoid HTTP Strict Transport Security (HSTS)
	protection mechanism.
endef

define Build/Compile
	$(call Build/Compile/PyMod,., \
		install --prefix="/usr" --root="$(PKG_INSTALL_DIR)" \
	)
endef

define Package/sslstrip2/install
	$(INSTALL_DIR) $(1)$(PYTHON_PKG_DIR)/
	$(CP) \
		$(PKG_INSTALL_DIR)$(PYTHON_PKG_DIR)/* \
		$(1)$(PYTHON_PKG_DIR)/
endef

$(eval $(call BuildPackage,sslstrip2))
