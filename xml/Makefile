# Copyright 2013-2021 The Khronos Group Inc.
#
# SPDX-License-Identifier: Apache-2.0

# If a recipe fails, delete its target file. Without this cleanup, the leftover
# file from the failed recipe can falsely satisfy dependencies on subsequent
# runs of `make`.
.DELETE_ON_ERROR:

QUIET	 ?= @
MKDIR	 = mkdir -p
CP	 = cp
RM	 = rm -f
RMRF	 = rm -rf

# Generator scripts and options
# GENOPTS can be e.g. '-noprotect'

PYTHON ?= python3
SCRIPTS = ../scripts
GENOPTS =
# default options for generating MISRA C or MISRA C++ headers
# can be overridden from commandline
MISRACOPTS ?= -misracstyle
MISRACPPOPTS ?= -misracppstyle

# Generate Vulkan headers from XML. Many other files can be generated
# from vk.xml using the scripts, but they are all generated from
# ../Makefile as part of the process of building the Specification.
#
# Targets:
#
# default / install - regenerate headers in ../include/vulkan/.
# validate - run XML validator on vk.xml against the schema.
# test - check if vulkan_core.h compiles.
# clean_dirt - remove intermediate files.
# clean - remove installed and intermediate files.

GENERATED   = ../gen
INCLUDE     = $(GENERATED)/include
TESTS	    = ../tests
VULKAN	    = $(INCLUDE)/vulkan
JSON	    = $(GENERATED)/out/json
SRC	    = ../src
VULKAN_VIDEO_CODECS_STD_H = $(INCLUDE)/vk_video

VULKAN_API ?= vulkansc
ifeq ($(VULKAN_API),vulkan)
API_SUFFIX :=
else
API_SUFFIX := _sc
endif

# Static files needed for a complete set of headers, cached in the
# repository
ifeq ($(VULKAN_API),vulkan)
VULKAN_VIDEO_HEADERS := $(VULKAN_VIDEO_CODECS_STD_H)/vulkan_video_codecs_common.h \
		 $(VULKAN_VIDEO_CODECS_STD_H)/vulkan_video_codec_h264std.h \
		 $(VULKAN_VIDEO_CODECS_STD_H)/vulkan_video_codec_h264std_decode.h \
		 $(VULKAN_VIDEO_CODECS_STD_H)/vulkan_video_codec_h264std_encode.h \
		 $(VULKAN_VIDEO_CODECS_STD_H)/vulkan_video_codec_h265std.h \
		 $(VULKAN_VIDEO_CODECS_STD_H)/vulkan_video_codec_h265std_decode.h \
		 $(VULKAN_VIDEO_CODECS_STD_H)/vulkan_video_codec_h265std_encode.h
else
VULKAN_VIDEO_HEADERS :=
endif
STATIC_HEADERS = $(VULKAN)/vulkan$(API_SUFFIX).h $(VULKAN)/vk_platform.h $(VULKAN_VIDEO_HEADERS)

# Where static headers are stored
STATIC				 = ../include/vulkan
STATIC_VULKAN_VIDEO_CODECS_STD_H = ../include/vk_video
STATIC_JSON_SRC			 = ../json

# Vulkan platform-specific headers
PLATFORM_HEADERS = \
    $(VULKAN)/vulkan_android.h \
    $(VULKAN)/vulkan_fuchsia.h \
    $(VULKAN)/vulkan_ggp.h \
    $(VULKAN)/vulkan_ios.h \
    $(VULKAN)/vulkan_macos.h \
    $(VULKAN)/vulkan_vi.h \
    $(VULKAN)/vulkan_wayland.h \
    $(VULKAN)/vulkan_win32.h \
    $(VULKAN)/vulkan_xcb.h \
    $(VULKAN)/vulkan_xlib.h \
    $(VULKAN)/vulkan_directfb.h \
    $(VULKAN)/vulkan_xlib_xrandr.h \
    $(VULKAN)/vulkan_metal.h \
    $(VULKAN)/vulkan_screen.h \
    $(VULKAN)/vulkan_sci.h \
    $(VULKAN)/vulkan_beta.h

