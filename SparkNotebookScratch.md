# Spark Notebooks (Jupyter and Scala) from Scratch

## Install Jupyter notebook

After ensuring that Python is installed:

    $ pip install jupyterlab
    $ pip install notebook
    $ jupyter notebook

### Kernel management

- List kernels: `jupyter kernelspec list`
- Remove kernel: `jupyter kernelspec uninstall unwanted-kernel`

## Install the Scala kernel

### Get `coursier`

    $ curl -Lo coursier https://git.io/coursier-cli
    $ chmod +x coursier

After ensuring that Java is installed (use Java 8):

### Install `almond` kernel (Spark does not yet support Scala 2.13)

    $ coursier launch almond:0.10.3 --scala 2.12.12 -- \
      --install \
      --id scala213 \
      --display-name "Scala (2.13)"


## Create a notebook

Start juptyter notebook and create a new notebook using Scala 2.12 kernel. Insert the following in the first cell and execute:

```scala
import $ivy.`org.apache.spark::spark-sql:3.0.0`

import org.apache.log4j.{Level, Logger}
Logger.getLogger("org").setLevel(Level.OFF)

import org.apache.spark.sql._

val spark = {
  NotebookSparkSession.builder()
    .master("local[*]")
    .getOrCreate()
}

```

Following instructions from [Almond for Spark](https://almond.sh/docs/usage-spark).