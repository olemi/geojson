1. Generate geojson admin-1

ogr2ogr -f GeoJSON admin_1.json ne_10m_admin_1_states_provinces_shp/ne_10m_admin_1_states_provinces_shp.shp -where "sr_adm0_a3 IN ('DEU','AUT','CHE')"

2. Generate geojson places

export SHAPE_ENCODING="ISO-8859-1"

ogr2ogr -f GeoJSON places.json ne_10m_populated_places/ne_10m_populated_places.shp -where "(MEGACITY=1 OR FEATURECLA='Admin-1 capital') AND (adm0_a3 IN ('DEU','CHE','AUT'))" -lco ENCODING=UTF-8


3. Pack them as topojson:

topojson --id-property su_a3 -p name=NAME -p adm0=ADM0_A3 -p name -p adm0=sr_adm0_a3 -p namepar=NAMEPAR -o dach.topojson places.json admin_1.json 

Note: retains these properties
:
name : the name of the object (e.g. Munich)
namepar: localized name (e.g. München)
adm0: the country code ('AUT', 'DEU', or 'CHE')