HEADERS_H = $(VULKAN)/vulkan$(API_SUFFIX)_core.h $(PLATFORM_HEADERS)
ifeq ($(VULKAN_API),vulkansc)
HEADERS_HPP = $(VULKAN)/vulkan$(API_SUFFIX)_core.hpp
STATIC_JSON = $(STATIC_JSON_SRC)/vkpcc.json
JSON_PCC    = $(JSON)/vkpcc.json
STATIC_CTS_COPY = \
         $(JSON)/cts/vkjson_data_default.h \
         $(JSON)/cts/vkjson_parser_default.h
JSON_SCHEMA = $(JSON)/vk.json
JSON_GENERATOR = $(JSON)/vulkan_json_data.hpp \
		 $(JSON)/vulkan_json_gen.h \
		 $(JSON)/vulkan_json_gen.c
JSON_PARSER = $(JSON)/vulkan_json_parser.hpp
JSON_CTS = $(JSON)/cts/vulkan_json_data.hpp  $(JSON)/cts/vulkan_json_parser.hpp
JSON_SCRIPTS = $(SCRIPTS)/json_parser.py $(SCRIPTS)/json_generator.py
endif
HEADERS = $(HEADERS_H) $(HEADERS_HPP)
JSON_FILES = $(JSON_SCHEMA) $(JSON_GENERATOR) $(JSON_PARSER)
JSON_CTS_FILES = $(JSON_CTS)

default install: $(HEADERS) $(STATIC_HEADERS) $(JSON_FILES) $(JSON_PCC) $(JSON_CTS_FILES) $(STATIC_CTS_COPY)

$(VULKAN)/vulkan$(API_SUFFIX).h: $(STATIC)/vulkan$(API_SUFFIX).h
	$(QUIET)$(MKDIR) $(VULKAN)
	$(CP) $? $@

$(VULKAN)/vulkan$(API_SUFFIX).hpp: $(STATIC)/vulkan$(API_SUFFIX).h
	$(QUIET)$(MKDIR) $(VULKAN)
	$(CP) $? $@

$(VULKAN)/vk_platform.h: $(STATIC)/vk_platform.h
	$(QUIET)$(MKDIR) $(VULKAN)
	$(CP) $? $@

$(VULKAN_VIDEO_CODECS_STD_H)/vulkan_video_codecs_common.h: $(STATIC_VULKAN_VIDEO_CODECS_STD_H)/vulkan_video_codecs_common.h
	$(QUIET)$(MKDIR) $(VULKAN_VIDEO_CODECS_STD_H)
	$(CP) $? $@

$(VULKAN_VIDEO_CODECS_STD_H)/vulkan_video_codec_h264std.h: $(STATIC_VULKAN_VIDEO_CODECS_STD_H)/vulkan_video_codec_h264std.h
	$(QUIET)$(MKDIR) $(VULKAN_VIDEO_CODECS_STD_H)
	$(CP) $? $@

$(VULKAN_VIDEO_CODECS_STD_H)/vulkan_video_codec_h264std_decode.h: $(STATIC_VULKAN_VIDEO_CODECS_STD_H)/vulkan_video_codec_h264std_decode.h
	$(QUIET)$(MKDIR) $(VULKAN_VIDEO_CODECS_STD_H)
	$(CP) $? $@

$(VULKAN_VIDEO_CODECS_STD_H)/vulkan_video_codec_h264std_encode.h: $(STATIC_VULKAN_VIDEO_CODECS_STD_H)/vulkan_video_codec_h264std_encode.h
	$(QUIET)$(MKDIR) $(VULKAN_VIDEO_CODECS_STD_H)
	$(CP) $? $@

$(VULKAN_VIDEO_CODECS_STD_H)/vulkan_video_codec_h265std.h: $(STATIC_VULKAN_VIDEO_CODECS_STD_H)/vulkan_video_codec_h265std.h
	$(QUIET)$(MKDIR) $(VULKAN_VIDEO_CODECS_STD_H)
	$(CP) $? $@

$(VULKAN_VIDEO_CODECS_STD_H)/vulkan_video_codec_h265std_decode.h: $(STATIC_VULKAN_VIDEO_CODECS_STD_H)/vulkan_video_codec_h265std_decode.h
	$(QUIET)$(MKDIR) $(VULKAN_VIDEO_CODECS_STD_H)
	$(CP) $? $@

