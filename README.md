# node-scrapeurl
A simple nodejs web scraper util that is more capable of scraping sites with anti-scraper functionality.
# introduction
`scrapeurl` is a web scrape util base on `superagent` to make requests and `iconv-lite` to support more charset of web pages. What `scrapeurl` does is to do the `useragent`-switching-like things under the hook, and express you a simple api. `scrapeurl` makes web scraping effective and harder be detected.

# installation
`npm install scrapeurl`

# features
- Do useragent switching, ip switching(change 'X_FORWARDED_FOR' header),referer switching and so on under the hook. Make scraping harder be detected.
- Charset support. Scrape `charset=gbk` like web pages easier.

# usage
you use scrapeurl as below:
`scrapeurl(config,callback).done(doneCallback).fail(failCallback)`
-  `config` is an object as:
```js
{
    url:'your url', //require
    delay:1000,//optional defaults to 0, delay by millisecond,
    retry:3,//optional defaults to 0, retry times if error when scraping the url
    charset:'utf8',//optional defaults to null
    userAgent:'custom useragent',//optional defaults to random agent generate by scrapeurl
    ip:'custom ip',//optional defaults to random ip generate by scrapeurl
    referer:'custom referer',//optional defaults to random referer generate by scrapeurl
}
```
 `scrapeurl` use `iconv-lite` to do the `charset` conversion, all supported encoding can be found in [iconv-lite](https://github.com/ashtuchkin/iconv-lite)

- `callback` will receive two auguments as `callback(err,response)`.`scrapeurl` is based on `superagent`, and the `err` and `respone` are coming from `superagent`,you may refer to [superagent](https://visionmedia.github.io/superagent/) for more details.

- `doneCallback` will be called when the scraping succeeds as `doneCallback(response)`

- `failCallback` will be called when the scraping fails as `failCallback(err,response)`

### example

```js
 const scrapeurl = require('scrapeurl')
 scrapeurl({
    url:'your url'
    },function(err,response){
        //do your thing with the response
    })
```

you may also use it with  `done` and `fail` :

```js
 const scrapeurl = require('scrapeurl')
 scrapeurl({
    url:'your url'
    }).done(function(response){
        //success code
    }).fail(function(err,response){
        //fail code
    })
```

### notes
Most of the time, you don't have to specify  `charset` in the `config`, but when you are scraping a Chinese web page with `charset=gb2312` or `charset=gbk` in the meta tag, if you don't specify `charset:'gbk'` in the config, the response text you scrapes will mess up with the wrong encoding.
Since `scrapeurl` use the `parse` api of `superagent` to do the `charset` job,if you specify `charset` in the config, the `reponse.body` will not be available.
# dependencies
[superagent](https://github.com/visionmedia/superagent)
[iconv-lite](https://github.com/ashtuchkin/iconv-lite)

