#
# Copyright (C) 2013-2016 OpenWrt.org
# Copyright (C) 2016 LEDE project
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=firewall
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL=https://github.com/nomagick/firewall3.git
PKG_SOURCE_DATE:=2019-05-06
PKG_SOURCE_VERSION:=802cd6e09a40625f305a52b346227036512138b0
PKG_MIRROR_HASH:=97050f68e2b2bab8b3fd931b19937c40f09b7a364285279a6f71782e88bf2049
PKG_MAINTAINER:=Yanlong Wang <yanlong.wang@naiver.org>
PKG_LICENSE:=ISC

PKG_CONFIG_DEPENDS := CONFIG_IPV6

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/firewall
  SECTION:=net
  CATEGORY:=Base system
  TITLE:=OpenWrt C Firewall
  DEPENDS:=+libubox +libubus +libuci +libip4tc +IPV6:libip6tc +libxtables +kmod-ipt-core +kmod-ipt-conntrack +IPV6:kmod-nf-conntrack6 +kmod-ipt-nat +IPV6:kmod-ipt-nat6
endef

define Package/firewall/description
 This package provides a config-compatible C implementation of the UCI firewall.
endef

define Package/firewall/conffiles
/etc/config/firewall
/etc/firewall.user
endef

TARGET_CFLAGS += -ffunction-sections -fdata-sections -flto
TARGET_LDFLAGS += -Wl,--gc-sections -flto
CMAKE_OPTIONS += $(if $(CONFIG_IPV6),,-DDISABLE_IPV6=1)

define Package/firewall/install
	$(INSTALL_DIR) $(1)/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/firewall3 $(1)/sbin/fw3
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/firewall.init $(1)/etc/init.d/firewall
	$(INSTALL_DIR) $(1)/etc/hotplug.d/iface
	$(INSTALL_DATA) ./files/firewall.hotplug $(1)/etc/hotplug.d/iface/20-firewall
	$(INSTALL_DIR) $(1)/etc/config/
	$(INSTALL_DATA) ./files/firewall.config $(1)/etc/config/firewall
	$(INSTALL_DIR) $(1)/etc/
	$(INSTALL_DATA) ./files/firewall.user $(1)/etc/firewall.user
	$(INSTALL_DIR) $(1)/usr/share/fw3
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/helpers.conf $(1)/usr/share/fw3
endef

# define Build/Prepare
# 	$(call Build/Prepare/Default)
# 	mkdir -p $(PKG_BUILD_DIR)
# 	$(CP) ./src/* $(PKG_BUILD_DIR)/
# endef


# SOURCE_DIR:=src

$(eval $(call BuildPackage,firewall))
