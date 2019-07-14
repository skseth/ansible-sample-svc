OUT_DIR=out
VERSION := $(shell cat VERSION)
BUILD_NUMBER=1

SERVICE_NAME=hello-service
SERVICE_NAME_VERSION=hello-service-${VERSION}
SERVICE_LOCAL_BINARY=${OUT_DIR}/${SERVICE_NAME}
SERVICE_BINARY=${OUT_DIR}/${SERVICE_NAME}-linux
SERVICE_SOURCES := $(shell find . -name '*.go')

RPM_SRC_IN_DIR=${OUT_DIR}/${SERVICE_NAME_VERSION}

RPM_TOP_DIR=${PWD}/${OUT_DIR}/hello-rpm
RPM_SRC_DIR=${RPM_TOP_DIR}/SOURCES
RPM_SRC_TARGZ=${RPM_SRC_DIR}/${SERVICE_NAME_VERSION}.tar.gz

RPM_RPMS_DIR=${RPM_TOP_DIR}/RPMS
SERVICE_RPM=${RPM_RPMS_DIR}/x86_64/${SERVICE_NAME_VERSION}-${BUILD_NUMBER}.x86_64.rpm

.PHONY: all local build rpm rpmtar publish

${SERVICE_LOCAL_BINARY}: ${SERVICE_SOURCES} go.mod go.sum
	go build -o $@ cmd/hello.go

${SERVICE_BINARY}: ${SERVICE_SOURCES} go.mod go.sum
	GOOS=linux go build -o $@ cmd/hello.go

${RPM_SRC_TARGZ}: ${SERVICE_BINARY} rpm/LICENSE rpm/hello.service rpm/hello.json rpm/hello.sysconfig
	rm -rf ${RPM_SRC_IN_DIR}
	mkdir -p ${RPM_SRC_IN_DIR}
	mkdir -p ${RPM_SRC_DIR}
	cp ${SERVICE_BINARY} ${RPM_SRC_IN_DIR}/${SERVICE_NAME}
	cp rpm/{LICENSE,hello.service,hello.json,hello.sysconfig} ${RPM_SRC_IN_DIR}
	tar -cvzf $@ --directory ${OUT_DIR} ${SERVICE_NAME_VERSION}

${SERVICE_RPM}: ${RPM_SRC_TARGZ} rpm/hello-service.spec
	rm -rf ${RPM_TOP_DIR}/{BUILD,RPMS}
	mkdir -p ${RPM_TOP_DIR}/{BUILD,RPMS}
	@rpmbuild -v -bb \
		--define "_topdir ${RPM_TOP_DIR}" \
		--define "version ${VERSION}" \
		--define "build_number ${BUILD_NUMBER}" \
		rpm/hello-service.spec

local: ${SERVICE_LOCAL_BINARY}

rpmtar: ${RPM_SRC_TARGZ}

build: ${SERVICE_BINARY} 

rpm: ${SERVICE_RPM}

publish: ${SERVICE_RPM}
	cp ${SERVICE_RPM} ../files

all: build rpm

