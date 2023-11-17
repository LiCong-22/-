

![[延锋应用程序开发框架(final).pdf]]

# 一. apal

message


# 二. build


Makefile文件
Makefile文件进行编译

例如：module.mk

```c
# Change this Variables for your project
# TEMPLATE can be 'app' for application, 'so' for shared library, 'ar' for static library
TEMPLATE = app
TARGET = DiagnosisServer

MODULE_TOP_DIR	:=	./
SRC_TOP_DIR		:=	$(MODULE_TOP_DIR)src/
OBJ_TOP_DIR		:=	$(MODULE_TOP_DIR)out/obj/
BIN_TOP_DIR		:=	$(MODULE_TOP_DIR)out/bin/
INC_TOP_DIR		:=	$(MODULE_TOP_DIR)inc/ 
PUBLIC_IF_DIR   := $(MODULE_TOP_DIR)client_inc/

EXTRA_CFLAGS += -Wno-unused-parameter

# INSTALL_DIR is this Feature SDK folder
INSTALL_DIR 		:=	$(SDK_TOP_DIR)/yf-service/DiagnosisServer/

# DEPLOY_DIR is the deploy rootfs folder; DEPLOY_TARGET_DIR is this Module deploy folder.
DEPLOY_DIR		    :=	$(PRO_TOP_DIR)/di.app.display.ic/ic/system_display/obj/aarch64le/
DEPLOY_TARGET_DIR	:=	$(DEPLOY_DIR)bin/

SOURCES += \
	$(SRC_TOP_DIR)IDiagnosisManagerBase.cpp \
	$(SRC_TOP_DIR)DiagnosisLogisticManager.cpp \
	$(SRC_TOP_DIR)DiagnosisInternalManager.cpp \
	$(SRC_TOP_DIR)DiagnosisConfigurationManager.cpp \
	$(SRC_TOP_DIR)DiagnosisBitmappedIOManager.cpp \
	$(SRC_TOP_DIR)DiagnosisNonbitmappedIOManager.cpp \
	$(SRC_TOP_DIR)DiagnosisDtcsListManager.cpp \
	$(SRC_TOP_DIR)DiagnosisDtcSnapshotManager.cpp \
	$(SRC_TOP_DIR)DiagnosisRidsApplicationManager.cpp \
	$(SRC_TOP_DIR)DiagnosisEcuProgrammingManager.cpp \
	$(SRC_TOP_DIR)DiagnosisApplicationDeviationsManager.cpp \
	$(SRC_TOP_DIR)DiagnosisBootloaderDeviationsManager.cpp \
	$(SRC_TOP_DIR)DiagnosisEOLManager.cpp \
	$(SRC_TOP_DIR)DiagEventManager.cpp \
	$(SRC_TOP_DIR)DiagnosisExtraManager.cpp \
	$(SRC_TOP_DIR)DiagnosisServer.cpp \

INCLUDEPATH += \
	$(INC_TOP_DIR) \
	$(PUBLIC_IF_DIR) \
	$(MODULE_TOP_DIR)../DiagnosisClient/include \
	$(QNX_INSTALL)/usr/include \
	$(MODULE_TOP_DIR)../BusInterface/BusService/client_inc \
	$(MODULE_TOP_DIR)../Utility/inc \
	$(SDK_TOP_DIR)/infra/fdbus/include \
	$(SDK_TOP_DIR)/infra/protobuf/include \
	$(SDK_TOP_DIR)/infra/yfipc/include \
	$(SDK_TOP_DIR)/infra/gwclient/include \
	$(SDK_TOP_DIR)/infra/lifecycleClient/include \
	$(QNX_TARGET)/usr/include/io-pkt      \
	$(SDK_TOP_DIR)/infra/lifecycleVhub/include \
	$(SDK_TOP_DIR)/infra/propertyClient/include \
	$(SDK_TOP_DIR)/infra/osal/include 

LIBS += \
	-L$(SDK_TOP_DIR)/../install/aarch64le/lib/ -li2c_client \
	-L$(MODULE_TOP_DIR)../BusInterface/BusService/out/obj/lib/  -lBusService \
	-L$(MODULE_TOP_DIR)../Utility/out/obj/lib/ -lUtility \
	-L$(SDK_TOP_DIR)infra/fdbus/lib/ -lcommon_base \
	-L$(SDK_TOP_DIR)infra/yfipc/lib -lyfipc \
	-L$(SDK_TOP_DIR)infra/protobuf/lib/ -lprotobuf \
	-L$(SDK_TOP_DIR)infra/gwclient/lib/ -lgwclient \
	-L$(SDK_TOP_DIR)infra/lifecycleClient/lib/ -llifecycleclient \
	-L$(SDK_TOP_DIR)/infra/propertyClient/lib/ -lyfproperty \
	-L$(SDK_TOP_DIR)/infra/osal/lib -losal \
	-L$(QNX_TARGET)/aarch64le/usr/lib -lscreen \
	-lsocket

ifeq ($(LOG_TYPE), LOG_BANMA)
DEFS 		+= -DLOG_BANMA
INCLUDEPATH +=   $(SDK_TOP_DIR)infra/logclient/include \
                 $(SDK_TOP_DIR)3rdParty/vhubd/include
LIBS 		+= -L$(SDK_TOP_DIR)3rdParty/vhubd/lib64 -llog
else ifeq ($(LOG_TYPE), LOG_CONSOLE)
DEFS 		+= -DLOG_CONSOLE
else ifeq ($(LOG_TYPE), LOG_ZONE)
DEFS 		+= -DLOG_ZONE
INCLUDEPATH +=   $(SDK_TOP_DIR)infra/logclient/include \
				 $(SDK_TOP_DIR)infra/dlt/include
LIBS 		+= -L$(SDK_TOP_DIR)infra/logclient/lib -llogclient \
			   -L$(SDK_TOP_DIR)infra/dlt/lib/ -ldlt
else
DEFS 		+= -DLOG_DLT
INCLUDEPATH +=   $(SDK_TOP_DIR)infra/logclient/include \
				 $(SDK_TOP_DIR)infra/dlt/include
LIBS 		+= -L$(SDK_TOP_DIR)infra/logclient/lib -llogclient \
			   -L$(SDK_TOP_DIR)infra/dlt/lib/ -ldlt
endif
# EXTRA_CFLAGS += -ggdb

DEFS += \
	-DLOG_DLT

```

