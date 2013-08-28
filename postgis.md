### install requirements

* postgresql(>=9.0)
* gcc
* make
* proj4
* geos
* libxml2
* json-c(not installed is also ok!)
* gdal

***

### install setups

1. proj4
	* ```wget http://download.osgeo.org/proj/proj-4.8.0.tar.gz```
	* ```./configure --prefix=/usr/local/pgsql/tools/proj```
2. geos
	* ```wget http://download.osgeo.org/geos/geos-3.3.8.tar.bz2```
	* ```./configure --prefix=/usr/local/pgsql/tools/geos```
3. libxml2
	* ```wget ftp://xmlsoft.org/libxml2/libxml2-git-snapshot.tar.gz```
	* ```./configure --prefix=/usr/local/pgsql/tools/libxml2```
4. json-c
5. gdal
	* ```wget http://download.osgeo.org/gdal/1.10.0/gdal-1.10.0.tar.gz```
	* ```./configure --prefix=/usr/local/pgsql/tools/gdal```
6. postgis
	* ```wget http://postgis.net/stuff/postgis-2.1.1dev.tar.gz```
	*  ```./configure --prefix=/usr/local/pgsql/tools/postgis --with-pgconfig=/usr/local/pgsql/bin/pg_config  --with-geosconfig=/usr/local/pgsql/tools/geos/bin/geos-config --with-gdalconfig=/usr/local/pgsql/tools/gdal/bin/gdal-config --with-xml2config=/usr/local/pgsql/tools/libxml2/bin/xml2-config --with-projdir=/usr/local/pgsql/tools/proj/```
	*  **检查是否安装成功** ```select name, default_version, installed_version from pg_available_extensions where name like 'postgis%';```
7. ```createdb postgis```
8. ```create extension postgis;```
	
```ERROR:  could not load library "/usr/local/pgsql/lib/postgis-2.1.so": libgeos_c.so.1: cannot open shared object file: No such file or directory ```

```root@ubuntu1:/usr/local/pgsql/lib# cp ../tools/geos/lib/libgeos_c.so.1 .```

```ERROR:  could not load library "/usr/local/pgsql/lib/postgis-2.1.so": libproj.so.0: cannot open shared object file: No such file or directory```

```root@ubuntu1:/usr/local/pgsql/lib# cp ../tools/proj/lib/libproj.so.0 .```

```ERROR:  could not load library "/usr/local/pgsql/lib/rtpostgis-2.1.so": libgdal.so.1: cannot open shared object file: No such file or directory```

```root@ubuntu1:/usr/local/pgsql/lib# cp ../tools/gdal/lib/libgdal.so.1 .```

***

### test
```
CREATE TABLE testtable (
    _id bigint NOT NULL,
    loc point DEFAULT '(0,0)'::point,
    "time" bigint DEFAULT 0,
    songid bigint DEFAULT 0,
    lastheartime bigint DEFAULT 0,
    valid smallint DEFAULT 1
);
```

```postgis=# insert into testtable values (22002, '(120.18670654296875,30.1901302337646484)', 1356773194145, 186624, 1356763605520, 1);```

```
select _id,loc,songid,lastheartime,loc <-> '(120, 30)' distance from testtable ;
```

***

see also: [manual-2.1](http://postgis.net/docs/manual-2.1/postgis_installation.html)