# This is free software, licensed under the Apache License, Version 2.0 .
#

include $(TOPDIR)/rules.mk

PKG_NAME:=luci-app-adguardhome
PKG_VERSION:=1.1.1
PKG_MAINTAINER:=<https://github.com/sirpdboy/luci-app-adguardhome>

LUCI_TITLE:=LuCI app for AdGuardHome
LUCI_PKGARCH:=all
LUCI_DEPENDS:=+ca-certs +!wget&&!curl&&!wget-ssl:curl
LUCI_DESCRIPTION:=LuCI support for AdGuardHome


define Package/$(PKG_NAME)/conffiles
/etc/config/AdGuardHome
/etc/AdGuardHome.yaml
endef

define Package/$(PKG_NAME)/preinst
#!/bin/sh
	uci -q batch <<-EOF >/dev/null 2>&1
		delete ucitrack.@AdGuardHome[-1]
		add ucitrack AdGuardHome
		set ucitrack.@AdGuardHome[-1].init=AdGuardHome
		commit ucitrack
	EOF
	rm -f /tmp/luci-indexcache
exit 0
endef

define Package/$(PKG_NAME)/postinst

#!/bin/sh
	chmod +x /usr/share/AdGuardHome/*
	chmod +x /etc/init.d/AdGuardHome
	/etc/init.d/AdGuardHome enable >/dev/null 2>&1
	enable=$(uci get AdGuardHome.AdGuardHome.enabled 2>/dev/null)
	if [ "$enable" == "1" ]; then
		/etc/init.d/AdGuardHome reload >/dev/null 2>&1
	fi
	rm -f /tmp/luci-indexcache
	rm -f /tmp/luci-modulecache/*
exit 0
endef

define Package/$(PKG_NAME)/prerm
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

define Package/$(PKG_NAME)/postrm
#!/bin/sh
rm -rf /etc/AdGuardHome/
exit 0
endef
include $(TOPDIR)/feeds/luci/luci.mk

# call BuildPackage - OpenWrt buildroot signature
