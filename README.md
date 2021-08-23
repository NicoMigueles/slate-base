# Slate base template

An easy dockerized environment to develop and build slate docs.

## Development

To run the development environment use the following command

```shell
docker run --rm --name slate -p 4567:4567 -v $(pwd -W)/source:/srv/slate/source slatedocs/slate serve
```

This compiles the docs on save and serves the generated static web into http://localhost:4567

## Build

```shell
docker run --rm --name slate -v $(pwd)/build:/srv/slate/build -v $(pwd)/source:/srv/slate/source slatedocs/slate
```

This builds the static web page in the `build` folder.

## Editing the docs

The main content is in `index.html.md`. And for visual customizations you can modify the css in the stylesheets' folder.

Also, you can add your own logotype under `images/logo.svg` (An SVG is recommended but not mandatory, you can change the import in `layouts/layout.erb` line 66)

For a full documentation on how markdown reacts in slate see https://github.com/slatedocs/slate/wiki/Markdown-Syntax