 docker run -v $(pwd):/data s22s/geo-swak gdalwarp -s_srs '+proj=sinu +R=6371007.181 +nadgrids=@null +wktext' -r cubic -t_srs '+proj=longlat +datum=WGS84 +no_defs' -of GTIFF 'HDF4_EOS:EOS_GRID:"MCD43A4.A2018220.h19v02.006.2018233203810.hdf":MOD_Grid_BRDF:Nadir_Reflectance_Band2'  utm.tif

gdal_merge.py -o rgb_1.tif -separate \
/vsis3/bucket1/MCD43A4.006/25/02/2019112/MCD43A4.A2019112.h25v02.006.2019121035050_B01.TIF \
/vsis3/bucket1/MCD43A4.006/25/02/2019112/MCD43A4.A2019112.h25v02.006.2019121035050_B04.TIF \
/vsis3/bucket1/MCD43A4.006/25/02/2019112/MCD43A4.A2019112.h25v02.006.2019121035050_B03.TIF \
 -co PHOTOMETRIC=RGB -co COMPRESS=DEFLATE 

gdal_translate rgb_1.tif rgb.tif -b 1 -b 2 -b 3 -co COMPRESS=DEFLATE -co PHOTOMETRIC=RGB


gdal_translate --config GDAL_CACHEMAX 512 -co NUM_THREADS=4 -co TILED=YES -co COPY_SRC_OVERVIEWS=YES -co COMPRESS=DEFLATE m_3008501_ne_16_1_20171018.tif cog.tif
gdaladdo -r lanczos --config COMPRESS_OVERVIEW JPEG --config PHOTOMETRIC_OVERVIEW YCBCR --config JPEG_QUALITY_OVERVIEW 85 src4b.tif 2 4 8 16 32 
gdal_translate --config GDAL_CACHEMAX 512 -co NUM_THREADS=4 -co TILED=YES -co COPY_SRC_OVERVIEWS=YES -co COMPRESS=DEFLATE  src4b.tif src4b-cog.tif 
gdal_translate -b 4  --config GDAL_CACHEMAX 512 -co NUM_THREADS=4 -co TILED=YES -co COPY_SRC_OVERVIEWS=YES -co COMPRESS=DEFLATE m_3607526_sw_18_1_20160708.mrf b4.tif 

gdaltindex band-extent.shp src4b-cog.tif 
ogr2ogr -segmentize 1000 -t_srs EPSG:4326 -lco RFC7946=YES -lco WRITE_BBOX=YES band-epsg4326.geojson band-extent.shp 


time gdal_translate -r cubic -outsize 1200 0 --config AWS_REQUEST_PAYER requester --config GDAL_CACHEMAX 512 /vsis3/sentinel-s2-l2a/tiles/41/D/NC/2019/4/1/0/R60m/TCI.jp2 /vsis3/s22s-phil/out.jpg 


time gdalbuildvrt -o /vsistdout/ -separate /vsis3/sentinel-s2-l1c/tiles/5/V/PL/2019/1/30/0/B04.jp2 /vsis3/sentinel-s2-l1c/tiles/5/V/PL/2019/1/30/0/B03.jp2 /vsis3/sentinel-s2-l1c/tiles/5/V/PL/2019/1/30/0/B02.jp2 --config AWS_REQUEST_PAYER requester | gdal_translate /vsistdin/ /vsistdout/ -ot Byte -scale 0 6000 0 255 -exponent 0.4 --config AWS_REQUEST_PAYER requester | gdal_translate -r cubic -outsize 1200 0 /vsistdin/ x.jpg

sudo add-apt-repository ppa:ubuntugis/ppa && sudo apt-get update

sudo apt-get install gdal-bin


time gdal_translate -r cubic -outsize 1200 0 \
--config AWS_REQUEST_PAYER requester \
--config GDAL_CACHEMAX 512 \
--config GDAL_PAM_ENABLED NO \
--config GDAL_DISABLE_READDIR_ON_OPEN YES \
--config CPL_VSIL_CURL_ALLOWED_EXTENSIONS jp2,tif,jpg \
--config CPL_VSIL_CURL_CHUNK_SIZE 1000000 /vsis3/sentinel-s2-l2a/tiles/31/R/FK/2018/12/28/0/R60m/TCI.jp2  x.tif

gdal_translate -ot Byte -a_nodata 255 -a_scale 1 -scale 0 6000 0 254 -exponent 0.4 --config AWS_REQUEST_PAYER requester --config GDAL_CACHEMAX 512 --config GDAL_PAM_ENABLED NO --config GDAL_DISABLE_READDIR_ON_OPEN YES --config CPL_VSIL_CURL_ALLOWED_EXTENSIONS jp2,tif,jpg,mrf,lrc,idx --config CPL_VSIL_CURL_CHUNK_SIZE 1000000 /vsis3/naip-visualization/al/2013/100cm/rgb/30085/m_3008506_ne_16_1_20131010.tif out.jpg