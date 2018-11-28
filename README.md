# Penrose Plugin: Linear Algebra

![assets/img/penrose-logo.png](assets/img/penrose-logo.png)

This is a plugin intended for install in [penrose](https://penrose.github.io). It
includes style, substance, and domain specific language (DSL) files.
The visual portion of the site is meant to be human friendly, while the
API served at `/index.json` is a programmatically accessible endpoint
to retrieve the style.

## Setup

 1. Install [Jekyll](https://jekyllrb.com/docs/installation/) locally. For Ruby, I recommend [rbenv](https://github.com/rbenv/rbenv).
 2. Install Jekyll dependencies with `bundle install`
 3. To serve the development server run `bundle exec jekyll serve`

## Folders Included
If you aren't familiar with the structure of a Jekyll site, here is a quick overview:

### Design Files
The design file trios refer to substance, domain specific language (dsl) and style files 
that can be used to generate diagrams.

 - [_src](_src): includes files that are used in "production" examples (those documented under [_domains](_domains)
 - [_dev](_dev): the same organization, but "work in progress" files.
 - [_domain](_domain) is a collection of subfolders (a grouping of designs) that are considered production, and get rendered into the static API. Each design has its own page for images, documentation and usage, and is linked to a specific style, dsl, and substance file.

### Documentation and Site

 - [_config.yml](_config.yml) is the primary configuration file for the site. Variables in this file render as `{{ site.var }}` in the various html includes and templates.
 - [_layouts](_layouts) are base html templates for pages
 - [_includes](_includes) are snippets of html added to layouts
 - [pages](pages) are generic pages that aren't considered docs
 - [assets](assets) includes all static assets
 - [_data](_data) has different data files (they can be in `.yml` or `.csv` to render into the site.

## How Do I...

### Develop new style, substance, or domain specific language files?

You can use the folder [_dev](_dev) to keep these works in progress. They
won't be rendered into the API. The organization is intuitive - you should
place each file in the appropriate subdirectory based on its extension:

```bash
_dev/
├── dsl
├── sty
└── sub
    ├── linearMap_add.sub
    ├── paperSpec.sub
    ├── threeVectorSpaces.sub
    └── vectorsAddition2.sub
```

### Add a new style or domain specific language?

#### 1. Add source files

The production styles and dsls are under [_src](_src), in the appropriately
named folder. Here is a simple example - this trio literally forms the
"negation" example under the "Simple" group, meaning the markdown file
[_domain/simple/negation.md](_domain/simple/negation.md).

```bash
_src/
├── dsl
│   └── linear-algebra.dsl
├── style
│   └── linear-algebra.sty
└── sub
    └── vectorsNegation.sub
```

For a more complex (realistic) example, since the base linear-algebra dsl
and style files can have many examples, we might see a structure like
this:

```bash
_src/
├── dsl
│   └── linear-algebra.dsl
├── style
│   └── linear-algebra.sty
└── sub
    ├── advanced-norm.sub
    ├── determinants.sub
    ├── linearMap.sub
    ├── norm.sub
    ├── scale.sub
    ├── twoVectorSpacesAdditionNegaton.sub
    ├── twoVectorSpaces.sub
    ├── twoVectors.sub
    ├── vectorsAddition-3.sub
    ├── vectorsAddition.sub
    └── vectorsNegation.sub
```

The files under development follow the same organization, but under [_dev](_dev).
They do not render into the API served by the Github repository.

#### 2. Add a domain page

The files above only can have meaning for the user if we *tell* them what trios
are safe to use together. This is the purpose of the [_domain](_domain) folder.
Within this collection are subfolders, each of which is named trio
of a substance, style, and dsl file that can be used with Penrose! Thus, if 
we want to add a trio for the three above, we add a new markdown file in
the "simple" folder. If you want a new category of linear algebra groupings?
Just create a new folder, no work beyond that. Let's look at the existing
"simple" folder for now:

```bash
$ tree _domain/simple/
_domain/simple/
├── negation.md
└── addition.md
```

The subfolder group isn't extremely important, but is more helpful for developers
and users to understand a higher level category for the group. For example,
the linear algebra domain might have a group for "simple" examples represented
by the category folder above, `simple`. It's up to the creator of the domain
folder to decide how to organize this. From an API standpoint, they all
get rendered and so it doesn't matter so much. 


#### 3. Describe it With Metadata

Let's take a look at the markdown for one of the files `negation.md`, first
the header of the file, which is called the "frontend matter". Notice that
it has the trio of the style, dsl, and substance file, like this:

```yml
---
title: Negation of Vector Spaces
category: Simple
order: 1

sty: linear-algebra.sty
dsl: linear-algebra.dsl
sub: vectorsNegation.sub
---
```

#### 3. Update the changelog

The files in [_posts](_posts) render into the site changelog. Please add a new
entry with meaningful information when you add, remove, or otherwise change content.
This isn't a perfect way to track changes, but it's a best effort

## Usage

```bash
$ /penrose {{ page.sub }} {{ page.sty }} {{ page.dsl }}
```

<img alt="negation" src="../img/negation.png">
```

Right below that is the content area. As you can see, we've written usage,
detailed examples, and anything else for the user to know! This is just markdown.
Pictures are very useful. 

**Special Attributes**

We have some special attributes, in case they are needed. You can optionally
add the following to the page frontend matter:

 - **hidden**: If you have a design trio that was considered production and you need to hide it from the API for some reason (you can imagine an emergency fix is needed and you want to take it offline, but perhaps not completely remove it from the documentation pages) you can add "hidden: true" to the frontend matter. It will render (for a human) on the site, but not be available via the API.


### API Interaction

The above details how to add content, and by simply adding files to [_domain](_domain).
They will automatically be added to the API. But where is it? You can find the API
at the /library.json endpoint served by the repository, and linked from the main
page. If I'm running a local server, here is how to access it via python:

```python
import requests
url = "http://localhost:4000/domain-linear-algebra/library.json"
response = requests.get(url)
```

Did we get a 200 (successful) response?

```python
response
<Response [200]>

response.status_code
200
```

Let's look at our data.

```python
results = response.json()
```

There are keys for a unique id (the Github repository url), a set of links,
and then the data (each trio that has been added).

```python
results.keys()
# dict_keys(['id', 'links', 'data'])
```

For example, here is the id. The id is the namespace for Penrose
designs - all of the "linear-algebra" domain can be found under this
namespace. The namespace is tested together to be added to the main domains
endpoint (to be developed) by validating that the designs in the API
function.

```python
results['id']
'penrose/domain-linear-algebra'
```

And here are links associated with the data. Importantly, we have a url to
get to the "human friendly" page, but also the Github repository serving the
data.

```python
results['links']
{'self': 'http://localhost:4000/domain-linear-algebra/library.json',
 'url': 'https://www.github.com/penrose/domain-linear-algebra'}
```

Finally, the data itself! This is a list of the design trios, each one
associated with one of the markdown files we described above.

```python
results['data'][0]
{'dsl': 'http://localhost:4000/domain-linear-algebra/src/dsl/linear-algebra.dsl',
 'group': 'Simple',
 'id': 'penrose/domain-linear-algebra:simple/addition',
 'name': 'simple/addition',
 'sty': 'http://localhost:4000/domain-linear-algebra/src/sty/linear-algebra.sty',
 'sub': 'http://localhost:4000/domain-linear-algebra/src/sub/vectorsAddition.sub'}
```

We mostly need a name, and then a link to each of the substance (sub), style (sty)
and dsl files. You will notice that each entry here has a name (its path
in the [_domains](_domains) folder, but also a unique id! In the example above,
we see `penrose/domain-linear-algebra:simple/addition`. This also tells the user
where to find it - under Github organization "penrose," repository "domain-linear-algebra"
and subfolder in domains "simple/addition.md".
