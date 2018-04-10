# fast.ai resources
http://course.fast.ai/

## Setup a machine on GCP
From the `fast.ai` directory:
```bash
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
