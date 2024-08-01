# Directory structure

> See [details](https://gohugo.io/getting-started/directory-structure/).

## Site skeleton

```
my-site/
├── archetypes/
│ └── default.md
├── assets/
├── content/
├── data/
├── i18n/
├── layouts/
├── public/       <-- created when you build your site
├── resources/    <-- created when you build your site
├── static/
├── themes/
└── hugo.toml <-- site configuration
```

## Directories

### archetypes

The `archetypes` directory contains templates for new content. See [details](https://gohugo.io/content-management/archetypes/).

### assets

The `assets` directory contains global resources typically passed through an asset pipeline. This includes resources such as images, CSS, Sass, JavaScript, and TypeScript. See [details](https://gohugo.io/hugo-pipes/introduction/).

### config

The `config` directory contains your site configuration, possibly split into multiple subdirectories and files. For projects with minimal configuration or projects that do not need to behave differently in different environments, a single configuration file named `hugo.toml` in the root of the project is sufficient. See [details](https://gohugo.io/getting-started/configuration/#configuration-directory).

### content

The `content` directory contains the markup files (typically Markdown) and page resources that comprise the content of your site. See [details](https://gohugo.io/content-management/organization/).

### data

The `data` directory contains data files (JSON, TOML, YAML, or XML) that augment content, configuration, localization, and navigation. See [details](https://gohugo.io/content-management/data-sources/).

### i18n

The `i18n` directory contains translation tables for multilingual sites. See [details](https://gohugo.io/content-management/multilingual/).

### layouts

The `layouts` directory contains templates to transform content, data, and resources into a complete website. See [details](https://gohugo.io/templates/).

### public

The `public` directory contains the published website, generated when you run the `hugo` or `hugo server` commands. Hugo recreates this directory and its content as needed. See [details](https://gohugo.io/getting-started/usage/#build-your-site).

### resources

The `resources` directory contains cached output from Hugo’s asset pipelines, generated when you run the `hugo` or `hugo server` commands. By default this cache directory includes CSS and images. Hugo recreates this directory and its content as needed.

### static

The `static` directory contains files that will be copied to the public directory when you build your site. For example: `favicon.ico`, `robots.txt`, and files that verify site ownership. Before the introduction of [page bundles](https://gohugo.io/getting-started/glossary/#page-bundle) and [asset pipelines](https://gohugo.io/hugo-pipes/introduction/), the `static` directory was also used for images, CSS, and JavaScript.

### themes

The `themes` directory contains one or more [themes](https://gohugo.io/getting-started/glossary/#theme), each in its own subdirectory.
