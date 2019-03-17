#
# Copyright (C) 2006-2014 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=hostapd-mana
PKG_VERSION:=2.6.5
PKG_RELEASE:=4-ssl

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/adde88/hostapd-mana.git
PKG_SOURCE_VERSION:=238699c4f6d97f924bd3364b41d6e683d8341403
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
#PKG_HASH:=d93179b3a9a0c1af2e3c0a8a756b381678afab354004a89f0e5aabdfcac34961

PKG_MAINTAINER:=Andreas Nilsen <adde88@gmail.com.com>

PKG_BUILD_PARALLEL:=1

PKG_CONFIG_DEPENDS:= \
	CONFIG_WPA_SUPPLICANT_NO_TIMESTAMP_CHECK \
	CONFIG_PACKAGE_kmod-ath9k \
	CONFIG_PACKAGE_kmod-cfg80211 \
	CONFIG_PACKAGE_hostapd \
	CONFIG_PACKAGE_hostapd-mini \
	CONFIG_PACKAGE_kmod-hostap \
	CONFIG_DRIVER_WEXT_SUPPORT \
	CONFIG_DRIVER_11N_SUPPORT \
	CONFIG_DRIVER_11AC_SUPPORT \
	CONFIG_TAXONOMY_SUPPORT

LOCAL_TYPE=$(strip \
		$(if $(findstring wpad,$(BUILD_VARIANT)),wpad, \
		$(if $(findstring supplicant,$(BUILD_VARIANT)),supplicant, \
		hostapd \
		)))

LOCAL_AND_LIB_VARIANT=$(patsubst hostapd-%,%,\
			$(patsubst wpad-%,%,\
			$(patsubst supplicant-%,%,\
			$(BUILD_VARIANT)\
			)))

LOCAL_VARIANT=$(patsubst %-internal,%,\
		$(patsubst %-openssl,%,\
		$(patsubst %-wolfssl,%,\
		$(LOCAL_AND_LIB_VARIANT)\
		)))

SSL_VARIANT=$(strip \
		$(if $(findstring openssl,$(LOCAL_AND_LIB_VARIANT)),openssl,\
		$(if $(findstring wolfssl,$(LOCAL_AND_LIB_VARIANT)),wolfssl,\
		internal\
		)))

ifeq ($(LOCAL_TYPE),supplicant)
  ifeq ($(LOCAL_VARIANT),full)
    PKG_CONFIG_DEPENDS += \
		CONFIG_WPA_SUPPLICANT_INTERNAL \
		CONFIG_WPA_SUPPLICANT_OPENSSL
  endif
endif

include $(INCLUDE_DIR)/package.mk

STAMP_CONFIGURED:=$(STAMP_CONFIGURED)_$(CONFIG_WPA_MSG_MIN_PRIORITY)

ifneq ($(CONFIG_DRIVER_11N_SUPPORT),)
  HOSTAPD_IEEE80211N:=y
endif

ifneq ($(CONFIG_DRIVER_11AC_SUPPORT),)
  HOSTAPD_IEEE80211AC:=y
endif

ifneq ($(CONFIG_TAXONOMY),)
  CONFIG_TAXONOMY:=y
endif

DRIVER_MAKEOPTS= \
	CONFIG_ACS=$(CONFIG_PACKAGE_kmod-cfg80211) \
	CONFIG_DRIVER_NL80211=$(CONFIG_PACKAGE_kmod-cfg80211) \
	CONFIG_IEEE80211N=$(HOSTAPD_IEEE80211N) \
	CONFIG_IEEE80211AC=$(HOSTAPD_IEEE80211AC) \
	CONFIG_DRIVER_WEXT=$(CONFIG_DRIVER_WEXT_SUPPORT) \
	CONFIG_TAXONOMY=$(CONFIG_TAXONOMY) \
	$(if $(CONFIG_WPA_RFKILL_SUPPORT),NEED_RFKILL=y) \

space :=
space +=

ifneq ($(LOCAL_TYPE),hostapd)
  ifeq ($(LOCAL_VARIANT),mesh)
	ifeq ($(SSL_VARIANT),openssl)
	  DRIVER_MAKEOPTS += CONFIG_TLS=openssl CONFIG_AP=y CONFIG_SAE=y CONFIG_MESH=y
	  TARGET_LDFLAGS += -lcrypto -lssl
	endif
	ifeq ($(SSL_VARIANT),wolfssl)
	  DRIVER_MAKEOPTS += CONFIG_TLS=wolfssl CONFIG_WPS_NFC=1 CONFIG_AP=y CONFIG_SAE=y CONFIG_MESH=y
	  TARGET_LDFLAGS += -lwolfssl
  	endif
  endif

  ifdef CONFIG_WPA_SUPPLICANT_NO_TIMESTAMP_CHECK
    TARGET_CFLAGS += -DNO_TIMESTAMP_CHECK
  endif
  ifdef CONFIG_WPA_RFKILL_SUPPORT
    DRIVER_MAKEOPTS += NEED_RFKILL=y
  endif
  DRIVER_MAKEOPTS += \
	CONFIG_DRIVER_ROBOSWITCH=$(CONFIG_PACKAGE_kmod-switch)
