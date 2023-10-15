---
title: 定时任务之@Scheduled注解
tags:
  - 定时任务
  - java
  - Scheduled注解
categories:
  - Scheduled注解
  - 定时任务
top_img: /img/dingshi.jpg
cover: /img/dingshi.jpg
keywords: 定时任务
description: 定时任务之@Scheduled注解
post_meta:
  page:
    date_type: both
    date_format: relative
    categories: true
    tags: true
    label: true
  post:
    date_type: both
    date_format: relative
    categories: true
    tags: true
    label: true
abbrlink: ce3886d7
date: 2023-10-14 20:50:18
---
# 一、注解介绍
 @Scheduled注解是Spring Boot提供的用于定时任务控制的注解，主要用于控制任务在某个指定时间执行，或者每隔一段时间执行。
 
#  二、注解参数
## 2.1. 参数简介


| 参数 | 说明 | 示例 |
| --- | --- | --- |
|cron |	 任务执行的cron表达式 |	见下文 |
|zone |	cron表达时解析使用的时区，默认为服务器的本地时区。使用java.util.TimeZone#getTimeZone(String)方法解析 |	GMT-8:00 |
| fixedRate |	固定速率。上一次任务执行开始到下一次执行开始的间隔时间固定，单位为ms。若在调度任务执行时,上一次任务还未执行完毕,会加入worker队列,等待上一次执行完成后，马上执行下一次任务|	1000
| fixedRateString |	与fixedRate一致，只是间隔时间使用java.time.Duration#parse解析 |	1000或PT1S |
| fixedDelay |	 固定延迟。上一次任务执行结束到下一次执行开始的间隔时间固定，单位为ms。|	1000 |
| fixedDelayString | 与fixedDelay一致，只是间隔时间使用java.time.Duration#parse解析 |	1000或PT1S |
| initialDelay |	首次延迟多长时间后执行，单位ms。之后按照fixedRate、fixedRateString、fixedDelay、fixedDelayString指定的规则执行，需要指定其中一个规则。注意：不能和cron一起使用  |	1000 |
| initialDelayString |	 与initialDelay 一致，只是间隔时间使用java.time.Duration#parse解析  |	 1000或PT1S |

## 2.2. 主要参数说明
 注意，@Scheduled需要配合@EnableScheduling使用。使用时，将@Scheduled注解放在待定时的方法名上方，将@EnableScheduling放在项目主启动类类名上方。@Scheduled主要有三种配置执行时间的方式：cron、fixedRate和fixedDelay。

### 2.2.1. cron表达式
cron是@Scheduled的一个参数，是一个字符串，以空格隔开。

```
cron表达式格式：
 
@Scheduled(cron = "{秒数} {分钟} {小时} {日期} {月份} {星期}")
 
cron表达式示例：
 
@Scheduled(cron = "0 00 07 * * *")
```
cron表达式可分为6或7个占位符，但在spring自带的定时任务中，cron只支持6个参数（详情请参考2.3的源码），若使用7个参数就会报错，示例如下：

```
@Scheduled(cron = "0/3 * * * * ? 2022-2023")
public void test(){
 
    logger.info("输出测试！");
 
}
 
 
org.springframework.beans.factory.BeanCreationException: 
Error creating bean with name '类名' defined in file 
[编译后class文件路径]: Initialization of bean failed; 
 
nested exception is java.lang.IllegalStateException: 
Encountered invalid @Scheduled method 'test': Cron expression 
must consist of 6 fields (found 7 in "0/3 * * * * ? 2022-2023")
```
各参数介绍：

| 单位 | 允许值 | 允许通配符 |
| --- | --- | --- |
| 毫秒 | 	0-59 | 	,  -  *  /  | 
| 分钟 | 	0-59 | 	,  -  *  / | 
| 小时 | 	0-23 |  	,  -  *  /  | 
| 日期 | 	1-31 | 	,  -  *  /  ?  L  W | 
| 月份 | 	1-12或JAN-DEC(大小写均可) | 	,  -  *  /  ? | 
| 星期 | 	1-7或SUN-SAT(大小写均可) | 注：星期日为每周第一天，所以1-7表示周末到周六 ,  -  *  /  ?  L  # | 

cron表达式各占位符解释：

