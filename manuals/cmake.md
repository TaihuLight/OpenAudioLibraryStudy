
# [CMAKE](../README.md)<a name = "TOP"></a>
1. [설치](#cmake-setup)
2. [사용](#cmake-execution)
3. [기초](#cmake-cmakelists)
	1. [예제 1](#cmake-ex1)
	2. [예제 2](#cmake-ex2)
4. [option](#option)
5. [message](#message)

cmake 는 linux환경에서는 Makefile을 Windows환경에서는 비주얼 스튜디오 프로젝트를 만든다.


## 설치<a name="cmake-setup"></a>

+ linux
```bash
    $ sudo apt-get install cmake       
```
+  windows 
  https://cmake.org/download/
  

## [사용](#TOP)<a name="cmake-execution"><a/>

1. 빌드할 코드가 있는 폴더에
 CMakeLists.txt 를 만든다.

2.    CmakeLists.txt 를 작성한뒤
3. cmake 를 사용한다  
	+ Linux  
	$ cmake 를 해주면 Makefile 이 생성된다.
	 ※builld 폴더 같은 걸 따로 만들어준 다음  
	 $ cmake .. 으로 결과물을 따로 보관하는 것이 좋다.
		     Makefile 을 작성해주는 것이아니라 Makefile 이 추가적으로 cmake가 만든 파일들을 이용하게 하는 것에 가깝디 

	+ Windows  
	  cmd에서 cmake를 해줘도 되지만 GUI를 사용하는 것이 좋다   
	  ~~ source code 에 CMakeLists.txt 위치  
	  ~~ the binaries 에 윈도우에서 사용할 프로젝트의 위치  
	  를 잡아준다  

	  옵션을 추가하려면  Add Entry로 추가하면된다  
	  그 뒤에, Configure 로 사용할 IDE를 고르고 Generate하면 binaries 경로에 프로젝트 파일이 생성된다  
	  Configure를 변경하려면, File/Delete Cache 하면 다시 설정할 수 있다  
 



## [CMakeLists.txt 작성](#TOP)<a name="cmake-cmakelists"></a>

필수  :

+ cmake_minimum_required(VERSION 내.cmake의.버전)  
```CMake
 cmake_minimum_required(VERSION 3.5.1)  
```
cmake 최소 버전 요구사항 설정, 버전이 다르면 설정된 값이나 명령어 사용이 다를수 있다.
cmake 의 버전은  
```bash
 cmake -version 으로 알 수 있다
```  
기본 명령 :   

 +   set (변수명 들어갈값 )   
```CMake
set(SOURCES src1.c src2.c src3.c)   	
```
  변수를 불러올 때에는 ${변수명} 으로 불러온다

+ add_executable(파일명 들어갈코드 )  
```CMake
add_executable(hello hello.c)
#			or  
add_executable(programm ${SOURCES})  
```

파일명에 해당하는 실행파일을 뒤의 인자로 들어가는 코드를로 빌드하게 한다  

---
## 예시 2-1<a name="cmake-ex1"></a>

/CMAKE

hello world 를 출력하는 hello.c가 있으면

CMakeLists.txt
```CMake
cmake_minimum_required(VERSION 3.5.1)
add_execuable(hello hello.c)
```

```bash
$ mkdir build    
$ cd build    
$ cmake ..    
$ make    
$ ./hello    
hello world    
```

---


다른 명령들 :  

+ prjoect(프로젝트명)  
 프로젝트의 이름을 정한다

+ message (STATUS "메세지")
cmake 도중에 메세지를 출력한다. 변수들의 값을 확인할 수 있다
```CMake
messgae (STATUS "src :  ${SOURCES}")	
```
+ include_directories()
헤더 폴더를 추가한다
```CMake
include_directories(header_folder)
```
headder_folder애서 헤더파일을 찾는다
		
+ find_package()
시스템에 있는 모듈을 찾는다  
cmake에 관련 값들은 cmake에서 설정해두었다  
```CMake
 find_package( Threads)
```
는	현재 OS의 쓰레드 관련 라이브러리를 찾는다  
			이 명령을 통해  
			MATH_THREAD_LIBS_INIT  
			CMAKE_USE_SPROC_INIT  
			CMAKE_USE_WIN32_THREAD_INIT  
			CMAKE_USE_PTHREADS_INIT  
			같은 변수들을 사용할 수 있게된다  
```CMake
 find_package(ALSA)  
```
는		ALSA_FOUND  
		ALSA_LIBRARIES  
		ALSA_INCLUDE_DIRS  
		ALSA_LIBRARY  
		ALSA_VERSION_STRING   
등을 사용할 수 있게된다			 


참고  
[modules](https://cmake.org/cmake/help/v3.0/manual/cmake-modules.7.html)
	- 패키지 목록
[find package](https://cmake.org/cmake/help/v3.0/command/find_package.html?highlight=find_package)



+	list(APPEND 값들)  
		- 변수에 값을 추가한다
		 set은 값을 덮어씌우는데  
		  list(APPEND  )를 쓰면 뒤에 이어 붙인다

+ add_definitions(-D변수 )  
		- 변수를 define 한다
		  코드 내에서 define 한것과 같은 효과를 가진다
		ex)
src.c
```C
if(_IS_DEFINE_)
	{
	 //do something
	}

```


이라는 코드가 있을때   
CMakeLists.txt에    
```CMake
add_definitions(-D_IS_DEFINE_)     
```
을 하면 코드에서 define 하지 않아도    
정의 된다    

+ if()  endif()
	- 조건문  

```CMake		
 if(LINUX)  
	... 리눅스 일 떄 
elseif(WIN32)  
	... 윈도우 일 때
endif()  
```			

OS별로 다르게 빌드 할 수 있다
			
	
	add_library(라이브러리명 옵션 파일)    
		- 라이브러리를 만든다  
		ex)add_library(hello SHARED ${SOURCES})  
			${SOURCES}에 있는 파일로 libhello.so 를 만든다  

		   add_library(bye STATIC ${CODES})  
			${COES}에 있는 파일로 bye.a를 만든다  

	target_link_libraries(TARGET LIBS)  
			- TARGET에 LIBS을 링크한다  
			  동적라이브러리를 실행파일에 링크할 수 있다  


```CMake
add_executable(out ${SOURCES})  
add_library(hello SHARED ${LIBSRC})  
target_link_libraries(out hello)  
```
SOURCES로 빌드한 out에
LIBSRC로 만든 libhello.so 라는 동적 라이브러리를
연결시켜준다

---


미리 설정된 변수 :  

	UNIX    : OS 가 UNIX 면 true 아니면 false
	APPLE   : OS 가 APPLE 이면 true 아니면 false, UNIX여도 true
	WIN32   : OS 가 WINDOWS 면 true 아니면 false,64bit이어도 true

참고  
 [variables](https://gitlab.kitware.com/cmake/community/wikis/doc/cmake/Useful-Variables)



## 예시 2-2<a name="cmake-ex2"></a>

/RtAudio
```CMake
 cmake_minimum_required(VERSION 3.5.1)
project(custom_RtAudio)

#----Release, Debug ...
set(CMAKE_BUILD_TYPE Release)

#set(LIST_DIRECTORIES true)
#set(CMAKE_INCLUDE_CURRENT_DIR ON)

#----directory for headers, cmake automatically sets dependency
#include_directories(dic_header_folder)

#set (SOURCES src/xx.c src/yy.c) : add src in var SOURCES

#----can add multiple sources with wildcard by 'GLOB'
#----not recommended, can be defined multiple times
#file(GLOB SOURCES src/*.c)

#----src files
#set(SOURCES RtAudio.cpp record.cpp)
set(SOURCES record.cpp RtAudio.cpp) 

#----set LINKLIBS for libs to be linked with the executable
set(LINKLIBS)

#----Check System OS
#----There are Variables such as "UNIX","WIN32","APPLE",
#----which are set by TRUE when that is target system's OS
#----In APPLE OS, APPLE and UNIX set TRUE
#----We set unix->alsa/windows->asio
#----or if(CMAKE_SYSTEM MATCHES Linux)
if(UNIX AND NOT APPLE) 
	message (STATUS "Target System is UNIX")


	add_executable(monitor monitor.c)

	#----find_package(package) : find moudle and define following vars 

	#----FindThreads( = find_package(Threads)) set variables 
	#----MAKE_THREAD_LIBS_INIT     - the thread library
	#----CMAKE_USE_SPROC_INIT       - are we using sproc?
	#----CMAKE_USE_WIN32_THREADS_INIT - using WIN32 threads?
	#----CMAKE_USE_PTHREADS_INIT    - are we using pthreads
	#----CMAKE_HP_PTHREADS_INIT     - are we using hp pthreads
		

	#----find_package for ALSA
    #----ALSA_FOUND       - True if ALSA_INCLUDE_DIR & ALSA_LIBRARY are found
    #----ALSA_LIBRARIES   - Set when ALSA_LIBRARY is found
    #----ALSA_INCLUDE_DIRS - Set when ALSA_INCLUDE_DIR is found
    #----ALSA_INCLUDE_DIR - where to find asoundlib.h, etc.
    #----ALSA_LIBRARY     - the asound library
    #----ALSA_VERSION_STRING - the version of alsa found (since CMake 2.8.8)
	#find_package(ALSA REQUIRED)
	find_package(ALSA)	

	#----REQUIRED : if packaged is not found stops with an error message
	#----CMAKE_THREAD_PREFER_PTHRREAD : if there are multiple lib, use pthread
	find_package(Threads REQUIRED CMAKE_THREAD_PREFER_PTHREAD)


	include_directories(${ALSA_INCLUDE_DIR})
	#append libs to LINKLIBS
	list(APPEND LINKLIBS ${ALSA_LIBRARY} ${CMAKE_THREAD_LIBS_INIT})

	#----define __LINUX_ALSA__ for RtAudio.cpp
	#----considering as LINUX uses ALSA in default
	add_definitions(-D__LINUX_ALSA__)
#----= if(CMAKE_SYSTEM MATCHES Windows)
#----also true in win64

elseif(WIN32) 	
	include_directories(include)
	list(APPEND LINKLIBS winmm ole32)
	add_definitions(-D__WINDOWS_WASAPI__)
	message(STATUS "using Windows WASAPI")
	list(APPEND LINKLIBS uuid ksuser)
endif()	

#Windows 7 uses WASAPI
#	message (STATUS "Target System is Windows")	
#	list(APPEND LINKLIBS winmm ole32)
#	list(APPEND SOURCES
#		include/asio.cpp
#		include/asiodrivers.cpp
#		include/asiolist.cpp
#		include/iasiothiscallresolver.cpp)
#	add_definitions(-D__WINDOWS_ASIO__)

#----create shared library
#----Generate the shared library from the sources
#add_library(hello SHARED ${SOURCES})
#add_library(rtaudio SHARED RtAudio.cpp)
message (STATUS rtaudio)
#list(APPEND LINKLIBS rtaudio)

#create executable file with SOURCES
add_executable(record ${SOURCES})
#add_executable(record record.cpp)

#----For Windows env, but global data must be handled manually.
#----see [ https://blog.kitware.com/create-dlls-on-windows-without-declspec-using-new-cmake-export-all-feature/ ]
if(WIN32)
	#!!!! Didn't check whether works or not,yet 
	set(BUILD_SHARED_LIBS ON)
	set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS ON)
endif()
message (STATUS ${LINKLIBS})
#----link external libraries with executable
#----order is quiet important

cmake_policy(set CMP0042 OLD)

target_link_libraries(record ${LINKLIBS})

#It is RtAudio.cpp which actually uses LINKLIBS so link LINKLIBS with librtauido.so
#target_link_libraries(rtaudio ${LINKLIBS})
#target_link_libraries(record rtaudio)

add_executable(audioprobe audioprobe.cpp RtAudio.cpp)
target_link_libraries(audioprobe ${LINKLIBS})


```

+ 참고사항


윈도우에서 DLL 사용시 아래의 ENTRY를 추가(bool,true)  
CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS  
BUILD_SHARED_LIBS  

단. global data value는 따로 처리해야한다.   
참고 :  https://blog.kitware.com/create-dlls-on-windows-without-declspec-using-new-cmake-export-all-feature/

---

## [옵션](#TOP)<a name="option"></a>

```CMake
option(<option_variable> "help string describing option" [initial value])

CMAKE_DEPENDENT_OPTION(USE_FOO "Use Foo" ON "USE_BAR;NOT USE_ZOT" OFF)

```
## [MESSAGE](#TOP)<a name = "message"></a>

```CMake
message( <keyword> "message" )

message(STATUS "SOURCE : " ${SOURCE})
message(FATAL_ERROR "ERROR... ABORT")
```

keyword | discrpiton
---|---
  (none)         | 중요 정보
  STATUS         | 사건 정보
  WARNING        | 경고, 계속 진행 
  AUTHOR_WARNING | 경고(개발자), 계속진행
  SEND_ERROR     | 에러, 계속 진행하지만 생성하지는 않음
  FATAL_ERROR    | 에러, 모든 프로세스 중단