这是一个C++项目的Makefile文件，用于编译和构建DiagnosisServer目标程序。下面是对其中各个变量的解释：

- TEMPLATE：指定项目类型，可以是'app'表示应用程序，'so'表示共享库，'ar'表示静态库。
- TARGET：指定目标名称，本例中为DiagnosisServer。
- MODULE_TOP_DIR：模块的顶级目录。
- SRC_TOP_DIR：源代码目录。
- OBJ_TOP_DIR：目标文件输出目录。
- BIN_TOP_DIR：可执行文件输出目录。
- INC_TOP_DIR：头文件目录。
- PUBLIC_IF_DIR：公共接口头文件目录。
- EXTRA_CFLAGS：额外的编译选项。
- INSTALL_DIR：安装目录，将目标文件安装到该目录中。
- DEPLOY_DIR：部署根文件系统目录。
- DEPLOY_TARGET_DIR：部署目标目录。
- SOURCES：源文件列表。
- INCLUDEPATH：头文件搜索路径。
- LIBS：链接库列表。
- DEFS：预定义宏列表。

请注意，这只是一个部分的Makefile文件，还可能包含其他规则和命令来完成编译、链接和安装等操作。如果你需要更详细的帮助，请提供完整的Makefile文件或具体的问题描述。

-L$(SDK_TOP_DIR)/../install/aarch64le/lib/ -li2c_client \是什么意思

`-L` 是编译器链接选项中的一个标志，用于指定库文件的搜索路径。在这个Makefile文件中，`-L$(SDK_TOP_DIR)/../install/aarch64le/lib/` 指定了一个库文件的搜索路径。
`$(SDK_TOP_DIR)` 是一个变量，表示SDK的顶级目录。`../install/aarch64le/lib/` 是相对于SDK顶级目录的路径，指定了库文件所在的目录。
`-li2c_client` 是指定要链接的库文件的名称，`-l` 标志后面跟着库文件的名称（不包括文件扩展名）。在这个例子中，`-li2c_client` 意味着要链接名为 `libi2c_client.so` 或 `libi2c_client.a` 的库文件。
综合起来，`-L$(SDK_TOP_DIR)/../install/aarch64le/lib/` 指定了要搜索库文件的路径，而 `-li2c_client` 则指定了要链接的具体库文件。这样，在编译和链接过程中，编译器将会在指定的路径中搜索 `libi2c_client.so` 或 `libi2c_client.a` 文件并进行链接操作。

