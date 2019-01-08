---
layout: post
title:  "Download the logs of AWS Lambda"
date:  2019-01-02
description: ""
img: aws.png
tags: [aws]
---

When we view logs at CloudWatch, the log streams are not in a single file. Sometimes it makes us a little trouble if we want to search something from the logs but we don't know the exact time of the log, we need to open the log files one by one to do that.
If we can download all the files to local machine, then it will be much easier.
Here are the ways to download all the log files. We can do that with two steps:

## Download
### Step 1: Export all logs to S3
View the logs at CloudWatch, and back to the parent level: Log Groups:
![log-groups](/assets/img/2019-01-02-download-lambda-logs/log-groups.png)

Select the stream by Lambda name which you want do download:
![export-to-s3](/assets/img/2019-01-02-download-lambda-logs/export-to-s3.png)

Make sure your S3 has the correct permission, uncheck those two checkboxes:
![](/assets/img/2019-01-02-download-lambda-logs/acl.png)

### Step 2: Download logs from S3
```
aws s3 sync s3://my-bucket /some/local/directory
```

## Unzip `*.gz` logs
By default, the log files are compressed, use `gunzip` can decompose it (macOS).

```bash
gunzip 000000.gz
```

Use the following script to decompose multiple log files:

run.sh:
```bash
#!/bin/bash
for D in $1/*; do
    if [ -d "${D}" ]; then
      # echo "${D}"
        gunzip "${D}/000000.gz"
    fi
done
```

Run it with directory as parameter:
```
./run.sh /directory
```

## References
[https://stackoverflow.com/a/45969853/2195426](https://stackoverflow.com/a/45969853/2195426)
