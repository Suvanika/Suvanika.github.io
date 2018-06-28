---
layout: post
title: Multipart download and Byte Ranges 
---

<h3>The why and how of it.</h3>
<blockquote>Let's break down what happens when you click on a button to download a file online.  You happen to send a HTTP get request to the website's respective web server, requesting for the object, the website responds back with a "200 OK" , "you're good to go" , with the requested object in it's body. Our browser takes care of the requests and processing the responses (pretty smart-ass). You can track all of this by toggling the network tab on your browser's developer toolkit (ctrl + shift + I ).

Typically a server sends out the entire object packed into one response message that we download . What if we could split the entire object into fragments and download them simultaneously? Picture a queue infront of an ice-cream shop, it extends for a few couple of meters and the the time taken to serve a customer is a few minutes. Boy oh boy, that's gonna take you a while till you get your cone. What if, instead of one long queue, we had three counters serving three parallel queues at the same rate?  This is what multipart download does in a nutshell. Break, grab concurrently , piece it back together and give it back to you, voila. We all use download managers to get our downloading done in a fast and efficient way, don't we? This is the underlying principle.

Now how do get this done?</blockquote>
<h2> <strong>Range Byte Requests</strong></h2>
HTTP protocol supports what is called a Range Byte request.  We add an extra <code>Range</code> header to our http request.  The format is <code>Range : bytes= 0-1000</code> or <code>Range : bytes=100-200/2700 </code>  where (2700 is the size of the object). Also known as <strong>byte serving,  </strong>range requests help you request for just a portion of a file , instead of downloading the entire file. This is very helpful while streaming large files, especially video files, one could skip to portions they wish to view without having to wait for the entire file to download. All you have to do is send a byte request for that portion of the video they wish to view. It serves useful while downloading a lot of files or while viewing pdf applications, you download just the portion that you are currently viewing and nothing more!

The server has to support range requests as well. You could check this by sending a head request. Which requests only for the headers of the response excluding the content.  You can check this out by using cURL, a command line tool.

<code>cURL -X -i -H "Range :bytes=0-100" " http://demo8127239.mockable.io/h"</code>

Is translated into :

<code>GET hi HTTP/1.1
Host: demo8127239.mockable.io
Range: bytes=0-100 </code>

Server responds back with a

<code> 200 OK
Accept-Range : bytes </code> if range requests are supported.
or else, it responds with a

<code> 200 OK
Accept-Range :none</code>

However if a GET request is placed we get either a <code> 200 OK </code>
response if range bytes isn't supported  or a <code>206 Partial Content </code> status.

<code>HTTP/1.1 206 Partial Content</code>
<code>Content-Range: bytes 0-100/343
Content Length :343
Content-Type :application/json</code>

The server sends out the entire file instead if it doesn't support range bytes.

Multiple range requests can also be placed together

<code>Range :bytes=0-100, 200-300</code>

<strong>BROWSER SUPPORT:</strong>

