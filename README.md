[简体中文](./README.md)|[English](./README_EN.md)

# sqlreplayer是什么
sqlreplayer能从mysql的general log，slow log以及csv文件获取raw sql，并在多个支持mysql协议的数据库上回放，得到sql执行的统计分析报告。

analyze部分支持 mysql 5.6,5.7,8.0下的general log，slow log

# 为什么需要sqlreplayer

这个工具使用的初衷是需要比较业务sql在多个数据库下的性能差异，并生成简单比对结果。
主要涉及到两个部分，一个是sql采集，工具支持从mysql的全量日志、慢日志、csv文件进行raw sql的采集，汇总。另一个sql回放，sql回放支持在多个数据源对raw sql进行回放，并得到比对结果。


# 三种模式

analyze：采集日志的raw sql，也可以对这部分raw sql按照sqlid进行聚合，给出建议统计报告  
replay：将指定的raw sql在多个数据源上回放，给出在多个数据源执行结果比对  
both：analyze和replay的结合  


## analyze 

analyze部分能够从mysql的全量日志，慢日志以及csv文件中获取raw sql，并以csv格式的文件输出，同时可以生成解析报告。

<div align="center">
  <div style="max-width: 1000px; margin: 20px auto; padding: 20px; box-shadow: 0 0 10px rgba(0,0,0,0.1); border-radius: 8px; background-color: #fff;">
    <img src="example/analyze_report_p1.png" alt="analyze统计结果" style="width: 100%; height: auto; display: block; margin: 0 auto;">
    <p style="margin-top: 15px; color: #666; font-size: 14px; font-style: italic;">
            SQL分析报告示例 Part 1 - 展示了SQL类型分布和表连接数量统计
    </p>
  </div>
</div>

<div align="center">
  <div style="max-width: 1000px; margin: 20px auto; padding: 20px; box-shadow: 0 0 10px rgba(0,0,0,0.1); border-radius: 8px; background-color: #fff;">
    <img src="example/analyze_report_p2.png" alt="analyze统计结果" style="width: 100%; height: auto; display: block; margin: 0 auto;">
    <p style="margin-top: 15px; color: #666; font-size: 14px; font-style: italic;">
            SQL分析报告示例 Part 2 - 展示SQL类型，执行次数
    </p>
  </div>
</div>

<div align="center">
  <div style="max-width: 1000px; margin: 20px auto;">
    <a href="https://luijianfie.github.io/sqlreplayer/example/rawsql_analyze_report.html" style="display: inline-block; padding: 12px 24px; background-color: #007bff; color: white; text-decoration: none; border-radius: 4px; font-weight: bold; transition: background-color 0.3s;">
      👉 查看完整分析报告
    </a>
  </div>
</div>


## replay 

replay对raw sql进行的回放，比如下面命令行讲raw sql在ip1:port1和ip2:port2两个数据源上进行回放，以此来比较性能差异。同时生成回放报告。


<div align="center">
  <div style="max-width: 1000px; margin: 20px auto; padding: 20px; box-shadow: 0 0 10px rgba(0,0,0,0.1); border-radius: 8px; background-color: #fff;">
    <img src="example/replay_report_p1.png" alt="analyze统计结果" style="width: 100%; height: auto; display: block; margin: 0 auto;">
    <p style="margin-top: 15px; color: #666; font-size: 14px; font-style: italic;">
            SQL回放报告示例 Part 1 - 展示了SQL回放基本统计信息，数据源的回放概况
    </p>
  </div>
</div>


<div align="center">
  <div style="max-width: 1000px; margin: 20px auto; padding: 20px; box-shadow: 0 0 10px rgba(0,0,0,0.1); border-radius: 8px; background-color: #fff;">
    <img src="example/replay_report_p2.png" alt="analyze统计结果" style="width: 100%; height: auto; display: block; margin: 0 auto;">
    <p style="margin-top: 15px; color: #666; font-size: 14px; font-style: italic;">
            SQL回放报告示例 Part 2 - 展示SQL回放的情况，各个数据源的执行情况，响应时间比较等
    </p>
  </div>
</div>


<div align="center">
  <div style="max-width: 1000px; margin: 20px auto;">
    <a href="https://luijianfie.github.io/sqlreplayer/example/replay_stats.html" style="display: inline-block; padding: 12px 24px; background-color: #007bff; color: white; text-decoration: none; border-radius: 4px; font-weight: bold; transition: background-color 0.3s;">
      👉 查看完整回放报告
    </a>
  </div>
</div>


## both

both模式是analyze和replay阶段结合，从日志采集到raw sql之后直接在配置的数据源下进行回放。  






# 快速开始

