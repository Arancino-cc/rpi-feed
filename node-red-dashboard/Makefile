#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NPM_NAME:=node-red-dashboard
PKG_NAME:=node-red-dashboard
PKG_VERSION:=2.9.1
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NPM_NAME)-$(PKG_VERSION).tgz
PKG_SOURCE_URL:=http://registry.npmjs.com/$(PKG_NPM_NAME)/-/
PKG_MD5SUM:=8ea3fc8d8b1afac63646832e6e23af3654d55090

PKG_BUILD_DEPENDS:=node
PKG_NODE_VERSION:=6.11.4

include $(INCLUDE_DIR)/package.mk

define Package/node-red-dashboard
  DEPENDS:=+node
  SUBMENU:=Node.js
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=A dashboard UI for Node-RED 
  URL:=https://www.npmjs.org/package/node-red-dashboard
endef

define Package/node-red-dashboard/description
	This module provides a set of nodes in Node-RED to quickly create a live data dashboard.
endef

NODEJS_CPU:=$(subst powerpc,ppc,$(subst aarch64,arm64,$(subst x86_64,x64,$(subst i386,ia32,$(ARCH)))))

define Build/Prepare
	$(INSTALL_DIR) $(PKG_BUILD_DIR)
endef

define Build/Compile
	$(MAKE_FLAGS) \
	$(MAKE_VARS) \
	npm_config_arch=$(NODEJS_CPU) \
	npm_config_nodedir=$(BUILD_DIR)/node-v$(PKG_NODE_VERSION)/ \
	npm_config_cache=$(TMP_DIR)/npm-cache \
	PREFIX="$(PKG_INSTALL_DIR)/usr/" \
	$(STAGING_DIR_HOSTPKG)/bin/npm install --target_arch=$(NODEJS_CPU) --build-from-source -g $(DL_DIR)/$(PKG_SOURCE)
endef

define Package/node-red-dashboard/install
	$(INSTALL_DIR) $(1)/usr/lib/node_modules
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/node_modules/* $(1)/usr/lib/node_modules
endef

define Package/$(PKG_NAME)/postrm
#!/bin/sh
# check if we are on a real system
if `grep -qs '/opt' /proc/mounts`
then
    rm -rvf /opt/usr/lib/node/$(PKG_NPM_NAME)
else
    rm -rvf /usr/lib/node/$(PKG_NPM_NAME)
fi
endef

$(eval $(call BuildPackage,node-red-dashboard))
