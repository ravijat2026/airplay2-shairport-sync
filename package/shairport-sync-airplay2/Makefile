include $(TOPDIR)/rules.mk

PKG_NAME:=shairport-sync
PKG_VERSION:=4.3.7
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/mikebrady/shairport-sync.git
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)

PKG_LICENSE:=MIT
PKG_MAINTAINER:=yourname

PKG_BUILD_DEPENDS:=libavahi-client libssl libsoxr libao

include $(INCLUDE_DIR)/package.mk

define Package/shairport-sync
  SECTION:=sound
  CATEGORY:=Sound
  TITLE:=Shairport Sync (AirPlay 2 capable build)
  DEPENDS:=+libavahi-client +libopenssl +libsoxr +libao
endef

define Package/shairport-sync/description
Lightweight Shairport-Sync with AirPlay2 support (minimal features enabled).
endef

define Build/Prepare
	$(call Build/Prepare/Default)
    # Ensure autotools files exist (some tags need autoreconf)
    ( cd $(PKG_BUILD_DIR); autoreconf -fi || true )
endef

define Build/Configure
    ( \
    cd $(PKG_BUILD_DIR); \
    ADDLDFLAGS="$(TARGET_LDFLAGS)" \
    CFLAGS="$(TARGET_CFLAGS) -Os -fdata-sections -ffunction-sections -march=mips32r2" \
    CXXFLAGS="$(TARGET_CXXFLAGS) -Os -fdata-sections -ffunction-sections" \
    ./configure \
      --host=$(GNU_TARGET_NAME) \
      --sysconfdir=/etc \
      --with-ao=yes \
      --with-avahi=yes \
      --with-ssl=openssl \
      --with-metadata=no \
      --with-soxr=no \
      --with-airplay-2=yes \
      --disable-alsamixer \
      --disable-ffmpeg \
      --disable-alsa-device-monitor \
    )
endef

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) $(MAKE_FLAGS) \
      CFLAGS="$(TARGET_CFLAGS) -Os -pipe -fomit-frame-pointer" \
      LDFLAGS="$(TARGET_LDFLAGS) -Wl,--gc-sections"
endef

define Package/shairport-sync/install
	$(INSTALL_DIR) $(1)/etc/init.d
    $(INSTALL_DIR) $(1)/usr/bin
    $(INSTALL_BIN) $(PKG_BUILD_DIR)/shairport-sync $(1)/usr/bin/shairport-sync
    $(INSTALL_BIN) ./files/shairport-sync.init $(1)/etc/init.d/shairport-sync
    $(INSTALL_DIR) $(1)/etc/config
    $(INSTALL_DATA) ./files/config.sample $(1)/etc/config/shairport-sync
endef

$(eval $(call BuildPackage,shairport-sync))