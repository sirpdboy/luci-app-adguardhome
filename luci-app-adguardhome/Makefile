#
# Copyright (C) 2025 https://github.com/sirpdboy/luci-app-adguardhome
#
# This is free software, licensed under the Apache License, Version 2.0

include $(TOPDIR)/rules.mk

PKG_NAME:=luci-app-adguardhome
PKG_VERSION:=2.1.1
PKG_RELEASE:=20251026
PKG_LICENSE:=Apache-2.0

PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=w9315273

PKG_BUILD_DEPENDS:=luci-base/host

include $(INCLUDE_DIR)/package.mk

define Package/luci-app-adguardhome
	SECTION:=luci
	CATEGORY:=LuCI
	SUBMENU:=3. Applications
	TITLE:=LuCI app for adguardhome
	PKG_MAINTAINER:=https://github.com/sirpdboy/luci-app-adguardhome
	PKGARCH:=all
	DEPENDS:=+adguardhome +wget-ssl +tar +xz-utils
endef



define Package/luci-app-adguardhome/description
	LuCI support for adguardhome
endef

define Package/luci-app-adguardhome/conffiles
/etc/config/AdGuardHome
/etc/AdGuardHome.yaml
endef


define Package/luci-app-adguardhome/postinst
#!/bin/sh
	chmod +x /usr/share/AdGuardHome/*
	chmod +x /usr/libexec/rpcd/luci.adguardhome
	chmod +x /etc/init.d/AdGuardHome
	/etc/init.d/AdGuardHome enable >/dev/null 2>&1
	enable=$(uci get AdGuardHome.AdGuardHome.enabled 2>/dev/null)
	if [ "$enable" == "1" ]; then
		/etc/init.d/AdGuardHome reload
	fi
	rm -f /tmp/luci-indexcache
	rm -f /tmp/luci-modulecache/*
exit 0
endef

define Package/luci-app-adguardhome/prerm
#!/bin/sh
if [ -z "$${IPKG_INSTROOT}" ]; then
     /etc/init.d/AdGuardHome disable
     /etc/init.d/AdGuardHome stop
uci -q batch <<-EOF >/dev/null 2>&1
	delete ucitrack.@AdGuardHome[-1]
	commit ucitrack
EOF
fi
exit 0
endef

include $(TOPDIR)/feeds/luci/luci.mk

# call BuildPackage - OpenWrt buildroot signature
