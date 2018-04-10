# fast.ai resources
http://course.fast.ai/

## Setup a machine on GCP
```bash
# First set the project in which you want to work
export GC_PROJECT = <my-project-id>
# Set the Zone in which the machine will be created
# See https://cloud.google.com/compute/docs/gpus/
export GC_ZONE = us-west1-b
# Create and setup the machine
make create
```

To connect to the machine:
```bash
make connect
# This will output the url to open the notebook:
# $ Connect to the notebook at http://localhost:8082/?token=2f6ae69ed792917362b939beda8219e356e579de83c274e7
```

Remember to stop the machine when not in use:
```bash
make stop
```

And to resume your work:
```bash
make start && sleep 45 && make connect
```

You can the open the notebooks at http://localhost:8082
