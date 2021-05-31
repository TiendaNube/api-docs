# Installation

## Requirements: 

- [Ruby](https://www.ruby-lang.org/en/downloads/) version 2.4.0 or higher, including all development headers (check your Ruby version using ruby -v)
- [RubyGems](RubyGems) (check your Gems version using gem -v)
- [GCC](https://gcc.gnu.org/install/) and [Make](https://www.gnu.org/software/make/) (check versions using gcc -v,g++ -v, and make -v)

## Install Jekyll

```bash
gem install jekyll bundler
```

For more information, please refer to the [official documentation](https://jekyllrb.com/docs/).


# Develop on the local environment

To build and run the site locally, run the following command: 

```bash
bundle exec jekyll serve
```

The site will be available on [http://locahost:4000](http://locahost:4000/tiendanube.api.docs)


# Assets

We are using Github Pages to publishing the site. For that, we need to add a baseUrl to refers to the routes and assets. 
We are using `/tiendanube.api.docs` as `baseUrl`. 

# Config 

The file `_config.yml` has the site configuration. You could use it to add global variables. 

# Creating a page

Either HTML or Markdown is supported to create a page. We are mostly using Markdown and a default template using the following header: 

```markdown
---
layout: default
---
```

# Publishing 

1. Make sure that the last version was built. 
2. Push your changes 
3. Make a new PR
4. Merge to `master`

Once the changes are in master will be automatically updated on the site. 
