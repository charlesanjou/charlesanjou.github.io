---
layout:     post
title:     FCC jquery获取天气数据(待完善)
subtitle:   (如何调用getJSON,使用和风天气接口)
date:       2019-08-18
author:     xieerwa
header-img: img/post-bg-rwd.png
catalog: true
tags:
    - 前端  
    - javascript
    - jquery
---

### 申请和风天气的接口

[官方连接](https://www.heweather.com/ ''和风天气api)

### 编写js

```js
// 在页面中引用jquery
var city = '';
    var update = '';
    var citynowweather = '';
    var yesterdayweather = '';
    var Todayweather = '';
    var tomorrowweather = '';
    var Todaylifestyle = '';
    var tomorrowlifestyle = '';
    $(document).ready(function() {
        var url = "https://free-api.heweather.net/s6/weather?location=beijing&key=xx";
        $.getJSON(url, function(weather) {
            city = weather.HeWeather6[0].basic.location;
           	update = weather.HeWeather6[0].update.loc;
            citynowweather = weather.HeWeather6[0].now.tmp;
            yesterdayweather = weather.HeWeather6[0].daily_forecast[1].tmp_max+"-"+weather.HeWeather6[0].daily_forecast[1].tmp_min;
        	$('.city-wrap').text(city);
        	$('.city-weather').text(citynowweather);
        });
    });
```

