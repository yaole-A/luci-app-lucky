# SPDX-License-Identifier: GPL-3.0-only
#

include $(TOPDIR)/rules.mk

PKG_NAME:=lucky
PKG_VERSION:=2.18.6
PKG_RELEASE:=1

PKG_LICENSE:=GPL-3.0-only
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=GDY666 <gdy666@foxmail.com>

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
PKG_HASH:=skip

# ---------------------------
# 架构映射
# ---------------------------
ifeq ($(ARCH),mipsel)
	LUCKY_ARCH:=mipsle_softfloat
endif
ifeq ($(ARCH),mips)
	LUCKY_ARCH:=mips_softfloat
endif
ifeq ($(ARCH),i386)
	LUCKY_ARCH:=i386
endif
ifeq ($(ARCH),x86_64)
	LUCKY_ARCH:=x86_64
endif
ifeq ($(ARCH),arm)
	ifeq ($(BOARD),bcm53xx)
		LUCKY_ARCH:=armv6
	else
		LUCKY_ARCH:=armv7
	endif
endif
ifeq ($(BOARD),bcm53xx)
	LUCKY_ARCH:=armv6
ifeq ($(word 2,$(subst +,$(space),$(call qstrip,$(CONFIG_CPU_TYPE)))),)
	LUCKY_ARCH:=armv5
endif
endif
ifeq ($(BOARD),kirkwood)
	LUCKY_ARCH:=armv5
endif
ifeq ($(ARCH),aarch64)
	LUCKY_ARCH:=arm64
endif

# ---------------------------
# 包定义
# ---------------------------
include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)
	SECTION:=net
	CATEGORY:=Network
	TITLE:=Lucky gdy
	DEPENDS:=@(i386||x86_64||arm||aarch64||mipsel||mips)
	URL:=https://github.com/gdy666/lucky
endef

define Package/$(PKG_NAME)/description
  Main functions of Lucky: ipv4/ipv6 portforward, DDNS, IoT wake on LAN,
  reverse proxy and more...
endef

# ---------------------------
# 下载 + 解压
# ---------------------------
define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	[ ! -f $(PKG_BUILD_DIR)/$(PKG_NAME)_$(PKG_VERSION)_Linux_$(LUCKY_ARCH).tar.gz ] && \
		wget https://github.com/gdy666/lucky/releases/download/v$(PKG_VERSION)/$(PKG_NAME)_$(PKG_VERSION)_Linux_$(LUCKY_ARCH).tar.gz -O $(PKG_BUILD_DIR)/$(PKG_NAME)_$(PKG_VERSION)_Linux_$(LUCKY_ARCH).tar.gz
	tar -xzf $(PKG_BUILD_DIR)/$(PKG_NAME)_$(PKG_VERSION)_Linux_$(LUCKY_ARCH).tar.gz -C $(PKG_BUILD_DIR)
endef

define Build/Compile
endef

# ---------------------------
# 配置文件
# ---------------------------
define Package/$(PKG_NAME)/conffiles
/etc/config/lucky
endef

# ---------------------------
# 安装
# ---------------------------
define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/etc/init.d/
	$(INSTALL_DIR) $(1)/etc/config/

	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lucky $(1)/usr/bin/lucky
	$(INSTALL_BIN) ./files/lucky.init $(1)/etc/init.d/lucky
	$(INSTALL_CONF) $(CURDIR)/files/luckyuci $(1)/etc/config/lucky
endef

# ---------------------------
# 编译
# ---------------------------
$(eval $(call BuildPackage,$(PKG_NAME)))
