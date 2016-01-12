# Ghost for Jekyll Theme
Theme for Jekyll based on default Ghost theme

## Instalation
I suppose you already know how to use Jekyll, and that’s why you were looking for Jekyll theme, so I’m not going to describe it any further here. If you need some help with Jekyll, it’s always best to refer official website at https://jekyllrb.com

In a nutshell, what you got is blueprint used for generating static pages of your website. The result is something you just grab and upload to your domain.

## Files and structure

This theme respects the structure of vanilla Jekyll installation:


	\_config.yml
	\_includes
		disqus.html
		follow.html
		image.html
		pagination.html
		share.html
		subscribe-home-bar.html
		tag-collector.html
	\_layouts
		default.html
		page.html
		post.html
	\_posts
		post-file.markdown
	\_sass
		\_syntax.scss
	\_site
		generated content of your site
	about.md
	assets
		css
			main.scss
			screen.css
		images
			author-image.png
			site-cover.png
		js
			index.js
			jquery-1.10.2.min.js
			jquery-1.11.1.min.js
			jquery.fitvids.js
	index.hml
	rss.xml
	tag-file.html

You don't need to modify most of these files. But you need to use \_config.yml to make basic setup of your site. Also you might want to change site-cover.png and author-image.png.

## Basic setup of your theme

1) Copy or move the whole directory structure to the preferred destination on your local machine. I suggest you don’t overwrite your current one, but create the new one instead.

2) Open \_config.yml and set name, description and most importantly baseurl.

Baseurl must point to the directory where your site will be presented on your server. For example if your domain name is mydomain.com and your Jekyll site will be inside directory jekyll, you will access it via http://www.mydomain.com/jekyll/ and your baseurl will be /jekyll

## Disqus integration

You can add comments to any post via Disqus.

1. Open \_config.yml
2. Set disqus_shortname. If you have no idea what shortname is, you probably didn’t set it yet. In that case go to Disqus help: https://help.disqus.com/customer/portal/articles/466208-what-s-a-shortname-
3. Save and close \_config.yml

## Social media integration

1. Open \_config.yml
2. Set your nickname for Social Media listed at the very bottom of the file
3. Save and close \_config.yml


## Authors

You can set different author for every post. If you don’t, then site author will be set as the default author for every post.

If you don’t set the site author, then all posts without the author specifically defined in header will be shown without the author.

To set default site author:

1. Open \_config.yml
2. Set author name and image file
3. Save and close \_config.yml

## Tags

Tags help you collect post based on similar topic.

You need to do two things:

1. You need to create collector file for each tag
2. You need to specify tags in the post which will be explained later

To create collector file for tag, let’s say you want to create new tag named “Jekyll”:

1. Create new blank file in root directory of your site and name it tag-jekyll.html
2. Open this file and write down this text:

        ---
        layout: default
        title: jekyll
        ---

        {% include tag-collector.html %}

3. Save the file

Alternatively you can just make a copy of existing tag-amet.html, rename it and change the title inside the file to correspond to the name of the tag.

The rest will be done automatically and you can enjoy collections of posts automatically generated by tags.

Since Jekyll doesn't support it yet, you can't paginate these collections. I know there are plugins for that, but I want to keep this usable on GitHub Pages for you, so no plugins.

## Cover image

You can turn cover image off if you don’t like the idea. In that case simply comment it out inside \_config.yml

To change cover image, go to assets/images and upload new site-cover.jpg, the file should be big enough, I suggest at least 1024 px wide.

## Pagination

The default value of posts per page is set to 6.

To change the number of post per page:

1. Open \_config.yml
2. Change paginate value
3. Save and close \_config.yml

## Posts

Post should follow the basic structure. It is a markdown file stored inside \_posts directory.

To write a new post, it is best to copy one of predefined files and modify it’s content.

Every post file includes header:

    ---
    layout: post
    title: This is a title of your post
    date: '2015-06-23 06:17:27'
    tags: windows linux
    author: John Doe
    author-image: assets/images/author-image.png
    disqus: true
    archive: false
    ---

You should update the title of your post, date and add some tags.

You can add multiple tags to one post, just DO NOT use commas between them, only space. Use lowercase for names, theme will do the rest for you.

You can control which posts will allow comments. This is achieved by disqus option set to true or false.

You can show list of other posts at the bottom of the post via archive option set to true.

## Assets

All assets like screenshots and images should be stored in assets directory. It’s up to you to create your own structure inside assets. I suggest creating new directory for every post and put all assets of this post inside that directory. If your post is “my-new-post”, then images should be linked like this:

http://yourwebsite.com/assets/my-new-post/kasper-2.0-index.png

## Deployment

Once you’re happy with your setup, it’s time to test your pages and upload them to web server.

1. From terminal on your local machine cd into the directory where you unarchived structure is and run this command:

        jekyll build

    This will create new (or update existing) folder \_site where the whole website will be generated and ready for use.

2. To check out locally what your generated site looks like, run this command from terminal:

        jekyll serve

    This will run server of your site on your localhost and you can easily access it at localhost:4000

3. Once you are happy with the result, you can move your generated site to you web hosting provider. Just about any traditional web hosting provider will let you upload files to their servers over FTP. To upload generated site to a web host using FTP, simply copy the content of generated \_site folder to the root folder of your hosting account. This is most likely to be the httpdocs or public_html folder on most hosting providers. If you don’t want to upload it to the root, refer again to baseurl setup at the beginning of this manual.

If you want to use GitHub as your web hosting provider, you can follow this easy tutorial https://pages.github.com/



## Copyright & License

Based on https://github.com/rosario/kasper

Copyright (C) 2016 Ghost Foundation - Released under the MIT License.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
