## Terrains data

We have obtained our data from publicly available sources:
- Pyrenees terrains correspond to mountain regions from the north of Catalonia. They were downloaded from [Institut Cartogràfic i Geològic de Catalunya](http://www.icc.cat/vissir3/index.html?lang=eng). They provide 2m DEM and 25cm aerial imagery, which we then resampled to 1m. 
- Tyrol terrains correspond to mountain regions from South Tyrol. They were downloaded from [Südtiroler Bürgernetz GeoKatalog] (http://geokatalog.buergernetz.bz.it/geokatalog). They provide 2.5m DEM and 20cm aerial imagery. We resampled the DEM to 2m and the aerial imagery to 1m.

We used QGIS to process the raw downloaded data. Edit operations consisted in merging downloaded tiles, cropping regions of interest and resampling. 

For convenience, we provide our downloaded and processed terrains [HERE](https://www.virvig.eu/fcn-terrains/terrains.zip) (1.20GB zip file, 3.51 GB uncompressed).
For each terrain region, four files are provided:

| file | description |
| ------ | --------- | 
| **terrain_2m.dem** | 2m resolution height field (float matrix stored as text) |
| **terrain_15m.dem** | 15m resolution height field (a float matrix stored as text). Note that the dimensions of this matrix are the same as the 2m.dem file, because this file has been obtained from the 2m.dem downsampling it by a factor of 7.5, and upsampling again by the same factor using bicubic upsampling. |
| **terrain_ortho1m.jpg** | aerial image RGB, 1m resolution |
| **terrain_dem2m.png** | not used, visualization of the 2m height field as 16-bit PNG |



## prepare_tiles.py

This script receives a terrain set, and crops 400x400m tiles from it. It can be executed as:
```
python prepare_tiles.py set_name out_dir [--augment] [--zeromean]
```
It will read *set_name_2m.dem*, *set_name_15m.dem* and *set_name_ortho1m.jpg*, split the terrain into tiles and save them in **out_dir/dem2m/set_name_X_Y.dem**, **out_dir/dem15m/set_name_X_Y.dem** and **out_dir/ortho/set_name_X_Y.ortho**. Note that the file name of a tile is the same inside each of the three folders. 

The two optional parameters are:

| parameter | description |
| --------- | ----------- |
| ```augment``` | data augmentation on each tile: 90, 180 and 270 degree rotations, vertical and horizontal flips, and transposed. |
| ```zeromean``` | substract the mean elevation of the DEM tiles. |

Example usage:
```
python prepareTiles.py pedraforca tiles --zeromean --augment
```

## split_tile_lists.py

This helper script can be used to generate training and validation splits. It has no parameters, but you might need to edit the variables inside. It contains the list of terrain sets and how many X and Y tiles it has been divided into, as well as the list of variations generated for data augmentation. Add or remove lines to those lists to modify the set of available tiles. This set will be then shuffled and the first TRAIN_SAMPLES entries will be saved to ```train.txt```and the next VALID_SAMPLES to ```valid.txt```. You may also need to edit the values of these two numeric variables for other list sizes.
