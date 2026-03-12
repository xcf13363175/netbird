include $(TOPDIR)/rules.mk

PKG_NAME:=netbird
PKG_VERSION:=0.66.4
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/netbirdio/netbird.git
PKG_SOURCE_VERSION:=b5489d4986e6131f663ba24e330cbb3af4b92580
PKG_SOURCE_DATE:=20260312
PKG_MIRROR_HASH:=skip

PKG_CONFIG_DEPENDS:=CONFIG_NETBIRD_COMPRESS_UPX

PKG_BUILD_DEPENDS:=golang/host
PKG_BUILD_PARALLEL:=1
PKG_BUILD_FLAGS:=no-mips16

GO_PKG:=github.com/netbirdio/netbird
GO_PKG_BUILD_PKG:=$(GO_PKG)/client
GO_PKG_LDFLAGS_X:=$(GO_PKG)/version.version=$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/golang/golang-package.mk

define Package/$(PKG_NAME)/config
config NETBIRD_COMPRESS_UPX
	bool "Compress executable files with UPX"
	default y
endef

define Package/netbird
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=VPN
  TITLE:=Connect your devices into a single secure private WireGuard®-based mesh network
  URL:=https://netbird.io
  DEPENDS:=$(GO_ARCH_DEPENDS) +kmod-wireguard
endef

define Build/Compile
	$(call GoPackage/Build/Compile)
ifneq ($(CONFIG_NETBIRD_COMPRESS_UPX),)
	$(STAGING_DIR_HOST)/bin/upx --lzma --best $(GO_PKG_BUILD_BIN_DIR)/client
endif
endef

define Package/netbird/description
  NetBird is an open-source VPN management platform built on top of WireGuard® making it easy to create
  secure private networks for your organization or home.

  It requires zero configuration effort leaving behind the hassle of opening ports, complex firewall rules, VPN
  gateways, and so forth.
endef

define Package/netbird/conffiles
/etc/netbird/config.json
endef

define Package/netbird/install
	$(call GoPackage/Package/Install/Bin,$(PKG_INSTALL_DIR))
	$(INSTALL_DIR) $(1)/usr/bin $(1)/etc/init.d
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/client $(1)/usr/bin/netbird
	$(INSTALL_BIN) ./files/netbird.init $(1)/etc/init.d/netbird
endef

$(eval $(call GoBinPackage,netbird))
$(eval $(call BuildPackage,netbird))
