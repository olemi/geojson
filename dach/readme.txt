1. Generate geojson admin-1

ogr2ogr -f GeoJSON admin_1.json ne_10m_admin_1_states_provinces_shp/ne_10m_admin_1_states_provinces_shp.shp -where "sr_adm0_a3 IN ('DEU','AUT','CHE')"

2. Generate geojson places

export SHAPE_ENCODING="ISO-8859-1"

ogr2ogr -f GeoJSON places.json ne_10m_populated_places/ne_10m_populated_places.shp -where "(MEGACITY=1 OR FEATURECLA='Admin-1 capital') AND (adm0_a3 IN ('DEU','CHE','AUT'))" -lco ENCODING=UTF-8


3. Pack them as topojson:

Without simplification:

topojson --id-property su_a3 -p name=NAME -p adm0=ADM0_A3 -p scalerank=SCALERANK -p name -p adm0=sr_adm0_a3 -p namepar=NAMEPAR -o dach.topojson places.json admin_1.json 

With simplification:

topojson --simplify-proportion .2 --id-property su_a3 -p name=NAME -p adm0=ADM0_A3 -p scalerank=SCALERANK -p name -p adm0=sr_adm0_a3 -p namepar=NAMEPAR -o dach_s02.topojson places.json admin_1.json

Note on properties:
-p name=NAME : retains property "NAME" form places.json as "name" [object name, e.g. "Munich"]
-p adm0=ADM0_A3 : retains property "ADM0_A3" form places.json as "adm0" [object admin0 county code, e.g. "DEU", "AUT", "CHE"] 
-p scalerank=SCALERANK : retains property "SCALERANK" form places.json as "scalerank" [object size rank]
-p namepar=NAMEPAR : retains property "NAMEPAR" form places.json as "namepar" [object localized name, e.g. "München"]
-p name : retains property "name" form admin_1.json [object name, e.g. "Bayern"]
-p adm0=sr_adm0_a3 : retains property "sr_adm0_a3" form admin_1.json as "adm0" [object admin0 county code, e.g. "DEU","AUT","CHE"] 

Note on simplification:
--simplify-proportion <fraction> : prunes boundary points to ratian approx <fraction> of the riginal amount of points 
