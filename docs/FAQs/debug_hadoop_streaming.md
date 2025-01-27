# Debug Hadoop Streaming Jobs

When you work with Hadoop MapReduce streaming jobs, it may be a good idea to debug your mapper and reducer codes using Linux pipes such as below. 

```shell
hadoop fs -cat input_data | head -n 100 | python mapper.py | sort | python reducer.py
```

This is how Hadoop Streaming will use these programs. This will let you quickly see bugs in your python program without the overhead of invoking MapReduce. 

Hope this helps!