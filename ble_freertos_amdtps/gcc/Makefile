#******************************************************************************
#
# Makefile - Rules for building the libraries, examples and docs.
#
# Copyright (c) 2019, Ambiq Micro
# All rights reserved.
# 
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions are met:
# 
# 1. Redistributions of source code must retain the above copyright notice,
# this list of conditions and the following disclaimer.
# 
# 2. Redistributions in binary form must reproduce the above copyright
# notice, this list of conditions and the following disclaimer in the
# documentation and/or other materials provided with the distribution.
# 
# 3. Neither the name of the copyright holder nor the names of its
# contributors may be used to endorse or promote products derived from this
# software without specific prior written permission.
# 
# Third party software included in this distribution is subject to the
# additional license terms as defined in the /docs/licenses directory.
# 
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
# AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
# IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
# ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE
# LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR
# CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF
# SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS
# INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN
# CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE)
# ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
# POSSIBILITY OF SUCH DAMAGE.
#
# This is part of revision v2.2.0-7-g63f7c2ba1 of the AmbiqSuite Development Package.
#
#******************************************************************************

#******************************************************************************
#
# User Options
#
#******************************************************************************

# COM_PORT is the serial port to use for uploading. For example COM#### on Windows or /dev/cu.usbserial-#### on *nix
COM_PORT		?=COM4
# ASB_UPLOAD_BAUD is the baud rate setting of the Ambiq Secue Bootloader (ASB) as it is configured on the Apollo3. Defautls to 115200 if unset
ASB_UPLOAD_BAUD	?=115200
# SVL_UPLOAD_BAUD is the baud rate setting of the SparkFun Variable Loader (SVL). Defaults to 921600 if unset
SVL_UPLOAD_BAUD	?=91600
# PYTHON3 should evaluate to a call to the Python3 executable on your machine
PYTHON3			?=

# *Optionally* specify absolute paths to the SDK and the BSP
# Make sure to use / instead of \ when on Windows
SDKPATH			?=# Set as the path to the SDK root if not located at ../../../../..
COMMONPATH		?=# Set as the path to the BSP common folder if not located at ../../../../common
BOARDPATH		?=# Set as the path to the board if not located at ../../..
PROJECTPATH		?=# Set as the path to the project if not located at ..

### Project Settings
TARGET := ble_freertos_amdtps
COMPILERNAME := gcc
PROJECT := ble_freertos_amdtps_gcc
CONFIG := bin

#******************************************************************************
#
# Warning Messages
#
#******************************************************************************
ifeq ($(COM_PORT),)
    COM_PORT=COM4
    $(warning warning: you have not defined COM_PORT. Assuming it is COM4)
endif
ifeq ($(PYTHON3),)
    PYTHON3=python3
    $(warning warning: you have not defined PYTHON3. assuming it is accessible by 'python3')
endif
ifeq ($(ASB_UPLOAD_BAUD),)
    ASB_UPLOAD_BAUD=115200
    $(warning defaulting to 115200 baud for ASB)
endif
ifeq ($(SVL_UPLOAD_BAUD),)
    SVL_UPLOAD_BAUD=921600
    $(warning defaulting to 921600 baud for SVL)
endif

ifeq ($(SDKPATH),)
    SDKPATH			=../../../../..
    $(warning warning: you have not defined SDKPATH so will continue assuming that the SDK root is at $(SDKPATH))
else
# When the SDKPATH is given export it
export SDKPATH
endif

ifeq ($(COMMONPATH),)
    COMMONPATH			=../../../../common
    $(warning warning: you have not defined COMMONPATH so will continue assuming that the COMMON root is at $(COMMONPATH))
else
# When the COMMONPATH is given export it
export COMMONPATH
endif

ifeq ($(BOARDPATH),)
    BOARDPATH			=../../..
    $(warning warning: you have not defined BOARDPATH so will continue assuming that the BOARD root is at $(BOARDPATH))
else
# When the BOARDPATH is given export it
export BOARDPATH
endif

ifeq ($(PROJECTPATH),)
    PROJECTPATH			=..
    $(warning warning: you have not defined PROJECTPATH so will continue assuming that the PROJECT root is at $(PROJECTPATH))
else
# When the PROJECTPATH is given export it
export PROJECTPATH
endif


#******************************************************************************
#
# User Defines / Includes / Sources / Libraries
#
#******************************************************************************
DEFINES = -DPART_$(PART)
DEFINES+= -DAM_FREERTOS
DEFINES+= -DWSF_TRACE_ENABLED
DEFINES+= -DAM_PART_APOLLO3
DEFINES+= -DAM_DEBUG_PRINTF
DEFINES+= -DAM_PACKAGE_BGA
DEFINES+= -DAM_CUSTOM_BDADDR

