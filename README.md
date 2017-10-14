# TimeNLP
# Time-NLP
#中文语句中的时间语义识别

author：Nagi

由 https://github.com/shinyke/Time-NLP 移植过来，如果有现成的就不用费心移植了，现在iOS可以直接用这个，支持pod

pod 'TimeNLP', '~> 0.1.1'

对应安卓的去上面的JAVA源码拿过去修改一下即可

本工具是由复旦NLP中的时间分析功能修改而来，做了很多细节和功能的优化，具体如下：


<font color=#0099ff>简而言之，这是一个输入一句话，能识别出话里的时间的工具。╮(╯▽╰)╭</fo>

使用方法详见测试类：
``` ObjectC
- (void)viewDidLoad {
[super viewDidLoad];
// Do any additional setup after loading the view, typically from a nib.

[self test:@"下月1号下午3点至5点到图书馆还书"];
[self test:@"6月3  春游"];
[self test:@"17年12月8日到明年1月半"];
[self test:@"去年11月起正式实施的刑法修正案（九）中明确，在法律规定的国家考试中，组织作弊的将入刑定罪，最高可处七年有期徒刑。另外，本月刚刚开始实施的新版《教育法》中也明确"];
[self test:@"《辽宁日报》今日报道，6月3日辽宁召开省委常委扩大会，会议从下午两点半开到六点半，主要议题为：落实中央巡视整改要求。2017-12-07-00-00-00"];
[self test:@"昨天上午，第八轮中美战略与经济对话气候变化问题特别联合会议召开。中国气候变化事务特别代表解振华表示，今年中美两国在应对气候变化多边进程中政策对话的重点任务，是推动《巴黎协定》尽早生效。 2016-06-07-00-00-00"];
[self test:@"周四下午三点到五点开会"];
[self test:@"明天早上跑步"];
[self test:@"6-3 春游"];
[self test:@"6:30 起床"];
[self test:@"下下周一开会"];
[self test:@"礼拜一开会"];
[self test:@"早上六点起床"];
[self test:@"下周一下午三点开会"];
[self test:@"我真的啊 本周日到下周日出差"];
}

- (void)test:(NSString*)content {
TimeNormalizer* normalizer = [TimeNormalizer new];
NSArray<TimeUnit*>* times = [normalizer parse:content];// 抽取时间
NSLog(@"%@\n\n", content);
[times enumerateObjectsUsingBlock:^(TimeUnit * _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
NSLog(@"全天:%@ %@ %@", obj.isAllDayTime?@"是":@"否" , obj.timeExpression, [obj.time ng_fs_stringWithFormat:@"yyyy-MM-dd HH:mm:ss"]);
}];
}
```
<br>

