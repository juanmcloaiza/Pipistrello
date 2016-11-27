.. _readme:

Using Pipistrello
=================


Launching a Pipistrello job is pretty simple, provided that a hadoop cluster is already properly 
mounted and files stored in HDFS using only **one block per file**:

::

	$ $HADOOP_HOME/bin/hadoop jar Pipistrello.jar \
	  -${generic_hadoop_mapreduce_options} \
	  -files *file1,file2,file3*... \
	  -input *input1,input2,input3*... \
	  -output *output_hdfs_dir* \
	  -mapper *your_mapper_script* \
	  -reducer *your_reducer_script*
	
::

Compare to hadoop-streaming:

::

	$ $HADOOP_HOME/bin/hadoop jar hadoop-streaming.jar \
	  -${generic_hadoop_mapreduce_options} \
	  -files *file1,file2,file3*... \
	  -input *input1,input2,input3*... \
	  -output *output_hdfs_dir* \
	  -mapper *your_mapper_script* \
	  -reducer *your_reducer_script*
	  
::


For **Pipistrello** to do your map-reduce job, both *your_mapper_script* and *your_reducer_script* must be executables 
that take as an argument two and one  strings respectively, i.e., running them locally from the command line should look like:

::

	$ your_mapper_script m_arg1 m_arg2
	$ map_result_filename
	
::

and

::

	$ your_reducer_script r_arg1
	$ reduce_result_filename
	
::

In the logic of Pipistrello, *m_arg1* will represent an hdfs fiename (extracted from your *input* paths, just a name) 
and *m_arg2* will be an actual local file path on which *your_mapper_script* will operate (you don't know it, but you have to
make your script be ready for receiving it and acting on it, as this represents the path to your data). As a result of the
mapping process, *your_mapper_script* should 1) write a file of any kind to the current working directory, and 2) write a
string to stdout with its filename (*map_result_filename* above).

*your_reducer_script* should instead be ready to read lines from a file *r_arg1*. These lines will be no other thing
than each of the *map_result_filename* emmited by the mappers. Once *your_reducer_script* knows all of these filenames, it
will be able to combine them and produce 1) a single file of any kind saved to the current working directory and 2) a string *reduce_result_filename* written to stdout.



