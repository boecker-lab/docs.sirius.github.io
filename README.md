# SIRIUS Documentation GitHub Repository
<span>**<span style="color: red">\[We are currently building this documentation, and some 
pages are still work in progress\]</span>**</span>

This repo contains the sources of the SIRIUS online documentation at 
<https://boecker-lab.github.io/docs.sirius.github.io/>. 
We try to keep this documentation as up-to-date as possible.
Contributions from the community are very welcome. The content of this documentation is written 
[Markdown](https://guides.github.com/features/mastering-markdown/), so no programming skills 
needed to create content.

If your are looking for the sources of the SIRIUS Software see  [*boecker-lab/sirius*](https://github.com/boecker-lab/sirius). 
To download the SIRIUS software see <https://bio.informatik.uni-jena.de/software/sirius/>.
For bug reports  and feature requests regarding the SIRIUS software please
use  [*boecker-lab/sirius*](https://github.com/boecker-lab/sirius/issues).

## Contributing to the SIRIUS Documentation
**Help us improving the SIRIUS Documentation!**

- For bug reports and feature requests regarding this documentation, please open an "Issue" 
[on this GitHub repository](https://github.com/boecker-lab/docs.sirius.github.io/issues).
- To contribute to the SIRIUS documentation, simply edit a page 
  (located in the "[_pages](https://github.com/boecker-lab/docs.sirius.github.io/tree/main/docs/_pages)" directory) and 
 click *promote changes* to perform an ad hoc pull request. 
- For more advanced editing, fork the [*boecker-lab/docs.sirius.github.io*](https://github.com/boecker-lab/docs.sirius.github.io) 
repository, and make a *Pull Request* with your edits from your fork to the primary repository. For further information 
on how to build and run this documentation locally see [Setup for Local Development](#setup-for-local-development)

### Creating a new page
New pages can be created by creating a new Markdown file (`.md`) under `docs/_pages/`.
The link where the new site will appear and its title can specified by adding the following header to the new markdown file:
```
---
permalink: /changelog/
title: "Changelog"
---

```

### Creating a menu entry
Me entries are defined in the`docs/_data/navigation.yml`. Let's say we want to create an entry for the `/io`
page with two children referencing the headlines ```# Input``` and ```# Output```: 

```yaml
- title: "Input, Output and Formats"
    url: /io
    children:
      - title: "Input"
        url: /io/#input
      - title: "Output"
        url: /io/#output
```

### Insert relative links
It is highly recommended to use links relative to the sites root for referencing site internal resources.
This can easily be achieved using the [Liquid templating system](https://jekyllrb.com/docs/datafiles/) ```{{ "/io/" | relative_url }}```.
Let's say we want to link "go to IO" to the page `/io` of our site:

```
[go to IO]({{ "/io" | relative_url }})
```


#### Link to anchors (e.g. headlines)
Anchors are automatically created for each headline. The *tag* used for referencing the anchor is created from the 
headlines text with the following rules.

1. It downcases the string
1. remove anything that is not a letter, number, space or hyphen (see the source for how Unicode is handled)
1. changes any space to a hyphen. 
1. If that is not unique, add "-1", "-2", "-3",... to make it unique

Example: ```### My great Heading!``` will get the *tag* ```#my-great-heading```. 

Custom tags can be defined with:  ```### My great Heading! {#custom-anchor-tag}```

Let's say we want to link "go to Input" to the heading `## Input` of the page `/io`:
```
[go to Input]({{ "/io/#input" | relative_url }})
```



### Insert images
The image directory is `<SIRIUS_DOC>/docs/assets/images/`. If there is no god reason to do it differently
we save all images at this location.
In principle standard markdown syntax can be used to show images. Path resolution can be done
with the [Liquid templating system](https://jekyllrb.com/docs/datafiles/), e.g. `{{ "/assets/images/project-space.svg" | relative_url }}`

#### Markdown image
```
![Schema of the SIRIUS project-space]({{ "/assets/images/project-space.svg" | relative_url }})
```
#### HTML image (scalable)
```
<img src="{{ "/assets/images/project-space.svg" | relative_url }}" alt="Schema of the SIRIUS project-space1" height="24" width="48">
```

#### HTML image (responsive)
This is the preferred method to include images but also a bit lengthy.
```
{% capture fig_img %}
![Foo]({{ "/assets/images/project-space.svg" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Schema of the SIRIUS project-space.</figcaption>
</figure>
``` 

### Insert Maths
You can insert mathematical fomulas using latex math syntax within the Markdown files.
The rendering is done by [MathJax](https://www.mathjax.org/).

#### Inline Maths
```
Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor 
invidunt ut labore et dolore magna aliquyam erat $(200 \cdot \frac{ppm_{max}}{10^6})$.
```

#### Maths Block
Note the empty line before and after the Maths block!
```
Lorem ipsum dolor sit amet, consetetur sadipscing elitr, 
sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat.

$$(200 \cdot \frac{ppm_{max}}{10^6})$$

At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, 
no sea takimata sanctus est Lorem ipsum dolor sit amet. 
```




## Setup for Local Development
This page is based on [Jekyll](https://jekyllrb.com/) and uses the [Minimal Mistakes](https://mmistakes.github.io/minimal-mistakes/).
The Markdown is parsed by [Kramdown](https://kramdown.gettalong.org/index.html).
We assume the repository has been cloned into `<SIRIUS_DOC>`. 

1. Install *Ruby*, *RubyGems* and *Jekyll*, see <https://jekyllrb.com/docs/installation/>.
2. Navigate into the Jekyll project `<SIRIUS_DOC>/docs`.
3. Install required gems

```bundle install```

4. (Optional) Update required gems. Might be necessary if incompatible versions of gems used
by this project are already on you system.

```bundle update```

5. Serve locally 

```bundle exec jekyll serve```

Per default the local server is running at <http://127.0.0.1:4000/docs.sirius.github.io/>.   
The output should look like this:
```
Configuration file: ~/docs.sirius.github.io/docs/_config.yml
            Source: ~/docs.sirius.github.io/docs
       Destination: ~/docs.sirius.github.io/docs/_site
 Incremental build: disabled. Enable with --incremental
      Generating... 
      Remote Theme: Using theme mmistakes/minimal-mistakes
       Jekyll Feed: Generating feed for posts
   GitHub Metadata: No GitHub API authentication could be found. Some fields may be missing or have incorrect data.
                    done in 11.675 seconds.
~/gems/gems/pathutil-0.16.2/lib/pathutil.rb:502: warning: Using the last argument as keyword parameters is deprecated
 Auto-regeneration: enabled for '/home/fleisch/workspace/docs.sirius.github.io/docs'
    Server address: http://127.0.0.1:4000/docs.sirius.github.io/
  Server running... press ctrl-c to stop.
```

6. Deploy to github pages by pushing your changes into the `master` branch of this 
repository. It may take a few minutes until the page is build by GitHub.


