# Geometric Breaks: Code to produce fractured models

Note that you need to have the following apt dependencies installed. 
```bash
sudo apt install python3.8-distutils python3.8-dev libgl1 libglew-dev freeglut3-dev
```

Clone the repo.
```bash
https://github.com/Terascale-All-sensing-Research-Studio/DeepJoin.git
cd DeepJoin
```

We recommend using virtualenv. The following snippet will create a new virtual environment, activate it, and install dependencies.
```bash
sudo apt-get install virtualenv && \
virtualenv -p python3.8 env && \
source env/bin/activate && \
pip install -r requirements.txt && \
./install.sh && \
./install_pymesh.sh && \
source setup.sh
```
Issues with compiling pyrender are typically solved by upgrading cython: `pip install --upgrade cython`.


To generate fractured shapes from scratch, download shapenet from https://shapenet.org/. Extract the data into a directory, e.g. "/path/to/above/ShapeNet".

Almost all scripts require that the following environment variable be set to the directory above ShapeNet.
```
export DATADIR="/path/to/above/ShapeNet"
```

## Data preparation
Fracturing steps:

1) Waterproof meshes.
2) Normalize meshes. 
3) Fracture meshes. 

The `scripts/fracture.sh` takes an integer corresponding to one of the above processes.

NOTE: waterproofing cannot be done on a PC that does not have a display.

This readme will demo the data processing pipeline for the mugs dataset from shapenet. We assume you've downloaded shapenet to:
```
$DATADIR/ShapeNetCore.v2
```

To perform a single step of data processing use the `scripts/fracture.sh` script. The script takes 4 arguments:
- Path to the directory containing shapenet data
- Path to a .json train/test split file (will be created if does not exist)
- Operation (int from 1 to 3, corresponding to the above list)
- Number of breaks

Example for waterproofing:
```
./scripts/fracture.sh \
    $DATADIR/ShapeNetCore.v2/03797390 \
    $DATADIR/ShapeNetCore.v2/mugs_split.json \
    1 1
```

Example for normalizing:
```
./scripts/fracture.sh \
    $DATADIR/ShapeNetCore.v2/03797390 \
    $DATADIR/ShapeNetCore.v2/mugs_split.json \
    2 1
```

Example for fracturing:
```
./scripts/fracture.sh \
    $DATADIR/ShapeNetCore.v2/03797390 \
    $DATADIR/ShapeNetCore.v2/mugs_split.json \
    3 1
```

We provide a wrapper, `scripts/run.sh`, to execute all preprocessing steps in order (except waterproofing). The following will perform all preprocessing steps in order, fracturing each object 1 time.
```
./scripts/run.sh \
    $DATADIR/ShapeNetCore.v2/03797390 \
    $DATADIR/ShapeNetCore.v2/mugs_split.json \
    1
```

