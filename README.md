INSTALLATION

1. $ git clone https://github.com/waybetterdev/dev.waybetter.com.git

2. $ cd dev.waybetter.com
3. $ sculpin install
4. $ php sculpin.phar generate --watch --server
5. Navigate to: http://localhost:8000/


Information from the original Sculpin README
=====================

A skeleton for a Sculpin based blog.

Powered by [Sculpin](http://sculpin.io). =)


Features
--------

A very basic Sculpin based blog supporting the following features:

 * Very minimal Bootstrap based theme.
 * A handful of existing posts in `source/_posts/` to get you started. Feel
   free to remove these when you are ready.
 * An about page at `/about`.
 * An index page at `/`. It displays all posts and paginates them.
 * A blog archive page at `/blog`. It displays post titles broken down by
   month and is paginated.
 * A blog categories page at `/blog/categories`.
 * A blog category index at `/blog/categories/$category`. Similar to the blog
   archive except broken down by each category.
 * A blog tags page at `/blog/tags`.
 * A blog tag index at `/blog/tags/$tag`. Similar to the blog archive
   except broken down by each tag.
 
Build
-----

### RUN IT

    php sculpin.phar generate --watch --server



Previewing Development Builds
-----------------------------

By default the site will be generated in `output_dev/`. This is the location
of your development build.

To preview it with Sculpin's built in webserver, run either of the following
commands. This will start a simple webserver listening at `localhost:8000`.

### Using Sculpin's Internal Webserver

#### Generate Command

To serve files right after generating them, use the `generate` command with
the `--server` option:

    sculpin generate --server

To listen on a different port, specify the `--port` option:

    sculpin generate --server --port=9999

Combine with `--watch` to have Sculpin pick up changes as you make them:

    sculpin generate --server --watch


##### Server Command

To serve files that have already been generated, use the `serve` command:

    sculpin serve

To listen on a different port, specify the `--port` option:

    sculpin serve --port=9999


### Using a Standard Webserver

The only special consideration that needs to be taken into account for standard
webservers **in development** is the fact that the URLs generated may not match
the path at which the site is installed.

This can be solved by overriding the `site.url` configuration option when
generating the site.

    sculpin generate --url=http://my.dev.host/blog-skeleton/output_dev

With this option passed, `{{ site.url }}/about` will now be generated as
`http://my.dev.host/blog-skelton/output_dev/about` instead of `/about`.


Publishing Production Builds
----------------------------

When `--env=prod` is specified, the site will be generated in `output_prod/`. This
is the location of your production build.

    sculpin generate --env=prod

These files are suitable to be transferred directly to a production host. For example:

    sculpin generate --env=prod
    rsync -avze 'ssh -p 999' output_prod/ user@yoursculpinsite.com:public_html

If you want to make sure that rsync deletes files that you deleted locally on the on the remote too, add the `--delete` option to the rsync command:

    rsync -avze 'ssh -p 999' --delete output_prod/ user@yoursculpinsite.com:public_html

In fact, `publish.sh` is provided to get you started. If you plan on deploying to an
Amazon S3 bucket, you can use `s3-publish.sh` alongside the `s3cmd` utility (must be
installed separately).

Adding New Authors
------------------
1. Add bios to /source/_views/partials/first-last 
2. Save small bio photo in /source/images/bio-pics/first-last.png
3. Author posts must be saved in /source/_posts/YYYY-MM-DD-this-is-a-new-post
  - Format post using example posts marked "DELETE ME"
  - Be sure to include author's name as a tag (ex. jon-doe)
   
Adding Images
-------------   
Add embedded images to your blog first by 
1. Save your image to: dev.waybetter.com/source/images/posts
2. Access your image by placing the desired tags within your blog post.    
Image tags should look like this:    
<img class="img-thumb" src="/images/posts/your-photo.jpg">     
<img class="img-small" src="/images/posts/your-photo.jpg">     
<img class="img-medium" src="/images/posts/your-photo.jpg">    

<img class="img-large" src="/images/posts/your-photo.jpg">     
<img class="img-huge" src="/images/posts/your-photo.jpg">     
 