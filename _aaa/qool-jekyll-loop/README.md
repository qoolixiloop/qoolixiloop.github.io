# Jekyll Tutorial
> Jekyll is a simple, blog-aware, static site generator perfect for personal, project, or organization sites. Think of it like a file-based CMS, without all the complexity. Jekyll takes your content, renders Markdown and Liquid templates, and spits out a complete, static website ready to be served by Apache, Nginx or another web server. Jekyll is the engine behind GitHub Pages, which you can use to host sites right from your GitHub repositories.  

## Links
Jekyll on Github  

    https://github.com/jekyll/jekyll  
    
Jekyll tutorial  

    https://jekyllrb.com/docs/step-by-step/01-setup/  
    
Ruby installation for Linux Ubuntu  

    https://www.digitalocean.com/community/tutorials/how-to-install-ruby-on-rails-with-rbenv-on-ubuntu-18-04  
    https://jekyllrb.com/docs/installation/ubuntu/

## Install dependecies for rbenv and ruby  
    $ sudo apt install autoconf bison build-essential libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libncurses5-dev libffi-dev libgdbm5 libgdbm-dev  

## Install rbenv  
    $ git clone https://github.com/rbenv/rbenv.git ~/.rbenv  
    $ vim ~/.bashrc  
    
Add some echo export commands if not already there, and source it. 

    $ echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
    $ echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
    $ echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
    $ source ~/.bashrc

## Install ruby-build with rbenv  
    $ git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build  

## Install local ruby with rbenv using ruby-build  
List all available versions. 

    $ rbenv install -l  

Choose a version to install.  

    $ rbenv install 2.6.0  
    
Set it global.  

    $ rbenv global 2.6.0  
    
### Explanation:
Show all installed versions of ruby.  

    $ rbenv versions  
    system (ubuntu installation, sudo apt install )  
    * 2.6.0 (rbenv local installation, set with $ rbenv global, gem install, bundle install, ruby build)  
    
Set a local ruby version to a folder.  

    $ rbenv local 2.1.0  
    applyied to a folder adds file: .ruby-version with content: 2.1.0  
    
Show active global ruby version.  

    $ ruby -version (if not in a folder containing file .ruby-version with content 2.1.0)  
    2.6.0 (if set with $ rbenv global)  
    
Show active global ruby version.  

    $ ruby -version  
    2.1.0 (if in a folder set with $ rbenv local and containing file .ruby-version with content 2.1.0)  

## Install packages (gems) with gem  
Useful information where gems are stored and $PATH.  

    $ gem env  
    
Turn off installing documentation, can take a long time.  

    echo "gem: --no-document" > ~/.gemrc  
    
Install bundler (Bundler is a tool that manages gem dependencies for projects).  
Reads file Gemfile, file Gemfile.lock-  

    $ gem install bundler  (installed under enabled version, global or local)  
    
Rehash, rbenv works by creating a directory of shims, which point to the files used by the Ruby version which is enabled.  

    $ rbenv rehash  
    
Install jekyll.  

    $ gem install jekyll bundler  

## Start jekyll server  

    $ jekyll serve  
    
Same as $ jekyll build && starting server at http://localhost:4000  

Where:   
* _site  

## Use global variables  
Info:  

    https://jekyllrb.com/docs/variables/  
    
Objects:   

    site.xyz , page.xyz, paginator.xyz, content, layout    
    
You can define own custom front matter (xyz) and use them as variables.  

## Use Liquid  
Objects (outputs a variable called page.title, page is the document).

Code:  
```html
    {{ page.title }}
```  
Tags (flow control logic, outputs sidebar if page.sidebar is true).

Code:  
```html
    {% if page.show_sidebar %}  
      <div class="sidebar">  
        sidebar content  
      </div>  
    {% endif %}  
```
Filters:
* Puts string through a filter called capitalize and outputs Hi.

Code:  
```html
    {{ "hi" | capitalize }}  
```
## Use Front Matter  
Purpose:   
* yaml snippet used to set variables  

Code:  
```html
    ---
    title: Home
    ---
    <!doctype html>
    <html>
      <head>
        <meta charset="utf-8">
        <title>{{ page.title }}</title>
      </head>
      <body>
        <h1>{{ "Hello World!" | downcase }}</h1>
      </body>
    </html>
```

## Create a Layout and use it  
Where:   
* _layout  

Purpose:   
* to avoid duplicate code  

Specialty:   
* no front matter, and a content variable  

Code:  
```html
    <!doctype html>
    <html>
      <head>
        <meta charset="utf-8">
        <title>{{ page.title }}</title>
      </head>
      <body>
        {{ content }}
      </body>
    </html>
```

Change:   
* index.html 

Code:  
```html
    ---
    layout: default
    title: Home
    ---
    <h1>{{ "Hello World!" | downcase }}</h1>
```

Add:   
* about.md  

Code:  
``` md
    ---
    layout: default
    title: About
    ---
    # About page
    This page tells you a little bit about me.
```
Access:  
    
    * with .html even though it is an .md file: 
    http://localhost:4000/about.html  

