
# Tracking HTTP/2 Prioritization Issues

## Introduction

[HTTP/2](https://datatracker.ietf.org/doc/rfc7540/) uses multiplexing to support multiple concurrent requests over the same TCP connection.

H2 also has a prioritization scheme (via weights & dependencies) that enables browsers to communicate the relative priority of each request.

Unfortunately not all servers are equal – some don't appear to implement prioritization and so serve responses on a  'first come, first served' basis, and others that implement prioritization seem to have a few bugs too – and these issues can create a suboptimal experience for the visitor.

Browsers implement their priority trees in quite different ways too – if you're interested in digging deeper [@DaanDeMeyer's h2vis](https://github.com/DaanDeMeyer/h2vis) can visualize these for Chrome and Firefox

Now [WebPageTest](https://www.webpagetest.org) visualises HEADERS and DATA frames for responses we can get a much clearer picture of how prioritization is implemented across different servers and CDNs.

This repo aims to track and highlight prioritisation issues – if they get fixed then the web become better for all of us.


## Getting Started

[@PatMeenan's http2priorities test page](https://github.com/pmeenan/http2priorities) is the easiest way to get started – deploy it on your server / host / CDN of choice and then test it using Chrome / Firefox / Edge / Safari using WebPageTest using the 3G network profile.

Pay close attention to requests 33-36, these are resources that can't be discovered by the pre-parser so their requests start later than the image requests before them, but as they have a higher priority their frames should be re-prioritized ahead of the images.

**CloudFlare - reprioritizes the requests**
![Waterfall with important resources being reprioritised](images/cloudflare.png)

**CloudFront - requests are only partially reprioritized**
![Waterfall with important resources being reprioritised](images/cloudfront.png)

If requests 33-36 don't appear to be prioritised correctly, please raise an issue with the relevant project or vendor.

If you create an issue in this repo with a link to your test (whether good or bad) we'll add it to the table below.


## Current Status

TODO(AD) add table of know tests here servers / CDNs as rows, browsers as columns???

### CDNs / Cloud Hosting Services

| CDN / Hosting  | Status | WPT URL 
| -------------- | ------ | ------- 
| Akamai         | Pass   | https://www.webpagetest.org/result/181128_WJ_4f388eef1d2e03e513ff74214860d2f0/5/details/
| Cloudflare     | Pass   | https://www.webpagetest.org/result/181126_G7_3abfb12925925f8debe527c779c46dfe/6/details/
| CloudFront     | Fail   | https://www.webpagetest.org/result/181126_05_fafd92c1036649029f5392851e0234c2/8/details/
| Fastly         | Fail   | https://www.webpagetest.org/result/181126_J0_0c0c0556ced1277ea34881b0b0b1d759/6/details/
| Firebase       | Fail   | https://www.webpagetest.org/result/181128_PA_9c3c428698111b81df1cc6eef2e0520c/8/details/
| Google Storage | Fail   | https://www.webpagetest.org/result/181126_XF_361c1789d782990b27a0141e838694bf/7/details/
| Netlify        | Fail++ | https://www.webpagetest.org/result/181126_05_fafd92c1036649029f5392851e0234c2/8/details/



### Servers



