# Elasticsearch

## docker
https://hub.docker.com/_/elasticsearch/

## On mappings

To ensure that you do not run into conflicts, it is advisable to ensure that fields with the same name are mapped in the same way in every type in an index.

Although you can add to an existing mapping, you can’t change existing field mappings. If a mapping already exists for a field, data from that field has probably been indexed. If you were to change the field mapping, the indexed data would be wrong and would not be properly searchable.

### View mapping
	GET /gb/_mapping/tweet
	
###	Custom mapping
* Distinguish between full-text string fields and exact value string fields
* Use language-specific analyzers
* Optimize a field for partial matching
* Specify custom date formats

### Syntax

"type": "string|date|double|long|boolean"
"index": "analyzed|not_analyzed|no"
"analyzer": "standard|whitespace|simple|english"

### Inner objects vs Nested objects
Inner objects is a simplistic mapping of objects within existing objects. The downside is that the correlation between the objects is lost. To tackle this, nested objects should be used.

### Multifields
It is often useful to index the same field in different ways for different purposes. For instance, a string field could be indexed as an analyzed field for full-text search, and as a not_analyzed field for sorting or aggregations. Alternatively, you could index a string field with the standard analyzer, the english analyzer, and the french analyzer. This is the purpose of multi-fields. Most datatypes support multi-fields via the fields parameter.

## On _source

* The full document is available directly from the search results need for a separate round-trip to fetch the document from another data store. 
* Partial update requests will not function without the _source field. 
* When your mapping changes and you need to reindex your data, you can do so directly from Elasticsearch instead of having to retrieve all of your documents from another (usually slower) data store. 
*  Individual fields can be extracted from the _source field and returned in get or search requests when you don’t need to see the whole document. 
* It is easier to debug queries, because you can see exactly what each document contains, rather than having to guess their contents from a list of IDs. 

In Elasticsearch, setting individual document fields to be stored is usually a false optimization. The whole document is already stored as the _source field. It is almost always better to just extract the fields that you need by using the _source parameter.

## On _all
_all field is a special field that indexes the values from all other fields as one big string. It is useful during the exploratory phase of a new application, while you are still unsure about the final structure that your documents will have.

As your application evolves and your search requirements become more exacting, you will find yourself using the _all field less and less. The _all field is a shotgun approach to search. By querying individual fields, you have more flexbility, power, and fine-grained control over which results are considered to be most relevant.

Inclusion in the _all field can be controlled on a field-by-field basis by using the include_in_all setting, which defaults to true. Setting include_in_all on an object (or on the root object) changes the default for all fields within that object. You may find that you want to keep the _all field around to use as a catchall full-text field just for specific fields, such as title, overview, summary, and tags. Instead of disabling the _all field completely, disable include_in_all for all fields by default, and enable it only on the fields you choose.

Remember that the _all field is just an analyzed string field. It uses the default analyzer to analyze its values, regardless of which analyzer has been set on the fields where the values originate. And like any string field, you can configure which analyzer the _all field should use:

## Reindex
To reindex all of the documents from the old index efficiently, use scroll to retrieve batches of documents from the old index, and the bulk API to push them into the new index.