## Include tag  
Where:   
* _includes  

Purpose:   
* allows you to include content from another file  

Add:   
* navigation.html  

Code:  
```html
    <nav>
      <a href="/">Home</a>
      <a href="/about.html">About</a>
    </nav>
```

Change:  
* Layout.html  

Code:  
```html
    <body>
        {% include navigation.html %}
        {{ content }}
    </body>
```

Add styling:  
* navigation.html  

Code:  
```html
    <nav>
      <a href="/" {% if page.url == "/" %}style="color: red;"{% endif %}>
        Home
      </a>
      <a href="/about.html" {% if page.url == "/about.html" %}style="color: red;"{% endif %}>
        About
      </a>
    </nav>
```

## Data files  
Where:   
* _data  

Purpose:  
* store an array of navigation items each with a name and link  

Add:  
* _data/navigation.yml  

Code:  
```yml
    - name: Home
      link: /
    - name: About
      link: /about.html
```

Change:  
* navigation.html 

Code:  
```html
    <nav>
      {% for item in site.data.navigation %}
        <a href="{{ item.link }}" {% if page.url == item.link %}style="color: red;"{% endif %}>
          {{ item.name }}
        </a>
      {% endfor %}
    </nav>
```

## Assets  
Where:  
* assets/css, assets/images, assets/js   

What:  
* CSS, JS, images  

Code:  
```html
    <nav>
      {% for item in site.data.navigation %}
        <a href="{{ item.link }}" {% if page.url == item.link %}
             class="current"{% endif %}>{{ item.name }}
        </a>
      {% endfor %}
    </nav>
```

Add:  
* css/styles.scss  

Code:  
```scss
    ---
    ---
    @import "main";
``` 
> empty front matter at the top tells Jekyll it needs to process the file. @import "main" tells Sass to look for a file called main.scss in the sass directory (_sass/ by default)  

Add:  
* _sass/main.scss  
    
Code:  
```scss
    .current {
      color: green;
    }
```

Add link to stylesheet:  
* layout/default.html  

Code:  
```html
    <link rel="stylesheet" href="/assets/css/styles.css">
```

## Blogging  
Where:  
* _posts  
    
Post:  
* _posts/2018-08-20-bananas.md 
    
Code:  
```md
    ---
    layout: post
    author: jill
    ---
    A banana is an edible fruit – botanically a berry – produced by several kinds
    of large herbaceous flowering plants in the genus Musa.

    In some countries, bananas used for cooking may be called "plantains",
    distinguishing them from dessert bananas. The fruit is variable in size, color,
    and firmness, but is usually elongated and curved, with soft flesh rich in
    starch covered with a rind, which may be green, yellow, red, purple, or brown
    when ripe.
```
> author is a custom variable, it’s not required and could have been named something like creator  

Create new layout:  
* _layouts/post.html  
    
Code:  
```html
    ---  
    layout: default
    ---
    <h1>{{ page.title }}</h1>
    <p>{{ page.date | date_to_string }} - {{ page.author }}</p>
    {{ content }}
```
> This is an example of layout inheritance. The post layout outputs the title, date, author and content body which is wrapped by the default layout.  

List posts:  
* /blog.html (in root directory)  
    
Code:  
```html
    ---
    layout: default
    title: Blog
    ---
    <h1>Latest Posts</h1>
    <ul>
      {% for post in site.posts %}
        <li>
          <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
          <p>{{ post.excerpt }}</p>
        </li>
      {% endfor %}
    </ul>
```

Typically a blog has a page which lists all the posts  

Jekyll makes posts available at site.posts  
* post.url is automatically set by Jekyll to the output path of the post
* post.title is pulled from the post filename and can be overridden by setting title in front matter
* post.excerpt is the first paragraph of content by default

Change:  
* _data/navigation.yml  
    
Code: 
```yml
    - name: Blog  
      link: /blog.html  
```

## Collections  
Like posts but content doesn't habe to be grouped by date.  

Where:  
* /_config.yml (in root directory)  
    
Code:  
```yml
    collections:
      authors:
        output: true
    defaults:
      - scope:
          path: ""
          type: "authors"
        values:
          layout: "author"
      - scope:
          path: ""
          type: "posts"
        values:
          layout: "post"
      - scope:
          path: ""
        values:
          layout: "default"
```
> All posts to automatically have the post layout, authors to have author and everything else to use the default. 
> Now you can remove layout from the front matter of all pages and posts (but not the layouts in `_layouts`). Note that any time you update _config.yml you’ll need to restart Jekyll for the changes to take affect.  

Where:  
* _authors  
    
Add:  
* _authors/jill.md  
    
Code:  
```md
    ---
    short_name: jill
    name: Jill Smith
    position: Chief Editor
    ---
    Jill is an avid fruit grower based in the south of France.
```

Add:  
* _authors/ted.md  
    
Code:  
```md
    ---
    short_name: ted
    name: Ted Doe
    position: Writer
    ---
    Ted has been eating fruit since he was baby.
```

Add:  
* _layouts/staff.html  
    
Iterate over site.authors  