```
{秒数}{分钟}{小时}{日期}{星期} ==> 不允许为空值，若值不合法，调度器将抛出SchedulerException异常
 
“/”代表触发步进(step)，”/”前面的值代表初始值(""等同"0")，后面的值代表偏移量，比如
 
"0/20"或者"/20"代表从0秒钟开始，每隔20秒钟触发1次，即第0秒触发1次，第20秒触发1次，第40秒触发1次；
 
"5/20"代表第5秒触发1次，第25秒触发1次，第45秒触发1次；
 
"10-45/20"代表在[10,45]内步进20秒命中的时间点触发，即第10秒触发1次，第30秒触发1次
 
```
### 2.2.2. cron通配符

| 符号 | 含义 |
| --- | --- |
|* |	所有值,在秒字段上表示每秒执行,在月字段上表示每月执行 |
|？	| 不指定值，不需要关心当前指定的字段的值，比如每天都执行但不需要关心周几就可以把周的字段设为? |
|- |	区间或者范围,如秒的0-2 ,表示0秒、1秒、2秒都会触发 |
|, |	指定值，比如在0秒、20秒、25秒触发,可以把秒的字段设为0,20,25 |
|/ |	递增触发,比如秒的字段上设0/3 ,表示从第0秒开始,每隔3秒触发 |
|L |	最后，只允许在日字段或周字段上,在日字段上使用L表示当月最后- -天,在周字段上使用3L表示该月最后一个周四 |
|W |	只允许用在日字段上,表示距离最近的该日的工作日,工作日指的是周一至周五 |
|# |	只允许在周字段上，表示每月的第几个周几，如2#3 , 每月的第3个周二 |


### 2.2.3. cron表达式示例
@Scheduled(cron = "* * * * * *")示例：*每秒执行


@Scheduled(cron = "10-45/20 * * * * *")示例：-和/占位符的组合使用


@Scheduled(cron = "*/5 * * * * *")示例：*和/的使用，程序启动后，每5秒执行一次

@Scheduled(cron = "25/5 * * * * *")示例：与上述示例对比，从25秒起，每5秒执行一次

其他示例：

```
“30 * * * * ?” 每半分钟触发任务 
 
“30 10 * * * ?” 每小时的10分30秒触发任务 
 
“30 10 1 * * ?” 每天1点10分30秒触发任务 
 
“30 10 1 20 * ?” 每月20号1点10分30秒触发任务 
 
“30 10 1 20 10 ? *” 每年10月20号1点10分30秒触发任务 
 
“30 10 1 20 10 ? 2011” 2011年10月20号1点10分30秒触发任务 
 
“30 10 1 ? 10 * 2011” 2011年10月每天1点10分30秒触发任务 
 
“30 10 1 ? 10 SUN 2011” 2011年10月每周日1点10分30秒触发任务 
 
“15,30,45 * * * * ?” 每15秒，30秒，45秒时触发任务 
 
“15-45 * * * * ?” 15到45秒内，每秒都触发任务 
 
“15/5 * * * * ?” 每分钟的每15秒开始触发，每隔5秒触发一次 
 
“15-30/5 * * * * ?” 每分钟的15秒到30秒之间开始触发，每隔5秒触发一次 
 
“0 0/3 * * * ?” 每小时的第0分0秒开始，每三分钟触发一次 
 
“0 15 10 ? * MON-FRI” 星期一到星期五的10点15分0秒触发任务 
 
“0 15 10 L * ?” 每个月最后一天的10点15分0秒触发任务 
 
“0 15 10 LW * ?” 每个月最后一个工作日的10点15分0秒触发任务 
 
“0 15 10 ? * 5L” 每个月最后一个星期四的10点15分0秒触发任务 
 
“0 15 10 ? * 5#3” 每个月第三周的星期四的10点15分0秒触发任务
```

### 2.2.4. cron表达式参数数量解疑

```
cron表达式格式：在此，表达式以空格分为6或7个域
 
@Scheduled(cron = "{秒数} {分钟} {小时} {日期} {月份} {星期} {年份(可为空)}")
 
{年份} ==> 允许值范围: 1970~2099 ,允许为空，若值不合法，调度器将抛出SchedulerException异常
 
注意：除了{日期}和{星期}可以使用”?”来实现互斥，表达无意义的信息之外，其他占位符都要具有具体的时间含义，且依赖关系为：年->月->日期(星期)->小时->分钟->秒数
```

## 2.3.源码

