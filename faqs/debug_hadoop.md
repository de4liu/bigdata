# How to Debug Hadoop MapReduce Jobs

When you work with Hadoop MapReduce jobs for this course, here are some of the tips for debugging your Hadoop MapReduce issues:

1. Do your input files exist, and in the format expected?
    - You can use `hadoop fs -cat file | head` command to find this out.
2. Does your output folder already exist?
    - Hadoop won't run if  the output folder already exists.
    - Use `hadoop fs -rm -r -f output_dir` to remove the output folder.
3. Does your python mapper/reducer have syntax or run-time errors?
    - You can [debug your mapper and reducer programs using Linux pipes](debug_hadoop_streaming.md). 
4. If you pass step 3, does your mapper/reducer file has wrong line endings?
    - Follow tips here to [view and convert line endings](line_endings.md).