Code:  
```html
    ---
    layout: default
    title: Staff
    ---
    <h1>Staff</h1>
    <ul>
      {% for author in site.authors %}
        <li>
          <h2><a href="{{ author.url }}">{{ author.name }}</a></h2>
          <h3>{{ author.position }}</h3>
          <p>{{ author.content | markdownify }}</p>
        </li>
      {% endfor %}
    </ul>
```
> Since the content is markdown, you need to run it through the markdownify filter. This happens automatically when outputting using {{ content }} in a layout.

Change:  
* _data/navigation.yml  
    
Code:   
```yml
    -name: Staff  
      link: /staff.html  
```

Add:  
* _layouts/author.html  
    
Code:
```html
    ---
    layout: default
    ---
    <h1>{{ page.name }}</h1>
    <h2>{{ page.position }}</h2>
    {{ content }}
    <h2>Posts</h2>
    <ul>
      {% assign filtered_posts = site.posts | where: 'author', page.short_name %}
      {% for post in filtered_posts %}
        <li><a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endfor %}
    </ul>
```

Change:  
* _layouts/post.html  
    
Code:  
```html
    ---
    layout: default
    ---
    <h1>{{ page.title }}</h1>
    <p>
      {{ page.date | date_to_string }}
      {% assign author = site.authors | where: 'short_name', page.author | first %}
      {% if author %}
        - <a href="{{ author.url }}">{{ author.name }}</a>
      {% endif %}
    </p>
    {{ content }}
```

## Deployment  
Get the site ready for production.  

### Gemfile  
Ensures the version of Jekyll and other gems remains consistent across different environments.

Where:  
* /Gemfile (at the root directory)  

Code:  
```
    source 'https://rubygems.org'  
    gem 'jekyll'  
```
Built: 

    $ bundle install   
    
> Installs the gems and creates Gemfile.lock which locks the current gem versions for a future bundle install. 

To see the installation path.  

    $ bundle info jekyll  
     Installed at: /home/benzro/.rbenv/versions/2.6.0/lib/ruby/gems/2.6.0  
     
Update:  
If you ever want to update your gem versions you can run.   

    $ bundle update  
    
### Run server with Gemfile  
When using a Gemfile, you’ll run commands like $ jekyll serve, with bundle exec prefixed. This restricts your Ruby environment to only use gems set in your Gemfile.  

    $ bundle exec jekyll serve  

### Build with Gemfile
You probably get an error message, when you run $ jekyll build or $ jekyll serve with a Gemfile.

    $ bundle exec jekyll build
    
### Plugins  
Create custom generated content specific to your site. many plugins available or you can write your own.  
* jekyll-sitemap - Creates a sitemap file to help search engines index content  
* jekyll-feed - Creates an RSS feed for your posts  
* jekyll-seo-tag - Adds meta tags to help with SEO  
    
Where:  
* /Gemfile  

Code:    
```
    group :jekyll_plugins do
      gem 'jekyll-sitemap'
      gem 'jekyll-feed'
      gem 'jekyll-seo-tag'
    end
```

Run again to bundle and change the Gemfile.lock:  

    $ bundle update  
    
Setup for gems:  
* jekyll-sitemap doesn’t need any setup, it will create your sitemap on build.  
* jekyll-feed and jekyll-seo-tag you need to add tags to `_layouts/default.html`  
    
Change:  
* _layouts/default.html  

Code:  
```html
    {% feed_meta %}
    {% seo %}
```

Run server again and check files:  
* _site/sitemap.xml  
* _site/feed.xml  
    
Check Browser F12, Elements, <HEAD> tag:  
* added one link for jekill-fee: file feed.xml  
* added many lines for: jekyy-seo-tag  
    
### Environments  
Output something in production but not in development.  

Set the environment by using the JEKYLL_ENV environment variable, default is "development".       
Running a command like bundle with global variable "JEKYLL_ENV=production", e.g.

    $ JEKYLL_ENV=production bundle exec jekyll build  
    
Example: an analytics script  
* my-analytics-script.js  

Code:  
```html
    {% if jekyll.environment == "production" %}  
      <script src="my-analytics-script.js"></script>  
    {% endif %}
```
> This code placed anywhere in your code, will only run, if you run build in production mode.      

To only run the code in development you would do the following:  

Code:  
```html
    {% if jekyll.environment == "development" %}
    ....
    {% endif %}
```

Configuration files:  
To switch part of your config settings depending on the environment, use the build/serve command option, for example ` --config _config.yml,_config_development.yml`. Settings in later files override settings in earlier files.  

    $ [bundle exec] jekyll (build | serve) --config _config.yml,_config_development.yml  
    
### Deploy  
Get the site onto a production server. The most basic way to do this is to run a production build.  

    $ JEKYLL_ENV=production bundle exec jekyll build  
    
And copy the contents of `_site` to your server.  
A better way is to automate this process using a CI or 3rd party  

    https://jekyllrb.com/docs/deployment/automated/  

## Cheetsheet  

    https://learn.cloudcannon.com/jekyll-cheat-sheet/  
