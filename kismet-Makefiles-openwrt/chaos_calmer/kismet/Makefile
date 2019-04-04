include $(TOPDIR)/rules.mk

# This package is primarily designed for the Wi-Fi Pineapple Tetra but can be 
# easily adapted to other OpenWRT builds.

PKG_NAME:=kismet
PKG_VERSION:=2019
PKG_RELEASE:=6

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://www.kismetwireless.net/git/kismet.git
PKG_SOURCE_VERSION:=c007c620fdf573cbfb5d5c7409e4f0e7f5249b3c
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz

PKG_MAINTAINER:=Andreas Nilsen <adde88@gmail.com>

PKG_USE_MIPS16:=0

include $(INCLUDE_DIR)/package.mk

HOST_BUILD_DEPENDS:=protobuf/host

define Package/kismet
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Kismet (2019)
  #DESCRIPTION:=This variable is obsolete. use the Package/name/description define instead!
  URL:=https://www.kismetwireless.net/
  DEPENDS:=+libdw +libpthread +libpcap +libpcre +libmicrohttpd +libnl +libcap +libstdcpp +libncurses +libsqlite3 +zlib +protobuf +libprotobuf-c
endef

define Package/kismet/description
 Kismet wireless capture and IDS
 Latest web-ui based Kismet
 Linux Wi-Fi and Bluetooth capture tools
endef

define Build/Configure
  $(call Build/Configure/Default,--with-linux-headers=$(LINUX_DIR))
endef

CONFIGURE_ARGS += \
	--sysconfdir=/etc/kismet \
	--bindir=/usr/bin \
	--disable-python-tools \
    	--with-protoc=$(STAGING_DIR_HOST)/bin/protoc \
	--disable-libusb \
	# This is quite dirty - We'd WANT protoc-c to be in the host tools, but the openwrt
	# package doesn't make a host version, so we have to compile on the host; this works
	# right now because there is only one version of protoc-c...
	#--with-protocc=$(STAGING_DIR_HOST)/bin/protoc-c

# Override libs to force lpthread because we don't detect it properly on openwrt
CONFIGURE_VARS += \
	CXXFLAGS="$$$$CXXFLAGS -DOPENWRT" \
    	CFLAGS+="$$$$CFLAGS -DOPENWRT" \
	LIBS="-lm -lpthread"

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) \
	LD="$(TARGET_CXX)" \
	all
endef

define Package/kismet/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/kismet $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/kismet_cap_pcapfile $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/capture_linux_wifi/kismet_cap_linux_wifi $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/capture_linux_bluetooth/kismet_cap_linux_bluetooth $(1)/usr/bin/

	$(INSTALL_DIR) $(1)/etc/kismet/
	$(INSTALL_CONF) ./files/kismet_site.conf $(1)/etc/kismet/
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/conf/kismet.conf $(1)/etc/kismet
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/conf/kismet_alerts.conf $(1)/etc/kismet
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/conf/kismet_httpd.conf $(1)/etc/kismet
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/conf/kismet_logging.conf $(1)/etc/kismet
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/conf/kismet_memory.conf $(1)/etc/kismet
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/conf/kismet_storage.conf $(1)/etc/kismet
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/conf/kismet_uav.conf $(1)/etc/kismet
	cp -Rv $(PKG_BUILD_DIR)/http_data $(1)/etc/kismet/
endef

$(eval $(call BuildPackage,kismet))
