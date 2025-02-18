spark.conf.set("fs.s3a.impl", "org.apache.hadoop.fs.s3a.S3AFileSystem")
spark.conf.set("spark.hadoop.fs.defaultFS", "file:///")

csv_path = r"file://server/shared/file.csv"
df = spark.read.csv(csv_path, header=True, inferSchema=True)