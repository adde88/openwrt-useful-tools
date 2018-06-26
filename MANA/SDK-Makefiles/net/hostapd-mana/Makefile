#
# Copyright (C) 2006-2014 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=hostapd-mana
PKG_VERSION:=2.6
PKG_RELEASE:=3

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/adde88/hostapd-mana.git
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
PKG_SOURCE_VERSION:=efc299eda3b06a26772ed81a2200875a29b276b5
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)

PKG_MAINTAINER:=Andreas Nilsen <adde88@gmail.com.com>

PKG_BUILD_PARALLEL:=1

PKG_BUILD_DEPENDS:= \
	PACKAGE_kmod-madwifi:madwifi \

PKG_CONFIG_DEPENDS:= \
	CONFIG_WPA_SUPPLICANT_NO_TIMESTAMP_CHECK \
	CONFIG_PACKAGE_kmod-ath9k \
	CONFIG_PACKAGE_kmod-mac80211 \
	CONFIG_PACKAGE_kmod-madwifi \
	CONFIG_PACKAGE_hostapd \
	CONFIG_PACKAGE_hostapd-mini \
	CONFIG_PACKAGE_kmod-hostap \
	CONFIG_WPA_RFKILL_SUPPORT \
	CONFIG_DRIVER_WEXT_SUPPORT \
	CONFIG_DRIVER_11N_SUPPORT

LOCAL_TYPE=$(strip \
		$(if $(findstring wpad,$(BUILD_VARIANT)),wpad, \
		$(if $(findstring supplicant,$(BUILD_VARIANT)),supplicant, \
		hostapd \
	)))
LOCAL_VARIANT=$(patsubst wpad-%,%,$(patsubst supplicant-%,%,$(BUILD_VARIANT)))

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

DRIVER_MAKEOPTS= \
	CONFIG_DRIVER_NL80211=$(CONFIG_PACKAGE_kmod-mac80211) \
	CONFIG_DRIVER_MADWIFI=$(CONFIG_PACKAGE_kmod-madwifi) \
	CONFIG_DRIVER_HOSTAP=$(CONFIG_PACKAGE_kmod-hostap) \
	CONFIG_IEEE80211N=$(HOSTAPD_IEEE80211N) \
	CONFIG_IEEE80211W=$(CONFIG_PACKAGE_kmod-ath9k) \
	CONFIG_DRIVER_WEXT=$(CONFIG_DRIVER_WEXT_SUPPORT) \
	$(if $(CONFIG_WPA_RFKILL_SUPPORT),NEED_RFKILL=y)

ifneq ($(LOCAL_TYPE),hostapd)
  ifdef CONFIG_WPA_SUPPLICANT_OPENSSL
    ifeq ($(LOCAL_VARIANT),full)
      DRIVER_MAKEOPTS += CONFIG_TLS=openssl
      TARGET_LDFLAGS += -lcrypto -lssl
    endif
  endif
  ifdef CONFIG_WPA_SUPPLICANT_NO_TIMESTAMP_CHECK
    TARGET_CFLAGS += -DNO_TIMESTAMP_CHECK
  endif
  DRIVER_MAKEOPTS += \
	CONFIG_DRIVER_ROBOSWITCH=$(CONFIG_PACKAGE_kmod-switch)
endif

ifdef CONFIG_USE_EGLIBC
  TARGET_LDFLAGS += -lrt
  TARGET_LDFLAGS_C += -lrt
endif

DRV_DEPENDS:=+PACKAGE_kmod-mac80211:libnl-tiny @(!TARGET_avr32||BROKEN)

define Package/hostapd-mana/Default
  SECTION:=net
  CATEGORY:=Network
  TITLE:=IEEE 802.1x Authenticator with support for the MANA and Karma attacks.
  DEPENDS:=$(DRV_DEPENDS) +libubus +tinyproxy +stunnel +ip +python +sslsplit
endef

define Package/hostapd-mana
$(call Package/hostapd-mana/Default)
  TITLE+= (full)
  VARIANT:=full
endef

