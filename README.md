# PostGis basico By @geojuanka

## Crear campos nuevos: 

alter table esquema.tabla add nombrecampo tipodecampo 
```
Ejemplo: alter table esquema01.municipios add perimetro numeric (20,2);
```
## Borrar un campo: 

alter table esquema.tabla drop column nombrecampo 
```
Ejemplo: alter table esquema01.municipios_line drop column perimetro;
```
## Actualizar valores de un campo: 

update esquema.tabla set nombrecampo = valores 
```
Ejemplo: update esquema01.municipios_pto SET tipo = 'residencial';
```

## Actualizar valores de un campo con condiciones: 
update esquema.tabla set nombrecampo = valores where nombrecampo = valores 
```
Ejemplo: update esquema01.municipios_pto SET tipo = 'capital' where municipio = 'LAS PALMAS DE GRAN CANARIA';
```

## Calcular el área (ST_AREA)

update esquema.tabla set nombrecampo = st_area (campodelageometría)
```
Ejemplo: update esquema01.municipios SET area = st_area(geom);
```
## Calcular el perímetro (ST_PERIMETER) 
update esquema.tabla set nombrecampo = st_perimeter (campodelageometría)
```
Ejemplo: update esquema01.municipios SET perimetro = st_perimeter(geom);
```
## Calcular la longitud (ST_LENGTH) 
update esquema.tabla set nombrecampo = st_length (campodelageometría) 
```
Ejemplo: update esquema01.municipios_line SET longitud = st_length(geom);
```
## Calcular la coordenada X (ST_X) 
update esquema.tabla set nombrecampo = st_x (campodelageometría) 
```
Ejemplo: update esquema01.municipios_pto SET coord_x = st_x(geom);
```

## Convertir una capa de polígonos a líneas (ST_BOUNDARY) 
create table esquema.nuevacapadelineas as select st_boundary (campodelageometría), campo1, campo2, campo3 from esquema.capadepoligonos
```
Ejemplo: create table esquema01.municipios_line as select ST_boundary(geom), codmun, municipio, isla from esquema01.municipios;
```

## Convertir una capa de polígonos a puntos (ST_CENTROID) 
create table esquema.nuevacapadepuntos as select st_centroid (campodelageometría), campo1, campo2, campo3 from esquema.capadepoligonos
```
Ejemplo: create table esquema01.municipios_pto as select ST_centroid(geom), codmun, municipio, isla from esquema01.municipios;
```
## Convertir una capa de polígonos a puntos (centroides inside) (ST_POINTONSURFACE) 
create table esquema.nuevacapadepuntos as select ST_PointOnSurface (campodelageometría), campo1, campo2, campo3 from esquema.capadepoligonos
```
Ejemplo: create table esquema01.municipios_pto_inside as select ST_PointOnSurface (geom), codmun, municipio, isla from esquema01.municipios;
```
# Añadir antiguedades del omisor a lass construcciones

Creamos un CSV desde el omisor con lo campos municipio, referencia, anyo
Añadimos columna anyo en la tabla del municipio en PostGis
Actualizamos campo
```
copy antiguedad (municipio, referencia, anyo) from 'C:\a\CAT\municipio.csv' delimiter ';' csv header;

update municipio set anyo= antiguedad.anyo
from antiguedad
where antiguedad.referencia=cox.refcat
``` 
