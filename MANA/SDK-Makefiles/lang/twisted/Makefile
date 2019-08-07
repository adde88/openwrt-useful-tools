# 
# Copyright (C) 2006-2011 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=twisted
PKG_VERSION:=2.5.0
PKG_RELEASE:=1

PKG_SOURCE:=Twisted-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=http://tmrc.mit.edu/mirror/twisted/Twisted/2.5
PKG_MD5SUM:=56df2ae66db57932515a491c03bf514f

PKG_BUILD_DIR:=$(BUILD_DIR)/Twisted-$(PKG_VERSION)
PKG_BUILD_DEPENDS:=python

PKG_MAINTAINER:=Andreas Nilsen <adde88@gmail.com>

include $(INCLUDE_DIR)/package.mk
$(call include_mk, python-package.mk)

define Package/twisted/default
  SUBMENU:=Python
  SECTION:=lang
  CATEGORY:=Languages
  URL:=http://twistedmatrix.com/
  DEPENDS:=+python
endef

define Package/twisted/default/description
 Twisted is a networking engine written in Python, supporting numerous 
 protocols. It contains a web server, numerous chat clients, chat servers, 
 mail servers, and more.
endef

define Package/twisted
$(call Package/twisted/default)
  TITLE:=Python networking engine
  DEPENDS+= +zope-interface
endef

define Package/twisted-conch
$(call Package/twisted/default)
  TITLE:=Twisted - Conch
  DEPENDS+= twisted
endef

define Package/twisted-lore
$(call Package/twisted/default)
  TITLE:=Twisted - Lore
  DEPENDS+= twisted
endef

define Package/twisted-mail
$(call Package/twisted/default)
  TITLE:=Twisted - Mail
  DEPENDS+= twisted
endef

define Package/twisted-names
$(call Package/twisted/default)
  TITLE:=Twisted - Names
  DEPENDS+= twisted
endef

define Package/twisted-news
$(call Package/twisted/default)
  TITLE:=Twisted - News
  DEPENDS+= twisted
endef

define Package/twisted-runner
$(call Package/twisted/default)
  TITLE:=Twisted - Runner
  DEPENDS+= twisted
endef

define Package/twisted-web
$(call Package/twisted/default)
  TITLE:=Twisted - Web
  DEPENDS+= twisted
endef

define Package/twisted-words
$(call Package/twisted/default)
  TITLE:=Twisted - Words
  DEPENDS+= twisted
endef

define Package/zope-interface
$(call Package/twisted/default)
  TITLE:=Zope interface
  DEPENDS+= python
endef

TWISTED_SUPPROJECTS:=core

ZOPE_IF_VER=3.3.0

ifneq ($(SDK)$(CONFIG_PACKAGE_twisted-conch),)
  TWISTED_SUBPROJECTS+=conch
endif
ifneq ($(SDK)$(CONFIG_PACKAGE_twisted-lore),)
  TWISTED_SUBPROJECTS+=lore
endif
ifneq ($(SDK)$(CONFIG_PACKAGE_twisted-mail),)
  TWISTED_SUBPROJECTS+=mail
endif
ifneq ($(SDK)$(CONFIG_PACKAGE_twisted-names),)
  TWISTED_SUBPROJECTS+=names
endif
ifneq ($(SDK)$(CONFIG_PACKAGE_twisted-news),)
  TWISTED_SUBPROJECTS+=news
endif
ifneq ($(SDK)$(CONFIG_PACKAGE_twisted-runner),)
  TWISTED_SUBPROJECTS+=runner
endif
ifneq ($(SDK)$(CONFIG_PACKAGE_twisted-web),)
  TWISTED_SUBPROJECTS+=web
endif
ifneq ($(SDK)$(CONFIG_PACKAGE_twisted-words),)
  TWISTED_SUBPROJECTS+=words
endif


define Build/Configure
endef

define PyPackage/twisted/filespec
+|/usr/bin/mktap
+|/usr/bin/twistd
+|$(PYTHON_PKG_DIR)/twisted/application
+|$(PYTHON_PKG_DIR)/twisted/copyright.py
+|$(PYTHON_PKG_DIR)/twisted/cred
+|$(PYTHON_PKG_DIR)/twisted/enterprise
+|$(PYTHON_PKG_DIR)/twisted/im.py
+|$(PYTHON_PKG_DIR)/twisted/__init__.py
+|$(PYTHON_PKG_DIR)/twisted/internet
+|$(PYTHON_PKG_DIR)/twisted/manhole
+|$(PYTHON_PKG_DIR)/twisted/persisted
+|$(PYTHON_PKG_DIR)/twisted/plugin.py
+|$(PYTHON_PKG_DIR)/twisted/protocols
+|$(PYTHON_PKG_DIR)/twisted/python
+|$(PYTHON_PKG_DIR)/twisted/scripts
+|$(PYTHON_PKG_DIR)/twisted/spread
+|$(PYTHON_PKG_DIR)/twisted/tap
+|$(PYTHON_PKG_DIR)/twisted/trial
-|$(PYTHON_PKG_DIR)/twisted/trial/test
+|$(PYTHON_PKG_DIR)/twisted/_version.py
+|$(PYTHON_PKG_DIR)/twisted/plugins/dropin.cache
+|$(PYTHON_PKG_DIR)/twisted/plugins/__init__.py
+|$(PYTHON_PKG_DIR)/twisted/plugins/notestplugin.py
+|$(PYTHON_PKG_DIR)/twisted/plugins/testplugin.py
+|$(PYTHON_PKG_DIR)/twisted/plugins/twisted_ftp.py
+|$(PYTHON_PKG_DIR)/twisted/plugins/twisted_inet.py
+|$(PYTHON_PKG_DIR)/twisted/plugins/twisted_manhole.py
+|$(PYTHON_PKG_DIR)/twisted/plugins/twisted_portforward.py
+|$(PYTHON_PKG_DIR)/twisted/plugins/twisted_reactors.py
+|$(PYTHON_PKG_DIR)/twisted/plugins/twisted_socks.py
+|$(PYTHON_PKG_DIR)/twisted/plugins/twisted_telnet.py
+|$(PYTHON_PKG_DIR)/twisted/plugins/twisted_trial.py
endef

