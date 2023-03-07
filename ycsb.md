
# Table of Contents

1.  [check](#orgf3bd358)
    1.  [src code](#org68e2357)
    2.  [default config <code>[2/2]</code>](#org6f914d0)
2.  [RocksDB settings](#org456be2d)
    1.  [Terminology](#org9a885f4)
3.  [WiredTiger settings](#org224201a)
4.  [Bench](#orgf1119c0)
    1.  [General](#orgf469aa5)
    2.  [Load](#org3683634)
    3.  [Run Workerload A (UPDATE heavy | UPDATE:READ=0.95:0.05)](#orga97ebb7)
    4.  [Run Workerload B (READ most | READ:UPDATE=0.95:0.05)](#orgddebf46)
    5.  [Run Workerload C (READ only)](#orgb0eb0b9)
    6.  [Run Workerload D (READ lastest | READ:INSERT=0.95:0.05)](#orgc401be5)
    7.  [Run Workerload E (SCAN heavy | SCAN:INSERT=0.95:0.05)](#org124f1ec)
    8.  [Run Workerload F(Read-Modify-Update | READ:WRITE=1.5:1)](#orgd9549b9)



<a id="orgf3bd358"></a>

# TODO check


<a id="org68e2357"></a>

## TODO src code


<a id="org6f914d0"></a>

## DONE default config <code>[2/2]</code>

-   State "DONE"       from "TODO"       <span class="timestamp-wrapper"><span class="timestamp">[2023-02-22 Wed 11:18] </span></span>   
    rocksdb: bloom bits 10 is recommended in both header and wiki.
    wiredtiger: block compressor snappy
-   [X] rocksdb
-   [X] wiredtiger


<a id="org456be2d"></a>

# RocksDB settings

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-right" />

<col  class="org-left" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">Properties</th>
<th scope="col" class="org-right">Value</th>
<th scope="col" class="org-left">Remark</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left">rocksdb.compression</td>
<td class="org-right">snappy</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">rocksdb.max_background_jobs</td>
<td class="org-right">2</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">rocksdb.target_file_size_base</td>
<td class="org-right">67108864</td>
<td class="org-left">64MB</td>
</tr>


<tr>
<td class="org-left">rocksdb.target_file_size_multiplier</td>
<td class="org-right">1</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">rocksdb.max_bytes_for_level_base</td>
<td class="org-right">268435456</td>
<td class="org-left">256MB</td>
</tr>


<tr>
<td class="org-left">rocksdb.write_buffer_size</td>
<td class="org-right">67108864</td>
<td class="org-left">64MB</td>
</tr>


<tr>
<td class="org-left">rocksdb.max_open_files</td>
<td class="org-right">-1</td>
<td class="org-left">files opened are always kept open</td>
</tr>


<tr>
<td class="org-left">rocksdb.max_write_buffer_number</td>
<td class="org-right">2</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">rocksdb.use_direct_io_for_flush_compaction</td>
<td class="org-right">false</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">rocksdb.use_direct_reads</td>
<td class="org-right">false</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">rocksdb.allow_mmap_writes</td>
<td class="org-right">false</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">rocksdb.allow_mmap_reads</td>
<td class="org-right">false</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">rocksdb.cache_size</td>
<td class="org-right">8388608</td>
<td class="org-left">8MB</td>
</tr>


<tr>
<td class="org-left">rocksdb.compressed_cache_size</td>
<td class="org-right">0</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">rocksdb.bloom_bits</td>
<td class="org-right">10</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">rocksdb.increase_parallelism</td>
<td class="org-right">false</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">rocksdb.optimize_level_style_compaction</td>
<td class="org-right">false</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="org9a885f4"></a>

## Terminology

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">Term</th>
<th scope="col" class="org-left">Description</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left">block cache</td>
<td class="org-left">in-memory data structure that cache the hot data blocks from the SST files</td>
</tr>


<tr>
<td class="org-left">target_file_size_base</td>
<td class="org-left">target_file_size_base is per-file size for level1, in bytes</td>
</tr>


<tr>
<td class="org-left">max_bytes_for_level_base</td>
<td class="org-left">max size for level1, in bytes</td>
</tr>


<tr>
<td class="org-left">write buffer</td>
<td class="org-left">memtable</td>
</tr>
</tbody>
</table>


<a id="org224201a"></a>

# WiredTiger settings

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">Properties</th>
<th scope="col" class="org-left">Value</th>
<th scope="col" class="org-left">Remark</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left">wiredtiger.cache_size</td>
<td class="org-left">100MB</td>
<td class="org-left">For BTree ONLY, including associated in-mem data structures</td>
</tr>


<tr>
<td class="org-left">wiredtiger.direct_io</td>
<td class="org-left">[]</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">wiredtiger.in_memory</td>
<td class="org-left">false</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">wiredtiger.lsm_mgr.merge</td>
<td class="org-left">true</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">wiredtiger.lsm_mgr.max_workers</td>
<td class="org-left">4</td>
<td class="org-left"># of threads doing background merging</td>
</tr>


<tr>
<td class="org-left">wiredtiger.blk_mgr.allocation_size</td>
<td class="org-left">4KB</td>
<td class="org-left">OS PAGE_SIZE</td>
</tr>


<tr>
<td class="org-left">wiredtiger.blk_mgr.bloom_bit_count</td>
<td class="org-left">16</td>
<td class="org-left">per key</td>
</tr>


<tr>
<td class="org-left">wiredtiger.blk_mgr.bloom_hash_count</td>
<td class="org-left">8</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">wiredtiger.blk_mgr.chunk_max</td>
<td class="org-left">5GB</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">wiredtiger.blk_mgr.chunk_size</td>
<td class="org-left">10MB</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">wiredtiger.blk_mgr.compressor</td>
<td class="org-left">snappy</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">wiredtiger.blk_mgr.btree.internal_page_max</td>
<td class="org-left">4KB</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">wiredtiger.blk_mgr.btree.leaf_key_max</td>
<td class="org-left">0</td>
<td class="org-left">1/10 the size of a newly split page</td>
</tr>


<tr>
<td class="org-left">wiredtiger.blk_mgr.btree.leaf_value_max</td>
<td class="org-left">0</td>
<td class="org-left">1/2 the size a newly split page</td>
</tr>


<tr>
<td class="org-left">wiredtiger.blk_mgr.btree.leaf_page_max</td>
<td class="org-left">32KB</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="orgf1119c0"></a>

# Bench


<a id="orgf469aa5"></a>

## General

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-right" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">Key size</th>
<th scope="col" class="org-left">Value size</th>
<th scope="col" class="org-left">Record count</th>
<th scope="col" class="org-left">Operation count</th>
<th scope="col" class="org-right">thread count</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left">8B</td>
<td class="org-left">24B</td>
<td class="org-left">1G (10M for wt)</td>
<td class="org-left">100M</td>
<td class="org-right">8</td>
</tr>
</tbody>
</table>

      ./ycsb -run -db rocksdb -P workloads/workloadf -P rocksdb/rocksdb.properties -s \
    -p fixedkey8b=true -p fixedfieldlen=true -p fieldcount=1 -p fieldlength=24 \
    -p recordcount=1000000000 -p operationcount=100000000 -p threadcount=8


<a id="org3683634"></a>

## Load

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-right" />

<col  class="org-left" />

<col  class="org-right" />

<col  class="org-right" />

<col  class="org-right" />

<col  class="org-right" />

<col  class="org-right" />

<col  class="org-right" />

<col  class="org-right" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">DB</th>
<th scope="col" class="org-right">Load runtime(sec)</th>
<th scope="col" class="org-left">Load throughput(ops/sec)</th>
<th scope="col" class="org-right">Max</th>
<th scope="col" class="org-right">Min</th>
<th scope="col" class="org-right">Avg(us)</th>
<th scope="col" class="org-right">90</th>
<th scope="col" class="org-right">99</th>
<th scope="col" class="org-right">99.9</th>
<th scope="col" class="org-right">99.99</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left">RocksDB-INSERT</td>
<td class="org-right">5669.34</td>
<td class="org-left">176,387</td>
<td class="org-right">32079.87</td>
<td class="org-right">4.40</td>
<td class="org-right">44.93</td>
<td class="org-right">20.02</td>
<td class="org-right">31.90</td>
<td class="org-right">14254.08</td>
<td class="org-right">15114.24</td>
</tr>


<tr>
<td class="org-left">WiredTiger-INSERT</td>
<td class="org-right">676.592</td>
<td class="org-left">14,780</td>
<td class="org-right">56492.03</td>
<td class="org-right">0.57</td>
<td class="org-right">532.24</td>
<td class="org-right">974.85</td>
<td class="org-right">7712.77</td>
<td class="org-right">16039.93</td>
<td class="org-right">24297.47</td>
</tr>
</tbody>
</table>


<a id="orga97ebb7"></a>

## Run Workerload A (UPDATE heavy | UPDATE:READ=0.95:0.05)

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-right" />

<col  class="org-left" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">DB</th>
<th scope="col" class="org-right">Run runtime(sec)</th>
<th scope="col" class="org-left">Run throughput(ops/sec)</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left">RocksDB</td>
<td class="org-right">1267.29</td>
<td class="org-left">78,908</td>
</tr>


<tr>
<td class="org-left">WiredTiger</td>
<td class="org-right">373.14</td>
<td class="org-left">26,799</td>
</tr>
</tbody>
</table>

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-right" />

<col  class="org-right" />

<col  class="org-right" />

<col  class="org-right" />

<col  class="org-right" />

<col  class="org-right" />

<col  class="org-right" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">DB</th>
<th scope="col" class="org-right">Max</th>
<th scope="col" class="org-right">Min</th>
<th scope="col" class="org-right">Avg(us)</th>
<th scope="col" class="org-right">90</th>
<th scope="col" class="org-right">99</th>
<th scope="col" class="org-right">99.9</th>
<th scope="col" class="org-right">99.99</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left">RocksDB-UPDATE</td>
<td class="org-right">201719.81</td>
<td class="org-right">4.50</td>
<td class="org-right">105.02</td>
<td class="org-right">273.41</td>
<td class="org-right">908.29</td>
<td class="org-right">2920.45</td>
<td class="org-right">5509.12</td>
</tr>


<tr>
<td class="org-left">RocksDB-READ</td>
<td class="org-right">203030.53</td>
<td class="org-right">0.30</td>
<td class="org-right">95.30</td>
<td class="org-right">261.12</td>
<td class="org-right">896.00</td>
<td class="org-right">2902.01</td>
<td class="org-right">5414.91</td>
</tr>


<tr>
<td class="org-left">Wiredtiger-UPDATE</td>
<td class="org-right">54329.34</td>
<td class="org-right">0.95</td>
<td class="org-right">297.49</td>
<td class="org-right">522.24</td>
<td class="org-right">3448.83</td>
<td class="org-right">14852.09</td>
<td class="org-right">24461.31</td>
</tr>


<tr>
<td class="org-left">WiredTiger-READ</td>
<td class="org-right">52527.10</td>
<td class="org-right">0.26</td>
<td class="org-right">293.46</td>
<td class="org-right">514.05</td>
<td class="org-right">3403.78</td>
<td class="org-right">14745.60</td>
<td class="org-right">24379.39</td>
</tr>
</tbody>
</table>


<a id="orgddebf46"></a>

## Run Workerload B (READ most | READ:UPDATE=0.95:0.05)

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-right" />

<col  class="org-left" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">DB</th>
<th scope="col" class="org-right">Run runtime(sec)</th>
<th scope="col" class="org-left">Run throughput(ops/sec)</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left">RocksDB</td>
<td class="org-right">1113.47</td>
<td class="org-left">89,810</td>
</tr>


<tr>
<td class="org-left">WiredTiger</td>
<td class="org-right">152.114</td>
<td class="org-left">65,740</td>
</tr>
</tbody>
</table>

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-right" />

<col  class="org-right" />

<col  class="org-right" />

<col  class="org-right" />

<col  class="org-right" />

<col  class="org-right" />

<col  class="org-right" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">DB</th>
<th scope="col" class="org-right">Max</th>
<th scope="col" class="org-right">Min</th>
<th scope="col" class="org-right">Avg(us)</th>
<th scope="col" class="org-right">90</th>
<th scope="col" class="org-right">99</th>
<th scope="col" class="org-right">99.9</th>
<th scope="col" class="org-right">99.99</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left">RocksDB-UPDATE</td>
<td class="org-right">18022.40</td>
<td class="org-right">5.00</td>
<td class="org-right">99.50</td>
<td class="org-right">276.99</td>
<td class="org-right">603.65</td>
<td class="org-right">2217.98</td>
<td class="org-right">3205.12</td>
</tr>


<tr>
<td class="org-left">RocksDB-READ</td>
<td class="org-right">32063.49</td>
<td class="org-right">0.30</td>
<td class="org-right">87.75</td>
<td class="org-right">263.94</td>
<td class="org-right">591.87</td>
<td class="org-right">2207.74</td>
<td class="org-right">3170.30</td>
</tr>


<tr>
<td class="org-left">Wiredtiger-UPDATE</td>
<td class="org-right">19251.20</td>
<td class="org-right">1.05</td>
<td class="org-right">130.26</td>
<td class="org-right">225.79</td>
<td class="org-right">575.49</td>
<td class="org-right">3772.41</td>
<td class="org-right">10911.74</td>
</tr>


<tr>
<td class="org-left">WiredTiger-READ</td>
<td class="org-right">24903.68</td>
<td class="org-right">0.22</td>
<td class="org-right">118.51</td>
<td class="org-right">219.90</td>
<td class="org-right">374.01</td>
<td class="org-right">2748.79</td>
<td class="org-right">8749.06</td>
</tr>
</tbody>
</table>


<a id="orgb0eb0b9"></a>

## Run Workerload C (READ only)

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-right" />

<col  class="org-left" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">DB</th>
<th scope="col" class="org-right">Run runtime(sec)</th>
<th scope="col" class="org-left">Run throughput(ops/sec)</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left">RocksDB</td>
<td class="org-right">1113.11</td>
<td class="org-left">89,837</td>
</tr>


<tr>
<td class="org-left">WiredTiger</td>
<td class="org-right">127.54</td>
<td class="org-left">78,407</td>
</tr>
</tbody>
</table>

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-right" />

<col  class="org-right" />

<col  class="org-right" />

<col  class="org-right" />

<col  class="org-right" />

<col  class="org-right" />

<col  class="org-right" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">DB</th>
<th scope="col" class="org-right">Max</th>
<th scope="col" class="org-right">Min</th>
<th scope="col" class="org-right">Avg(us)</th>
<th scope="col" class="org-right">90</th>
<th scope="col" class="org-right">99</th>
<th scope="col" class="org-right">99.9</th>
<th scope="col" class="org-right">99.99</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left">RocksDB-READ</td>
<td class="org-right">93061.12</td>
<td class="org-right">1.20</td>
<td class="org-right">88.31</td>
<td class="org-right">264.70</td>
<td class="org-right">515.58</td>
<td class="org-right">1478.65</td>
<td class="org-right">4313.09</td>
</tr>


<tr>
<td class="org-left">WiredTiger-READ</td>
<td class="org-right">14417.92</td>
<td class="org-right">0.20</td>
<td class="org-right">100.05</td>
<td class="org-right">214.53</td>
<td class="org-right">256.13</td>
<td class="org-right">673.79</td>
<td class="org-right">1638.40</td>
</tr>
</tbody>
</table>


<a id="orgc401be5"></a>

## Run Workerload D (READ lastest | READ:INSERT=0.95:0.05)

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-right" />

<col  class="org-left" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">DB</th>
<th scope="col" class="org-right">Run runtime(sec)</th>
<th scope="col" class="org-left">Run throughput(ops/sec)</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left">RocksDB</td>
<td class="org-right">446.869</td>
<td class="org-left">223,779</td>
</tr>


<tr>
<td class="org-left">WiredTiger</td>
<td class="org-right">133.523</td>
<td class="org-left">74,893</td>
</tr>
</tbody>
</table>

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-right" />

<col  class="org-right" />

<col  class="org-right" />

<col  class="org-right" />

<col  class="org-right" />

<col  class="org-right" />

<col  class="org-right" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">DB</th>
<th scope="col" class="org-right">Max</th>
<th scope="col" class="org-right">Min</th>
<th scope="col" class="org-right">Avg(us)</th>
<th scope="col" class="org-right">90</th>
<th scope="col" class="org-right">99</th>
<th scope="col" class="org-right">99.9</th>
<th scope="col" class="org-right">99.99</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left">RocksDB-INSERT</td>
<td class="org-right">6078.46</td>
<td class="org-right">4.20</td>
<td class="org-right">11.75</td>
<td class="org-right">15.61</td>
<td class="org-right">22.63</td>
<td class="org-right">29.50</td>
<td class="org-right">78.91</td>
</tr>


<tr>
<td class="org-left">RocksDB-READ</td>
<td class="org-right">30605.31</td>
<td class="org-right">0.20</td>
<td class="org-right">36.29</td>
<td class="org-right">123.71</td>
<td class="org-right">409.34</td>
<td class="org-right">2026.49</td>
<td class="org-right">2951.17</td>
</tr>


<tr>
<td class="org-left">Wiredtiger-INSERT</td>
<td class="org-right">19218.43</td>
<td class="org-right">2.20</td>
<td class="org-right">172.26</td>
<td class="org-right">245.25</td>
<td class="org-right">914.94</td>
<td class="org-right">3348.48</td>
<td class="org-right">10280.96</td>
</tr>


<tr>
<td class="org-left">WiredTiger-READ</td>
<td class="org-right">21200.90</td>
<td class="org-right">0.24</td>
<td class="org-right">101.55</td>
<td class="org-right">220.29</td>
<td class="org-right">407.55</td>
<td class="org-right">2729.98</td>
<td class="org-right">8699.90</td>
</tr>
</tbody>
</table>


<a id="org124f1ec"></a>

## Run Workerload E (SCAN heavy | SCAN:INSERT=0.95:0.05)

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-right" />

<col  class="org-left" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">DB</th>
<th scope="col" class="org-right">Run runtime(sec)</th>
<th scope="col" class="org-left">Run throughput(ops/sec)</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left">RocksDB</td>
<td class="org-right">1000.00</td>
<td class="org-left">49,308</td>
</tr>


<tr>
<td class="org-left">WiredTiger</td>
<td class="org-right">158.148</td>
<td class="org-left">63,232</td>
</tr>
</tbody>
</table>

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-right" />

<col  class="org-right" />

<col  class="org-right" />

<col  class="org-right" />

<col  class="org-right" />

<col  class="org-right" />

<col  class="org-right" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">DB</th>
<th scope="col" class="org-right">Max</th>
<th scope="col" class="org-right">Min</th>
<th scope="col" class="org-right">Avg(us)</th>
<th scope="col" class="org-right">90</th>
<th scope="col" class="org-right">99</th>
<th scope="col" class="org-right">99.9</th>
<th scope="col" class="org-right">99.99</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left">RocksDB-INSERT</td>
<td class="org-right">3143.68</td>
<td class="org-right">4.30</td>
<td class="org-right">14.34</td>
<td class="org-right">17.50</td>
<td class="org-right">24.21</td>
<td class="org-right">31.61</td>
<td class="org-right">81.86</td>
</tr>


<tr>
<td class="org-left">RocksDB-SCAN</td>
<td class="org-right">41123.84</td>
<td class="org-right">5.70</td>
<td class="org-right">167.98</td>
<td class="org-right">380.67</td>
<td class="org-right">708.61</td>
<td class="org-right">2022.40</td>
<td class="org-right">3336.19</td>
</tr>


<tr>
<td class="org-left">Wiredtiger-INSERT</td>
<td class="org-right">20234.24</td>
<td class="org-right">2.15</td>
<td class="org-right">162.22</td>
<td class="org-right">239.49</td>
<td class="org-right">807.42</td>
<td class="org-right">4093.95</td>
<td class="org-right">13197.31</td>
</tr>


<tr>
<td class="org-left">WiredTiger-SCAN</td>
<td class="org-right">30064.62</td>
<td class="org-right">0.25</td>
<td class="org-right">121.98</td>
<td class="org-right">231.55</td>
<td class="org-right">329.21</td>
<td class="org-right">2684.93</td>
<td class="org-right">10616.83</td>
</tr>
</tbody>
</table>


<a id="orgd9549b9"></a>

## Run Workerload F(Read-Modify-Update | READ:WRITE=1.5:1)

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-right" />

<col  class="org-left" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">DB</th>
<th scope="col" class="org-right">Run runtime(sec)</th>
<th scope="col" class="org-left">Run throughput(ops/sec)</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left">RocksDB(actually 1.5G records)</td>
<td class="org-right">1192.88</td>
<td class="org-left">125,843</td>
</tr>


<tr>
<td class="org-left">WiredTiger(actually 15M records)</td>
<td class="org-right">355.461</td>
<td class="org-left">42,204</td>
</tr>
</tbody>
</table>

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-right" />

<col  class="org-right" />

<col  class="org-right" />

<col  class="org-right" />

<col  class="org-right" />

<col  class="org-right" />

<col  class="org-right" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">DB</th>
<th scope="col" class="org-right">Max</th>
<th scope="col" class="org-right">Min</th>
<th scope="col" class="org-right">Avg(us)</th>
<th scope="col" class="org-right">90</th>
<th scope="col" class="org-right">99</th>
<th scope="col" class="org-right">99.9</th>
<th scope="col" class="org-right">99.99</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left">RocksDB-READ</td>
<td class="org-right">62226.43</td>
<td class="org-right">0.30</td>
<td class="org-right">87.69</td>
<td class="org-right">242.69</td>
<td class="org-right">685.57</td>
<td class="org-right">2836.48</td>
<td class="org-right">5316.61</td>
</tr>


<tr>
<td class="org-left">RocksDB-UPDATE</td>
<td class="org-right">11034.62</td>
<td class="org-right">4.30</td>
<td class="org-right">13.26</td>
<td class="org-right">20.91</td>
<td class="org-right">28.51</td>
<td class="org-right">37.50</td>
<td class="org-right">91.84</td>
</tr>


<tr>
<td class="org-left">Wiredtiger-READ</td>
<td class="org-right">50200.57</td>
<td class="org-right">0.22</td>
<td class="org-right">279.12</td>
<td class="org-right">270.33</td>
<td class="org-right">4042.75</td>
<td class="org-right">15335.42</td>
<td class="org-right">24887.29</td>
</tr>


<tr>
<td class="org-left">WiredTiger-UPDATE</td>
<td class="org-right">27394.05</td>
<td class="org-right">0.72</td>
<td class="org-right">4.32</td>
<td class="org-right">4.51</td>
<td class="org-right">7.17</td>
<td class="org-right">35.42</td>
<td class="org-right">2942.97</td>
</tr>
</tbody>
</table>

