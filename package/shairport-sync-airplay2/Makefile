include $(TOPDIR)/rules.mk

PKG_NAME:=shairport-sync-airplay2
PKG_VERSION:=4.3.2
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/mikebrady/shairport-sync.git
PKG_SOURCE_VERSION:=airplay2
PKG_MIRROR_HASH:=skip

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)

PKG_FIXUP:=autoreconf
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/shairport-sync-airplay2
  SECTION:=sound
  CATEGORY:=Sound
  TITLE:=Shairport-Sync with AirPlay 2 support
  DEPENDS:=+libpthread +librt +alsa-lib +libdaemon +libpopt +libconfig +libssl
endef

define Package/shairport-sync-airplay2/description
 Shairport-Sync 4 (AirPlay 2 fork) with volume, multi-room, and playback control.
 Optimized for OpenWrt (MT7628AN, 128MB RAM).
endef

CONFIGURE_ARGS += \
	--with-alsa \
	--with-ssl=openssl \
	--with-avahi \
	--with-systemd=no \
	--with-pipe=no \
	--with-metadata \
	--with-apple-alac

define Package/shairport-sync-airplay2/install
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/shairport-sync.init $(1)/etc/init.d/shairport-sync
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) ./files/etc/config/shairport-sync $(1)/etc/config/shairport-sync
endef

$(eval $(call BuildPackage,shairport-sync-airplay2))