Browsers place a restriction on the number of concurrent TCP connections one might establish with a particular server. The list of browsers and their limit is given below.
<table class="table thin-table" border="1" summary="" rules="all" cellspacing="0" cellpadding="4"><caption><span class="tablecap"> Maximum supported connections</span></caption>
<thead class="thead" style="text-align: left;">
<tr class="row">
<th id="d116953e124" class="entry cellrowborder" style="text-align: left; vertical-align: top;">Version</th>
<th id="d116953e127" class="entry cellrowborder" style="text-align: left; vertical-align: top;">Maximum connections</th>
</tr>
</thead>
<tbody class="tbody">
<tr class="row">
<td class="entry cellrowborder" style="text-align: left; vertical-align: top;" headers="d116953e124 "><span class="ph">Internet
Explorer<sup>®</sup></span> 7.0</td>
<td class="entry cellrowborder" style="text-align: left; vertical-align: top;" headers="d116953e127 ">2</td>
</tr>
<tr class="row">
<td class="entry cellrowborder" style="text-align: left; vertical-align: top;" headers="d116953e124 "><span class="ph">Internet
Explorer</span> 8.0 and 9.0</td>
<td class="entry cellrowborder" style="text-align: left; vertical-align: top;" headers="d116953e127 ">6</td>
</tr>
<tr class="row">
<td class="entry cellrowborder" style="text-align: left; vertical-align: top;" headers="d116953e124 "><span class="ph">Internet
Explorer</span> 10.0</td>
<td class="entry cellrowborder" style="text-align: left; vertical-align: top;" headers="d116953e127 ">8</td>
</tr>
<tr class="row">
<td class="entry cellrowborder" style="text-align: left; vertical-align: top;" headers="d116953e124 "><span class="ph">Internet
Explorer</span> 11.0</td>
<td class="entry cellrowborder" style="text-align: left; vertical-align: top;" headers="d116953e127 ">13</td>
</tr>
<tr class="row">
<td class="entry cellrowborder" style="text-align: left; vertical-align: top;" headers="d116953e124 "><span class="ph">Firefox<sup>®</sup></span></td>
<td class="entry cellrowborder" style="text-align: left; vertical-align: top;" headers="d116953e127 ">6</td>
</tr>
<tr class="row">
<td class="entry cellrowborder" style="text-align: left; vertical-align: top;" headers="d116953e124 "><span class="ph">Chrome™</span></td>
<td class="entry cellrowborder" style="text-align: left; vertical-align: top;" headers="d116953e127 ">6</td>
</tr>
<tr class="row">
<td class="entry cellrowborder" style="text-align: left; vertical-align: top;" headers="d116953e124 "><span class="ph">Safari<sup>®</sup></span></td>
<td class="entry cellrowborder" style="text-align: left; vertical-align: top;" headers="d116953e127 ">6</td>
</tr>
<tr class="row">
<td class="entry cellrowborder" style="text-align: left; vertical-align: top;" headers="d116953e124 "><span class="ph">Opera<sup>®</sup></span></td>
<td class="entry cellrowborder" style="text-align: left; vertical-align: top;" headers="d116953e127 ">6</td>
</tr>
<tr class="row">
<td class="entry cellrowborder" style="text-align: left; vertical-align: top;" headers="d116953e124 "><span class="ph">iOS<sup>®</sup></span></td>
<td class="entry cellrowborder" style="text-align: left; vertical-align: top;" headers="d116953e127 ">6</td>
</tr>
<tr class="row">
<td class="entry cellrowborder" style="text-align: left; vertical-align: top;" headers="d116953e124 "><span class="ph">Android™</span></td>
<td class="entry cellrowborder" style="text-align: left; vertical-align: top;" headers="d116953e127 ">6</td>
</tr>
</tbody>
</table>
<code></code>

