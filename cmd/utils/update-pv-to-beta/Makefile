# Copyright 2018 The Kubernetes Authors.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

ifeq ($(REGISTRY),)
	REGISTRY = registry.k8s.io/sig-storage/
endif

ifeq ($(VERSION),)
	VERSION = latest
endif

IMAGE = $(REGISTRY)local-volume-update-pv-to-beta:$(VERSION)
MUTABLE_IMAGE = $(REGISTRY)local-volume-update-pv-to-beta:latest

all build:
	CGO_ENABLED=0 GOOS=linux go build -a -ldflags '-extldflags "-static"' -o local-volume-update-pv-to-beta .
.PHONY: all build

container: build quick-container
.PHONY: container

quick-container:
	cp local-volume-update-pv-to-beta deployment/docker
	docker build -t $(MUTABLE_IMAGE) deployment/docker
	docker tag $(MUTABLE_IMAGE) $(IMAGE)
.PHONY: quick-container

push: container
	docker push $(IMAGE)
	docker push $(MUTABLE_IMAGE)
.PHONY: push

test:
	go test ./...
.PHONY: test

clean:
	rm -f local-volume-update-pv-to-beta
	rm -f deployment/docker/local-volume-update-pv-to-beta
.PHONY: clean

