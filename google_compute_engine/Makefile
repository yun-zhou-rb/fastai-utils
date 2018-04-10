
GC_PROJECT ?= none
GC_ZONE ?= us-west1-b
INSTANCE_NAME = ${USER}-fastai
MACHINE_TYPE ?= n1-standard-1
# n1-highmem-2
# n1-highmem-4 for 4 CPUs 26 Gb

.PHONY: create connect delete stop start

help:
	@echo "Available commands"
	@grep -E '^[a-zA-Z0-9_-]+:.*?' $(MAKEFILE_LIST) \
		| sed -E "s/^Makefile:([a-zA-Z0-9_-]+):(.* ## (.*))?/\1	\3/" \
		| column -s $$'\t' -t \
		| sort -f
	@echo "Current configuration"
	@echo "INSTANCE_NAME=${INSTANCE_NAME}, MACHINE_TYPE=${MACHINE_TYPE}"
	@echo "Set the GC_PROJECT first"


create:
	-gcloud compute instances create $(INSTANCE_NAME) \
			--project $(GC_PROJECT) --zone $(GC_ZONE) --machine-type $(MACHINE_TYPE) \
			--boot-disk-size 500 --image-project ubuntu-os-cloud --image-family ubuntu-1604-lts \
			--maintenance-policy TERMINATE --restart-on-failure \
			--accelerator type=nvidia-tesla-k80

	@echo "Waiting for the machine to start"
	sleep 30

	gcloud compute scp \
		--project $(GC_PROJECT) --zone $(GC_ZONE) \
		gcp_setup_script $(INSTANCE_NAME):~

	gcloud compute ssh $(INSTANCE_NAME) \
    --project $(GC_PROJECT) --zone $(GC_ZONE) \
		--command "source ~/gcp_setup_script"

	@echo "Rebooting the machine"
	gcloud compute instances reset $(INSTANCE_NAME) \
    --project $(GC_PROJECT) --zone $(GC_ZONE)

connect:
	# echo Connect to the notebook at $(cat cronrun.log | grep token=  | tail -n 1 | sed -e 's/8888/8082/g')
	gcloud compute ssh $(INSTANCE_NAME) \
    --project $(GC_PROJECT) --zone $(GC_ZONE) \
    -- -L 8082:localhost:8888

ssh:
	gcloud compute --project $(GC_PROJECT)  ssh --zone $(GC_ZONE) $(INSTANCE_NAME)

delete:
	gcloud compute instances delete --project $(GC_PROJECT) --zone $(GC_ZONE) $(INSTANCE_NAME)

stop:
	gcloud compute instances stop --project $(GC_PROJECT) --zone $(GC_ZONE) $(INSTANCE_NAME)

start:
	-gcloud compute instances start  --project $(GC_PROJECT) --zone $(GC_ZONE) $(INSTANCE_NAME)
	gcloud compute ssh $(INSTANCE_NAME) \
		--project $(GC_PROJECT) --zone $(GC_ZONE) \
		--command=jupyter-lab

update_machine_type:
	gcloud compute instances set-machine-type $(INSTANCE_NAME) \
			--project $(GC_PROJECT) --zone $(GC_ZONE) --machine-type $(MACHINE_TYPE)
