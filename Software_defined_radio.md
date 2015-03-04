# 开源无线电

参考 http://blog.sina.com.cn/s/blog_67cdafe201014odm.html

# 在debian上

安装参考1提到的一些软件
```text
gr-air-modes
rtl-sdr
```

安装`rtl-sdr`

运行程序如果出现以下的提示:
```text
Kernel driver is active, or device is claimed by second instance of librtlsdr.
In the first case, please either detach or blacklist the kernel module
(dvb_usb_rtl28xxu), or enable automatic detaching at compile time.
```
则试试这个,来源: https://groups.google.com/d/topic/ultra-cheap-sdr/6_sSON94Azo (谷歌搜到的)
```bash
sudo rmmod dvb_usb_rtl28xxu rtl2832 
```

# 查看信息

```bash
rtl_eeprom
```
大致信息如下
```
zodiac1111@debian:~$ rtl_eeprom
Found 1 device(s):
  0:  Generic RTL2832U OEM

Using device 0: Generic RTL2832U OEM
Found Rafael Micro R820T tuner

Current configuration:
__________________________________________
Vendor ID:		0x0bda
Product ID:		0x2838
Manufacturer:		Realtek
Product:		RTL2838UHIDIR
Serial number:		00000001
Serial number enabled:	yes
IR endpoint enabled:	yes
Remote wakeup enabled:	no
__________________________________________
```

下面是几种玩法介绍

# 逆向433Mhz

http://oszine.com/matlab-sdr-433mhz/

# 听fm

参考 http://kmkeen.com/rtl-demod-guide/

参考 http://sdr.osmocom.org/trac/wiki/rtl-sdr

```bash
rtl_fm -f 93M -M wbfm -s 200000 -r 48000 - | aplay -r 48k -f S16_LE
```

