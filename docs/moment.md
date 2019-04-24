# moment
## 安装
npm install moment --save   # npm
yarn add moment 

## 常用
```
moment().startOf(String);
//moment().startOf('year');    // set to January 1st, 12:00 am this year
//moment().startOf('month');   // set to the first of this month, 12:00 am
//moment().startOf('quarter');  // set to the beginning of the current quarter, 1st day of months, 12:00 am
//moment().startOf('week');    // set to the first day of this week, 12:00 am
//moment().startOf('isoWeek'); // set to the first day of this week according to ISO 8601, 12:00 am
//moment().startOf('day');     // set to 12:00 am today
//moment().startOf('hour');    // set to now, but with 0 mins, 0 secs, and 0 ms
//moment().startOf('minute');  // set to now, but with 0 seconds and 0 milliseconds
//moment().startOf('second');  // same as moment().milliseconds(0);

moment().endOf(String);

moment().isAfter(Moment|String|Number|Date|Array);
moment().isAfter(Moment|String|Number|Date|Array, String);
//moment('2010-10-20').isAfter('2010-10-19'); // true

moment().isBetween(moment-like, moment-like);
moment().isBetween(moment-like, moment-like, String);
//moment('2010-10-20').isBetween('2010-10-19', '2010-10-25'); // true

moment().isSame(Moment|String|Number|Date|Array);
moment().isSame(Moment|String|Number|Date|Array, String);
//moment('2010-10-20').isSame('2010-10-20'); // true

```

## 日期格式化
```
moment().format('MMMM Do YYYY, h:mm:ss a'); // 四月 24日 2019, 2:54:26 下午
moment().format('dddd');                    // 星期三
moment().format("MMM Do YY");               // 4月 24日 19
moment().format('YYYY [escaped] YYYY');     // 2019 escaped 2019
moment().format();                          // 2019-04-24T14:54:26+08:00
```
## 相对时间
```
moment("20111031", "YYYYMMDD").fromNow(); // 7 年前
moment("20120620", "YYYYMMDD").fromNow(); // 7 年前
moment().startOf('day').fromNow();        // 15 小时前
moment().endOf('day').fromNow();          // 9 小时内
moment().startOf('hour').fromNow();       // 1 小时前
```
## 日历时间
```
moment().subtract(10, 'days').calendar(); // 2019年4月14日
moment().subtract(6, 'days').calendar();  // 上周四下午2点55
moment().subtract(3, 'days').calendar();  // 上周日下午2点55
moment().subtract(1, 'days').calendar();  // 昨天下午2点55分
moment().calendar();                      // 今天下午2点55分
moment().add(1, 'days').calendar();       // 明天下午2点55分
moment().add(3, 'days').calendar();       // 本周六下午2点55
moment().add(10, 'days').calendar();      // 2019年5月4日

```


