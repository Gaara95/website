# website

These are the content files for our website, written in Markdown.

The WordPress editor is a pain to use, it randomly re-formats code snippets,
removes HTML and generally causes chaos, so we write all our content in **real** text  
editors like Atom and Sublime Text, then we copy it over from this repository to our site.

## Writing guidelines:

* All documents must be in Markdown.

* All images should use the HTML image tags with the appropriate
  WordPress classes to align them in the center,like this:
  ```
  <img class="aligncenter wp-image-147 size-full" src="https://thewebsitesurl.com/picture.png" alt="" width="600" height="783" />
  ```
  this is because images are on the same server.

* Italicize inline code instead of using the inline code tags, the theme we use
  makes inline code look awful, so *this* instead of `this`.


## Post structure:

* The first post of a series of posts should be tagged `featured` so it comes up
  on the front page.

* Put a number before the posts names so finding the first or sixth or whichever
  one is easier.
