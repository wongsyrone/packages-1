#
# Copyright (C) 2009-2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

BASE_VERSION:=4.7.25

PKG_NAME:=db47
PKG_VERSION:=$(BASE_VERSION).4.NC
PKG_RELEASE:=7

PKG_BUILD_DIR:=$(BUILD_DIR)/db-$(BASE_VERSION).NC
PKG_SOURCE:=db-$(BASE_VERSION).NC.tar.gz
PKG_SOURCE_URL:=http://download.oracle.com/berkeley-db/
PKG_HASH:=cd39c711023ff44c01d3c8ff0323eef7318660772b24f287556e6bf676a12535

PKG_MAINTAINER:=Marcel Denia <naoir@gmx.net>
PKG_LICENSE:=Sleepycat
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_DEPENDS:=libxml2
PKG_FIXUP:=autoreconf
PKG_LIBTOOL_PATHS:=. build_unix
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

define Package/libdb47
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=Berkeley DB library (4.7)
  URL:=http://www.oracle.com/us/products/database/berkeley-db
  PROVIDES:=libdb47-full
endef

define Package/libdb47/description
  Berkeley DB library (4.7).
endef

define Package/libdb47xx
  SECTION:=libs
  CATEGORY:=Libraries
  DEPENDS:=+libdb47 +libstdcpp
  TITLE:=Berkeley DB library (4.7) for C++
  URL:=http://www.oracle.com/us/products/database/berkeley-db
  PROVIDES:=libdb47xx-full
endef

define Package/libdb47xx/description
  Berkeley DB library (4.7).  C++ wrapper.
endef

define Package/db47-util
  SECTION:=utils
  CATEGORY:=Utilities
  DEPENDS:=+libdb47
  TITLE:=Berkeley DB library (4.7) Utilities
  URL:=http://www.sleepycat.com/products/db.shtml
  MAINTAINER:=W. Michael Petullo <mike@flyn.org>
endef

define Package/db47-util/description
  Berkeley DB library (4.7) Utilities.
endef

define Package/db47-util/config
	source "$(SOURCE)/Config.in"
endef

CONFIGURE_PATH = build_unix
CONFIGURE_CMD = ../dist/configure

CONFIGURE_ARGS += \
	--enable-shared \
	--enable-static \
	--disable-java \
	--with-mutex=POSIX/pthreads/library \
	--disable-tcl \
	--disable-rpc \
	--enable-compat185 \
	--enable-smallbuild \
	--disable-debug \
	$(if $(CONFIG_PACKAGE_libdb47xx),--enable-cxx,--disable-cxx)

TARGET_CFLAGS += $(FPIC)

define Build/Compile
	+$(MAKE) $(PKG_JOBS) -C $(PKG_BUILD_DIR)/build_unix \
		DESTDIR="$(PKG_INSTALL_DIR)" all
	$(MAKE) -C $(PKG_BUILD_DIR)/build_unix \
		DESTDIR="$(PKG_INSTALL_DIR)" install
endef

define Package/libdb47/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libdb-*.so $(1)/usr/lib/
endef

define Package/libdb47xx/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libdb_cxx-*.so $(1)/usr/lib/
endef

define Package/db47-util/install
	$(INSTALL_DIR) $(1)/usr/bin
	if [ "$(CONFIG_DB47_ARCHIVE)" == "y" ]; then $(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/db_archive $(1)/usr/bin/; fi
	if [ "$(CONFIG_DB47_CHECKPOINT)" == "y" ]; then $(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/db_checkpoint $(1)/usr/bin/; fi
	if [ "$(CONFIG_DB47_DEADLOCK)" == "y" ]; then $(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/db_deadlock $(1)/usr/bin/; fi
	if [ "$(CONFIG_DB47_DUMP)" == "y" ]; then $(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/db_dump $(1)/usr/bin/; fi
	if [ "$(CONFIG_DB47_HOTBACKUP)" == "y" ]; then $(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/db_hotbackup $(1)/usr/bin/; fi
	if [ "$(CONFIG_DB47_LOAD)" == "y" ]; then $(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/db_load $(1)/usr/bin/; fi
	if [ "$(CONFIG_DB47_PRINTLOG)" == "y" ]; then $(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/db_printlog $(1)/usr/bin/; fi
	if [ "$(CONFIG_DB47_RECOVER)" == "y" ]; then $(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/db_recover $(1)/usr/bin/; fi
	if [ "$(CONFIG_DB47_STAT)" == "y" ]; then $(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/db_stat $(1)/usr/bin/; fi
	if [ "$(CONFIG_DB47_UPGRADE)" == "y" ]; then $(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/db_upgrade $(1)/usr/bin/; fi
	if [ "$(CONFIG_DB47_VERIFY)" == "y" ]; then $(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/db_verify $(1)/usr/bin/; fi
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/db.h $(1)/usr/include/
	$(CP) $(PKG_INSTALL_DIR)/usr/include/db_cxx.h $(1)/usr/include/
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libdb*.{a,so} $(1)/usr/lib
endef

$(eval $(call BuildPackage,libdb47))
$(eval $(call BuildPackage,libdb47xx))
$(eval $(call BuildPackage,db47-util))
