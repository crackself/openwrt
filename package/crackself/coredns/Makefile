include $(TOPDIR)/rules.mk

PKG_NAME:=coredns
PKG_VERSION:=1.8.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/coredns/coredns.git
PKG_SOURCE_VERSION:=272ccb195d31cd1622d48f961f3a189ce3abb937
PKG_MAINTAINER:=crackself <crackself@gmail.com>

PKG_SOURCE_SUBDIR:=$(PKG_NAME)
PKG_SOURCE:=$(PKG_SOURCE_SUBDIR)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_SOURCE_SUBDIR)

PKG_BUILD_DEPENDS:=golang/host
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0

GO_PKG:=github.com/coredns/coredns
GO_PKG_LDFLAGS:=-s -w
GO_PKG_LDFLAGS_X:= \
	$(GO_PKG)/version.Version=$(PKG_VERSION)

export GO111MODULE=on
export GOPROXY=https://goproxy.io
# export GOPROXY=https://mirrors.aliyun.com/goproxy/

include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/golang/golang-package.mk

define Package/$(PKG_NAME)
	SECTION:=net
	CATEGORY:=Network
	TITLE:=A DNS and Service Discovery
	DEPENDS:=$(GO_ARCH_DEPENDS)
	URL:=https://github.com/coredns/coredns
	SUBMENU:=CoreDNS
endef

# presetting config file
# define Package/adguardhome/conffiles
	# /etc/config/Corefile
# endef

define Package/$(PKG_NAME)/description
	DNS and Service Discovery (Golang)
endef

define Build/Compile
	$(eval GO_PKG_BUILD_PKG:=$(GO_PKG))
	$(call GoPackage/Build/Configure)
	$(call GoPackage/Build/Compile)
	$(STAGING_DIR_HOST)/bin/upx --lzma --best $(GO_PKG_BUILD_BIN_DIR)/coredns
	chmod +wx $(GO_PKG_BUILD_BIN_DIR)/coredns
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(GO_PKG_BUILD_BIN_DIR)/coredns $(1)/usr/bin/coredns
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/coredns.ini $(1)/etc/init.d/coredns
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_BIN) ./files/Corefile $(1)/etc/config/Corefile
endef

$(eval $(call GoBinPackage,$(PKG_NAME)))
$(eval $(call BuildPackage,$(PKG_NAME)))