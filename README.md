# BigStitcher-Spark
Running compute-intense parts of BigStitcher distributed. For now we support fusion using affine transformation models (including translations of course). It should scale very well to large datasets as it tests for each block that is written which images are overlapping. You simply need to specify the `XML` of a BigSticher project and decide which channels, timepoints, etc. to fuse.

Sharing this early as it might be useful ...

Here is my example config:

```
-x '/Users/spreibi/Documents/Microscopy/Stitching/Truman/standard/dataset.xml'
-o '/Users/spreibi/Documents/Microscopy/Stitching/Truman/standard/test-spark.n5'
-d '/ch488/s0'
--UINT8
--minIntensity 10
--maxIntensity 512
--channelId 0
```
*Note: here I save it as UINT8 [0..255] and scale all intensities between `10` and `512` to that range. If you omit `UINT8`, it'll save as `FLOAT32` and no `minIntensity` and `maxIntensity` are required. `UINT16` [0..65535] is also supported.*


And for local spark you need JVM paramters (8 cores, 50GB RAM):

```
-Dspark.master=local[8] -Xmx50G
```
Ask your sysadmin for help how to run it on your cluster. `mvn clean package` builds `target/BigStitcher-Spark-jar-with-dependencies.jar` for distribution.


You can open the N5 in Fiji (`File > Import > N5`) or by using `n5-view` from the n5-utils package (https://github.com/saalfeldlab/n5-utils).

You can create a multiresolution pyramid of this data using https://github.com/saalfeldlab/n5-spark
