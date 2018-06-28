---
layout: post
title: Javascript in Angular
---


<blockquote>
<p class="graf graf--p">I recently stumbled across whammy.js , a tool that converts images to video on the client side (I know, you’re going whaaaaaaat). It requires images of the webp format, a format brought around by google and which is supported only in chromium based browsers. It doesn’t really seem like firefox would be supporting that in the near future. These images are substantially smaller than their png or jpeg counterparts. whammy.js converts these itsy-bitsy images into webm format video, (mostly supported by browsers). It also makes it easier to embed the video in your html document.</p>
</blockquote>
<p class="graf graf--p"><em>I knew I needed this. It was perfect, client side image to video conversion, that’s a developer’s and a user’s dream. Server-hassle free. Instant conversion, reduced size without the loss of resolution or pixels.</em></p>
<p class="graf graf--p">However I was riddled with the problem of using the javascript file in angular. :(</p>
<p class="graf graf--p">After about half a day of googling up articles on the internet, I was convinced that I would either have to include a typings.d.ts file (which does not exist) for the javascript file, or completely rewrite it in typescript. I was pretty beaten up and frustrated, I mean, typescript is a superset of javascript , it’s going to end up being javascript at some point of time.O.o</p>
<p class="graf graf--p">Turns out, it isn’t that hard. You just have to be an expert at googling stuff and putting together pieces and it didn’t take me more than ten minutes to put it all together and get it working. Although the code was riddled with bugs, so i spent more than an entire day, figuring out how it worked and tracing where the execution with horrendously wrong, but in the end I had a working script, I’ve given users (chrome and opera users only, sorry :( ..) the power to download >5o mb worth of images as a super compressed video in milliseconds (I do give whammy most of the credit though, can’t deny that).</p>

<h2 class="graf graf--h3"><em class="markup--em markup--h3-em">So how did I do it</em></h2>
<p class="graf graf--p">Easy-peasy.</p>
<p class="graf graf--p">Add your javascript file to the assets folder.</p>
<p class="graf graf--p">Open up your angular-cli.json, and under scripts, add the location of your javascript file:</p>
<img src="https://cdn-images-1.medium.com/max/800/1*GzwzuEjuSuiw6fDdPzKPTA.png" />

 
<p class="graf graf--p">And now all you have to do is hop over to your component, or your service or wherever it is that you want to use the file , and declare the function explicitly over there:</p>
 

<img src="https://cdn-images-1.medium.com/max/800/1*0NO_sgZ8e4xQKfnysuYOvg.png" />
<p class="graf graf--p">and now you are good to start using.</p>

<figure class="graf graf--figure"><img class="graf-image" src="https://cdn-images-1.medium.com/max/800/1*aOWK64k9DsvPxrf6jgS_Lw.png" data-image-id="1*aOWK64k9DsvPxrf6jgS_Lw.png" data-width="441" data-height="56" /></figure>
<p class="graf graf--p">That is how it’s done.</p>
<p class="graf graf--p">Do check out whammy.js, or ccapture(built on whammy.js) they are pretty awesome, if you wanna do image to video conversion on the client side. :)</p>
