---
layout: post
title:  "Use AWS Services in Node.js"
date:  2019-04-02
description: ""
img: js.jpg
tags: [js]
---

## AWS S3

### Read all the files (more that 1000 files) from a S3 bucket
From the [document](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#listObjectsV2-property) we can know, if your S3 bucket has thousands of files, the listObjectsV2() function has a limitation, for each call, it only return **up to 1000** objects.

But don't worry, here is the code to solve this limitation:
```javascript
do {
  try {
    await new Promise((resolve, reject) => {
      s3.listObjectsV2({
        ContinuationToken: continuationToken,
        Bucket: process.env.S3BUCKET,
        MaxKeys: 1000
      }, (err, newData) => {
        if (err) return reject(err);

        data = newData;
        continuationToken = data.NextContinuationToken;

        const allFilePath = data.Contents.map((content) => content.Key);
        keyList.push(...allFilePath);
        
        resolve();
      });
    });
  } catch (err) {
    reject(err);
  }
} while (data.IsTruncated);
```