INCLUDES = -I../../../../../third_party/exactle/sw/stack/include
INCLUDES+= -I../../../../../third_party/exactle/sw/apps/app
#INCLUDES+= -I../../../../../ambiq_ble/apps/amdtps
INCLUDES+= -I../../../../../third_party/exactle/sw/profiles/bas
INCLUDES+= -I../../../../../third_party/exactle/sw/profiles/hid
INCLUDES+= -I../../../../../third_party/exactle/sw/apps/app/include
INCLUDES+= -I../../../../../third_party/uecc/micro-ecc
INCLUDES+= -I../../../../../mcu/apollo3
INCLUDES+= -I../../../../../CMSIS/AmbiqMicro/Include
INCLUDES+= -I../../../../../third_party/exactle/ws-core/sw/wsf/include
INCLUDES+= -I../../../../../third_party/exactle/sw/sec/include
INCLUDES+= -I../../../../../CMSIS/ARM/Include
INCLUDES+= -I../../../../../third_party/FreeRTOSv10.1.1/Source/include
INCLUDES+= -I../../../../../ambiq_ble/profiles/amdtps
INCLUDES+= -I../../../../../third_party/exactle/apollo-host/projects/common
INCLUDES+= -I../../../../../third_party/exactle/sw/services
INCLUDES+= -I../../../../../third_party/exactle/sw/hci/ambiq
INCLUDES+= -I../../../../../third_party/exactle/sw/stack/hci
INCLUDES+= -I../../../../../third_party/exactle/sw/hci/ambiq/apollo3
INCLUDES+= -I../../../bsp
INCLUDES+= -I../../../../../ambiq_ble/profiles/amdtpcommon
INCLUDES+= -I../../../../../third_party/exactle/sw/profiles/uribeacon
INCLUDES+= -I../../../../../third_party/exactle/ws-core/include
INCLUDES+= -I../../../../../third_party/exactle/ws-core/sw/util
INCLUDES+= -I../../../../../third_party/FreeRTOSv10.1.1/Source/portable/GCC/AMapollo2
INCLUDES+= -I../../../../../utils
INCLUDES+= -I../../../../../third_party/exactle/ws-core/sw/wsf/ambiq
INCLUDES+= -I../../../../../third_party/exactle/sw/sec/common
INCLUDES+= -I../../../../../devices
INCLUDES+= -I../../../../../ambiq_ble/services
INCLUDES+= -I../src
INCLUDES+= -I../../../../../third_party/exactle/sw/stack/cfg
INCLUDES+= -I../../../../..
INCLUDES+= -I../../../../../third_party/exactle/sw/hci/include

SRC = hci_main.c
SRC += app_db.c
SRC += app_hw.c
SRC += app_ui.c
SRC += amdtp_main.c
SRC += amdtp_common.c
SRC += bas_main.c
SRC += hid_main.c
SRC += bda.c
SRC += bstream.c
SRC += calc128.c
SRC += crc32.c
SRC += print.c
SRC += terminal.c
SRC += wstr.c
SRC += port.c
SRC += wsf_assert.c
SRC += wsf_math.c
SRC += wsf_os.c
SRC += wsf_trace.c
SRC += sec_aes.c
SRC += sec_cmac.c
SRC += sec_ecc_hci.c
SRC += sec_main.c
SRC += wsf_buf.c
SRC += wsf_efs.c
SRC += wsf_msg.c
SRC += wsf_queue.c
SRC += wsf_timer.c
SRC += svc_amdtp.c
SRC += svc_amotas.c
SRC += svc_amvole.c
SRC += ble_freertos_amdtps.c
SRC += radio_task.c
SRC += rtos.c
SRC += amdtps_main.c
SRC += hidapp_main.c
SRC += cfg_stack.c
SRC += hci_drv_apollo3.c
SRC += hci_cmd.c
SRC += hci_core.c
SRC += hci_core_ps.c
SRC += hci_evt.c
SRC += hci_tr.c
SRC += hci_vs.c
SRC += heap_2.c
SRC += app_disc.c
SRC += app_main.c
SRC += app_master.c
SRC += app_master_leg.c
SRC += app_server.c
SRC += app_slave.c
SRC += app_slave_leg.c
SRC += app_terminal.c
SRC += am_devices_button.c
SRC += am_devices_led.c
SRC += am_util_ble.c
SRC += am_util_debug.c
SRC += am_util_delay.c
SRC += am_util_faultisr.c
SRC += am_util_id.c
SRC += am_util_stdio.c
SRC += svc_batt.c
SRC += svc_bps.c
SRC += svc_core.c
SRC += svc_cps.c
SRC += svc_cscs.c
SRC += svc_dis.c
SRC += svc_gls.c
SRC += svc_gyro.c
SRC += svc_hid.c
SRC += svc_hrs.c
SRC += svc_hts.c
SRC += svc_ipss.c
SRC += svc_plxs.c
SRC += svc_px.c
SRC += svc_rscs.c
SRC += svc_scpss.c
SRC += svc_temp.c
SRC += svc_uricfg.c
SRC += svc_wdxs.c
SRC += svc_wp.c
SRC += svc_wss.c
SRC += event_groups.c
SRC += list.c
SRC += queue.c
SRC += tasks.c
SRC += timers.c
SRC += startup_gcc.c

