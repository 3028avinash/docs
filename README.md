# Redis Hugo site template

## Files and folders

* **/archetypes**: A Markdown file needs to have some front matter. An archetype defines which front matter is used when using `hugo new content`. Right now, the only supported archetype is the default one. **Note:** We might want to add additional archetypes in the future because most of our pages contain additional meta data properties like `linkTitle`. 
* **/content**: This folder contains the markdown files. We will have the subfolders like `/develop`, `/integrate`, and `/operate`
* **/assets**: CSS files, site-wide icons and images
* **/data**: Data files that are accessed by Hugo. The data is then rendered with the help of short codes or partials.
* **/layouts/partials**: HTML templates that are used across sites. Examples are TOC-s, breadcrumps, or headers. 
* **/layouts/$type**: Each page type has at least the following templates to implement `single.html` and `list.html`. The `single` template is used to render a concrete page. The `list` template is used to render a collection (e.g., all sub-pages).
* **/layouts/home.html**: The home page of the site. So the page that is entered when you open the root path
* **/layouts/404.html**: The default 404 page.
* **/layouts/shortcodes** HTML template snippets that you want to leverage within Markdown files. Examples are tabbed code examples or notification blocks.
* **/public**: This is the folder that is generated by Hugo. This content needs to be published by you.
* **/static**: Any static files that need to be accessed by the site, e.g., CSS or JavaScript.
* **/package.json**: Node.js dependencies. Tailwind, for instance, is installed via the Node package manager (`npm`).
* **/config.toml**: Hugo's site configuration, like the root path and menu items. Hugo can access configuration elements when rendering the site. So you can define custom configuration settings here.
* **/syntax.css**: Hugo supports syntax highlighting via shortcodes. The highligher is configured via this CSS file.
* **/Makefile**: We use make to wrap some Hugo commands and to add addtional build steps.
* **/tailwind.config.js**: This is the Tailwind CSS framwork's configuration file.
* **/postcss.config.js**: Needed to make Tailwind statically accessible to the site.

## Requirements

You will need the following tools to build the site locally:

- `python3`: Python >=3.8
- `node` and `npm`: Node.js >=19.7.0, and the Node.js package manager >= 9.5.0
- `hugo`: Hugo site generator >= v0.111.2 as extended edition
- `make`: To run the make script
- `git`: To manage the source code of the documentation

## Build script and data files

The site can be built via `make all`. Here is what's executed:

1. Clean the current folder by deleting the generated output of a previous build
2. Install the necessary Node.js and Python dependencies
3. Build the components that were fetched from external Github repositories (e.g., client examples). This fetches source code files and generates data files.
4. Execute the `hugo` command to generate the HTML pages. The build result can be found within the folder `/public`

The command `make serve` performs the same steps, but serves the web pages on the local host for development purposes.

The `Makefile` contains details about the executed commands.

## Build pipeline

The build pipeline that is defined within `.github/workflows/main.yml` builds the documentation and uploads it into a GCS bucket:

1. Install Hugo
2. Check the branch out
3. Perform the build as stated before by using `make all`
4. Install the Google Cloud CLI
5. Authenticate to the GCS bucket
6. Validate the branch name
7. Sync the branch with a GCS folder


## Hugo specifics

### Relative links

We are using the following syntax for Hugo relrefs:

```
[Link title]({{< relref "link relative to the site's base url" >}})
```

Here is an example:

```
[Data structure store]({{< relref "/develop/get-started/data-store" >}})
```

It's strongly advised to use `relref` because it provides the following advantages:

1. Links are checked at build time. Any broken reference within the site is reported and prevents a successful build.
2. References are prefixed with the site's base URL, which means that they work as in builds with a different base URL


The following needs to be taken into account when using `relref`: The reference `/develop/get-started/data-store` and `/develop/get-started/data-store/` don't mean the same. You must use the trailing slash if the referenced article is an `_index.md` file within a folder (e.g., `.../data-store/` for `.../data-store/_index.md`). Othersise, you should not use the trailing slash (e.g., `.../get-started/data-store.md`).

RelRefs with dots (`.`) and hashtags (`#`) in the reference name, such as `/commands/ft.create` or `/develop/data-types/timeseries/configuration#compaction_policy`, don't seem to work. Please use the `{{< baseurl >}}` as a workaround in that case. Here is an example:

```
[compaction]({{< baseurl >}}/develop/data-types/timeseries/configuration#compaction_policy)
```

## Images

The image shortcode doesn't need to be closed anymore. Here is an example;

```
{{< image filename="/images/rc/icon-database-update-status-pending.png" alt="Database update" >}}
```

The `filename` property value can be any file name path which is relative to the site's base path.

We added a new property `class` which allows you to override the CSS class of the image. Images have by default a `block` display in Tailwind. You can change this by setting the class property to `inline`. The following example shows two images that are in a single line:

```
{{< image filename="/images/rc/icon-database-update-status-pending.png#no-click" alt="Pending database status" class="inline" >}} &nbsp; {{< image filename="/images/rc/icon-database-update-status-active.png#no-click" alt="Active database status" class="inline" >}}
```

## Templating

Variables are scoped in the context of their block. Overriding a variable within an if block doesn't change the variable value as soon as you leave that block. This is a specific limitation of the Go templating language.

The following will give you the `/` outside of the condition block if the path was initially set to `/`:

```
{{ $path := $parsed.Path }}
{{ if (eq $path "/") }}
	{{ $path = "" }}
{{ end }}

{{- $path -}}
```

The solution is to use a scratchpad for storing the 'global' state:

```
{{ $path := $parsed.Path }}
{{ if (eq $path "/") }}
	{{ .Scratch.Set "path" ""}}
{{ else }}
    {{ .Scratch.Set "path" $path}}
{{ end }}

{{- .Scratch.Get "path" -}}
```

### Children pages

The redis.io template does add a list of links to child pages by default to the bottom of each section page. You can prevent this by adding the following front matter property:

```
hideListLinks: true
```