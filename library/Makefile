#
# =BEGIN MIT LICENSE
# 
# The MIT License (MIT)
#
# Copyright (c) 2014 Andras Csizmadia
# http://www.vpmedia.hu
# 
# Permission is hereby granted, free of charge, to any person obtaining a copy
# of this software and associated documentation files (the "Software"), to deal
# in the Software without restriction, including without limitation the rights
# to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
# copies of the Software, and to permit persons to whom the Software is
# furnished to do so, subject to the following conditions:
# 
# The above copyright notice and this permission notice shall be included in
# all copies or substantial portions of the Software.
# 
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
# AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
# OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
# THE SOFTWARE.
# 
# =END MIT LICENSE
#
#

# Purpose: Air Native Extension C library makefile
# Author: Andras Csizmadia
# Version: 1.0.0
# Platform: Windows
# Runtime: MingW32-GCC | VS-MSVC

# @see http://gcc.gnu.org/onlinedocs/gcc-4.4.1/gcc/Option-Summary.html#Option-Summary
# @see https://www.gnu.org/software/make/manual/html_node/Implicit-Variables.html

# C++ Compiler arguments
CXXFLAGS=-Wall -O4 -Wno-unused-function -Wno-unused-value -Wno-unused-variable -Wno-parentheses -Wno-unknown-pragmas \
	-Wno-maybe-uninitialized
CC=g++
LDLIBS=

all: clean init build test 

.PHONY: all clean init build test 

clean:
	@echo "------------------------------------------- Clean --------------------------------------------"
	rm -f *.o *.a ClientExtension.dll *.exe *.lib *.def
    
init:
	@echo "-------------------------------------------- Init --------------------------------------------"
	cp $(AIR_HOME)/lib/win/FlashRuntimeExtensions.lib .
	cp cryptopp/libcryptopp.a .

build:
	@echo "-------------------------------------------- Build -------------------------------------------"
# Generate DLL Resource
	windres -i resource.rc -o resource.o
# Compile Program
	$(CC) $(CXXFLAGS) -I./cryptopp -DEXPORT -c dllmain.cpp ClientExtension.cpp
# Assemble DLL
	$(CC) -o ClientExtension.dll -shared -static-libgcc -static-libstdc++ \
	-Wl,--subsystem,windows,--output-def,ClientExtension.def,--out-implib,libClientExtension.a \
	dllmain.o ClientExtension.o resource.o FlashRuntimeExtensions.lib -L./. -lcryptopp

test:
	@echo "--------------------------------------------- Test --------------------------------------------"
	$(CC) $(CXXFLAGS) -DEXPORT -c ClientExtensionTest.cpp -I./cryptopp
	$(CC) -o ClientExtensionTest.exe -static-libgcc -static-libstdc++ ClientExtensionTest.o -I./cryptopp -L./. -lcryptopp
	./ClientExtensionTest
