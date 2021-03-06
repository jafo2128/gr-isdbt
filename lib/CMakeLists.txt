# Copyright 2011,2012,2016,2018,2019 Free Software Foundation, Inc.
#
# This file was generated by gr_modtool, a tool from the GNU Radio framework
# This file is a part of gr-isdbt
#
# GNU Radio is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 3, or (at your option)
# any later version.
#
# GNU Radio is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with GNU Radio; see the file COPYING.  If not, write to
# the Free Software Foundation, Inc., 51 Franklin Street,
# Boston, MA 02110-1301, USA.

########################################################################
# Setup library
########################################################################
include(GrPlatform) #define LIB_SUFFIX

list(APPEND isdbt_sources
    ofdm_sym_acquisition_impl.cc
    sync_and_channel_estimation_impl.cc
    subset_of_carriers_impl.cc
    frequency_deinterleaver_impl.cc
    time_deinterleaver_impl.cc
    symbol_demapper_impl.cc
    tmcc_decoder_impl.cc
    bit_deinterleaver_impl.cc
    d_viterbi.c
    d_metrics.c
    d_tab.c
    energy_descrambler_impl.cc
    byte_deinterleaver_impl.cc
    #bit_deinterleaver_streamed_impl.cc
    ofdm_sym_acquisition_dvbt_impl.cc
    reed_solomon_dec_isdbt_impl.cc 
    #reed_solomon.cc
    frequency_deinterleaver_1seg_impl.cc
    time_deinterleaver_1seg_impl.cc
    symbol_demapper_1seg_impl.cc
    viterbi_decoder_impl.cc
    ofdm_synchronization_impl.cc
    ofdm_synchronization_1seg_impl.cc
    tmcc_decoder_1seg_impl.cc
    pilot_signals_impl.cc
    frequency_interleaver_impl.cc
    time_interleaver_impl.cc
    hierarchical_combinator_impl.cc
    carrier_modulation_impl.cc
    byte_interleaver_impl.cc
    energy_dispersal_impl.cc
    tmcc_encoder_impl.cc
)

set(isdbt_sources "${isdbt_sources}" PARENT_SCOPE)
if(NOT isdbt_sources)
    MESSAGE(STATUS "No C++ sources... skipping lib/")
    return()
endif(NOT isdbt_sources)

add_library(gnuradio-isdbt SHARED ${isdbt_sources})
target_link_libraries(gnuradio-isdbt gnuradio::gnuradio-runtime gnuradio-fft gnuradio-filter gnuradio-fec)
target_include_directories(gnuradio-isdbt
    PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/../include>
    PUBLIC $<INSTALL_INTERFACE:include>
  )
set_target_properties(gnuradio-isdbt PROPERTIES DEFINE_SYMBOL "gnuradio_isdbt_EXPORTS")

if(APPLE)
    set_target_properties(gnuradio-isdbt PROPERTIES
        INSTALL_NAME_DIR "${CMAKE_INSTALL_PREFIX}/lib"
    )
endif(APPLE)

########################################################################
# Install built library files
########################################################################
include(GrMiscUtils)
GR_LIBRARY_FOO(gnuradio-isdbt)

########################################################################
# Print summary
########################################################################
message(STATUS "Using install prefix: ${CMAKE_INSTALL_PREFIX}")
message(STATUS "Building for version: ${VERSION} / ${LIBVER}")

########################################################################
# Build and register unit test
########################################################################
include(GrTest)

find_package(CppUnit)

if(NOT CPPUNIT_FOUND)
    message(FATAL_ERROR "CppUnit required to compile isdbt")
endif()

# If your unit tests require special include paths, add them here
#include_directories()
# List all files that contain Boost.UTF unit tests here
list(APPEND test_isdbt_sources
    #test_isdbt.cc
    #qa_isdbt.cc
    #qa_tmcc_encoder.cc
    #qa_energy_dispersal.cc
    #qa_byte_interleaver.cc
    #qa_carrier_modulation.cc
    #qa_hierarchical_combinator.cc
    #qa_time_interleaver.cc
    #qa_frequency_interleaver.cc
    #qa_pilot_signals.cc
    #qa_tmcc_decoder_1seg.cc
    #qa_ofdm_synchronization_1seg.cc
    #qa_ofdm_synchronization.cc
    #qa_reed_solomon_dec_isdbt.cc
    #qa_byte_deinterleaver.cc
    #qa_energy_descrambler.cc
    #qa_bit_deinterleaver.cc
    #qa_symbol_demapper.cc
    #qa_time_deinterleaver.cc
    #qa_frequency_deinterleaver.cc
    #qa_ofdm_sym_acquisition.cc
)
# Anything we need to link to for the unit tests go here
list(APPEND GR_TEST_TARGET_DEPS ${CPPUNIT_LIBRARIES} gnuradio-isdbt)

if(NOT test_isdbt_sources)
    MESSAGE(STATUS "No C++ unit tests... skipping")
    return()
endif(NOT test_isdbt_sources)

foreach(qa_file ${test_isdbt_sources})
    GR_ADD_CPP_TEST("isdbt_${qa_file}"
        ${CMAKE_CURRENT_SOURCE_DIR}/${qa_file}
    )
endforeach(qa_file)