define Package/hostapd-mana/description
 This package contains a full featured IEEE 802.1x/WPA/EAP/RADIUS Authenticator with support for the MANA/KARMA attack.
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
	#$(CP) ./files/wpa_supplicant-$(LOCAL_VARIANT).config $(PKG_BUILD_DIR)/wpa_supplicant/.config
endef

TARGET_CPPFLAGS := \
	-I$(STAGING_DIR)/usr/include/libnl-tiny \
	-I$(PKG_BUILD_DIR)/src/crypto \
	$(TARGET_CPPFLAGS) \
	-I$(CURDIR)/madwifi \
	-DCONFIG_LIBNL20 \
	-D_GNU_SOURCE \
	$(if $(CONFIG_WPA_MSG_MIN_PRIORITY),-DCONFIG_MSG_MIN_PRIORITY=$(CONFIG_WPA_MSG_MIN_PRIORITY))

TARGET_CFLAGS += -ffunction-sections -fdata-sections
TARGET_LDFLAGS += -Wl,--gc-sections
ifeq ($(findstring supplicant,$(BUILD_VARIANT)),)
  TARGET_LDFLAGS += -lubox -lubus
endif

ifdef CONFIG_PACKAGE_kmod-mac80211
  TARGET_LDFLAGS += -lm -lnl-tiny
endif

define Build/RunMake
	CFLAGS="$(TARGET_CPPFLAGS) $(TARGET_CFLAGS)" \
	$(MAKE) $(PKG_JOBS) -C $(PKG_BUILD_DIR)/$(1) \
		$(TARGET_CONFIGURE_OPTS) \
		$(DRIVER_MAKEOPTS) \
		LIBS="$(TARGET_LDFLAGS)" \
		LIBS_c="$(TARGET_LDFLAGS_C)" \
		BCHECK= \
		$(2)
endef

define Build/Compile/hostapd
	$(call Build/RunMake,hostapd, \
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
if [ -e $${1}/usr/bin/hostapd-mana ]; then
  python -m compileall;
  python -m compileall $${1}/usr/share/mana-toolkit/;
fi
endef


config_files=	dnsmasq-dhcpd.conf \
				dnsmasq-dhcpd-two.conf \
				dnsspoof.conf \
				hostapd.accept \
				hostapd.deny \
				hostapd.eap_user \
				hostapd-hidden.conf \
				hostapd-mana.conf \
				hostapd-mana-all.conf \
				hostapd-mana-eap.conf \
				hostapd-mana-eaponly.conf \
				karmetasploit.rc \
				stunnel.conf \
				tinyproxy.conf \
				hostapd.ssid_filter

cert_config_files=	dhparam.pem \
					radius.csr \
					radius.key \
					radius.pem \
					rogue-ca.crt \
					rogue-ca.der \
					rogue-ca.key \
					rogue-ca.p12 \
					rogue-ca.pem

mana_start_scripts=	firelamb-view.sh \
					launch-mana.sh \
					kill-mana.sh

crackapd_files=	crackapd.conf crackapd.py
firelamb_files=	firelamb.py helper.py publicsuffix.py publicsuffix.txt quick-run-stats.sh README
net-creds_files= LICENSE net-creds.py README.md requirements.txt
sslstrip_files=	COPYING \
				debug_ssl.log \
				lock.ico \
				poc.log \
				README \
				README.md \
				setup.py \
				sslstrip.log \
				sslstrip.py
sslstrip_files2=	ClientRequest.py \
					CookieCleaner.py \
					DnsCache.py \
					__init__.py \
					ServerConnection.py \
					ServerConnectionFactory.py \
					SSLServerConnection.py \
					StrippingProxy.py \
					URLMonitor.py
dns2proxy_files=	dns2proxy.py \
					dnsalert.txt \
					dnslog.txt \
					domains.cfg \
					fhtagn.sh \
					ia.sh \
					IPBouncer.sh \
					nospoof.cfg \
					nospoofto.cfg \
					README.md \
					resolv.conf \
					spoof.cfg \
					transform.cfg \
					victims.cfg

define Install/hostapd-mana
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_DIR) $(1)/etc/mana-toolkit/
	$(INSTALL_DIR) $(1)/usr/share/mana-toolkit/
	$(INSTALL_DIR) $(1)/usr/share/mana-toolkit/cert/
	$(INSTALL_DIR) $(1)/usr/share/mana-toolkit/crackapd/
	$(INSTALL_DIR) $(1)/usr/share/mana-toolkit/firelamb/
	$(INSTALL_DIR) $(1)/usr/share/mana-toolkit/net-creds/
	$(INSTALL_DIR) $(1)/usr/share/mana-toolkit/run-mana/
	$(INSTALL_DIR) $(1)/usr/share/mana-toolkit/sslstrip-hsts/
	$(INSTALL_DIR) $(1)/usr/share/mana-toolkit/sslstrip-hsts/dns2proxy/
	$(INSTALL_DIR) $(1)/usr/share/mana-toolkit/sslstrip-hsts/sslstrip2/
	$(INSTALL_DIR) $(1)/usr/share/mana-toolkit/sslstrip-hsts/sslstrip2/sslstrip
	$(INSTALL_DIR) $(1)/var/lib/mana-toolkit/
	$(INSTALL_DIR) $(1)/var/lib/mana-toolkit/lamb_braai/
	$(INSTALL_DIR) $(1)/var/lib/mana-toolkit/sslsplit/
	$(INSTALL_DIR) $(1)/usr/lib/python2.7/