define PyPackage/twisted-conch/filespec
+|$(PYTHON_PKG_DIR)/twisted/conch
-|$(PYTHON_PKG_DIR)/twisted/conch/test
+|$(PYTHON_PKG_DIR)/twisted/plugins/twisted_conch.py
endef

define PyPackage/twisted-lore/filespec
+|$(PYTHON_PKG_DIR)/twisted/lore
-|$(PYTHON_PKG_DIR)/twisted/lore/test
+|$(PYTHON_PKG_DIR)/twisted/plugins/twisted_lore.py
endef

define PyPackage/twisted-mail/filespec
+|$(PYTHON_PKG_DIR)/twisted/mail
-|$(PYTHON_PKG_DIR)/twisted/mail/test
+|$(PYTHON_PKG_DIR)/twisted/plugins/twisted_mail.py
endef

define PyPackage/twisted-names/filespec
+|$(PYTHON_PKG_DIR)/twisted/names
-|$(PYTHON_PKG_DIR)/twisted/names/test
+|$(PYTHON_PKG_DIR)/twisted/plugins/twisted_names.py
endef

define PyPackage/twisted-news/filespec
+|$(PYTHON_PKG_DIR)/twisted/news
-|$(PYTHON_PKG_DIR)/twisted/news/test
+|$(PYTHON_PKG_DIR)/twisted/plugins/twisted_news.py
endef

define PyPackage/twisted-runner/filespec
+|$(PYTHON_PKG_DIR)/twisted/runner
endef

define PyPackage/twisted-web/filespec
+|$(PYTHON_PKG_DIR)/twisted/web
-|$(PYTHON_PKG_DIR)/twisted/web/test
+|$(PYTHON_PKG_DIR)/twisted/plugins/twisted_web.py
endef

define PyPackage/twisted-words/filespec
+|$(PYTHON_PKG_DIR)/twisted/words
-|$(PYTHON_PKG_DIR)/twisted/words/test
+|$(PYTHON_PKG_DIR)/twisted/plugins/twisted_words.py
endef

define PyPackage/zope-interface/filespec
+|$(PYTHON_PKG_DIR)/zope
-|$(PYTHON_PKG_DIR)/zope/interface/tests
endef

define Build/Compile
	$(if $(Build/Compile/PyMod),,@echo Python packaging code not found.; false)
	$(call Build/Compile/PyMod,., \
		install --prefix="/usr" --root="$(PKG_INSTALL_DIR)" --no-compile, \
		TWISTED_SUBPROJECTS="$(TWISTED_SUBPROJECTS)" \
	)
	$(call Build/Compile/PyMod,zope.interface-$(ZOPE_IF_VER), \
		install --prefix="/usr" --root="$(PKG_INSTALL_DIR)" --no-compile, \
	)
	$(SED) 's,#!$(PYTHON),#!/usr/bin/python,' $(PKG_INSTALL_DIR)/usr/bin/*
endef

$(eval $(call PyPackage,twisted))
$(eval $(call PyPackage,twisted-conch))
$(eval $(call PyPackage,twisted-lore))
$(eval $(call PyPackage,twisted-mail))
$(eval $(call PyPackage,twisted-names))
$(eval $(call PyPackage,twisted-news))
$(eval $(call PyPackage,twisted-runner))
$(eval $(call PyPackage,twisted-web))
$(eval $(call PyPackage,twisted-words))
$(eval $(call PyPackage,zope-interface))

$(eval $(call BuildPackage,twisted))
$(eval $(call BuildPackage,twisted-conch))
$(eval $(call BuildPackage,twisted-lore))
$(eval $(call BuildPackage,twisted-mail))
$(eval $(call BuildPackage,twisted-names))
$(eval $(call BuildPackage,twisted-news))
$(eval $(call BuildPackage,twisted-runner))
$(eval $(call BuildPackage,twisted-web))
$(eval $(call BuildPackage,twisted-words))
$(eval $(call BuildPackage,zope-interface))
