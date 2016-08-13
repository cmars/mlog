# CLI UX

## Authoring Commands

    mlog edit <name> # opens an editor on given page

    mlog new         # open editor on new page

A YAML representation of the page object model is opened in $EDITOR either way.

Both commands support attaching files with a flag:

        -attach file1[,file2,...]
        -attach name1=file1[,name2=file2,...]

Files may be attached with a logical name (local to the page) or not. These
files are appended to the page content when $EDITOR is opened.

Files may be URLs.

## Search & Administration Commands

    mlog list        # list pages

    mlog show        # show matching pages

    mlog delete      # remove matching pages

## Page object model

Author interacts with the page as a YAML text file.

When a new page is created, it is populated with some defaults to get started.

When an existing page is edited, it is populated with the most recent (or given) revision.

When a page is deleted, it is removed from the pages list (but is still available in history).

## Operating Commands

    mlog serve \     # HTTP server
        -addr=:8080
        -noms=http://localhost:8000::mlog

    mlog render      # Render some or all of the site as static files

# Data Model

## Mlog

An Mlog consists of:

- Header and footer contents (markdown)
- A list of pages
- An index of pages by IDs and aliases.
- A map of tag names to page refs

    struct Mlog {
        header: String,
        footer: String,
        pages: struct Page,
        index: Map<String, struct Page>,
        tags: Map<String, List<struct Page>>,
    }

## Page

- Contents (markdown)
- Attachments
 
    struct Page {
        contents: String,
        attachments: Map<String, Attachment>,
    }

## Attachment

- Contents (binary blob)
- MIME type

    struct Attachment {
        contents: Blob,
        mimeType: String,
    }

# Package dependencies

Packages will be vendored with git subtree. Reusable components will be spun
off into reusable separate upstreams.

## Storage

[Noms](github.com/attic-labs/noms/go) of course.

## Page contents

[Blackfriday](github.com/russross/blackfriday) is used to render markdown.
[Options.ReferenceOverride](https://godoc.org/github.com/russross/blackfriday#Options) is
set up to resolve attachments.

HTML output is then filtered by [bluemonday](https://github.com/microcosm-cc/bluemonday).

## Command line

gopkg.in/urfave/cli.v1
