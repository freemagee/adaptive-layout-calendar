**Published** 2012-08-21

I redesigned and developed [Concept Design’s website](http://www.conceptdesignltd.co.uk), a design agency based in Leicester. The old site was out of date so developing a new site was a good way to freshen up the brand and also stretch my current technical skills.

One practice that I wanted to incorporate into the new design was responsive web design and an adaptive layout. This would allow the new site to adapt to phones, tablets, desktops and hopefully most other devices as well.

![Preview of Concept Design's Website](http://neilmagee.com/library/img/article-assets/concept-design-website-preview.jpg)

*Preview of Concept Design's Website*

### A method to make images adaptive

Whilst I have experience of making websites mobile ready, one particular technique I wanted to address was a method to allow images on the new site to adapt to new layouts and offer maximum flexibility.

An image in HTML is traditionally a fixed width element. So if a website is normally 960 pixels wide and images of a product on that site are 600 pixels wide if that websites width is adjusted to less than 600px then the image will begin to break out of the layout and horizontal scroll backars begin to appear.

[Example of two different image behaviours]
(http://neilmagee.com/demo/adaptive-images-initial.html)

One method to address this problem is to use the following CSS to create [fluid images](http://www.alistapart.com/articles/fluid-images/):

```CSS
img {
    max-width: 100%;
    width: 100%;
}
```

This works very well, but it simply reduces an image proportionately to it’s parent container. This unfortunately has a consequence that horizontal images will become very small on a phone or similar device and portrait images will take up a whole screen. Images will also expand to widths greater than their original dimensions. This will cause blurriness and make your images look less attractive because the web browser is resizing the image on the fly.

I like this method as it is simple to implement, but I want more control over my layout at different media-query breakpoints, and that means a way to control my images at different screen sizes.

The current problem is that the `<img>` does not support multiple images. So you can not dynamically swap out images at different screen sizes. Which would be great and solve the immediate problem. In the near future this will be possible with the newly proposed `<picture>` element that the [W3C](http://www.w3.org/community/respimg/) are agreeing upon. Until this element has proper support another solution is needed.

### Thinking outside the box

I began to think about how I could dynamically control an image. Javascript is certainly a solution, but could become clumsy and hard to maintain over time. I thought about what could be achieved using a scripting language such as PHP, but PHP has limitations as it is a server side language and does not know the dimensions of a users browser window.

#### CSS to the rescue

I looked at what I could achieve using CSS alone. I knew that with CSS I could control the dimensions of a ‘box’ at different screen sizes. I also knew that I could set an image as the background on that box. This meant that if I used a standard `<div>` instead of a traditional image tag I could achieve my goal.

Here is a condensed example of the CSS and some of the media-query breakpoints I used:

```CSS
/* Mobile first loaded on screens up to 480px wide */

/* Case study height will change at different breakpoints */
.case-study-preview {
    background-repeat: no-repeat;
    background-position: center top;
    height: 305px;
}
#project-a .case-study-preview {
    background-image: url('../img/work-examples/case-studies/project-a/spread-430x305.jpg');
}

@media screen and (min-width: 480px) {
    .case-study-preview {
        height: 310px;
    }
    #project-a .case-study-preview {
        background-image: url('../img/work-examples/case-studies/project-a/spread-562x310.jpg');
    }
}

@media screen and (min-width: 640px) {
    .case-study-preview {
        height: 380px;
    }
    #project-a .case-study-preview {
        background-image: url('../img/work-examples/case-studies/project-a/spread-690x380.jpg');
    }
}

@media screen and (min-width: 1024px) {
    .case-study-preview {
        height: 458px;
    }
    #project-a .case-study-preview {
        background-image: url('../img/work-examples/case-studies/project-a/spread-648x458.jpg');
    }
}
```

This example shows how the method works. At different screen resolutions new images are loaded in and maximum control is asserted over the website layout. I assign a unique ID (or class) to the parent element of the image. This allows me to target that particular image directly.

The design for Concept Design’s new website is fluid, constrained by use of `max-width` on the site’s wrapper, so I carefully adjusted the browser window to see at which dimensions the layout ‘broke’ and a new image would need to be loaded in. My breakpoints ended up as 480px, 640px, 768px, 1024px and 1280px. I ended up creating 5 separate images to successfully cover all the different screen sizes. This may increase the workload a little as you will need to create these images, but the end result is worth it.

![Concept Design website on different devices](http://neilmagee.com/library/img/article-assets/concept-design-different-devices.jpg)

*Concept Design website on different devices*

#### Image Optimisation

The fact I am using a [mobile first approach to the site layout](http://www.abookapart.com/products/mobile-first/) means that large images are not unnecessarily loaded onto mobile devices. This saves bandwidth and importantly battery life on those devices. If the images have been optimised with tools like [Smush.it](http://www.smushit.com/ysmush.it/) then you are providing the best experience a mobile user of your site can have.

### Web standards considerations

Normally I would advocate using HTML elements for their standard purpose only. So if you need a way to layout tabular data then you use a table element rather than a list element. If you are listing ingredients in a recipe then you use a unordered list rather than a bunch of paragraph elements.

So using a DIV instead of an IMG took some thought as to whether I was doing the right thing or not. Fortunately on the Concept Design website, the images are purely illustrative. The text is more important and it tells the story. Fortunately I found some accessibility solutions that I feel makes the DIV method justifiable. It seems that Yahoo had the same issues as me during a redesign of [Flickr](http://yaccessibilityblog.com/library/aria-fix-non-standard-images.html) and they advocate the use of ARIA roles and attributes to address screen-readers and web users with disabilities. An example of this solution is below:

```HTML
<div role="img" aria-label="A squirrel in a tree" ></div>
```

I will definitely use this method to integrate adaptive images into future designs. It meets all the needs I had prior to redesigning [Concept Design](http://www.conceptdesignltd.co.uk). In six months time adaptive images may not be a problem anymore, but with a lack of support for new elements in older web browsers this seems like a good option that provides great results.
