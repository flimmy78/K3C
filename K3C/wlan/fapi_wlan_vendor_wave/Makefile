#
# Openwrt Makefile for fapi_wlan_vendor_wave
#
#

#### Includes ###################################
include $(TOPDIR)/rules.mk

#### Package Info ############################### 
PKG_NAME:=fapi_wlan_vendor_wave
PKG_VERSION:=1.0.0.1
PKG_RELEASE:=1
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
PKG_DEFAULT_CUSTOM_SOURCE_DIR:=$(shell pwd)/src

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/package-version-override.mk
include $(INCLUDE_DIR)/ugw-loglevel.mk

PKG_BUILD_DEPENDS:=+PACKAGE_ltq-wlan-wave_5_x:hostapd_2_3_wave_5_1_1

define Package/$(PKG_NAME)
  SECTION:=net
  CATEGORY:=Lantiq
  SUBMENU:=UGW Software
  TITLE:=Lantiq WLAN Service Library
  URL:=http://www.lantiq.com
  MAINTAINER:=LANTIQ DEUTSCHLAND GMBH
  DEPENDS:= +fapi_wlan_common +libugwhelper @+BUSYBOX_CONFIG_SORT
  MENU:=1
endef

define Package/$(PKG_NAME)/description
	Lantiq FAPI for WLAN
endef

ifeq ($(CONFIG_PACKAGE_ltq-wlan-wave_5_x),y)
export VENDOR_WAVE_BUILD = $(CONFIG_PACKAGE_ltq-wlan-wave_5_x)
endif

### Menuconfig ##################################
define Package/$(PKG_NAME)/config
$(call Package/$(PKG_NAME)/override_version)
$(call Package/$(PKG_NAME)/override_source_path)
$(call Package/$(PKG_NAME)/config_debug_level)
endef

#### Target Rules ###############################
define Build/Prepare

endef

# To ipkg / Rootfs
define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/opt/lantiq/lib
	$(INSTALL_DIR) $(1)/opt/lantiq/usr/sbin
	$(INSTALL_DIR) $(1)/opt/lantiq/wave/scripts
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/libfapiwave.so $(1)/opt/lantiq/lib
	#$(INSTALL_BIN) $(PKG_BUILD_DIR)/test_wlan $(1)/opt/lantiq/usr/sbin
	# Copy Scripts
	$(CP) ./files/scripts/fapi* $(1)/opt/lantiq/wave/scripts
	$(INSTALL_DIR) $(STAGING_DIR)/usr/docs
	$(CP) $(PKG_BUILD_DIR)/fapi_wlan_wave.h $(STAGING_DIR)/usr/docs/.
endef

define Build/Clean
	if [ -d $(PKG_BUILD_DIR) ]; then $(MAKE) -C $(PKG_BUILD_DIR) clean; \
	$(RM) -r $(PKG_BUILD_DIR)/ipkg-$(BOARD); fi
endef

$(eval $(call BuildPackage,$(PKG_NAME)))