```
@Target({ElementType.METHOD, ElementType.ANNOTATION_TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Repeatable(Schedules.class)
public @interface Scheduled {
 
	/**
	 * A special cron expression value that indicates a disabled trigger: {@value}.
	 * 一个特殊的cron表达式值，指示禁用的触发器：｛@value｝。
	 * <p>This is primarily meant for use with <code>${...}</code> placeholders,
	 * allowing for external disabling of corresponding scheduled methods.
	 * @since 5.1
	 * @see ScheduledTaskRegistrar#CRON_DISABLED
	 */
	String CRON_DISABLED = ScheduledTaskRegistrar.CRON_DISABLED;
 
 
	/**
	 * A cron-like expression, extending the usual UN*X definition to include triggers
	 * on the second, minute, hour, day of month, month, and day of week.
	 * 类似cron的表达式，将通常的UN*X定义扩展为包括触发器在秒、分钟、小时、天、月份和星期几。
	 * 
	 * <p>For example, {@code "0 * * * * MON-FRI"} means once per minute on weekdays
	 * (at the top of the minute - the 0th second).
	 * <p>The fields read from left to right are interpreted as follows.
	 * 从左到右读取的字段解释如下。
	 * <ul>
	 * <li>second</li>
	 * <li>minute</li>
	 * <li>hour</li>
	 * <li>day of month</li>
	 * <li>month</li>
	 * <li>day of week</li>
	 * </ul>
	 * <p>The special value {@link #CRON_DISABLED "-"} indicates a disabled cron
	 * trigger, primarily meant for externally specified values resolved by a
	 * <code>${...}</code> placeholder.
	 * @return an expression that can be parsed to a cron schedule
	 * @see org.springframework.scheduling.support.CronExpression#parse(String)
	 */
	String cron() default "";
 
	/**
	 * A time zone for which the cron expression will be resolved. 
	 * 将解析cron表达式的时区。
	 * By default, this attribute is the empty String (i.e. the server's local time zone will be used).
	 * 默认情况下，此属性为空字符串（即将使用服务器的本地时区）。
	 * @return a zone id accepted by {@link java.util.TimeZone#getTimeZone(String)},
	 * or an empty String to indicate the server's default time zone
	 * @since 4.0
	 * @see org.springframework.scheduling.support.CronTrigger#CronTrigger(String, java.util.TimeZone)
	 * @see java.util.TimeZone
	 */
	String zone() default "";
 
	/**
	 * Execute the annotated method with a fixed period in milliseconds between the
	 * end of the last invocation and the start of the next.
	 * 在上一次调用结束和下一次调用开始之间以毫秒为单位的固定周期内执行带注释的方法。
	 * 
	 * @return the delay in milliseconds
	 */
	long fixedDelay() default -1;
 
	/**
	 * Execute the annotated method with a fixed period in milliseconds between the
	 * end of the last invocation and the start of the next.
	 * 在上一次调用结束和下一次调用开始之间以毫秒为单位的固定周期内执行带注释的方法。
	 * 
	 * @return the delay in milliseconds as a String value, e.g. a placeholder
	 * or a {@link java.time.Duration#parse java.time.Duration} compliant value
	 * @since 3.2.2
	 */
	String fixedDelayString() default "";
 
	/**
	 * Execute the annotated method with a fixed period in milliseconds between
	 * invocations.
	 * 在两次调用之间以毫秒为单位执行带注释的方法。
	 *
	 * @return the period in milliseconds
	 */
	long fixedRate() default -1;
 
	/**
	 * Execute the annotated method with a fixed period in milliseconds between
	 * invocations.
	 * 在两次调用之间以毫秒为单位执行带注释的方法。
	 *
	 * @return the period in milliseconds as a String value, e.g. a placeholder
	 * or a {@link java.time.Duration#parse java.time.Duration} compliant value
	 * @since 3.2.2
	 */
	String fixedRateString() default "";
 
	/**
	 * Number of milliseconds to delay before the first execution of a
	 * {@link #fixedRate} or {@link #fixedDelay} task.
	 * 首次执行｛@link#fixedRate｝或｛@link#fixedDelay｝任务之前要延迟的毫秒数。
	 *
	 * @return the initial delay in milliseconds
	 * @since 3.2
	 */
	long initialDelay() default -1;
 
	/**
	 * Number of milliseconds to delay before the first execution of a
	 * {@link #fixedRate} or {@link #fixedDelay} task.
	 * 首次执行｛@link#fixedRate｝或｛@link#fixedDelay｝任务之前要延迟的毫秒数。
	 *
	 * @return the initial delay in milliseconds as a String value, e.g. a placeholder
	 * or a {@link java.time.Duration#parse java.time.Duration} compliant value
	 * @since 3.2.2
	 */
	String initialDelayString() default "";
 
}
```

# 三、cron生成
在[在线Cron表达式生成器](https://cron.qqe2.com/)可以生成Cron表达式，同时，也可以反解析。还有一些程序员常用的工具。