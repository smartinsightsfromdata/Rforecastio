Rforecastio
===========

v1.2.0 update:

I've bumped up the version number of `Rforecastio` to `1.1.1`. The new features are:

- fixed horrible bug in the package code that will finally teach me to clear the environment before testing
- added `...` to the `fio.forecast` function call to let users pass in `ssl.verifypeer=FALSE` and `proxy="host:port"` options (any CURL options, actually). h/t Stefan Fritsch
- using `plyr` for easier conversion of JSON->data frame
- adding in a new `daily` forecast data frame
- roxygen2 inline documentation
- the `minutely` conversion code now handles missing element properly

*********

This is a simple R interface to forecast.io weather data. It uses `RCurl` and `RJSONIO` and `plyr` to fetch and extract the JSON weather/forecast data from http://forecast.io and returns the metadata and readings

- forecast.io API Docs: https://developer.forecast.io/docs/v2
- forecast.io Dev site: https://developer.forecast.io/

Install it from straight from github:

    library("devtools")
    install_github("Rforecastio", "hrbrmstr")


Usage quick start: 

    library(Rforecastio)
    library(ggplot2)
    library(plyr)

    # NEVER put API keys in revision control systems or source code!
    fio.api.key= readLines("~/.forecast.io")

    my.latitude = "43.2673"
    my.longitude = "-70.8618"

    # can add proxy='host:port' and ssl.verifypeer=FALSE to the end of the fio.forecast call
    # if you need to run behind a proxy or still have certiifcate issues
    
    fio.list <- fio.forecast(fio.api.key, my.latitude, my.longitude)

    fio.gg <- ggplot(data=fio.list$hourly.df, aes(x=time, y=temperature))
    fio.gg <- fio.gg + labs(y="Readings", x="Time", title="Houry Readings")
    fio.gg <- fio.gg + geom_line(aes(y=humidity*100), color="green")
    fio.gg <- fio.gg + geom_line(aes(y=temperature), color="red")
    fio.gg <- fio.gg + geom_line(aes(y=dewPoint), color="blue")
    fio.gg <- fio.gg + theme_bw()
    fio.gg

![hourly](/examples/hourly.png)

    fio.gg <- ggplot(data=fio.list$daily.df, aes(x=time, y=temperature))
    fio.gg <- fio.gg + labs(y="Readings", x="Time", title="Daily Readings")
    fio.gg <- fio.gg + geom_line(aes(y=humidity*100), color="green")
    fio.gg <- fio.gg + geom_line(aes(y=temperatureMax), color="red")
    fio.gg <- fio.gg + geom_line(aes(y=temperatureMin), color="red", linetype=2)
    fio.gg <- fio.gg + geom_line(aes(y=dewPoint), color="blue")
    fio.gg <- fio.gg + theme_bw()
    fio.gg
    
![hourly](/examples/daily.png)

Hit: http://rud.is/b/tag/rforecastio/ occassionally to see if the blog has any more info (it will!)
