Here's a scratchpad to note timing numbers of various builds, so that we can keep the relative magnitudes in mind.

## Linux

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

### Target: Chrome -- OS: Linux -- Ninja: v1.1.0 -- Hardware: fast CPU, slow disk, cold cache
```
$ time ninja -d stats -C out/Release chrome
ninja: Entering directory `out/Release'
[1/1] RULE Generate assembly offsets ../...n_asm_offsets_vp8.gen/asm_enc_offsets.o
metric           	count 	avg (us) 	total (ms)
.ninja parse     	660   	1225.8  	809.0
canonicalize str 	101010	0.2     	15.3
canonicalize path	1446311	0.1     	151.0
lookup node      	1446311	0.1     	189.0
.ninja_log load  	1     	93559.0 	93.6
node stat        	39018 	444.5   	17343.1
depfile load     	9702  	2536.9  	24613.1
StartEdge        	1     	779.0   	0.8
FinishEdge       	1     	6757.0  	6.8

path->node hash load 0.50 (49319 entries / 98317 buckets)

real	0m43.043s
```

Same, with a warm cache:

```
$ time ninja -d stats -C out/Release chrome
ninja: Entering directory `out/Release'
[1/1] RULE Generate assembly offsets ../...n_asm_offsets_vp8.gen/asm_enc_offsets.o
metric           	count 	avg (us) 	total (ms)
.ninja parse     	660   	334.6   	220.9
canonicalize str 	101010	0.1     	13.7
canonicalize path	1446311	0.1     	104.0
lookup node      	1446311	0.1     	129.1
.ninja_log load  	1     	8407.0  	8.4
node stat        	39018 	0.8     	32.9
depfile load     	9702  	49.3    	478.4
StartEdge        	1     	477.0   	0.5
FinishEdge       	1     	5416.0  	5.4

path->node hash load 0.50 (49319 entries / 98317 buckets)

real	0m0.742s
```

## Mac

### Target: Chrome -- OS: Mac OS X 10.6.8 -- Ninja: v1 -- Hardware: MacPro4,1, HDD
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
Empty build takes 1.33s warm-cache, 58s cold-cache
```

### Target: Chrome -- OS: Mac OS X 10.8.2 -- Ninja: v1 -- Hardware: Retina MBP, SSD
```
ninja -C out/Release/ chrome -d stats
ninja: Entering directory `out/Release/'
ninja: no work to do.
metric           	count 	avg (us) 	total (ms)
.ninja parse     	685   	550.7   	377.2
canonicalize str 	117383	0.2     	19.4
canonicalize path	1161235	0.1     	97.4
lookup node      	1161235	0.1     	130.1
.ninja_log load  	1     	15513.0 	15.5
node stat        	39611 	2.7     	107.6
depfile load     	9783  	53.3    	521.5

path->node hash load 0.50 (49367 entries / 98317 buckets)
Empty build takes 0.77s warm-cache, 8s cold-cache
```

## Windows

### Target: Chrome's base_unittests -- OS: Windows 7 -- Ninja: v1 93e5094 -- Hardware: fast CPU, slow disk, cold cache
```
C:\Users\evmar\chrome\src>ninja -C out\Debug -d stats base_unittests
ninja: Entering directory `out\Debug'
ninja: no work to do.
metric                  count   avg (us)        total (ms)
.ninja parse            843     4975.8          4194.6
canonicalize str        138729  0.0             0.5
canonicalize path       187273  0.0             0.1
lookup node             187273  0.0             1.3
.ninja_log load         1       37763.0         37.8
node stat               1879    543.0           1020.3
depfile load            635     6834.0          4339.6

path->node hash load 0.15 (39437 entries / 262144 buckets)
```

### Same, with hot cache
```
C:\Users\evmar\chrome\src>ninja -C out\Debug -d stats base_unittests
ninja: Entering directory `out\Debug'
ninja: no work to do.
metric                  count   avg (us)        total (ms)
.ninja parse            843     693.6           584.7
canonicalize str        138729  0.0             0.4
canonicalize path       187273  0.0             0.1
lookup node             187273  0.0             0.3
.ninja_log load         1       23197.0         23.2
node stat               1879    110.1           206.9
depfile load            635     78.3            49.7

path->node hash load 0.15 (39437 entries / 262144 buckets)
```

### Same, `chrome` target
```
C:\Users\evmar\chrome\src>ninja -C out\Debug -d stats chrome
ninja: Entering directory `out\Debug'
ninja: no work to do.
metric                  count   avg (us)        total (ms)
.ninja parse            843     641.1           540.4
canonicalize str        138729  0.0             0.1
canonicalize path       3058444 0.0             1.6
lookup node             3058444 0.0             13.7
.ninja_log load         1       21766.0         21.8
node stat               39061   685.9           26792.4
depfile load            9524    13010.2         123909.2

path->node hash load 0.19 (50685 entries / 262144 buckets)

C:\Users\evmar\chrome\src>ninja -C out\Debug -d stats chrome
ninja: Entering directory `out\Debug'
ninja: no work to do.
metric                  count   avg (us)        total (ms)
.ninja parse            843     662.4           558.4
canonicalize str        138729  0.0             0.0
canonicalize path       3058444 0.0             0.4
lookup node             3058444 0.0             0.7
.ninja_log load         1       21500.0         21.5
node stat               39061   150.1           5864.5
depfile load            9524    190.7           1816.3

path->node hash load 0.19 (50685 entries / 262144 buckets)
```