VPATH = ../../../../../third_party/exactle/sw/stack/hci
VPATH+=:../../../../../third_party/exactle/sw/apps/app/generic
VPATH+=:../../../../../third_party/exactle/sw/apps/app
#VPATH+=:../../../../../ambiq_ble/apps/amdtps
#VPATH+=:../../../../../ambiq_ble/profiles/amdtpcommon
VPATH+=:../../../../../third_party/exactle/sw/profiles/bas
VPATH+=:../../../../../third_party/exactle/sw/profiles/hid
VPATH+=:../../../../../third_party/exactle/ws-core/sw/util
VPATH+=:../../../../../third_party/FreeRTOSv10.1.1/Source/portable/GCC/AMapollo2
VPATH+=:../../../../../third_party/uecc/micro-ecc
VPATH+=:../../../../../utils
VPATH+=:../../../../../third_party/exactle/ws-core/sw/wsf/ambiq
VPATH+=:../../../../../third_party/FreeRTOSv10.1.1/Source/portable/MemMang
VPATH+=:../../../../../third_party/exactle/sw/sec/common
VPATH+=:../../../../../third_party/exactle/ws-core/sw/wsf/common
VPATH+=:../../../../../devices
VPATH+=:../../../../../ambiq_ble/services
VPATH+=:../../../../../third_party/FreeRTOSv10.1.1/Source
VPATH+=:../src
VPATH+=:../../../../../third_party/exactle/sw/stack/cfg
VPATH+=:../../../../../ambiq_ble/profiles/amdtps
VPATH+=:../../../../../third_party/exactle/sw/apps/hidapp
VPATH+=:../../../../../third_party/exactle/sw/services
VPATH+=:../../../../../third_party/exactle/sw/hci/ambiq/apollo3
VPATH+=:../../../../../third_party/exactle/sw/hci/ambiq

LIBS = ../../../../../third_party/exactle/projects/generic/stacklib/gcc/bin-cortex-m4/libstacklib.a
LIBS += ../../../bsp/gcc/bin/libam_bsp.a
LIBS += ../../../../../mcu/apollo3/hal/gcc/bin/libam_hal.a
LIBS += ../../../../../third_party/uecc/gcc/bin/lib_uecc.a
### Bootloader Tools
AMBIQ_BIN2BOARD=$(PYTHON3) $(COMMONPATH)/tools_sfe/ambiq/ambiq_bin2board.py
ARTEMIS_SVL=$(PYTHON3) $(COMMONPATH)/tools_sfe/artemis/artemis_svl.py

SHELL:=/bin/bash
#### Setup ####

TOOLCHAIN ?= arm-none-eabi
PART = apollo3
CPU = cortex-m4
FPU = fpv4-sp-d16
# Default to FPU hardware calling convention.  However, some customers and/or
# applications may need the software calling convention.
#FABI = softfp
FABI = hard

LINKER_FILE := ./ble_freertos_amdtps.ld
STARTUP_FILE := ./startup_$(COMPILERNAME).c

#### Required Executables ####
CC = $(TOOLCHAIN)-gcc
GCC = $(TOOLCHAIN)-gcc
CPP = $(TOOLCHAIN)-cpp
LD = $(TOOLCHAIN)-ld
CP = $(TOOLCHAIN)-objcopy
OD = $(TOOLCHAIN)-objdump
RD = $(TOOLCHAIN)-readelf
AR = $(TOOLCHAIN)-ar
SIZE = $(TOOLCHAIN)-size
RM = $(shell which rm 2>/dev/null)

EXECUTABLES = CC LD CP OD AR RD SIZE GCC
K := $(foreach exec,$(EXECUTABLES),\
        $(if $(shell which $($(exec)) 2>/dev/null),,\
        $(info $(exec) not found on PATH ($($(exec))).)$(exec)))
$(if $(strip $(value K)),$(info Required Program(s) $(strip $(value K)) not found))

ifneq ($(strip $(value K)),)
all clean:
	$(info Tools $(TOOLCHAIN)-$(COMPILERNAME) not installed.)
	$(RM) -rf bin
else

CSRC = $(filter %.c,$(SRC))
ASRC = $(filter %.s,$(SRC))

