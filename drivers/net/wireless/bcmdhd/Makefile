# bcmdhd
#
#
#
#
#

DHDCFLAGS = -Wall -Wstrict-prototypes -Dlinux -DBCMDRIVER               \
	-DBCMDONGLEHOST -DUNRELEASEDCHIP -DBCMDMA32 -DBCMFILEIMAGE            \
	-DDHDTHREAD -DSHOW_EVENTS -DBCMDBG -DCUSTOMER_HW2 \
	-DWIFI_ACT_FRAME -DARP_OFFLOAD_SUPPORT -DWLP2P     \
	-DKEEP_ALIVE -DPKT_FILTER_SUPPORT -DCONFIG_DTS            \
	-DEMBEDDED_PLATFORM -DPNO_SUPPORT         \
	-DDHD_USE_IDLECOUNT -DSET_RANDOM_MAC_SOFTAP -DROAM_ENABLE -DVSDB      \
	-DWL_CFG80211_VSDB_PRIORITIZE_SCAN_REQUEST                            \
	-DESCAN_RESULT_PATCH -DSUPPORT_PM2_ONLY -DWLTDLS                      \
	-DDHD_DONOT_FORWARD_BCMEVENT_AS_NETWORK_PKT -DRXFRAME_THREAD       \
	-DMIRACAST_AMPDU_SIZE=8	-DROAM_ENABLE	-DWL_IFACE_COMB_NUM_CHANNELS	\
	-DSDTEST -DBDC -DDHD_BCMEVENTS -DPROP_TXSTATUS \
	-DBCMSDIO -DBCMLXSDMMC -DSDIO_CRC_ERROR_FIX  \
	-DCUSTOM_SDIO_F2_BLKSIZE=128 -DUSE_SDIOFIFO_IOVAR \
	-Idrivers/net/wireless/bcmdhd -Idrivers/net/wireless/bcmdhd/include \
	-Idrivers/net/wireless/bcmdhd/common/include

#-DGET_CUSTOM_MAC_ENABLE

DHDOFILES = aiutils.o siutils.o sbutils.o bcmutils.o bcmwifi_channels.o \
	dhd_linux.o dhd_linux_platdev.o dhd_linux_sched.o dhd_pno.o \
	dhd_common.o dhd_ip.o dhd_linux_wq.o dhd_custom_gpio.o \
	bcmevent.o hndpmu.o linux_osl.o wldev_common.o wl_android.o \
	hnd_pktq.o hnd_pktpool.o \
	bcmsdh.o \
	bcmsdh_linux.o \
	bcmsdh_sdmmc.o \
	bcmsdh_sdmmc_linux.o \
	dhd_sdio.o \
	dhd_cdc.o \
	dhd_wlfc.o \
	dhd_cfg_vendor.o


CONFIG_BCMDHD_HW_OOB=n
#CONFIG_BCMDHD_HW_OOB=y
ifeq ($(CONFIG_BCMDHD_HW_OOB),y)
DHDCFLAGS += -DCUSTOMER_HW2 -DOOB_INTR_ONLY -DHW_OOB -DMMC_SDIO_ABORT
else
DHDCFLAGS += -DSDIO_ISR_THREAD
endif

obj-$(CONFIG_BCMDHD) += bcmdhd.o
bcmdhd-objs += $(DHDOFILES)

ifeq ($(KBUILD_SRC),)
DHD_SRC = $(src)
else
DHD_SRC = $(KBUILD_SRC)/$(src)
endif

ifneq ($(wildcard $(DHD_SRC)/dhd_nvram.obj),)
include $(src)/makefile.my
endif


ifneq ($(CONFIG_WIRELESS_EXT),)
bcmdhd-objs += wl_iw.o
DHDCFLAGS += -DSOFTAP -DWL_WIRELESS_EXT -DUSE_IW
endif
ifneq ($(CONFIG_CFG80211),)
bcmdhd-objs += wl_cfg80211.o wl_cfgp2p.o wl_linux_mon.o dhd_cfg80211.o wl_cfg_btcoex.o
DHDCFLAGS += -DWL_CFG80211 -DWL_CFG80211_STA_EVENT -DWL_CFG80211_P2P_DEV_IF
DHDCFLAGS += -DCUSTOM_ROAM_TRIGGER_SETTING=-65
DHDCFLAGS += -DCUSTOM_ROAM_DELTA_SETTING=15
DHDCFLAGS += -DCUSTOM_KEEP_ALIVE_SETTING=28000
DHDCFLAGS += -DCUSTOM_PNO_EVENT_LOCK_xTIME=7
endif
ifneq ($(CONFIG_DHD_USE_SCHED_SCAN),)
DHDCFLAGS += -DWL_SCHED_SCAN
endif
EXTRA_CFLAGS = $(DHDCFLAGS)
ifeq ($(CONFIG_BCMDHD),m)
EXTRA_LDFLAGS += --strip-debug
else
DHDCFLAGS += -DENABLE_INSMOD_NO_FW_LOAD
endif

#########################
# Chip dependent feature
#########################
ifneq ($(CONFIG_BCM4339),)
DHDCFLAGS += -DCUSTOM_GLOM_SETTING=8 -DCUSTOM_RXCHAIN=1
DHDCFLAGS += -DUSE_DYNAMIC_F2_BLKSIZE -DDYNAMIC_F2_BLKSIZE_FOR_NONLEGACY=128
DHDCFLAGS += -DBCMSDIOH_TXGLOM -DCUSTOM_TXGLOM=1 -DBCMSDIOH_TXGLOM_HIGHSPEED
DHDCFLAGS += -DCUSTOM_SDIO_F2_BLKSIZE=512
DHDCFLAGS += -DDHDTCPACK_SUPPRESS
DHDCFLAGS += -DUSE_WL_TXBF
DHDCFLAGS += -DUSE_WL_FRAMEBURST
DHDCFLAGS += -DCUSTOM_AMPDU_BA_WSIZE=32
DHDCFLAGS += -DPROP_TXSTATUS_VSDB
DHDCFLAGS += -DCUSTOM_MAX_TXGLOM_SIZE=32
DHDCFLAGS += -DREPEAT_READFRAME
DHDCFLAGS += -DROAM_AP_ENV_DETECTION
endif



