# SPDX-License-Identifier: GPL-3.0-only
#
# Copyright (C) 2021 ImmortalWrt.org

include $(TOPDIR)/rules.mk

PKG_NAME:=lal
PKG_VERSION:=0.26.0
PKG_RELEASE:=$(AUTORELEASE)

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/q191201771/lal/tar.gz/v$(PKG_VERSION)?
PKG_HASH:=4e1d90fe0199c4537618ce7f3873505acc13ec6025be469d0253e378b18af2e7

PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=Tianling Shen <cnsztl@immortalwrt.org>

PKG_CONFIG_DEPENDS:= \
	CONFIG_LAL_GOPROXY \
	CONFIG_LAL_COMPRESS_UPX
	
PKG_BUILD_DEPENDS:=golang/host
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0

GO_PKG:=github.com/q191201771/lal
GO_PKG_BUILD_PKG:=$(GO_PKG)/app/lalserver

GO_PKG_LDFLAGS_X:= \
	$(GO_PKG)/constant.Version=$(PKG_VERSION) \
	$(GO_PKG)/constant.Commit=v$(PKG_VERSION)
GO_PKG_TAGS:=full

include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/golang/golang-package.mk

define Package/lal
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Web Servers/Proxies
  TITLE:=An audio/video live streaming broadcast server written in Go.
  DEPENDS:=$(GO_ARCH_DEPENDS)
  URL:=https://pengrl.com/lal/
endef

define Package/lal/description
  LAL is an audio/video live streaming broadcast server written in Go.
endef

define Package/lal/config
config CONFIG_LAL_GOPROXY
           bool "Compiling with GOPROXY proxy"
           default n

config CONFIG_LAL_COMPRESS_UPX
	bool "Compress executable files with UPX"
	depends on !mips64
	default n
endef

ifeq ($(CONFIG_LAL_GOPROXY),y)
	export GO111MODULE=on
	export GOPROXY=https://goproxy.baidu.com
endif

define Build/Compile
	$(call GoPackage/Build/Compile)
ifeq ($(CONFIG_LAL_COMPRESS_UPX),y)
	$(STAGING_DIR_HOST)/bin/upx --lzma --best $(GO_PKG_BUILD_BIN_DIR)/lal
endif
endef

$(eval $(call GoBinPackage,lal))
$(eval $(call BuildPackage,lal))