OBJS = $(CSRC:%.c=$(CONFIG)/%.o)
OBJS+= $(ASRC:%.s=$(CONFIG)/%.o)

DEPS = $(CSRC:%.c=$(CONFIG)/%.d)
DEPS+= $(ASRC:%.s=$(CONFIG)/%.d)


CFLAGS = -mthumb -mcpu=$(CPU) -mfpu=$(FPU) -mfloat-abi=$(FABI)
CFLAGS+= -ffunction-sections -fdata-sections
CFLAGS+= -MMD -MP -std=c99 -Wall -g
CFLAGS+= -O0
CFLAGS+= $(DEFINES)
CFLAGS+= $(INCLUDES)
CFLAGS+= 

LFLAGS = -mthumb -mcpu=$(CPU) -mfpu=$(FPU) -mfloat-abi=$(FABI)
LFLAGS+= -nostartfiles -static
LFLAGS+= -Wl,--gc-sections,--entry,Reset_Handler,-Map,$(CONFIG)/$(TARGET).map
LFLAGS+= -Wl,--start-group -lm -lc -lgcc $(LIBS) -Wl,--end-group
LFLAGS+= 

# Additional user specified CFLAGS
CFLAGS+=$(EXTRA_CFLAGS)

CPFLAGS = -Obinary

ODFLAGS = -S

#### Rules ####
#******************************************************************************
#
# Targets / Rules
#
#******************************************************************************
all: directories $(CONFIG)/$(TARGET)_asb.bin

directories: $(CONFIG)

$(CONFIG):
	@mkdir -p $@

$(CONFIG)/%.o: %.c $(CONFIG)/%.d
	@echo " Compiling $(COMPILERNAME) $<" ;\
	$(CC) -c $(CFLAGS) $< -o $@

$(CONFIG)/%.o: %.s $(CONFIG)/%.d
	@echo " Assembling $(COMPILERNAME) $<" ;\
	$(CC) -c $(CFLAGS) $< -o $@

$(CONFIG)/$(TARGET)_asb.axf: LINKER_FILE = $(COMMONPATH)/tools_sfe/templates/asb_linker.ld
$(CONFIG)/$(TARGET)_asb.axf: $(OBJS) $(LIBS)
	@echo " Linking $(COMPILERNAME) $@ with script $(LINKER_FILE)";\
	$(CC) -Wl,-T,$(LINKER_FILE) -o $@ $(OBJS) $(LFLAGS)

$(CONFIG)/$(TARGET)_svl.axf: LINKER_FILE = $(COMMONPATH)/tools_sfe/templates/asb_svl_linker.ld
$(CONFIG)/$(TARGET)_svl.axf: $(OBJS) $(LIBS)
	@echo " Linking $(COMPILERNAME) $@ with script $(LINKER_FILE)";\
	$(CC) -Wl,-T,$(LINKER_FILE) -o $@ $(OBJS) $(LFLAGS)

$(CONFIG)/$(TARGET)_%.bin: $(CONFIG)/$(TARGET)_%.axf
	@echo " Copying $(COMPILERNAME) $@..." ;\
	$(CP) $(CPFLAGS) $< $@ ;\
	$(OD) $(ODFLAGS) $< > $(CONFIG)/$(TARGET).lst

bootload_asb: directories $(CONFIG)/$(TARGET)_asb.bin
	$(AMBIQ_BIN2BOARD) --bin $(CONFIG)/$(TARGET)_asb.bin --load-address-blob 0x20000 --magic-num 0xCB -o $(CONFIG)/$(TARGET) --version 0x0 --load-address-wired 0xC000 -i 6 --options 0x1 -b $(ASB_UPLOAD_BAUD) -port $(COM_PORT) -r 2 -v 

bootload_svl: directories $(CONFIG)/$(TARGET)_svl.bin
	$(ARTEMIS_SVL) $(COM_PORT) -f $(CONFIG)/$(TARGET)_svl.bin -b $(SVL_UPLOAD_BAUD) -v

bootload: bootload_svl

clean:
	@echo "Cleaning..." ;\
	$(RM) -R -f $(CONFIG)

$(CONFIG)/%.d: ;

$(SDKPATH)/mcu/apollo3/hal/gcc/bin/libam_hal.a:
	$(MAKE) -C $(SDKPATH)/mcu/apollo3/hal/gcc

$(SDKPATH)/third_party/uecc/gcc/bin/lib_uecc.a:
	$(MAKE) -C $(SDKPATH)/third_party/uecc

$(BOARDPATH)/bsp/gcc/bin/libam_bsp.a:
	$(MAKE) -C $(BOARDPATH)/bsp/gcc

# Automatically include any generated dependencies
-include $(DEPS)
endif
.PHONY: all clean directories bootload bootload_asb bootload_svl