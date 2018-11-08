# fast.ai Utils and Tools
Content

| Directory | Description |
|-----|-----|
| `google_compute_engine` | Scripts to run fast.ai notebooks on Google Compute Engine |

## ML 1
We will store the data in the home folder:
```
mkdir -p ~/datasets/kaggle
```

Install the dependencies:
```
pip install -q --upgrade keras tensorflow kaggle-cli numpy
```

### Lesson 1
Getting the data
```
cd ~/datasets/kaggle
wget  http://files.fast.ai/data/dogscats.zip 
```
