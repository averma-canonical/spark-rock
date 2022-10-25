# spark-rock
ROCK for Spark on Kubernetes. It provides a direct port of this Spark provided [Dockerfile](https://github.com/apache/spark/blob/master/resource-managers/kubernetes/docker/src/main/dockerfiles/spark/Dockerfile).

After cloning the repo, run the following commands.

- Build the Spark ROCK.
```bash
rockcraft pack --verbose
```

- Publish the built Spark ROCK to private registry running at ```localhost:32000```
```bash
skopeo --insecure-policy copy oci-archive:spark_0.1_amd64.rock docker-daemon:localhost:32000/sparkrock:latest
docker push localhost:32000/sparkrock:latest
```

- With the spark client snap installed, run the following command to launch a Spark job using the above created image for validation.
```bash
spark-client.spark-submit --deploy-mode cluster --conf spark.kubernetes.container.image=localhost:32000/sparkrock:latest --class org.apache.spark.examples.SparkPi local:///opt/spark/examples/jars/spark-examples_2.12-3.3.0.jar 100
```

- Play with a pyspark script placed in S3.
```bash
spark-client.spark-submit  --deploy-mode cluster --name pyspark-s3a --properties-file <path to spark-defaults.conf> --conf spark.kubernetes.container.image='localhost:32000/sparkrock:latest' <S3 location of pyspark script>
```