This restriction is placed since multiple connections to the same server might lead to flooding of requests and this might lead the server into thinking that this is a DDoS attack (denial of service attack).
<h1>Why multipart</h1>
I get that the number of chunks you can download concurrently is limited, and the limit is pretty meagre, so how beneficial can it actually be?
Most servers treat each connection uniquely and allocates bandwidth to it. When we make several connections to a server at once, each connection is treated as an individual unique connection and is allocated the same bandwidth. So what happens now, is that the server is pushing out more data to the same user at a given time. So how's that for beneficial huh?
Although many users requesting several connections could overload the server or mislead the server into thinking it is under attack, this is where the built in browser restrictions come into place, although there are workarounds around this,(firefox) where the user gets to set the limit.
Also when the server from where you are requesting content is placed all the way on the other side of the planet, this adds latency to the response, (i.e, the propogation delay). In such a scenario, the bits transmitted in multipart trumps the delay, so yay! It also depends heavily on your internet connection, your bandwidth, the distance between you and the requested server and the bandwidth of the server (that's a lot of factors coming into play). However multipart download helps save you time by utilizing the entire bandwidth as long as you have download requests lined up and this is what makes range bytes a better option than our single stream/channel download.
Most download managers support what is called dynamic fragmenting or segmentation, where threads are spawned for each byte range (in an application, the number of parallel threads set by default is 8, for a browser extension, it depends on the number of parallel connections the browser supports , view the above table to see your browser's capacity), if a thread frees up, the segment yet to be downloaded or a thread whose download is in progress is dynamically fragmented in a such a way that the total bandwidth is used all the time. This speeds up your download process.
<p id="81fb" class="graf graf--p graf-after--p">Also range byte request comes to the rescue if your download fails mid way. You don’t have to download the entire file all over again , you just pick off where you left off.</p>
<p id="87ca" class="graf graf--p graf-after--p">Since the size of each chunk is going to be way lesser than the actual file when sent, the chances of bit errors creeping on to your packet is less.</p>
<p id="1753" class="graf graf--p graf-after--p">Pretty cool huh?</p>
<p id="81db" class="graf graf--p graf-after--p">But wait.</p>
There might be one teeny tiny drawback. When a range header is set, the browser sends what is called a <strong>preflight request. This is done to check if the range header and the requested method is allowed on the server side CORS  (Cross Origin Resource Sharing) specification. The actual request is placed only after an OPTIONS response is received that explicitly states that the requested method and header is allowed. </strong>

access-control-allow-headers :range //<strong>accept requests with the range header and process it.</strong>

access-control-allow-methods :DELETE,GET,PATCH,POST,PUT,OPTIONS //<strong>accept only those requests for the given methods.</strong>

access-control-allow-origin : * //<strong>accept request from any client </strong>

This was brought about to prevent CSRF attacks.  Yay for user's security, nay for multipart downloading, since each byte request has to go through the preflight - OPTIONS response process which adds substantial overhead. :(
<h1>Getting over this.</h1>
Browsers don't tend to send preflight requests as long as the generated HTTP request is a simple one. Now what categorizes as simple  ?

If the following methods are used:
<ul class="postList">
	<li id="0583" class="graf graf--li graf-after--p"><code class="markup--code markup--li-code"><a class="markup--anchor markup--li-anchor" title="The HTTP GET method requests a representation of the specified resource. Requests using GET should only retrieve data." href="https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/GET" target="_blank" rel="nofollow noopener">GET</a> ,</code><code class="markup--code markup--li-code"><a class="markup--anchor markup--li-anchor" title="The HTTP HEAD method requests the headers that are returned if the specified resource would be requested with an HTTP GET method. Such a request can be done before deciding to download a large resource to save bandwidth, for example." href="https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/HEAD" target="_blank" rel="nofollow noopener">HEAD</a> ,</code><code class="markup--code markup--li-code"><a class="markup--anchor markup--li-anchor" title="The HTTP POST method sends data to the server. The type of the body of the request is indicated by the Content-Type header." href="https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST" target="_blank" rel="nofollow noopener">POST</a></code></li>
</ul>
If the following headers are used:
<ul class="postList">
	<li id="bcca" class="graf graf--li graf-after--p"><code class="markup--code markup--li-code"><a class="markup--anchor markup--li-anchor" title="The Accept request HTTP header advertises which content types, expressed as MIME types, the client is able to understand. Using content negotiation, the server then selects one of the proposals, uses it and informs the client of its choice with the Content-Type response header. Browsers set adequate values for this header depending of the context where the request is done: when fetching a CSS stylesheet a different value is set for the request than when fetching an image, video or a script." href="https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept" target="_blank" rel="nofollow noopener">Accept</a></code></li>
	<li id="eb41" class="graf graf--li graf-after--li"><code class="markup--code markup--li-code"><a class="markup--anchor markup--li-anchor" title="The Accept-Language request HTTP header advertises which languages the client is able to understand, and which locale variant is preferred. Using content negotiation, the server then selects one of the proposals, uses it and informs the client of its choice with the Content-Language response header. Browsers set adequate values for this header according their user interface language and even if a user can change it, this happens rarely (and is frown upon as it leads to fingerprinting)." href="https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept-Language" target="_blank" rel="nofollow noopener">Accept-Language</a></code></li>
	<li id="6d5c" class="graf graf--li graf-after--li"><code class="markup--code markup--li-code"><a class="markup--anchor markup--li-anchor" title="The Content-Language entity header is used to describe the language(s) intended for the audience, so that it allows a user to differentiate according to the users' own preferred language." href="https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Language" target="_blank" rel="nofollow noopener">Content-Language</a></code></li>
	<li id="ab17" class="graf graf--li graf-after--li"><code class="markup--code markup--li-code"><a class="markup--anchor markup--li-anchor" title="The Content-Type entity header is used to indicate the media type of the resource." href="https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type" target="_blank" rel="nofollow noopener">Content-Type</a></code> (but note the additional requirements below)</li>
	<li id="b187" class="graf graf--li graf-after--li"><code class="markup--code markup--li-code"><a class="markup--anchor markup--li-anchor" href="http://httpwg.org/http-extensions/client-hints.html#dpr" target="_blank" rel="nofollow noopener">DPR</a></code></li>
	<li id="336a" class="graf graf--li graf-after--li"><code class="markup--code markup--li-code"><a class="markup--anchor markup--li-anchor" href="http://httpwg.org/http-extensions/client-hints.html#downlink" target="_blank" rel="nofollow noopener">Downlink</a></code></li>
	<li id="1638" class="graf graf--li graf-after--li"><code class="markup--code markup--li-code"><a class="markup--anchor markup--li-anchor" href="http://httpwg.org/http-extensions/client-hints.html#save-data" target="_blank" rel="nofollow noopener">Save-Data</a></code></li>
	<li id="44b4" class="graf graf--li graf-after--li"><code class="markup--code markup--li-code"><a class="markup--anchor markup--li-anchor" href="http://httpwg.org/http-extensions/client-hints.html#viewport-width" target="_blank" rel="nofollow noopener">Viewport-Width</a></code></li>
	<li id="0318" class="graf graf--li graf-after--li"><code class="markup--code markup--li-code"><a class="markup--anchor markup--li-anchor" href="http://httpwg.org/http-extensions/client-hints.html#width" target="_blank" rel="nofollow noopener">Width</a></code></li>
</ul>
<p id="f3af" class="graf graf--p graf-after--li">Only if the <code class="markup--code markup--p-code"><a class="markup--anchor markup--p-anchor" title="The Content-Type entity header is used to indicate the media type of the resource." href="https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type" target="_blank" rel="nofollow noopener">Content-Type</a></code> header has any of the following values:</p>

<ul class="postList">
	<li id="30fc" class="graf graf--li graf-after--p"><code class="markup--code markup--li-code">application/x-www-form-urlencoded</code></li>
	<li id="8598" class="graf graf--li graf-after--li"><code class="markup--code markup--li-code">multipart/form-data</code></li>
	<li id="98e9" class="graf graf--li graf-after--li"><code class="markup--code markup--li-code">text/plain</code></li>
</ul>
Cool.

<strong>ME : But how do we bypass the range header? </strong>

<strong>Byte Ranges with URL : Bypass range header you say,  say no more fam.</strong>

byte ranges can be set in the url :
<code>http://host/dir/foo;bytes=0-499 //requesting for the first 500 bytes
http://host/dir/foo;bytes=0-99,500-1499,-200 // multiple range requests , first hundred bytes, bytes from 500 to 1500 and the last 200 bytes
</code>

Again, the server must support this.

Even if it doesn't multipart download makes a difference while downloading files of large size. So if you ask me, I'd say multipart for the win !

 
