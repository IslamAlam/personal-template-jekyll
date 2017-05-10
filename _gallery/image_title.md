http://stackoverflow.com/questions/17677094/jekyll-for-loop-over-all-images-in-a-folder

Here's the short version:

Create a YAML data file (_data/galleries.yml) with the list of images:
- id: gallery1
  description: This is the first gallery
  imagefolder: /img/demopage
  images:
  - name: image-1.jpg
    thumb: thumb-1.jpg
    text: The first image
  - name: image-2.jpg
    thumb: thumb-2.jpg
    text: The second image
  - name: image-3.jpg
    thumb: thumb-3.jpg
    text: The third image
- id: anothergallery
  description: This is even another gallery!
  imagefolder: /img/demopage
  images:
  - name: image-4.jpg
    thumb: thumb-4.jpg
    text: Another gallery, first image
  - name: image-5.jpg
    thumb: thumb-5.jpg
    text: Another gallery, second image
  - name: image-6.jpg
    thumb: thumb-6.jpg
    text: Another gallery, third image
For a list of available galleries, just loop through the data file:
{% for gallery in site.data.galleries %}
- [{{ gallery.description }}]({{ gallery.id }})
{% endfor %}
Create a layout file (_layouts/gallery.html) that all galleries will be based on:

(in my example, I'm using Lightbox2 to display the images, so there's additional HTML in my example that you don't even need when you just want to use <img src="photos/01.jpg">)
---
layout: default
---

<script src="/js/jquery-1.10.2.min.js"></script>
<script src="/js/lightbox-2.6.min.js"></script>
<link href="/css/lightbox.css" rel="stylesheet" />

{% for gallery in site.data.galleries %}
  {% if gallery.id == page.galleryid %}
    <h1>{{ gallery.description }}</h1>
    <ol>
    {% for image in gallery.images %}
      <li>
        {{ image.text }}<br>
        <a href="{{ gallery.imagefolder }}/{{ image.name }}" data-lightbox="{{ gallery.id }}" title="{{ image.text }}">
          <img src="{{ gallery.imagefolder }}/{{ image.thumb }}">
        </a>
      </li>
    {% endfor %}
    </ol>
  {% endif %}
{% endfor %}
For each gallery page, create a .html or .md file that just contains three lines of YAML front-matter:

---
title: the first gallery page
layout: gallery
galleryid: gallery1
--- 
The layout: gallery line refers to the layout file from step 3.
The galleryid: gallery1 line refers to the data file from step 1, so that the layout file "knows" that it has to show the images from the first gallery.