cd cmd  
make  
./sqlreplayer -config config_analyze_demo.yaml  
> Using configuration file: config_analyze_demo.yaml  
2025-03-06T17:18:06.335 [info]  sqlreplayer/sqlreplayer.go:492  worker 3 start.  
2025-03-06T17:18:06.335 [info]  sqlreplayer/sqlreplayer.go:492  worker 1 start.  
2025-03-06T17:18:06.335 [info]  sqlreplayer/sqlreplayer.go:492  worker 2 start.  
2025-03-06T17:18:06.335 [info]  sqlreplayer/sqlreplayer.go:722  begin to analyze general_sample.log from pos 0  
2025-03-06T17:18:06.335 [info]  sqlreplayer/sqlreplayer.go:502  worker 2 exit.  
2025-03-06T17:18:06.335 [info]  sqlreplayer/sqlreplayer.go:492  worker 0 start.  
2025-03-06T17:18:06.335 [info]  sqlreplayer/sqlreplayer.go:502  worker 0 exit.  
2025-03-06T17:18:06.335 [info]  sqlreplayer/sqlreplayer.go:722  begin to analyze general_sample_2.log from pos 0  
2025-03-06T17:18:06.340 [info]  sqlreplayer/sqlreplayer.go:539  finish parse GENLOG general_sample_2.log  
2025-03-06T17:18:06.340 [info]  sqlreplayer/sqlreplayer.go:502  worker 1 exit.  
2025-03-06T17:18:06.340 [info]  sqlreplayer/sqlreplayer.go:539  finish parse GENLOG general_sample.log  
2025-03-06T17:18:06.340 [info]  sqlreplayer/sqlreplayer.go:502  worker 3 exit.  
2025-03-06T17:18:06.346 [info]  sqlreplayer/sqlreplayer.go:1484 Raw SQL report saved to ./test/sqlreplayer_task_20250306171806/rawsql_analyze_report.html  
2025-03-06T17:18:06.346 [info]  sqlreplayer/sqlreplayer.go:595  task finished.  
2025-03-06T17:18:06.346 [info]  sqlreplayer/sqlreplayer.go:606  Memory statistic   
2025-03-06T17:18:06.346 [info]  sqlreplayer/sqlreplayer.go:607  Allocated Memory: 1655 KB  
2025-03-06T17:18:06.346 [info]  sqlreplayer/sqlreplayer.go:608  Total Allocated Memory: 4488 KB  
2025-03-06T17:18:06.346 [info]  sqlreplayer/sqlreplayer.go:609  Heap Memory: 1655 KB  
2025-03-06T17:18:06.346 [info]  sqlreplayer/sqlreplayer.go:610  Heap Memory System: 7648 KB  
2025-03-06T17:18:06.346 [info]  sqlreplayer/sqlreplayer.go:611  MaxHeapAlloc: 3128 KB  
2025-03-06T17:18:06.346 [info]  sqlreplayer/sqlreplayer.go:613  exit.  

更多用法可以参考demo.yaml文件的配置


# 参数说明

  执行类型
  -exec string  
        exec type [analyze|replay|both]  
        analyze:generate raw sql from log file.  
        replay:replay raw sql in connections.  

## analyze

分析日志时候加入时间条件  
  -begin string  
        filter sql according to specified begin time from log,format 2023-01-01 13:01:01 (default "0000-01-01 00:00:00")  
  -end string  
        filter sql according to specified end time from log,format 2023-01-01 13:01:01 (default "9999-12-31 23:59:59")  

分析日志路径  
  -filelist string  
        filename,multiple file seperated by ','  

日志格式  
  -logtype string  
        log type [genlog|slowlog|csv]  

生成raw sql的报告，按照sqlid进行汇总，主要是针对slowlog带有运行时间的日志格式，其他格式只能进行汇总，统计次数  
  -generate-report  
        generate report for analyze phrase  

生成报告是否保存raw sql信息
  -save-raw-sql  
        save raw sql in report  





## replay
连接charset  
 -charset string  
        charset of connection (default "utf8mb4")  

数据库连接  
 -conn string  
        mysql connection string,support multiple connections seperated by ',' which can be used for comparation,format   user1:passwd1:ip1:port1:db1[,user2:passwd2:ip2:port2:db2]  

回放文件  
  -filelist string  
        filename,multiple file seperated by ','  

回放倍数  
  -m int  
        number of times a raw sql to be executed while replaying (default 1)  

只回访查询语句  
  -sql-mode
        replay statement [query|dml|ddl|all], moer than one type can be specified by comma, for example query,ddl,default:query

并发数  
  -threads int  
        thread num while replaying (default 1)  

replay报告是否保存raw sql信息
  -save-raw-sql  
        save raw sql in report  


# 联系我

如果您在使用过程中有任何问题或建议，欢迎通过以下方式与我联系：

- 📱 **微信**: `418901779`
- 📧 **邮箱**: [luwenhaoterry@163.com](mailto:luwenhaoterry@163.com)
- 💬 **问题反馈**: 欢迎在 [GitHub Issues](https://github.com/luijianfie/sqlreplayer/issues) 提交问题
- 🤝 **贡献代码**: 欢迎提交 Pull Request 来改进这个项目

您的反馈对于改进 sqlreplayer 非常重要！
