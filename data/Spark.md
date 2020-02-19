# Recipies

## Rename dataframe columns

### Load with a schema

```scala
val schema = StructType(
  StructField("country_code", StringType) ::
  StructField("postal_code", StringType) ::
  StructField("place_name", StringType) ::
  StructField("admin_name1", StringType) ::
  StructField("admin_code1", StringType) ::
  StructField("admin_name2", StringType) ::
  StructField("admin_code2", StringType) ::
  StructField("admin_name3", StringType) ::
  StructField("admin_code3", StringType) ::
  StructField("latitude", StringType) ::
  StructField("longitude", StringType) ::
  StructField("accuracy", StringType) :: Nil)

val postal_codes_no_header_1 = spark.read
  .option("header", "false")
  .option("delimiter", "\t")
  .schema(schema)
  .csv("sample-noheader.txt")
```

### Produce a new dataframe with renamed columns

```scala
val newNames = Seq(
    "country_code",
    "postal_code",
    "place_name",
    "admin_name1",
    "admin_code1",
    "admin_name2",
    "admin_code2",
    "admin_name3",
    "admin_code3",
    "latitude",
    "longitude",
    "accuracy")

val postal_codes_no_header_2 = spark.read
  .option("header", "false")
  .option("delimiter", "\t")
  .csv("sample-noheader.txt")
  .toDF(newNames: _*)
```

### Convert epoch time to timestamp

```scala
df.select(from_unixtime('epoch_column).as("timestamp")).show()
```
