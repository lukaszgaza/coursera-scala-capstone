# Coursera Scala Capstone

Note that the sbt project requires Java 8. Be sure that you have this version of the JVM installed on your environment.

Our goal in this project is to give you as much freedom as possible in the solution space. Unfortunately, in order to be able to grade your work, we will ask you to implement some methods, for which we have fixed the type signature, and which may influence your solution. For instance, most of them are using Scala’s standard **Iterable** datatype, but not all concrete implementations of **Iterable** may scale to the high volume of data required by the project (the grader is not going to use a high volume of data, though). So, while you must not change the code that is provided with the project, your actual implementation should use appropriate and efficient data types in order to perform incremental and parallel computations. For this purpose, we have added several dependencies to the build: Spark, Akka Streams, Monix and fs2. You can use any of these libraries, if you want to, or just use the standard Scala library. However, note that the provided build just makes these libraries available in the classpath, but it does not configure their execution environment.

If you decide to use one of the above libraries, you might find it easier to implement the actual functionality in separate methods with different signatures. You can just consider the provided methods as "probes" into your code that the grader can use. For instance, if you want to use Spark RDDs and we provide a **locateTemperatures** method that requires an **Iterable**, you could implement it in the following way:
``` scala
// Provided method:
def locationYearlyAverageRecords(
  records: Iterable[(LocalDate, Location, Temperature)]
): Iterable[(Location, Temperature)] = {
  sparkAverageRecords(sc.parallelize(records)).collect().toSeq
}

// Added method:
def sparkAverageRecords(
  records: RDD[(LocalDate, Location, Temperature)]
): RDD[(Location, Temperature)] = {
  ??? // actual work done here
}
```
Ultimately, you'll have to write your own **Main** to generate the map, so you have the freedom to organize your code the way you want. The **Main** doesn't have to use the provided methods, you can simply chain your own methods together instead. We want you to think about program structure, so don't feel too boxed in by the provided methods.

Last, note that there is a **src/test/** directory with test files for each milestone.

We recommend that you write tests here to check your solutions, but do not remove the existing code.

### Before you start coding

Before you start coding, there are two files that you should know about. First, **package.scala** introduces type aliases that will be used throughout the project:
``` scala
type Temperature = Double
type Year = Int
```
We will introduce them again when appropriate. Second, **models.scala** has a few useful case classes:
``` scala
case class Location(lat: Double, lon: Double)
case class Tile(x: Int, y: Int, zoom: Int)
case class GridLocation(lat: Int, lon: Int)
case class CellPoint(x: Double, y: Double)

case class Color(red: Int, green: Int, blue: Int)
```
There is one case class for every coordinate system that we use, and an additional **Color** case class that may be useful for creating images. We will introduce each one of these again when relevant. **You are free to add methods to these case classes if you deem it appropriate, but you must not change their parameters**.

### Notes for Spark users

The output of the grader is limited to 64 kB, but Spark produces a lot of logs by default. Often, this makes it impossible to read the whole grader feedback. Consequently, we suggest you to tune the log level of Spark like so:
``` scala
import org.apache.log4j.{Level, Logger}
Logger.getLogger("org.apache.spark").setLevel(Level.WARN)
```
You may also want to refer to the Dataset vs DataFrame vs RDD discussion at the end of the last video of the Spark course so that you can choose the best API for this project.
