Here's a scratchpad to note timing numbers of various builds, so that we can keep the relative magnitudes in mind.

### Target: Chrome -- OS: Linux -- Ninja: v1 -- Hardware: beastly Googler machine
```
%n -d stats chrome
ninja: Entering directory `out/Release'
ninja: no work to do.
metric           	count 	avg (us) 	total (ms)
.ninja parse     	619   	403.7   	249.9
canonicalize str 	96655 	0.2     	16.9
canonicalize path	1283411	0.1     	95.5
lookup node      	1283411	0.1     	118.7
.ninja_log load  	1     	13398.0 	13.4
node stat        	36391 	1.9     	67.4
depfile load     	8993  	48.4    	435.7

path->node hash load 0.94 (46017 entries / 49157 buckets)
  0.57s user 0.15s system 98% cpu 0.728 total
```
