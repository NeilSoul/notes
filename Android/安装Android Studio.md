[Android Studio](http://developer.android.com/sdk/index.html) is an android development SDK.

## install android studio

## install android sdk manager

* vi `/etc/hosts` to speed

```sh
# add this comment into hosts file
203.208.46.146 dl.google.com
203.208.46.146 dl-ssl.google.com
```

* open sdk manager -> preference
```
HTTP Proxy Server: mirrors.neusoft.edu.cn
HTTP Proxy Port: 80
select "force use http:..."
```

## first time create project

it will cost much time to build gradle project info

### solved

1. when it building gradle file , force quit;
2. cd folder `~\.gradle\wrapper\dists\`;
3. from <http://www.gradle.org/downloads> download correspond gradle.all zip file;
4. put into some child folder name such as messy code `6vpvhqu0efs1fqmqr2decq1v12`;