endif

ifdef CONFIG_USE_GLIBC
  TARGET_LDFLAGS += -lrt
  TARGET_LDFLAGS_C += -lrt
endif

DRV_DEPENDS:=+PACKAGE_kmod-cfg80211:libnl-tiny

define Package/hostapd-mana/config
  source "$(SOURCE)/Config.in"
endef

define Package/hostapd-mana-light/config
  source "$(SOURCE)/Config.in"
endef

define Package/hostapd-mana/Default
  SECTION:=net
  CATEGORY:=Network
  TITLE:=IEEE 802.1x Authenticator with support for the MANA and Karma attacks.
  DEPENDS:=$(DRV_DEPENDS) +libubus +libopenssl
endef

define Package/hostapd-mana-light/Default
  SECTION:=net
  CATEGORY:=Network
  TITLE:=IEEE 802.1x Authenticator with support for the MANA and Karma attacks. (ONLY MANA BINARIES)
  DEPENDS:=$(DRV_DEPENDS) +libubus +libopenssl
endef

define Package/hostapd-mana
$(call Package/hostapd-mana/Default)
  TITLE+= (full)
  VARIANT:=full
endef

define Package/hostapd-mana-light
$(call Package/hostapd-mana-light/Default)
  TITLE+= (full)
  VARIANT:=full
endef

define Package/hostapd-mana/description
 This package contains hostapd with support for the MANA/KARMA attacks.
endef

define Package/hostapd-mana-light/description
  This package contains a hostapd with support for the MANA/KARMA attacks.
endef

ifneq ($(wildcard $(PKG_BUILD_DIR)/.config_*),$(subst .configured_,.config_,$(STAMP_CONFIGURED)))
  define Build/Configure/rebuild
	$(FIND) $(PKG_BUILD_DIR) -name \*.o -or -name \*.a | $(XARGS) rm -f
	rm -f $(PKG_BUILD_DIR)/hostapd/hostapd
	rm -f $(PKG_BUILD_DIR)/wpa_supplicant/wpa_supplicant
	rm -f $(PKG_BUILD_DIR)/.config_*
	touch $(subst .configured_,.config_,$(STAMP_CONFIGURED))
  endef
endif

define Build/Configure
	$(Build/Configure/rebuild)
	$(if $(wildcard ./files/hostapd-$(LOCAL_VARIANT).config), \
		$(CP) ./files/hostapd-$(LOCAL_VARIANT).config $(PKG_BUILD_DIR)/hostapd/.config \
	)
	#I'm commenting out wpa_supplicant. As i don't see any scenario where we need it...
	#$(CP) ./files/wpa_supplicant-$(LOCAL_VARIANT).config $(PKG_BUILD_DIR)/wpa_supplicant/.config
endef

TARGET_CPPFLAGS := \
	-I$(STAGING_DIR)/usr/include/libnl-tiny \
	-I$(PKG_BUILD_DIR)/src/crypto \
	$(TARGET_CPPFLAGS) \
	-DCONFIG_LIBNL20 \
	-D_GNU_SOURCE \
	$(if $(CONFIG_WPA_MSG_MIN_PRIORITY),-DCONFIG_MSG_MIN_PRIORITY=$(CONFIG_WPA_MSG_MIN_PRIORITY))

TARGET_CFLAGS += -ffunction-sections -fdata-sections -flto
TARGET_LDFLAGS += -Wl,--gc-sections -flto=jobserver -fuse-linker-plugin -lubox -lubus

ifeq ($(findstring supplicant,$(BUILD_VARIANT)),)
  TARGET_LDFLAGS += -lubox -lubus
endif

ifdef CONFIG_PACKAGE_kmod-cfg80211
  TARGET_LDFLAGS += -lm -lnl-tiny
endif

define Build/RunMake
        CFLAGS="$(TARGET_CPPFLAGS) $(TARGET_CFLAGS)" \
        $(MAKE) $(PKG_JOBS) -C $(PKG_BUILD_DIR)/$(1) \
                $(TARGET_CONFIGURE_OPTS) \
                $(DRIVER_MAKEOPTS) \
                LIBS="$(TARGET_LDFLAGS)" \
                LIBS_c="$(TARGET_LDFLAGS_C)" \
                AR="$(TARGET_CROSS)gcc-ar" \
                BCHECK= \
                $(2)
