# Hadoop过滤，映射下推与谓词下推

> 传统的OLAP System中，在进行 Join 的时候使用过滤和映射会极大的提高性能。
> 

`projection`和`predicate pushdown`通过对存储格式进行进一步的过滤和筛选，介以直接跳过整个record或这个block，减少不必要的io和网络传输。

## <font color="green">一、PushDown and Column Pruning</font>

谓词下推 （predicate pushdown）属于逻辑优化。优化器可以将谓词过滤下推到数据源，从而使物理执行跳过无关数据。在使用 `Parquet` 的情况下，更可能存在 **文件被整块跳过** 的情况，同事系统还通过 `dictionary encode` 将字符串对比转化为更小的整数对比。

### 谓词下推的目的

**将过滤条件尽可能的下沉到数据源**。

> 谓词（predicate）：用来描述或判定客观性质、特征或者课题之间关系的此项。
> 

### 列裁剪

> 列裁剪 ColumnPruning只把那些查询不需要的字段过滤掉。是的扫描的数据量减少。如果底层的文件格式为 列式存储。则可以进一步的映射下推，映射可以理解为 表结构映射，Parquet每一列所有值都是连续存储的，所以分区去除每一列的所有值就可以实现 TableScan 算子，进而毕淼扫描整个表文件内容。
> 







