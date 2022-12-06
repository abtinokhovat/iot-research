PROG ?= ns3
PROG_VER ?= 3.37
REPO ?= abtinokhovat/${PROG}_${PROG_VER}

all: build

build:
	echo "Building ${PROG} version = ${PROG_VER}"
	docker build -t ${REPO}-${PROG_VER} .
	