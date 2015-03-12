---
layout: default
title: Static Website using Amazon S3
---

First using s3 I posted a simple HTML webpage called index.html Just that one file into a bucket I called shazart.info which is the domain I registered for this tutorial.

I can use the Bucket Policy to give access to anyone to get an object, used for static websites:

{% highlight JSON %}
{
  "Version":"2012-10-17",
  "Statement":[{
	"Sid":"AddPerm",
        "Effect":"Allow",
	  "Principal": "*",
      "Action":["s3:GetObject"],
      "Resource":["arn:aws:s3:::admin.shazart/*"
      ]
    }
  ]
}
{% endhighlight %}

* [Tutorial on Amazon Bucket Permissions](http://docs.aws.amazon.com/AmazonS3/latest/UG/EditingBucketPermissions.html)
* [Amazon's Policy Generator](http://awspolicygen.s3.amazonaws.com/policygen.html) is a tool to help you make these policies 

Route 53 handles DNS records. Heres how I pointed my hover registered domain to use Route 53,

Changed the Hover Name servers to something like this

	ns-8888.awsdns-88.net
	ns-8888.awsdns-88.org
	ns-8888.awsdns-88.co.uk
	ns-8888.awsdns-88.com

Created two new A records using Route 53 to point the domain to the shazart.info bucket.

Amazon's tutorials are very dense and reference other tutorials in the middle of their own instructions. Its frustrating because one tutorial implies, 'learn about this here and then do it right in this tutorial to complete this step' So I stopped an learned [about DNS records from human beings](https://help.hover.com/entries/21234623-How-does-DNS-work-), in other words Hover. Then I found an article about using s3 and Hover.

I learned that all that route 53 stuff wasn't required. My Domain name registrar is Hover (I like them a lot) and they can let me point things where I want using their website, Doing the work of Amazon Route 53.

Basic Steps I found on [Hover](https://help.hover.com/entries/21124908-i-want-to-use-amazon-s-s3-static-webpage-hosting)

1. Signed up for AWS and set up an S3 bucket (done)
2. Upload my site to S3. (done)
3. With Hover, pointed my domain + www to S3 via a CNAME (what I needed to do) [http://help.hover.com/entries/21204757-how-to-edit-dns-records-a-cname-mx-txt-and-srv](http://help.hover.com/entries/21204757-how-to-edit-dns-records-a-cname-mx-txt-and-srv) Because I copy & pasted the bucket's endpoint without looking too closely, I initially had the http:// and a trailing / in there. This seemed to break things. Removing those fixed it.
4. Set up a redirect with Hover to point my 'naked' domain (ie without www) to the domain + www, that in turns point to S3 (what I needed to do next)[http://help.hover.com/entries/21196383-how-to-forward-your-domain-name](http://help.hover.com/entries/21196383-how-to-forward-your-domain-name) 
5. Opened a beer and basked in my success. (lunch time maybe?)

In the end I went the route of using Amazon's Route 53. It allowed me to point www.shazart.info to simply shazart.info and I don't like those extra characters (www) in the beginning of a URL. The key to my error was in the DNS servers. I kept editing the Hover.com DNS settings when the name servers where in the Hover 'Domain Details' tab.

Next up posting changed to the s3 bucket.

######Links and resources I dug up during this task
* [github.com/laurilehmijoki/s3_website](https://github.com/laurilehmijoki/s3_website)
* [www.allthingsdistributed.com/2011/08/Jekyll-amazon-s3.html](http://www.allthingsdistributed.com/2011/08/Jekyll-amazon-s3.html)
* [lynda.com/AWS-tutorials/Amazon-Web-Services-Essential-Training/163929-2.html](http://www.lynda.com/AWS-tutorials/Amazon-Web-Services-Essential-Training/163929-2.html)
* [docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html](http://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html)
* [docs.aws.amazon.com/AmazonS3/latest/dev/website-hosting-custom-domain-walkthrough.html](http://docs.aws.amazon.com/AmazonS3/latest/dev/website-hosting-custom-domain-walkthrough.html  )
