# Elasticsearch

## Resources

* [Awesome Elasticsearch](https://github.com/dzharii/awesome-elasticsearch)

Todo:
* indexes and aliases, and query
* re-indexing within ES
* Dump db

## Query

Elasticsearch query is different than SQL in that the original purpose was to do free-text query over text documents
to give a total ordering of them based on score, rather than simply use predicates to restrict them.  If you're
also sorting, the score never matters.

### Must vs. Should

* `must` - term is in doc and contributes to score (logical AND)
* `filter` - term is in doc but does not contribute to score (logical AND)
* `should` - term only contributes to score, but may or may not be in doc (logical OR when minimum_should_match=1)
* `must_not` - term is NOT in doc (logical NOT)

```json5
{
  "query": {
    "bool" : {
      "must" : {
        "term" : { "user.id" : "kimchy" }
      },
      "filter": {
        "term" : { "tags" : "production" }
      },
      "must_not" : {
        "range" : {
          "age" : { "gte" : 10, "lte" : 20 }
        }
      },
      "should" : [
        { "term" : { "tags" : "env1" } },
        { "term" : { "tags" : "deployed" } }
      ],
      "minimum_should_match" : 1 // at least one of the should terms must match
    }
  }
}
```

### Query if field exists

```
GET /_search
{
  "query": {
    "exists": {
      "field": "some_field_that_may_or_may_not_exist"
    }
  }
}
```

## mappings 

* add `keyword` to any values that should not be tokenized
* add `geo_shape` to geojson shapes

## Backup and Restore across Regions

Create a backup in east:
```
PUT https://abc.us-east-1.aws.found.io:9243/_snapshot/s3_repo
{
  "type": "s3",
  "settings": {
    "bucket": "abc-es-backup-us-east-1",
    "region": "us-east-1",
    "access_key": "KEY",
    "secret_key": "SECRET"
  }
}
```

Create a backup in west:
```
PUT https://abc.us-west-2.aws.found.io:9243/_snapshot/s3_repo
{
  "type": "s3",
  "settings": {
    "bucket": "abc-es-backup-us-west-2",
    "region": "us-west-2",
    "access_key": "KEY",
    "secret_key": "SECRET"
  }
}
```

Do the backup:
```
PUT https://abc.us-east-1.aws.found.io:9243/_snapshot/s3_repo/snapshot_1
{
  "indices":"my_index",
  "ignore_unavailable": true,
  "include_global_state": false,
  "metadata": {
    "taken_by": "pvarner",
    "taken_because": "backup before migration"
  }
}
```

Copy to other region, and then restore.


Index Alias - automatically converted in all apis 
https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-aliases.html

Reindex - reindex all documents between indicies 
https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-reindex.html


Entire Cluster - Snapshot and Restore 

https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-snapshots.html



POST _search
{
  "query": {
    "term" : { "user" : "Kimchy" } 
  }
}

{
  "size": 0,
  "aggs": {
    "collections": {
      "terms": { "field": "collection" } 
    }
  }
}


POST my_docs/_delete_by_query
{
  "query": {
    "term": {
      "collection": "foobar"
    }
  }
}


_update_by_query

Update

properties.foobar requires two array-style accesses to distinguish from a field named "properties.foobar"
{
  "script": {
    "source": "ctx._source['properties']['foobar'] = 'bar'",
    "lang": "painless"
  },
   "query": {
    "prefix": {
      "id": {
        "value": "LO08"
      }
    }
  }
}

Remove field
  "script": {
    "source": "ctx._source.remove(\"foobar\"),
    "lang": "painless"
  },

Backup

/_snapshot/s3_repo/snapshot_myindex?wait_for_completion=false
{
  "indices": "myindex",
  "include_global_state": false
}



https://www.elastic.co/blog/improving-the-performance-of-high-cardinality-terms-aggregations-in-elasticsearch

https://www.elastic.co/blog/advanced-tuning-finding-and-fixing-slow-elasticsearch-queries



step_september152014_70rndsel_igbpcl.geojson

cb_2017_us_state_500k.geojson

tnc_terr_ecoregions.geojson

tl_2018_us_aiannh.geojson



Elasticsearch


https://github.com/radiantearth/stac-spec/blob/master/item-spec/common-metadata.md#instrument

/_search
/_all/{type}/_search
/index1,index2/{type}/_search

/_count
/_aggregate
/_all/{type}/{id}

POST /{index}/_doc


parameters
* size - number of documents returned 0 to 10000
* fields
* sort

ignore_malformed: false

{
  "query": {
    "filtered": {
      "filter": {
        "term": {
          "name": "elasticsearch"
        }
      }
    }
  }
}

{
  "aggregations" : {
    "organizers" : {
      "terms" : { "field" : "organizer" }
    }
  }
}
Geoshape query https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-geo-shape-query.html

Elastic's Guide to Mapping and Geospatial Search
https://www.youtube.com/watch?v=ugvvAy-QNqI

Geospatial Advancements in Elasticsearch by Knize
https://www.slideshare.net/elasticsearch/geospatial-advancements-in-elasticsearch


partial document - POST .../_update "doc"
POST .../_update "doc" and "upsert"

update with script

to replace, use "index" instead of "create"
?version=3 -- set with the version you expect it to have

geo_bounding_box (only points) vs. geo_shape




￼

https://www.elastic.co/guide/en/elasticsearch/reference/current/paginate-search-results.html

query (scoring order) vs. filter (must match)


MUST MUST MUST

query / filtered / filter / bool / must

term
range

exists
missing