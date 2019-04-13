ToolGood.AntiDuplication
===

欢迎使用`ToolGood.AntiDuplication`！`ToolGood.AntiDuplication` 是一款轻量级防重复提交组件。


### 快速上手

```` csharp
	private readonly static AntiDupCache<int, int> antiDupCache = new AntiDupCache<int, int>(50, 1);
	private readonly static AntiDupQueue<int, int> antiDupQueue = new AntiDupQueue<int, int>(50);
	private readonly static DictCache<int, int> dictCache = new DictCache<int, int>();

	antiDupCache.Execute(key, () => {
		....
		return val;
	});

	antiDupQueue.Execute(key, () => {
		....
		return val;
	});

	dictCache.Execute(key, () => {
		....
		return val;
	});

`````

注意：`AntiDupCache`、`AntiDupQueue`、`DictCache`类的`Execute`方法中的`key`不要设置为`null`，`key`为null时，不缓存值。

注意：`DictCache`需要手动调用`Clear`方法清空缓存。




### 性能：
````
----------------------- 测试缓存性能 从1到100 重复次数：10000 单位： ms -----------------------
    并发数量： 1    2    3    4    5    6    7    8    9    10   11   12
AntiDupCache： 235  176  215  242  205  198  205  222  198  220  225  242
AntiDupQueue： 82   94   104  118  147  155  148  182  195  168  170  208
   DictCache： 80   90   113  123  135  134  164  192  162  167  159  149
       Cache： 75   87   102  121  114  148  138  189  165  213  123  244

----------------------- 仿线上环境  从1到1000  单位： ms -----------------------
      并发数量： 1    2    3    4    5    6    7    8    9    10   11   12
      普通并发： 1852 950  636  480  388  331  280  241  213  198  181  168
  AntiDupCache： 1844 949  633  481  382  320  267  239  210  195  174  170
  AntiDupQueue： 1835 929  628  479  386  318  272  241  208  194  174  166
     DictCache： 1841 935  629  480  378  324  269  241  207  199  176  168
         Cache： 1832 1854 1851 1866 1858 1858 1832 1825 1801 1797 1788 1785
第二次普通并发： 1854 943  640  468  389  321  273  237  209  198  177  172

----------------------- 开始  从1到100   重复次数：1 单位： ms -----------------------
      并发数量： 1    2    3    4    5    6    7    8    9    10   11   12
      普通并发： 188  93   65   46   38   36   28   31   22   20   18   19
  AntiDupCache： 190  97   63   48   37   34   29   30   22   18   17   21
  AntiDupQueue： 188  95   63   46   37   33   30   25   21   19   17   21
     DictCache： 185  96   64   47   38   33   28   29   22   19   17   21
         Cache： 185  186  186  188  188  188  184  179  180  184  184  176
第二次普通并发： 180  92   63   47   38   36   26   28   20   17   16   20

----------------------- 开始  从1到100   重复次数：2 单位： ms -----------------------
      并发数量： 1    2    3    4    5    6    7    8    9    10   11   12
      普通并发： 368  191  124  93   73   61   55   47   44   37   34   44
  AntiDupCache： 180  90   66   48   37   31   28   24   21   17   17   22
  AntiDupQueue： 181  93   65   46   39   31   27   23   21   19   18   19
     DictCache： 176  97   61   46   38   30   31   23   21   18   18   22
         Cache： 183  187  186  182  186  185  184  177  181  177  176  177
第二次普通并发： 366  185  127  95   71   62   56   48   43   38   34   43

----------------------- 开始  从1到100   重复次数：3 单位： ms -----------------------
      并发数量： 1    2    3    4    5    6    7    8    9    10   11   12
      普通并发： 538  280  187  137  111  95   88   77   65   53   54   46
  AntiDupCache： 180  96   67   47   37   35   31   28   22   19   23   16
  AntiDupQueue： 188  95   65   49   38   36   28   29   24   19   17   17
     DictCache： 186  100  61   48   36   31   29   27   21   19   19   16
         Cache： 180  181  182  179  179  181  169  184  180  182  182  172
第二次普通并发： 547  275  190  140  113  95   86   75   67   56   54   46

----------------------- 开始  从1到100   重复次数：4 单位： ms -----------------------
      并发数量： 1    2    3    4    5    6    7    8    9    10   11   12
      普通并发： 726  371  253  190  152  132  106  91   86   74   71   69
  AntiDupCache： 189  95   64   49   37   33   28   26   22   19   17   18
  AntiDupQueue： 184  97   65   51   39   35   28   24   21   18   17   17
     DictCache： 182  95   64   45   39   34   29   23   21   18   18   16
         Cache： 170  181  180  184  182  183  181  181  176  179  179  178
第二次普通并发： 723  375  250  186  150  129  107  94   87   74   71   67

----------------------- 开始  从1到100   重复次数：5 单位： ms -----------------------
      并发数量： 1    2    3    4    5    6    7    8    9    10   11   12
      普通并发： 914  464  314  239  185  159  136  124  109  92   90   91
  AntiDupCache： 182  91   64   43   38   34   27   26   21   20   18   21
  AntiDupQueue： 184  96   62   50   36   35   31   27   21   20   16   20
     DictCache： 184  93   65   49   40   34   31   25   21   19   17   16
         Cache： 183  184  183  182  182  186  179  177  178  182  183  177
第二次普通并发： 902  477  319  240  187  159  136  121  110  93   89   90

----------------------- 开始  从1到100   重复次数：6 单位： ms -----------------------
      并发数量： 1    2    3    4    5    6    7    8    9    10   11   12
      普通并发： 1102 569  381  287  227  185  163  143  137  110  112  93
  AntiDupCache： 179  96   61   46   39   34   28   24   22   18   18   18
  AntiDupQueue： 183  95   65   47   41   37   28   23   22   19   18   17
     DictCache： 184  95   63   46   39   33   29   23   20   19   19   17
         Cache： 182  187  184  181  187  183  182  188  179  182  180  174
第二次普通并发： 1109 560  377  287  229  188  165  144  130  111  110  94

----------------------- 开始  从1到100   重复次数：7 单位： ms -----------------------
      并发数量： 1    2    3    4    5    6    7    8    9    10   11   12
      普通并发： 1284 655  445  331  269  223  184  167  154  131  129  113
  AntiDupCache： 180  95   64   46   38   35   29   27   22   19   19   19
  AntiDupQueue： 185  95   65   48   40   35   29   26   22   18   17   19
     DictCache： 187  96   65   46   36   32   28   23   21   18   19   18
         Cache： 180  190  182  183  187  184  178  189  184  175  178  180
第二次普通并发： 1277 661  438  329  266  225  188  165  151  129  129  112

----------------------- 开始  从1到100   重复次数：8 单位： ms -----------------------
      并发数量： 1    2    3    4    5    6    7    8    9    10   11   12
      普通并发： 1463 748  508  382  308  252  219  186  176  152  145  134
  AntiDupCache： 189  95   63   43   40   35   29   26   21   21   17   17
  AntiDupQueue： 186  95   64   43   37   36   29   26   20   21   16   18
     DictCache： 179  91   62   46   42   33   28   23   22   21   18   16
         Cache： 177  185  185  182  186  185  178  174  183  178  173  178
第二次普通并发： 1454 745  504  381  303  254  217  189  177  149  151  135

----------------------- 开始  从1到100   重复次数：9 单位： ms -----------------------
      并发数量： 1    2    3    4    5    6    7    8    9    10   11   12
      普通并发： 1647 848  563  421  340  286  243  217  192  168  165  140
  AntiDupCache： 178  95   65   48   38   36   29   25   23   18   17   17
  AntiDupQueue： 180  95   66   49   39   35   29   27   24   17   19   16
     DictCache： 182  94   61   49   39   31   31   26   24   19   18   19
         Cache： 178  189  184  180  184  183  177  187  180  174  181  173
第二次普通并发： 1638 844  555  428  341  285  244  215  191  167  167  138

----------------------- 开始  从1到100   重复次数：10 单位： ms -----------------------
      并发数量： 1    2    3    4    5    6    7    8    9    10   11   12
      普通并发： 1823 935  627  478  388  322  276  236  206  182  182  157
  AntiDupCache： 182  94   67   46   40   33   29   24   25   19   18   21
  AntiDupQueue： 187  97   67   48   39   34   27   24   24   21   18   19
     DictCache： 184  96   66   49   39   32   29   25   22   21   18   18
         Cache： 185  191  182  182  186  185  175  179  182  187  178  176
第二次普通并发： 1815 942  625  464  380  319  275  239  213  190  183  159

----------------------- 开始  从1到100   重复次数：11 单位： ms -----------------------
      并发数量： 1    2    3    4    5    6    7    8    9    10   11   12
      普通并发： 2010 1028 687  526  422  350  301  257  232  208  185  184
  AntiDupCache： 186  97   61   49   38   33   29   28   23   17   19   19
  AntiDupQueue： 187  94   65   48   41   34   28   28   23   18   20   21
     DictCache： 182  94   62   48   37   33   29   24   22   17   19   17
         Cache： 181  182  180  184  179  178  181  176  184  180  175  177
第二次普通并发： 2010 1037 690  527  417  350  294  258  228  208  187  183

----------------------- 开始  从1到100   重复次数：12 单位： ms -----------------------
      并发数量： 1    2    3    4    5    6    7    8    9    10   11   12
      普通并发： 2170 1108 762  569  450  389  325  283  253  228  206  186
  AntiDupCache： 182  95   64   51   41   32   28   25   26   20   18   18
  AntiDupQueue： 189  93   67   44   37   35   29   30   27   22   20   17
     DictCache： 184  97   59   50   38   29   27   26   24   19   18   17
         Cache： 174  189  181  184  184  177  182  180  176  176  180  179
第二次普通并发： 2190 1116 753  560  456  377  324  286  249  227  202  189



````
