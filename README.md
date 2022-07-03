# odoo-calculator
Analyzes the characteristics of the server and helps calculate the number of workers and memory for each

Hardware Matrix for recommended values:
(Note these are for REAL hardware, Virtual Hardware has it's own
issues that can arise with too many guest on a host with too many
CPU cores etc, so remember that when looking at the chart below)
(One other note that when I tested these, I did use VMware but
no other VMs where competing for resources)
(One last note, really. These numbers are to show the relationship
between the config settings and hardware. We also assume you are running
the database server on the same server. I know at some point in the
higher numbers that this would not be the case. These are not numbers
set in stone nor numbers gotten from Odoo. These are numbers I have
come up with from the testing I have done. Real world examples if you
have them would be great and these numbers can and should be updated!

Heading	Description
CPUs	Number of CPU Cores not threads
Physical	Physical memory, not virtual or swap
workers	Number of workers specified in config file (workers = x)
cron	Number of workers for cron jobs (max_cron_threads = xx)
Mem Per	Memory in MB that is the max memory for request per worker
Max Mem	Maximum amount that can be used by all workers
limit_memory_soft	Number in bytes that you will use for this setting
Note: Max Memory if notice is less than total memory this is on purpose. As
workers process requests they can grow beyond the Mem Per limit so a
server under heavy load could go past this amount. This is why there
is "head room" built in.

CPUs	Physical	workers	cron	Mem Per	Max Mem	limit_memory_soft
ANY	=< 256MB	NR	NR	NR	NR	NR
1	512MB	0	N/A	N/A	N/A	N/A
1	512MB	1	1	177MB	354MB	185127901
1	1GB	2	1	244MB	732MB	255652815
1	2GB	2	1	506MB	1518MB	530242876
2	1GB	3	1	183MB	732MB	191739611
2	2GB	5	2	217MB	1519MB	227246947
2	4GB	5	2	450MB	3150MB	471974428
4	2GB	5	2	217MB	1519MB	227246947
4	4GB	9	2	286MB	3146MB	300347363
4	8GB	9	3	546MB	6552MB	572662306
4	16GB	9	3	1187MB	14244MB	1244918057
6	12GB	10	4	763MB	10682MB	800304465
6	16GB	13	4	838MB	14246MB	878765687
6	32GB	13	4	1676MB	28492MB	81757531374
8	32GB	17	5	1295MB	28490MB	1358092426
8	64GB	17	5	2590MB	56980MB	2716184851
Calculations on how we got the above chart

M = Total memory in bytes (1048576 = 1MB)
W = Workers (workers )
CW = Cron workers (max_cron_threads)
TW = W + CW

For ram 512 to 999MB
((TW * 0.45 ) + TW) / M = limit_soft_memory

1GB - 1.99GB
(TW * 0.40 ) + (TW - 1) / M = limit_soft_memory

2GB - 3.99GB
(TW * 0.35 ) + (TW - 1) / M = limit_soft_memory

4GB - 7.99GB
(TW * 0.30 ) + (TW - 2) / M = limit_soft_memory

8GB + 11.99GB
(TW * 0.25 ) + (TW - 3) / M = limit_soft_memory

12GB +
(TW * 0.15 ) + (TW - 3) / M = limit_soft_memory

Memory Matrix Discussion
#49

Shakedown Testing Discussions
