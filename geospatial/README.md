# Geospatial

## Reference Information

### GeoJSON bbox order

These coordinates aren't just min/max lat/lon, because the bbox could span the antimeridian,
in which case the southwest longitude is positive and the northeast longitute is negative,
so you have to apply some heuristics to determine, e.g., we know our images span less than
180 degrees, so the bbox can't wrap nearly an entire latitute band.

See [The Antimeridian](https://datatracker.ietf.org/doc/html/rfc7946#section-5.2) and [The Poles](https://datatracker.ietf.org/doc/html/rfc7946#section-5.3).

#### 2D

```python
[sw_lon, sw_lat, ne_lon, ne_lat]
```

```json
[100.0, 0.0, 105.0, 1.0]
```

#### 3D

```python
[sw_lon, sw_lat, bottom, ne_lon, ne_lat, top]
```

```json
[100.0, 0.0, -100.0, 105.0, 1.0, 0.0]
```

### GeoJSON

Positions in a geometry are a 2- or 3- tuple. The order is `[lon, lat, elevation]` or
`[easting, northing, elevation]`.  If you see a second point outside -90 to 90, the points
are probably flipped!

### Decimal Places to Distance

| Decimal Places | Degrees      | Distance  |
|---------------|--------------|-----------|
| 0             | 1.0          | 111 km    |
| 1             | 0.1          | 11.1 km   |
| 2             | 0.01         | 1.11 km   |
| 3             | 0.001        | 111 m     |
| 4             | 0.0001       | 11.1 m    |
| 5             | 0.00001      | 1.11 m    |
| 6             | 0.000001     | 111 mm    |
| 7             | 0.0000001    | 11.1 mm   |
| 8             | 0.00000001   | 1.11 mm   |

## Libraries and Tools

- [antimeridian](https://github.com/gadomski/antimeridian) - tools for fixing the winding
  order of polygons, splitting across the antimeridian and poles into multipolygon

## Other Sources

### Projections

- <https://lyzidiamond.com/posts/4326-vs-3857>

### Geospatial Database Support

- [DuckDB Spatial](https://duckdb.org/docs/stable/extensions/spatial/overview.html)
- [Elasticsearch Geo Queries](https://www.elastic.co/guide/en/elasticsearch/reference/current/geo-queries.html) and [Geo-shape field type](https://www.elastic.co/guide/en/elasticsearch/reference/current/geo-shape.html)
- [PostGIS](https://postgis.net/) for PostgreSQL
- [MongoDB Geospatial Queries](https://docs.mongodb.com/manual/geospatial-queries/)
- [MySQL Spatial Analysis Functions](https://dev.mysql.com/doc/refman/8.0/en/spatial-analysis-functions.html)

### Organizations

- [The Open Source Geospatial Foundation](https://www.osgeo.org/)
