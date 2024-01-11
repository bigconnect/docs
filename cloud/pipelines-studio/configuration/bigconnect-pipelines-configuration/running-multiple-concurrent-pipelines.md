# Running Multiple Concurrent Pipelines

By default, BigConnect Pipelines can run approximately 22 standalone pipelines concurrently. If you plan to run a larger number of pipelines at the same time, increase the thread pool size.

The `runner.thread.pool.size` property in the BigConnect Pipelines configuration file, `$SDC_CONF/sdc.properties`, determines the number of threads in the pool that are available to run standalone pipelines. One running pipeline requires five threads, and pipelines share threads in the pool.

1. Use a text editor to open `sdc.properties`.
2. Calculate the approximate runner thread pool size by multiplying the number of running pipelines by 2.2.
3. Set the `runner.thread.pool.size` property to your calculated value.
4. Restart BigConnect Pipelines to enable the changes.
