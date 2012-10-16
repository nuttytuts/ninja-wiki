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

### Target: Chrome -- OS: Mac -- Ninja: v1 -- Hardware: MacPro4,1, HDD, OS X 10.6.8
```
$ ninja -C out/Release/ chrome -d stats
ninja: Entering directory `out/Release/'
ninja: no work to do.
metric           	count 	avg (us) 	total (ms)
.ninja parse     	685   	827.5   	566.8
canonicalize str 	117297	0.3     	31.6
canonicalize path	1160719	0.2     	181.0
lookup node      	1160719	0.2     	210.0
.ninja_log load  	1     	39455.0 	39.5
node stat        	39611 	4.6     	183.3
depfile load     	9784  	90.7    	887.8

path->node hash load 0.50 (49370 entries / 98317 buckets)
```