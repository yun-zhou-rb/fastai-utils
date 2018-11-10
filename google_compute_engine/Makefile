#!make
# ENVFILE=env
# export $(shell [ ! -n "$(ENVFILE)" ] || cat $(ENVFILE) | grep -v \
#     --perl-regexp '^('$$(env | sed 's/=.*//'g | tr '\n' '|')')\=')

-include env

INSTANCE_NAME = ${USER}-fastai
MACHINE_TYPE ?= n1-standard-1
# n1-highmem-2
# n1-highmem-4 for 4 CPUs 26 Gb

ifneq ($(origin GC_PROJECT), undefined)
  GC_PROJECT_ARG = --project $(GC_PROJECT)
endif
ifneq ($(origin GC_ZONE), undefined)
  GC_ZONE_ARG = --zone $(GC_ZONE)
endif

GC_ARGS = $(GC_PROJECT_ARG) $(GC_ZONE_ARG)

.PHONY: create connect delete stop start

help:
	@echo "Available commands"
	@grep -E '^[a-zA-Z0-9_-]+:.*?' $(MAKEFILE_LIST) \
		| sed -E "s/^Makefile:([a-zA-Z0-9_-]+):(.* ## (.*))?/\1	\3/" \
		| column -s $$'\t' -t \
		| sort -f
	@echo "Current configuration"
	@echo "GC_PROJECT=${GC_PROJECT}, INSTANCE_NAME=${INSTANCE_NAME}, MACHINE_TYPE=${MACHINE_TYPE}"
		@echo "gcloud args: ${GC_ARGS}"


create:
	-gcloud compute instances create $(INSTANCE_NAME) $(GC_ARGS)\
			--machine-type $(MACHINE_TYPE) \
			--boot-disk-size 500 --image-project ubuntu-os-cloud --image-family ubuntu-1604-lts \
			--maintenance-policy TERMINATE --restart-on-failure \
			--accelerator type=nvidia-tesla-k80

	@echo "Waiting for the machine to start"
	sleep 30

	gcloud compute scp $(GC_ARGS) \
		gcp_setup_script $(INSTANCE_NAME):~

	gcloud compute ssh $(INSTANCE_NAME) $(GC_ARGS) \
		--command "chmod +x ~/gcp_setup_script; ~/gcp_setup_script"
	@echo "==========================="	
	@echo "|  Rebooting the machine  |"
	@echo "==========================="
	gcloud compute instances reset $(INSTANCE_NAME) $(GC_ARGS)
	sleep 60
	@echo "<========= Connecting to the machine =========>"
	make connect
connect:
	gcloud compute ssh $(INSTANCE_NAME) $(GC_ARGS) \
    -- -L 8082:localhost:8888

ssh:
	gcloud compute ssh $(INSTANCE_NAME) $(GC_ARGS)

delete:
	gcloud compute instances delete $(INSTANCE_NAME) $(GC_ARGS)

stop:
	gcloud compute instances stop $(INSTANCE_NAME) $(GC_ARGS)

start:
	-gcloud compute instances start $(INSTANCE_NAME) $(GC_ARGS)
	sleep 45
	make connect

update_machine_type:
	gcloud compute instances set-machine-type $(INSTANCE_NAME) $(GC_ARGS) \
			--machine-type $(MACHINE_TYPE)
