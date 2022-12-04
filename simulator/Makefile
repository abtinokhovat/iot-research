PROG ?= ns3
PROG_VER ?= 3.36.1
REPO ?= morik/${PROG}_${PROG_VER}

all: build

build:
	echo "Building ${PROG} version = ${PROG_VER}"
	docker build -t ${REPO} .