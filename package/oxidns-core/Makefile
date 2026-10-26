#
# oxidns-core — prebuilt OxiDNS core for x86_64
#
# luci-app-oxidns deliberately does NOT bundle the core: its own installer
# downloads the release from GitHub on the router and verifies the SHA256.
# Shipping the same release inside the image means the LuCI page finds
# /usr/bin/oxidns already in place, so the router works offline right after
# flashing and there is nothing to download behind a captive portal.
#
include $(TOPDIR)/rules.mk

PKG_NAME:=oxidns-core
PKG_VERSION:=1.6.1
PKG_RELEASE:=1

# Release asset name is fixed per architecture; keep it verbatim.
PKG_SOURCE:=oxidns-x86_64-unknown-linux-musl.tar.gz
PKG_SOURCE_URL:=https://github.com/svenshi/oxidns/releases/download/v$(PKG_VERSION)
PKG_HASH:=2ac8e103e03407a73c29da848f6ce2d524e751ce1da8a744aa9168fc1384e3b0

PKG_LICENSE:=GPL-3.0-or-later
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=stunningtoo

include $(INCLUDE_DIR)/package.mk

define Package/oxidns-core
  SECTION:=net
  CATEGORY:=Network
  TITLE:=OxiDNS core daemon (prebuilt static musl x86_64)
  URL:=https://oxidns.org
  DEPENDS:=@TARGET_x86_64
endef

define Package/oxidns-core/description
 OxiDNS core binary, its bundled WebUI and the upstream default config file
 (which listens on :5335, so it does not fight dnsmasq for port 53).
 Installed to /usr/bin/oxidns, the exact path luci-app-oxidns manages.
endef

# The release tarball is flat: oxidns, config.yaml, LICENSE and webui/ sit at
# its top level with no leading directory. The default Prepare unpacks into
# $(PKG_BUILD_DIR)/.., which would scatter those four entries next to the build
# directory instead of inside it, so unpack into $(PKG_BUILD_DIR) explicitly.
define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	gzip -dc $(DL_DIR)/$(PKG_SOURCE) | $(TAR) -C $(PKG_BUILD_DIR) -xf -
endef

# prebuilt ELF binary: nothing to compile
define Build/Compile
endef

define Package/oxidns-core/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/oxidns $(1)/usr/bin/oxidns
	$(INSTALL_DIR) $(1)/usr/share/oxidns/webui
	$(CP) $(PKG_BUILD_DIR)/webui/. $(1)/usr/share/oxidns/webui/
	$(INSTALL_DIR) $(1)/etc/oxidns
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/config.yaml $(1)/etc/oxidns/config.yaml
endef

$(eval $(call BuildPackage,oxidns-core))
