# SPDX-License-Identifier: GPL-3.0-only
#
# Copyright (C) 2022-2023  gdy
#
# This is free software, licensed under the Apache License, Version 2.0 .
#

include $(TOPDIR)/rules.mk

PKG_NAME:=luci-app-lucky
PKG_VERSION:=2.2.2
PKG_RELEASE:=1

LUCI_TITLE:=LuCI Support for lucky
LUCI_PKGARCH:=all

# ==========================
# 依赖兼容处理
# OpenWrt 21.02 以后才有 luci-lua-runtime
# 老版本继续用 luci-compat
# ==========================
ifeq ($(shell [ -d "$(TOPDIR)/feeds/luci/modules/luci-lua-runtime" ] && echo 1),1)
  LUCI_DEPENDS:=+lucky +luci-lua-runtime
else
  LUCI_DEPENDS:=+lucky +luci-compat
endif


define Package/$(PKG_NAME)/install
	$(INSTALL_BIN) ./root/usr/bin/luckyarch $(1)/usr/bin/luckyarch
endef

include $(TOPDIR)/feeds/luci/luci.mk

# call BuildPackage - OpenWrt buildroot signature
