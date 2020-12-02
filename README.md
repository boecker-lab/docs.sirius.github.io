# SIRIUS Documentation GitHub Repository
<span>**<span style="color: red">\[We are currently building this documentation, and some 
pages are still work in progress\]</span>**</span>

This repo contains the sources of the SIRIUS online documentation at 
<https://boecker-lab.github.io/docs.sirius.github.io/>. 
We try to keep this documentation as uptodate as possible. 
Contributions from the community are very welcome.

If your are looking for the sources of the SIRIUS Software see  [*boecker-lab/sirius*](https://github.com/boecker-lab/sirius). 
To download the SIRIUS software see <https://bio.informatik.uni-jena.de/software/sirius/>.
For bug reports  and feature requests regarding the SIRIUS software please
use  [*boecker-lab/sirius*](https://github.com/boecker-lab/sirius/issues).

## Contributing to the SIRIUS Documentation

Help us improving SIRIUS Documentation!
- For bug reports and feature requests regarding this documentation, please open an "Issue" 
[on this GitHub repository](https://github.com/boecker-lab/docs.sirius.github.io/issues).
- To contribute to the SIRIUS documentation, simply edit a page
 (located in the "[_pages](https://github.com/boecker-lab/docs.sirius.github.io/tree/main/docs/_pages)" directory) and 
 click *promote changes* to perform an ad hoc pull request. 
- For more advanced editing, fork the [*boecker-lab/docs.sirius.github.io*](https://github.com/boecker-lab/docs.sirius.github.io) 
repository, and make a *Pull Request* with your edits from your fork to the primary repository. For further information 
on how to build and run this documentation locally see [Setup for Local Development](#setup-for-local-development)

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