2017-10-14 21:44:19.670847+0800 TimeNLP[5322:878941] 下月1号下午3点至5点到图书馆还书
2017-10-14 21:44:19.671145+0800 TimeNLP[5322:878941] 全天:否 下月1号下午3点 2017-11-01 15:00:00
2017-10-14 21:44:19.671302+0800 TimeNLP[5322:878941] 全天:否 5点 2017-11-01 17:00:00
2017-10-14 21:44:19.688480+0800 TimeNLP[5322:878941] 6月3  春游
2017-10-14 21:44:19.688709+0800 TimeNLP[5322:878941] 全天:是 6月3 2018-06-03 08:00:00
2017-10-14 21:44:19.708527+0800 TimeNLP[5322:878941] 17年12月8日到明年1月半
2017-10-14 21:44:19.708722+0800 TimeNLP[5322:878941] 全天:是 17年12月8日 2017-12-08 08:00:00
2017-10-14 21:44:19.708876+0800 TimeNLP[5322:878941] 全天:是 明年1月 2017-01-14 21:44:19
2017-10-14 21:44:19.746537+0800 TimeNLP[5322:878941] 去年11月起正式实施的刑法修正案（九）中明确，在法律规定的国家考试中，组织作弊的将入刑定罪，最高可处七年有期徒刑。另外，本月刚刚开始实施的新版《教育法》中也明确
2017-10-14 21:44:19.746724+0800 TimeNLP[5322:878941] 全天:是 去年11月 2016-11-14 21:44:19
2017-10-14 21:44:19.746857+0800 TimeNLP[5322:878941] 全天:是 本月 2017-10-14 21:44:19
2017-10-14 21:44:19.791960+0800 TimeNLP[5322:878941] 《辽宁日报》今日报道，6月3日辽宁召开省委常委扩大会，会议从下午两点半开到六点半，主要议题为：落实中央巡视整改要求。2017-12-07-00-00-00
2017-10-14 21:44:19.792417+0800 TimeNLP[5322:878941] 全天:是 今日 2017-10-14 08:00:00
2017-10-14 21:44:19.792723+0800 TimeNLP[5322:878941] 全天:是 6月3日 2017-06-03 08:00:00
2017-10-14 21:44:19.792923+0800 TimeNLP[5322:878941] 全天:否 下午2点半 2017-06-03 14:30:00
2017-10-14 21:44:19.793797+0800 TimeNLP[5322:878941] 全天:否 6点半 2017-06-03 18:30:00
2017-10-14 21:44:19.794073+0800 TimeNLP[5322:878941] 全天:是 2017-12-07 2017-12-07 08:00:00
2017-10-14 21:44:19.858263+0800 TimeNLP[5322:878941] 昨天上午，第八轮中美战略与经济对话气候变化问题特别联合会议召开。中国气候变化事务特别代表解振华表示，今年中美两国在应对气候变化多边进程中政策对话的重点任务，是推动《巴黎协定》尽早生效。 2016-06-07-00-00-00
2017-10-14 21:44:19.858465+0800 TimeNLP[5322:878941] 全天:否 昨天上午 2017-10-13 10:00:00
2017-10-14 21:44:19.858584+0800 TimeNLP[5322:878941] 全天:是 今年 2017-10-14 21:44:19
2017-10-14 21:44:19.858701+0800 TimeNLP[5322:878941] 全天:是 2016-06-07 2018-06-06 08:00:00
2017-10-14 21:44:19.879486+0800 TimeNLP[5322:878941] 周四下午三点到五点开会
2017-10-14 21:44:19.879824+0800 TimeNLP[5322:878941] 全天:否 周4下午3点 2017-10-19 15:00:00
2017-10-14 21:44:19.880456+0800 TimeNLP[5322:878941] 全天:否 5点 2017-10-19 17:00:00
2017-10-14 21:44:19.896510+0800 TimeNLP[5322:878941] 明天早上跑步
2017-10-14 21:44:19.896790+0800 TimeNLP[5322:878941] 全天:否 明天早上 2017-10-15 08:00:00
2017-10-14 21:44:19.916051+0800 TimeNLP[5322:878941] 6-3 春游
2017-10-14 21:44:19.916286+0800 TimeNLP[5322:878941] 全天:是 6-3 2018-06-03 08:00:00
2017-10-14 21:44:19.929403+0800 TimeNLP[5322:878941] 6:30 起床
2017-10-14 21:44:19.929649+0800 TimeNLP[5322:878941] 全天:否 6:30 2017-11-15 06:30:00
2017-10-14 21:44:19.941842+0800 TimeNLP[5322:878941] 下下周一开会
2017-10-14 21:44:19.942032+0800 TimeNLP[5322:878941] 全天:是 下下周1 2017-10-23 08:00:00
2017-10-14 21:44:19.953874+0800 TimeNLP[5322:878941] 礼拜一开会
2017-10-14 21:44:19.954065+0800 TimeNLP[5322:878941] 全天:是 礼拜1 2017-10-16 08:00:00
2017-10-14 21:44:19.966870+0800 TimeNLP[5322:878941] 早上六点起床
2017-10-14 21:44:19.967131+0800 TimeNLP[5322:878941] 全天:否 早上6点 2017-11-15 06:00:00
2017-10-14 21:44:19.981731+0800 TimeNLP[5322:878941] 下周一下午三点开会
2017-10-14 21:44:19.981989+0800 TimeNLP[5322:878941] 全天:否 下周1下午3点 2017-10-16 15:00:00
2017-10-14 21:44:20.001429+0800 TimeNLP[5322:878941] 我真的啊 本周日到下周日出差
2017-10-14 21:44:20.001655+0800 TimeNLP[5322:878941] 全天:是 本周7 2017-10-15 08:00:00
2017-10-14 21:44:20.001913+0800 TimeNLP[5322:878941] 全天:是 下周7 2017-10-22 08:00:00

<br/>
<br/>
如果您使用并有意见和建议，欢迎在Issue和我交流。若觉得好用，你的star是对作者最好的支持。<br/>
