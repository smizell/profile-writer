# Profile Writer

The goal of this library is to make it easier to write ALPS documents that can be distributed as a website, XML file, or JSON file. Instead of writing XML or JSON directly, write Markdown files and build those files when you're ready to distribute.

Look at the [example directory](./example) to see how this might work.

## Usage

```sh
npm install profile-writer -g
```

To build the directory, run:

```sh
profile-writer build
```

This will look for a `profile` directory and output to a `build` directory. Look at the commands arguments with `--help` to determine ways to override these defaults.

## Build

The structure of the ALPS document is determined by the directory and file names. The first part of file name determines the `id` of the descriptor and the extension of the file determines the type of content. Directories provide a way for nesting descriptors.

### Directory structure

- An `index.<ext>` file at the root of the profile directory will be used as documentation for the entire ALPS document.
- Any file with the pattern `<id>.<ext>` will be used as the descriptor with a matching `id`. The contents will be used as the `doc` value.
- Any file with `<id>/index.<ext>` will be used as the descriptor with a matching `id`. The contents of the `index.<ext>` file will be used as the `doc` value.
- Any file with `<id1>/<id2>.<ext>` will mean that a descriptor of `id1` will have a nested descriptor of `id2`. The contents of the file `<id2>.<ext>` will be used as the `doc` for the `id2` descriptor.

### File types

The supported file extensions are:

- `.md` will result in Markdown content
- `.txt` will result in plain text content
- `.html` will result in HTML content

Any extension that is not understood will be treated as plain text.

### Frontmatter

ALPS attributes go in the frontmatter of the documents. It will look something like this:

```md
---
type: safe
rt: contact
---

A link to a contact.
```

Here we see the attributes `type` and `rt` which pertain to `descriptor` attributes specifying the values within the file.

**Note**: The default type will be `semantic`.

### Output

Building the profile will result in three outputs:

- HTML version of the entire profile directory
- XML version of the profile
- JSON version of the profile

## CLI

### Build the profile

```sh
profile-writer build --dir ./profile --format json
```

### Creating new descriptors

This will create a new file for writing in the `docs` directory. It will handle the nesting if applicable.

```sh
profile-writer new customer/address/street_name --format markdown
```

In this example, if `customer/address.md` is a file, this will:

- Create a `customer/address` directory
- Move `customer/address.md` to be `customer/address/index.md`
- Create `customer/address/street_name.md` with frontmatter
