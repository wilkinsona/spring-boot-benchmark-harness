# Spring Boot Benchmark Harness

Benchmark harness for measuring the startup performance of Spring Boot applications.

## Methodology

The harness measures that startup performance of a Spring Boot application by running
it as a separate process as a user typically would. The startup time is measured by
capturing the output of the application and extracting the JVM running time that's logged
during startup. Two variants of the same application are started alternately multiple
times. The first variant provides a baseline and the second some results that can be
compared to the baseline. The harness briefly sleeps between each execution to reduce the
possibility of the results being skewed by CPU throttling.

Startup performance can be measured with the application being run as a fat jar using
`java -jar`, or by calling its main method directly. In the latter case the harness will
extract the jar and run the app using the class specified in its manifest's `Start-Class`
attribute and a classpath derived from `BOOT-INF/lib` and `BOOT-INF/classes`.

## Usage

The harness is written in Ruby and is executable.

```
$ ./benchmark <baseline.jar> <new.jar> <mode>
```

The mode may be `main_method`, `fat_jar`, or it may be omitted to benchmark both modes.

## Results

Results are output as tables formatted as GitHub-flavored Markdown, as shown in the
following example:

```
|       | Baseline |  New  |
| ----- | -------: | ----: |
|       |    2.485 | 1.971 |
|       |    2.013 | 2.052 |
|       |    2.343 | 2.005 |
|       |    2.512 | 2.170 |
|       |    2.395 | 2.059 |
|       |    2.366 | 2.128 |
|       |    2.180 | 2.203 |
|       |    2.168 | 2.396 |
|       |    2.152 | 1.963 |
|       |    2.242 | 1.977 |
|  Mean |    2.286 | 2.092 |
| Range |    0.499 | 0.433 |
```

The range is the difference between the minimum and maximum startup time.