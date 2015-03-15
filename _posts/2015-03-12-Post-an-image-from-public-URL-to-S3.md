---
layout: default
title: Post an image from public URL to S3
---

S3 has a javascript API. It works on the browser and on node.js. Ussually the javascript is just used for getting objects from buckets. in 2012 CORS support was added to use it to place objects onto S3. [CORS](http://en.wikipedia.org/wiki/Cross-Origin_Resource_Sharing) support must be added to your bucket to allow a user to put an object on a bucket. 

In the S3 console I added the following CORS configuration:

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
    <CORSRule>
        <AllowedOrigin>*</AllowedOrigin>
        <AllowedMethod>HEAD</AllowedMethod>
        <AllowedMethod>GET</AllowedMethod>
        <AllowedMethod>PUT</AllowedMethod>
        <AllowedMethod>POST</AllowedMethod>
        <AllowedMethod>DELETE</AllowedMethod>
        <MaxAgeSeconds>3000</MaxAgeSeconds>
        <AllowedHeader>*</AllowedHeader>
    </CORSRule>
</CORSConfiguration>
{% endhighlight %}

Then the simplest Javascript I could find using the AWS javascript SDK was this:

{% highlight javascript %}
// This sets up AWS
// See the Configuring section to configure credentials in the SDK
AWS.config.update({
	accessKeyId: '[Key ID]',
	secretAccessKey: '[Secret Access Key]'
});
// Configure your region
AWS.config.region = 'cn-north-1';

//variable
var imageURL = 'https://avatars3.githubusercontent.com/u/195169?v=3&s=50';
var fileNameForS3 = 'filename';

// This is the upload to S3 part
var img = new Image();
img.onload = function () {
	var imageName = fileNameForS3 + "." + img.contentType;
	var params = {
	  Bucket: 'shazart.images', /* required */
	  Key: imageName, /* required */
	  ACL: 'public-read',
	  Body:img.data,
	  ContentType:img.contentType
	};
	new AWS.S3().putObject(params, function(err, data) {
	  if (err) console.log(err, err.stack); // an error occurred
	  else     console.log("successful upload, got this Data:",data); // successful response
	});
};
img.src=imageURL;
{% endhighlight %}

[The stackoverflow question that really taught me how to do this](http://stackoverflow.com/questions/11240127/uploading-image-to-amazon-s3-with-html-javascript-jquery-with-ajax-request-n#)



* [http://docs.aws.amazon.com/AmazonS3/latest/dev/HTTPPOSTExamples.html](http://docs.aws.amazon.com/AmazonS3/latest/dev/HTTPPOSTExamples.html)
* [https://docs.amazonaws.cn/en_us/AmazonS3/latest/dev/cors.html#how-do-i-enable-cors](https://docs.amazonaws.cn/en_us/AmazonS3/latest/dev/cors.html#how-do-i-enable-cors)
* [http://docs.aws.amazon.com/AWSJavaScriptSDK/guide/browser-making-requests.html](http://docs.aws.amazon.com/AWSJavaScriptSDK/guide/browser-making-requests.html)
* [http://docs.amazonaws.cn/en_us/AWSJavaScriptSDK/guide/browser-configuring.html](http://docs.amazonaws.cn/en_us/AWSJavaScriptSDK/guide/browser-configuring.html)
* [http://aws.amazon.com/articles/1434](http://aws.amazon.com/articles/1434)
* [Relearning the Javascript to make an image](http://www.techrepublic.com/article/preloading-and-the-javascript-image-object/)
* [A Tumblr post on the subject](http://bencoe.tumblr.com/post/30685403088/browser-side-amazon-s3-uploads-using-cors)
* [http://blog.danguer.com/2011/10/25/upload-s3-files-directly-with-ajax/](http://blog.danguer.com/2011/10/25/upload-s3-files-directly-with-ajax/)
* [https://github.com/tweetegy/save_image_directly_to_s3_using_backbone](https://github.com/tweetegy/save_image_directly_to_s3_using_backbone)
* [http://www.tweetegy.com/2012/01/save-an-image-file-directly-to-s3-from-a-web-browser-using-html5-and-backbone-js/](http://www.tweetegy.com/2012/01/save-an-image-file-directly-to-s3-from-a-web-browser-using-html5-and-backbone-js/)