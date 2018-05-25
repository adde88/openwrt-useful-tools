include $(TOPDIR)/rules.mk

PKG_NAME:=kismet-remote
PKG_VERSION:=25.05.2018
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://www.kismetwireless.net/git/kismet.git
PKG_SOURCE_VERSION:=5158958f01556380c5d6e363268f8dd0ad204ce4
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz

PKG_MAINTAINER:=Andreas Nilsen <adde88@gmail.com.com>

include $(INCLUDE_DIR)/package.mk

PKG_BUILD_DEPENDS:=+libpthread +libpcap +libnl +libprotobuf-c

define Package/kismet-remote
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Kismet (2018)
  URL:=https://www.kismetwireless.net/
  DEPENDS:=+libpthread +libpcap +libnl +libprotobuf-c
endef

define Package/kismet-remote/description
 Kismet remote-capture binary for 2018+ Kismet
endef

define Build/Configure
  $(call Build/Configure/Default,--with-linux-headers=$(LINUX_DIR)) 
endef

CONFIGURE_ARGS += \
	--bindir=/usr/bin \
	--enable-capture-tools-only \
	--disable-python-tools \
	--with-netlink \
    --with-protocc=$(STAGING_DIR_HOST)/bin/protoc-c

# Override libs to force lpthread because we don't detect it properly on openwrt
CONFIGURE_VARS += \
	LIBS="-lpthread" 

TARGET_CFLAGS+=-I$(STAGING_DIR)/usr/include/libnl
TARGET_CPPFLAGS+=-I$(STAGING_DIR)/usr/include/libnl

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