endef

define Package/hostapd-mana/install
	$(call Install/hostapd-mana,$(1))
	$(INSTALL_BIN)  $(PKG_BUILD_DIR)/hostapd/hostapd $(1)/usr/sbin/hostapd-mana
	$(INSTALL_BIN)  $(PKG_BUILD_DIR)/hostapd/hostapd_cli $(1)/usr/sbin/mana_cli
	$(INSTALL_BIN)	$(PKG_BUILD_DIR)/files/launch-mana.sh $(1)/usr/sbin/launch-mana
	$(INSTALL_BIN)  $(PKG_BUILD_DIR)/files/kill-mana.sh $(1)/usr/sbin/kill-mana
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/files/conf/* $(1)/etc/mana-toolkit/
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/files/cert/* $(1)/usr/share/mana-toolkit/cert/
	$(INSTALL_BIN)  $(addprefix $(PKG_BUILD_DIR)/files/,$(mana_start_scripts)) $(1)/usr/share/mana-toolkit/run-mana/
	$(INSTALL_DATA) $(addprefix $(PKG_BUILD_DIR)/files/crackapd/,$(crackapd_files)) $(1)/usr/share/mana-toolkit/crackapd/
	$(INSTALL_DATA) $(addprefix $(PKG_BUILD_DIR)/files/firelamb/,$(firelamb_files)) $(1)/usr/share/mana-toolkit/firelamb/
	$(INSTALL_DATA) $(addprefix $(PKG_BUILD_DIR)/files/net-creds/,$(net-creds_files)) $(1)/usr/share/mana-toolkit/net-creds/
	$(INSTALL_DATA) $(addprefix $(PKG_BUILD_DIR)/files/sslstrip-hsts/sslstrip2/,$(sslstrip_files)) $(1)/usr/share/mana-toolkit/sslstrip-hsts/sslstrip2/
	$(INSTALL_DATA) $(addprefix $(PKG_BUILD_DIR)/files/sslstrip-hsts/sslstrip2/sslstrip/,$(sslstrip_files2)) $(1)/usr/share/mana-toolkit/sslstrip-hsts/sslstrip2/sslstrip/
	$(INSTALL_DATA) $(addprefix $(PKG_BUILD_DIR)/files/sslstrip-hsts/dns2proxy/,$(dns2proxy_files)) $(1)/usr/share/mana-toolkit/sslstrip-hsts/dns2proxy/
	cp -r $(PKG_BUILD_DIR)/python2.7/* $(1)/usr/lib/python2.7/
endef

$(eval $(call BuildPackage,hostapd-mana))