$(VULKAN_VIDEO_CODECS_STD_H)/vulkan_video_codec_h265std_encode.h: $(STATIC_VULKAN_VIDEO_CODECS_STD_H)/vulkan_video_codec_h265std_encode.h
	$(QUIET)$(MKDIR) $(VULKAN_VIDEO_CODECS_STD_H)
	$(CP) $? $@

################################################

# Autogenerate vulkan header from XML API description

# Python and XML files on which vulkan_core.h depends
GENSCRIPT   = $(SCRIPTS)/genvk.py
VKXML	    = vk.xml
VKH_DEPENDS = $(VKXML) $(GENSCRIPT) $(SCRIPTS)/reg.py $(SCRIPTS)/generator.py

$(HEADERS_H): $(VKH_DEPENDS)
	$(MKDIR) $(VULKAN)
	$(PYTHON) $(GENSCRIPT) $(MISRACOPTS) $(GENOPTS) -registry $(VKXML) \
	    -o $(VULKAN) $(notdir $@)

$(HEADERS_HPP): $(VKH_DEPENDS)
	$(MKDIR) $(VULKAN)
	$(PYTHON) $(GENSCRIPT) $(MISRACPPOPTS) $(GENOPTS) -registry $(VKXML) \
	    -o $(VULKAN) $(notdir $@)

platform: $(PLATFORM_HEADERS)

# Verify registry XML file against the schema
validate:
	jing -c registry.rnc $(VKXML)
	$(PYTHON) $(GENSCRIPT) -validate -registry $(VKXML)

# Test that vulkan_core.h compiles

# Platforms to test
TESTDEFS = -DVK_USE_PLATFORM_XCB_KHR -DVK_USE_PLATFORM_XLIB_KHR

TESTFILE = $(TESTS)/htest$(API_SUFFIX).c

test: $(HEADERS) $(STATIC_HEADERS)
	gcc -Wall -std=c99 -c -I$(INCLUDE) -I$(TESTS) $(TESTFILE)
	gcc -Wall -std=c11 -c -I$(INCLUDE) -I$(TESTS) $(TESTFILE)
	g++ -Wall -c -std=c++98 -I$(INCLUDE) -I$(TESTS) $(TESTFILE)
	g++ -Wall -c -std=c++11 -I$(INCLUDE) -I$(TESTS) $(TESTFILE)
	g++ -Wall -c -std=c++11 -I$(INCLUDE) -I$(TESTS) -DUSE_HPP=1 $(TESTFILE)
	$(RM) $(TESTS)/test.o

################################################

# Autogenerate JSON Schema and utils from the XML API description
$(JSON_FILES): $(VKH_DEPENDS) $(JSON_SCRIPTS)
	$(QUIET)$(MKDIR) $(JSON)
	$(PYTHON) $(GENSCRIPT) $(GENOPTS) -registry $(VKXML) \
	    -o $(JSON) $(notdir $@)

$(JSON_CTS_FILES): $(VKH_DEPENDS) $(JSON_SCRIPTS)
	$(QUIET)$(MKDIR) $(JSON)/cts
	$(PYTHON) $(GENSCRIPT) $(GENOPTS) -registry $(VKXML) --iscts \
	    -o $(JSON)/cts $(notdir $@)

$(JSON)/cts/vkjson_data_default.h: $(STATIC_JSON_SRC)/vkjson_data_default.h
	$(QUIET)$(MKDIR) $(JSON)/cts
	$(CP) $? $@

$(JSON)/cts/vkjson_parser_default.h: $(STATIC_JSON_SRC)/vkjson_parser_default.h
	$(QUIET)$(MKDIR) $(JSON)/cts
	$(CP) $? $@

$(JSON_PCC): $(STATIC_JSON)
	$(QUIET)$(MKDIR) $(JSON)
	$(CP) $? $@

################################################

# Files to clean up
PYDIRT = diag.txt dumpReg.txt errwarn.txt *.pyc regdump.txt
DIRT = $(PYDIRT) ERRS \#*

# Clean intermediate files
clean_dirt:
	-$(RM) $(DIRT)

# Clean generated targets and intermediates
clean clobber: clean_dirt
	-$(RMRF) $(INCLUDE) $(JSON)