# 飞机
[飞机通讯的基础](http://zh.wikipedia.org/wiki/%E9%A3%9E%E6%9C%BA%E9%80%9A%E4%BF%A1%E5%AF%BB%E5%9D%80%E4%B8%8E%E6%8A%A5%E5%91%8A%E7%B3%BB%E7%BB%9F)

[更简单的方式](http://sdr-x.github.io/rtl-sdr-rtl2832%E7%94%B5%E8%A7%86%E6%A3%92%E8%B7%9F%E8%B8%AA%E9%A3%9E%E6%9C%BA-%E6%9B%B4%E7%AE%80%E5%8D%95%E7%9A%84%E6%96%B9%E6%B3%95%E5%9C%A8Windows%E5%92%8CLinux%E4%B8%8B(simpler%20way%20to%20track%20plane%20by%20rtl-sdr)/) 已经测试成功

    ./dump1090 --aggressive --net --interactive --enable-agc 

参考  http://www.hamradioscience.com/the-rtl-2832u-sdr-and-ads-b/

参考 追踪飞机 http://www.irrational.net/2012/08/06/tracking-planes-for-20-or-less/

```bash
modes_rx -d -P use -s osmocom --kml=1.kml
```

安装 gr_
```bash
zodiac1111@debian:tmp$ git clone https://github.com/bistromath/gr-air-modes.git
正克隆到 'gr-air-modes'...
remote: Reusing existing pack: 2302, done.
remote: Total 2302 (delta 0), reused 0 (delta 0)
接收对象中: 100% (2302/2302), 1.10 MiB | 238.00 KiB/s, 完成.
处理 delta 中: 100% (1385/1385), 完成.
检查连接... 完成。
zodiac1111@debian:tmp$ git clone https://github.com/bistromath/gr-air-modes.git
fatal: 目标路径 'gr-air-modes' 已经存在，并且不是一个空目录。
zodiac1111@debian:tmp$ cd gr-air-modes/
zodiac1111@debian:gr-air-modes$ cmake .
-- The CXX compiler identification is GNU 4.8.2
-- The C compiler identification is GNU 4.8.2
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Build type not specified: defaulting to release.
-- Boost version: 1.54.0
-- Found the following Boost libraries:
--   date_time
--   program_options
--   filesystem
--   system
--   thread
-- Found PythonLibs: /usr/lib/i386-linux-gnu/libpython2.7.so (found suitable version "2.7.6", minimum required is "2") 
-- Found PkgConfig: /usr/bin/pkg-config (found version "0.28") 
Checking for GNU Radio Module: RUNTIME
-- checking for module 'gnuradio-runtime'
--   found gnuradio-runtime, version 3.7.3
 * INCLUDES=/usr/include
 * LIBS=/usr/lib/i386-linux-gnu/libgnuradio-runtime.so;/usr/lib/i386-linux-gnu/libgnuradio-pmt.so
-- Found GNURADIO_RUNTIME: /usr/lib/i386-linux-gnu/libgnuradio-runtime.so;/usr/lib/i386-linux-gnu/libgnuradio-pmt.so  
GNURADIO_RUNTIME_FOUND = TRUE
-- Found PythonInterp: /usr/bin/python2 (found suitable version "2.7.6", minimum required is "2") 
-- 
-- Python checking for PyZMQ
-- Python checking for PyZMQ - found
-- Could NOT find SWIG (missing:  SWIG_EXECUTABLE SWIG_DIR) 
-- Found PythonLibs: /usr/lib/i386-linux-gnu/libpython2.7.so (found version "2.7.6") 
-- Found Doxygen: /usr/bin/doxygen (found version "1.8.7") 
-- pyuic4 not found, not installing GUI application
-- Configuring done
-- Generating done
-- Build files have been written to: /home/zodiac1111/tmp/gr-air-modes
zodiac1111@debian:gr-air-modes$ cmake .
-- Build type not specified: defaulting to release.
-- Boost version: 1.54.0
-- Found the following Boost libraries:
--   date_time
--   program_options
--   filesystem
--   system
--   thread
-- Found PythonLibs: /usr/lib/i386-linux-gnu/libpython2.7.so (found suitable version "2.7.6", minimum required is "2") 
Checking for GNU Radio Module: RUNTIME
 * INCLUDES=/usr/include
 * LIBS=/usr/lib/i386-linux-gnu/libgnuradio-runtime.so;/usr/lib/i386-linux-gnu/libgnuradio-pmt.so
GNURADIO_RUNTIME_FOUND = TRUE
-- 
-- Python checking for PyZMQ
-- Python checking for PyZMQ - found
-- Found SWIG: /usr/bin/swig2.0 (found version "2.0.12") 
-- Found PythonLibs: /usr/lib/i386-linux-gnu/libpython2.7.so (found version "2.7.6") 
-- Looking for sys/types.h
-- Looking for sys/types.h - found
-- Looking for stdint.h
-- Looking for stdint.h - found
-- Looking for stddef.h
-- Looking for stddef.h - found
-- Check size of size_t
-- Check size of size_t - done
-- Check size of unsigned int
-- Check size of unsigned int - done
-- Found PythonLibs: /usr/lib/i386-linux-gnu/libpython2.7.so (found suitable version "2.7.6", minimum required is "2") 
-- Found PyQt4 version: 4.10.4
-- Found pyuic4: /usr/bin/pyuic4
-- QWT Version: 6.0.0
-- Found Qwt: /usr/lib/libqwt.so  
-- Configuring done
-- Generating done
-- Build files have been written to: /home/zodiac1111/tmp/gr-air-modes
zodiac1111@debian:gr-air-modes$ make
Scanning dependencies of target air_modes
[  5%] Building CXX object lib/CMakeFiles/air_modes.dir/preamble_impl.cc.o
In file included from /usr/include/gnuradio/block.h:29:0,
                 from /home/zodiac1111/tmp/gr-air-modes/lib/preamble_impl.h:5,
                 from /home/zodiac1111/tmp/gr-air-modes/lib/preamble_impl.cc:29:
/usr/include/gnuradio/logger.h:116:31: fatal error: log4cpp/Category.hh: 没有那个文件或目录
 #include <log4cpp/Category.hh>
                               ^
compilation terminated.
lib/CMakeFiles/air_modes.dir/build.make:54: recipe for target 'lib/CMakeFiles/air_modes.dir/preamble_impl.cc.o' failed
make[2]: *** [lib/CMakeFiles/air_modes.dir/preamble_impl.cc.o] Error 1
CMakeFiles/Makefile2:121: recipe for target 'lib/CMakeFiles/air_modes.dir/all' failed
make[1]: *** [lib/CMakeFiles/air_modes.dir/all] Error 2
Makefile:123: recipe for target 'all' failed
make: *** [all] Error 2
zodiac1111@debian:gr-air-modes$ make
[  5%] Building CXX object lib/CMakeFiles/air_modes.dir/preamble_impl.cc.o
[ 10%] Building CXX object lib/CMakeFiles/air_modes.dir/slicer_impl.cc.o
[ 15%] Building CXX object lib/CMakeFiles/air_modes.dir/modes_crc.cc.o
Linking CXX shared library libair_modes.so
[ 15%] Built target air_modes
Scanning dependencies of target _air_modes_swig_swig_tag
[ 20%] Building CXX object swig/CMakeFiles/_air_modes_swig_swig_tag.dir/_air_modes_swig_swig_tag.cpp.o
Linking CXX executable _air_modes_swig_swig_tag
[ 20%] Built target _air_modes_swig_swig_tag
[ 25%] Generating air_modes_swig.tag
[ 30%] Swig source
Scanning dependencies of target _air_modes_swig
[ 30%] Swig source
[ 35%] Building CXX object swig/CMakeFiles/_air_modes_swig.dir/air_modesPYTHON_wrap.cxx.o
Linking CXX shared module _air_modes_swig.so
[ 35%] Built target _air_modes_swig
Scanning dependencies of target pygen_swig_83972
[ 40%] Generating air_modes_swig.pyc
[ 45%] Generating air_modes_swig.pyo
[ 45%] Built target pygen_swig_83972
Scanning dependencies of target pygen_python_a76d8
[ 50%] Generating __init__.pyc, altitude.pyc, az_map.pyc, cpr.pyc, html_template.pyc, mlat.pyc, exceptions.pyc, flightgear.pyc, gui_model.pyc, kml.pyc, parse.pyc, msprint.pyc, radio.pyc, raw_server.pyc, rx_path.pyc, sbs1.pyc, sql.pyc, types.pyc, zmq_socket.pyc, Quaternion.pyc
[ 55%] Generating __init__.pyo, altitude.pyo, az_map.pyo, cpr.pyo, html_template.pyo, mlat.pyo, exceptions.pyo, flightgear.pyo, gui_model.pyo, kml.pyo, parse.pyo, msprint.pyo, radio.pyo, raw_server.pyo, rx_path.pyo, sbs1.pyo, sql.pyo, types.pyo, zmq_socket.pyo, Quaternion.pyo
[ 55%] Built target pygen_python_a76d8
Scanning dependencies of target pygen_apps_84470
[ 60%] Shebangin modes_rx
[ 65%] Shebangin modes_gui
[ 70%] Shebangin uhd_modes.py
[ 70%] Built target pygen_apps_84470
Scanning dependencies of target pygen_res_65095
[ 75%] Generating modes_rx_ui_borked.py
[ 80%] Generating modes_rx_ui.py
[ 85%] Generating modes_rx_ui.pyc
[ 90%] Generating modes_rx_ui.pyo
[ 90%] Built target pygen_res_65095
Scanning dependencies of target rx_ui
[100%] Built target rx_ui
zodiac1111@debian:gr-air-modes$ sudo make install
[sudo] password for zodiac1111: 
[ 15%] Built target air_modes
[ 20%] Built target _air_modes_swig_swig_tag
[ 25%] Swig source
[ 30%] Building CXX object swig/CMakeFiles/_air_modes_swig.dir/air_modesPYTHON_wrap.cxx.o
Linking CXX shared module _air_modes_swig.so
[ 35%] Built target _air_modes_swig
[ 40%] Generating air_modes_swig.pyc
[ 45%] Generating air_modes_swig.pyo
[ 45%] Built target pygen_swig_83972
[ 55%] Built target pygen_python_a76d8
[ 70%] Built target pygen_apps_84470
[ 90%] Built target pygen_res_65095
[100%] Built target rx_ui
Install the project...
-- Install configuration: "Release"
-- Installing: /usr/local/include/gr_air_modes/preamble.h
-- Installing: /usr/local/include/gr_air_modes/slicer.h
-- Installing: /usr/local/include/gr_air_modes/types.h
-- Installing: /usr/local/include/gr_air_modes/api.h
-- Installing: /usr/local/lib/libair_modes.so.0.0
-- Installing: /usr/local/lib/libair_modes.so.0
-- Installing: /usr/local/lib/libair_modes.so
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/_air_modes_swig.so
-- Removed runtime path from "/usr/local/lib/python2.7/dist-packages/air_modes/_air_modes_swig.so"
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/air_modes_swig.py
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/air_modes_swig.pyc
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/air_modes_swig.pyo
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/__init__.py
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/altitude.py
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/az_map.py
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/cpr.py
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/html_template.py
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/mlat.py
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/exceptions.py
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/flightgear.py
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/gui_model.py
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/kml.py
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/parse.py
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/msprint.py
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/radio.py
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/raw_server.py
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/rx_path.py
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/sbs1.py
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/sql.py
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/types.py
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/zmq_socket.py
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/Quaternion.py
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/__init__.pyc
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/altitude.pyc
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/az_map.pyc
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/cpr.pyc
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/html_template.pyc
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/mlat.pyc
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/exceptions.pyc
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/flightgear.pyc
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/gui_model.pyc
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/kml.pyc
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/parse.pyc
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/msprint.pyc
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/radio.pyc
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/raw_server.pyc
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/rx_path.pyc
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/sbs1.pyc
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/sql.pyc
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/types.pyc
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/zmq_socket.pyc
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/Quaternion.pyc
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/__init__.pyo
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/altitude.pyo
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/az_map.pyo
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/cpr.pyo
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/html_template.pyo
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/mlat.pyo
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/exceptions.pyo
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/flightgear.pyo
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/gui_model.pyo
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/kml.pyo
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/parse.pyo
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/msprint.pyo
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/radio.pyo
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/raw_server.pyo
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/rx_path.pyo
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/sbs1.pyo
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/sql.pyo
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/types.pyo
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/zmq_socket.pyo
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/Quaternion.pyo
-- Installing: /usr/local/bin/modes_rx
-- Installing: /usr/local/bin/modes_gui
-- Installing: /usr/local/bin/uhd_modes.py
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/modes_rx_ui.py
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/modes_rx_ui.pyc
-- Installing: /usr/local/lib/python2.7/dist-packages/air_modes/modes_rx_ui.pyo
zodiac1111@debian:gr-air-modes$ sudo ldconfig
zodiac1111@debian:gr-air-modes$ 
```