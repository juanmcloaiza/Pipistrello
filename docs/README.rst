.. _readme:

Pipistrello
===========

	import project
	# Get your stuff done
	pipistrello.do_stuff()

Pipistrello is a utility that allows you to create and run Map/Reduce jobs with 
(almost) any executable or script as the mapper and/or the reducer in pretty much 
the same way as hadoop-streaming. The fundamental difference is that Pipistrello
is specially designed to operate on binary files 
(NetCDF, jpg, png, mp3... you name it!).

Launching it is pretty simple, provided that a hadoop cluster is already properly 
mounted:

	$HADOOP_HOME/bin/hadoop jar Pipistrello.jar \
	-${generic_hadoop_mapreduce_options} \
	-files $file1,$file2,$file3 \
	-input $input1,$input2,$input3 \
	-output $output_hdfs_dir \
	-mapper $your_mapper_script \
	-reducer $your_reducer_script

Compare to hadoop-streaming:

	$HADOOP_HOME/bin/hadoop jar hadoop-streaming.jar \
	-input $input1,$input2,$input3 \
	-output $output_hdfs_dir \
	-mapper $your_mapper_script \
	-reducer $your_mapper_script 


In the above example, both $your_mapper_script and $your_reducer_script must be executables that take as an argument one and two 
strings. Respectively, i.e., running them from the command line should look like:

	$ *$your_mapper_script* *arg1* *arg2*
	$ *map_result_filename*

and

	$ *$your_reducer_script* *arg1* *arg2*
	$ *map_result_filename*

In the logic of Pipistrello, *arg1* will represent an hdfs fiename and *arg2* will be an actual local file path on which 
*$your_mapper_script* will operate. *$your_mapper_script* should also produce a file of any kind to the current working directory 
(the desired result of the mapping process), and print its filename to stdout as a string (*map_result_filename*) with no breaking 
line at the end (This is a small inconvenience that could be easily avoided --the developer to blame).



