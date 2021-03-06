############################
# Parameters:
#  BUILD_DIR     absolute path
#  DOC_DIR       absolute path
#  INSTALL_DIR   absolute path
#  VERSION_NAME  name of this compilation
#  SUFFIX_LIB    it must be defined as ".dll" on windows and as ".so" on linux 
#  SUFFIX_EXE    it must be defined as ".exe" on windows and as "" on linux 
############################

PROJECT_NAME := metricknn_cli
VERSION_NAME := 0.0.0
BUILD_DIR    := $(realpath $(PROJECT_NAME)_$(VERSION_NAME)/build)
DOC_DIR      := $(realpath $(PROJECT_NAME)_$(VERSION_NAME)/doc)
INSTALL_DIR  := $(realpath $(PROJECT_NAME)_$(VERSION_NAME)/install)
EXECUTABLE   := $(BUILD_DIR)/$(PROJECT_NAME)$(SUFFIX_EXE)

##########

CFLAGS   += 
LDFLAGS  += 

##########

ifneq ($(shell find . -maxdepth 0),.)
    $(error command "find" is incorrect, please add GNU's find command to PATH.)
endif

SOURCE_DIR  := src
H_FILES     := $(shell find $(SOURCE_DIR) -type f -name \*.h)
C_FILES     := $(shell find $(SOURCE_DIR) -type f -name \*.c)
C_O_FILES   := $(addprefix $(BUILD_DIR)/c/,$(C_FILES:.c=.o))
HPP_FILES   := $(shell find $(SOURCE_DIR) -type f -name \*.hpp)
CPP_FILES   := $(shell find $(SOURCE_DIR) -type f -name \*.cpp)
CPP_O_FILES := $(addprefix $(BUILD_DIR)/cpp/,$(CPP_FILES:.cpp=.o))

#### RULES ####

.PHONY: all install clean doc

all: $(EXECUTABLE)

$(EXECUTABLE): $(C_O_FILES) $(CPP_O_FILES)
	mkdir -p "$(BUILD_DIR)"
	g++ -o "$(EXECUTABLE)" $(C_O_FILES) $(CPP_O_FILES) $(LDFLAGS)

$(BUILD_DIR)/c/%.o: %.c $(H_FILES)
	mkdir -p $(@D)
	gcc -std=gnu99 -c $(CFLAGS) "-DVERSION_NAME=$(VERSION_NAME)" -o $@ $<
	
$(BUILD_DIR)/cpp/%.o: %.cpp $(HPP_FILES) $(H_FILES)
	mkdir -p $(@D)
	g++ -std=gnu++11 -c $(CFLAGS) "-DVERSION_NAME=$(VERSION_NAME)" -o $@ $<

install:
	install -d "$(INSTALL_DIR)/bin"
	install "$(EXECUTABLE)" "$(INSTALL_DIR)/bin/metricknn$(SUFFIX_EXE)"

clean:
	rm -rf "$(BUILD_DIR)"

doc:
	rm -rf "$(DOC_DIR)"
	sleep 1
	mkdir -p "$(DOC_DIR)"
	sed -e "s#^OUTPUT_DIRECTORY.*#OUTPUT_DIRECTORY = $(DOC_DIR)#" Doxyfile | doxygen -
