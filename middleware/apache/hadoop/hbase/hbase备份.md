# hbase备份

## 导出 Export

- **usage：**

```bash
Usage: Export [-D <property=value>]* <tablename> <outputdir> [<versions> [<starttime> [<endtime>]] [^[regex pattern] or [Prefix] to filter]]
 
  Note: -D properties will be applied to the conf used. 
  For example: 
   -D mapreduce.output.fileoutputformat.compress=true
   -D mapreduce.output.fileoutputformat.compress.codec=org.apache.hadoop.io.compress.GzipCodec
   -D mapreduce.output.fileoutputformat.compress.type=BLOCK
此外，可以指定以下扫描属性控制/限制出口。
   -D hbase.mapreduce.scan.column.family=<family1>,<family2>, ...
   -D hbase.mapreduce.include.deleted.rows=true
   -D hbase.mapreduce.scan.row.start=<ROWSTART>
   -D hbase.mapreduce.scan.row.stop=<ROWSTOP>
   -D hbase.client.scanner.caching=100
   -D hbase.export.visibility.labels=<labels>
对于非常宽的表，可以考虑设置如下的批大小:
   -D hbase.export.scanner.batch=10
   -D hbase.export.scanner.caching=100
   -D mapreduce.job.name=jobName - use the specified mapreduce job name for the export
对于MR性能，考虑以下特性:
   -D mapreduce.map.speculative=false
   -D mapreduce.reduce.speculative=false
```

## 导入 Import

- **usage：**

```bash
Usage: Import [options] <tablename> <inputdir>
 默认情况下，import会将数据直接加载到hbase中。要生成数据文件以准备大容量数据加载，请传递选项:
  -Dimport.bulk.output=/path/for/output
 如果有一个大的结果，其中包含了太多可能由Memery Sort in Reducer引起的单元格空白，请传递选项： 
  -Dimport.bulk.hasLargeResult=true
 要对输入应用通用org.apache.hadoop.hbase.filter.filter，请使用 :
  -Dimport.filter.class=<name of filter class>
  -Dimport.filter.args=<comma separated list of args for filter
 注意:过滤器将在通过HBASE_IMPORTER_RENAME_CFS属性进行键重命名之前生效。
此外，过滤器将仅使用Filter#filterRowKey(byte[] buffer, int offset, int length)方法来确定是否需要完全忽略当前行进行处理，而Filter#filterCell(Cell)方法来确定是否应该添加Cell;
Filter.ReturnCode#INCLUDE 和 #INCLUDE_AND_NEXT_COL将被视为包含 Cell。
要导入从HBase 0.94导出的数据，请使用
  -Dhbase.import.version=0.94
  -D mapreduce.job.name=jobName - use the specified mapreduce job name for the import
就表现而言，可考虑下列方案:
  -Dmapreduce.map.speculative=false
  -Dmapreduce.reduce.speculative=false
  -Dimport.wal.durability=<在将数据写入hbase时使用。允许的值是受支持的持久性值，如SKIP_WAL/ASYNC_WAL/SYNC_WAL/…>
```