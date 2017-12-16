---
title: "无配置文件log4Net"
layout: "post"
categories:
  - 学习
tags:
  - 学习
  - C#
keywords:
  - log4net配置
  - 无配置文件log4net
  - 代码配置log4net
date: "2017-12-16 18:22"
updated: "2017-12-16 18:22"
abbrlink:
comments:
---
## 代码配置log4net

主要简单的项目不想写配置文件，所以研究了一下不需要配置文件下配置long4net写日志。
主要是配置文件写配置直接上代码，也以防我自己忘记记录一下.


```C#
    private  Hierarchy _hierarchy = null;
		private  ILoggerRepository _repostitory =null;
		private PatternLayout _patternLayout = null;
		private Logger _logger = null;

    var repostitorys = LogManager.GetAllRepositories();
			foreach (var loggrepostitory in repostitorys)
			{
				if (!loggrepostitory.Name.Equals(RepositoriesName)) continue;
				_repostitory = loggrepostitory;
				break;
			}
			if (_repostitory == null)
			{
				//1.创建Repositorie2.获取默认
				_repostitory = LogManager.CreateRepository(RepositoriesName);
				//_repostitory = LogManager.GetRepository(Assembly.GetEntryAssembly());
			}
			_hierarchy = (Hierarchy)_repostitory;

      SetPatternLayout();
      RollingFileAppender fileAppender = new RollingFileAppender
			{
				Name = LoggerName,
				Layout = _patternLayout,
				File = base.FileAppenderFile,
				AppendToFile = true,
				MaxSizeRollBackups = 10,
				MaxFileSize = 100,
				RollingStyle = RollingFileAppender.RollingMode.Date,
				DatePattern = "yyyyMMdd"
			};
			fileAppender.AddFilter(SetLevelRangeFilter());
			fileAppender.ActivateOptions();
      _hierarchy.Root.Level = _hierarchy.LevelMap["ALL"];
			_hierarchy.Root.AddAppender(appender);
      BasicConfigurator.Configure(_repostitory);
```
##### 输出格式方法

```
    /// <summary>
		/// 设置layout输出格式
		/// </summary>
		private void SetPatternLayout()
		{

			_patternLayout = new PatternLayout {ConversionPattern = base.ConversionPattern};
			_patternLayout.ActivateOptions();
		}
```
##### 过滤日志级别
```
    /// <summary>
		/// 设置日志过滤输出级别
		/// </summary>
		/// <returns></returns>
		private LevelRangeFilter SetLevelRangeFilter()
		{
			var levelrangefilter = new LevelRangeFilter();
			var map = (_hierarchy ==null)? LogManager.GetRepository().LevelMap:_hierarchy.LevelMap;
			levelrangefilter.LevelMax = map[base.LevelMax];
			levelrangefilter.LevelMin = map[base.LevelMin];
			return levelrangefilter;
		}
```

基本上就这些，如果你还有配置数据库或者其他的参照官网日志配置
官网配置示范[链接](https://logging.apache.org/log4net/release/config-examples.html)
这里放一下我的[日志帮助类](https://github.com/StreetWind/WindRat/blob/master/Wind.Common/log4Net.cs)