endef

define Build/Compile/hostapd
	+$(call Build/RunMake,hostapd, \
		hostapd hostapd_cli \
	)
endef

define Build/Compile
	echo "LOCAL TYPE: $(LOCAL_TYPE)"
	echo "BUILD VARIANT: $(BUILD_VARIANT)"
	$(Build/Compile/hostapd)
	$(Build/Compile/full)
endef

define Package/hostapd-mana/postinst
#!/bin/sh
# Let's download Sycophant (dirty fix.. Will fix this properly in a later release)
mkdir -p /tmp/mana_dependencies/
echo -e '\nDownloading Sycopant binary...\n\n'
if [ -d /sd ]; then
  wget https://github.com/adde88/hostapd-mana-openwrt/raw/master/bin/ar71xx/packages/base/wpa_sycophant -O /sd/usr/sbin/wpa_sycophant
else
  wget https://github.com/adde88/hostapd-mana-openwrt/raw/master/bin/ar71xx/packages/base/wpa_sycophant -O /usr/sbin/wpa_sycophant
fi
chmod +x /usr/sbin/wpa_sycophant
# We need to install some python dependencies...
echo -e '\nDownloading python-related dependencies...\n'
sleep 1
wget -i /etc/mana-toolkit/dependencies.txt -P /tmp/mana_dependencies/ --progress=bar:force:noscroll
#
# Let's pre-compile python-files and libraries.
echo -e '\nPre-compiling python files and libraries...\nThis can take a little while, please be patient...\n'
python -m compileall;
python -m compileall $${1}/usr/share/mana-toolkit/;
#
echo -e '\nFinished installing hostapd-mana. However, you will not be able to use any python-related tools before installing their dependencies...'
echo -e '\nThey should already be downloaded.\nTo install them enter the following command: "install-mana-depends"'
echo -e '\n\n -Zylla   <adde88@gmail.com> '
endef

define Install/hostapd-mana
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_DIR) $(1)/etc/mana-toolkit/
	$(INSTALL_DIR) $(1)/usr/share/mana-toolkit/
	$(INSTALL_DIR) $(1)/usr/share/mana-toolkit/cert/
endef

define Install/hostapd-mana-light
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_DIR) $(1)/etc/mana-toolkit/
	$(INSTALL_DIR) $(1)/usr/share/mana-toolkit/
	$(INSTALL_DIR) $(1)/usr/share/mana-toolkit/cert/
endef

define Package/hostapd-mana/install
	$(call Install/hostapd-mana,$(1))
	$(INSTALL_BIN)  $(PKG_BUILD_DIR)/hostapd/hostapd		$(1)/usr/sbin/hostapd-mana
	$(INSTALL_BIN)  $(PKG_BUILD_DIR)/hostapd/hostapd_cli		$(1)/usr/sbin/mana_cli
	$(INSTALL_BIN)	$(PKG_BUILD_DIR)/files/wpa_sycophant.sh		$(1)/usr/sbin/wpa_sycophant.sh
	$(INSTALL_BIN)	$(PKG_BUILD_DIR)/files/launch-mana.sh		$(1)/usr/sbin/launch-mana
	$(INSTALL_BIN)  $(PKG_BUILD_DIR)/files/kill-mana.sh		$(1)/usr/sbin/kill-mana
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/files/conf/*			$(1)/etc/mana-toolkit/
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/files/cert/*			$(1)/usr/share/mana-toolkit/cert/
	$(CP)		$(PKG_BUILD_DIR)/dependencies.txt		$(1)/etc/mana-toolkit/
	$(INSTALL_BIN)	$(PKG_BUILD_DIR)/files/install-mana-depends	$(1)/usr/sbin/install-mana-depends
endef

define Package/hostapd-mana-light/install
	$(call Install/hostapd-mana-light,$(1))
	$(INSTALL_BIN)  $(PKG_BUILD_DIR)/hostapd/hostapd                $(1)/usr/sbin/hostapd-mana
	$(INSTALL_BIN)  $(PKG_BUILD_DIR)/hostapd/hostapd_cli            $(1)/usr/sbin/mana_cli
	$(INSTALL_BIN)  $(PKG_BUILD_DIR)/files/wpa_sycophant.sh         $(1)/usr/sbin/wpa_sycophant.sh
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/files/conf/*                   $(1)/etc/mana-toolkit/
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/files/cert/*                   $(1)/usr/share/mana-toolkit/cert/
endef

$(eval $(call BuildPackage,hostapd-mana))
$(eval $(call BuildPackage,hostapd-mana-light))
