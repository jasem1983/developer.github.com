# developer.github.com

**NOTE: The developer.github.com website is no longer open-source.**

We have moved this repository into [our github-archive organization](https://github.com/github-archive) to signify that we are no longer accepting open-source contributions to this repository. We want to thank the hundreds of contributors for their assistance over the years.

The decision to close-source the site stems from a variety of reasons:

1. We actually already _have_ a close-sourced site, which is where we wrote documentation for unreleased features. We designed additional tooling to support this workflow, but updating the documentation is a process we'd like to simplify.
2. We believe that any open-source project—be it documentation or software—ought to have dedicated maintainers. It became difficult to keep this repository open-source because it was maintained by the best efforts of a small group of people. Closing the site allows us to focus on what's important, without feeling guilty at missing reviews from open-source contributors.

We think that the tooling we used to build this site is pretty interesting, so we're not getting rid of everything. We hope that what remains can be used as a source of inspiration for your own static site.

If you find something that needs to be fixed, you can always [contact our terrific Support team](https://github.com/contact?form%5Bsubject%5D=Moving+developer.github.com+to+github-archive).

Thank you!

* * *

This was the GitHub API documentation, built with [Nanoc][nanoc].

## Development

You can fetch the latest dependencies by opening the command line and running `script/bootstrap`:

``` sh
$ script/bootstrap
==> Installing gem dependencies…
==> Installing npm dependencies…
```

You'll need Ruby and Node installed on your system. The required versions for each of these languages can be found in the *.ruby-version* and *package.json* files, respectively.

You can run `bundle exec rake build` to generate the site, but it's often more useful
to simply build the server *and* start the site at the same time.

Nanoc compiles the site into static files living in `output`.  It's
smart enough not to try to compile unchanged files.

You can start the site with `script/server`:

``` sh
$ script/server
Loading site data...
Compiling site...
   create     [0.28s]  output/index.html
   create     [1.31s]  output/v3/gists/comments/index.html
   identical  [1.92s]  output/v3/gists/index.html
   identical  [0.25s]  output/v3/issues/comments/index.html
   update     [0.99s]  output/v3/issues/labels/index.html
   update     [0.05s]  output/v3/index.html
   …

Site compiled in 5.81s.
```

The site is hosted at `http://localhost:4000`.

Nanoc has [some nice documentation](http://nanoc.ws/docs/tutorial/) to get you started.  Though if you're mainly concerned with editing or adding content, you won't need to know much about Nanoc.

[nanoc]: http://nanoc.ws/

### Enterprise

To generate the `/enterprise` versions, pass in the Enterprise version to `script/server`. For example:

``` sh
$ script/server 2.6
```

Note that live reloading is not available for Enterprise documentation.

## Styleguide

Not sure how to structure the docs?  Here's what the structure of the
API docs should look like:

    # API title

    {:toc}

    ## API endpoint title

        [VERB] /path/to/endpoint

    ### Parameters

    Name | Type | Description
    -----|------|--------------
    `name`|`type` | Description.

    ### Input (request JSON body)

    Name | Type | Description
    -----|------|--------------
    `name`|`type` | Description.

    ### Response

    <%= headers 200, :pagination => default_pagination_rels, 'X-Custom-Header' => "value" %>
    <%= json :resource_name %>

**Note**: We're using [Kramdown Markdown extensions](http://kramdown.gettalong.org/syntax.html), such as definition lists.

### JSON Responses

We specify the JSON responses in Ruby so that we don't have to write
them by hand all over the docs.  You can render the JSON for a resource
like this:

```erb
<%= json :issue %>
```

This looks up `GitHub::Resources::ISSUE` in `lib/resources.rb`.

Some actions return arrays.  You can modify the JSON by passing a block:

```erb
<%= json(:issue) { |hash| [hash] } %>
```

There is also a rake task for generating JSON files from the sample responses in the documentation:

``` sh
$ rake generate_json_from_responses
```

The generated files will end up in *json-dump/*.

### Terminal blocks

You can specify terminal blocks by using the `command-line` syntax highlighting.

    ``` command-line
    $ curl foobar
    ```

You can use certain characters, like `$` and `#`, to emphasize different parts
of commands.

    ``` command-line
    # call foobar
    $ curl <em>foobar<em>
    ....
    ```

For more information, see [the reference documentation](https://github.com/gjtorikian/extended-markdown-filter#command-line-highlighting).

## Licenses

The code to generate the site (everything excluding the assets, content,
and layouts directories) as well as the code samples on the site are
licensed under
[CC0-1.0](https://creativecommons.org/publicdomain/zero/1.0/legalcode).
CC0 waives all copyright restrictions but does not grant you any trademark
permissions.

Site content (everything in the assets, content, and layouts directories,
excluding files under open source licenses individually marked) is licensed
under [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/). CC-BY-4.0
gives you permission to use content for almost any purpose but does not grant
you any trademark permissions, so long as you note the license and give credit,
such as follows:

> Content based on
> <a href="https://github.com/github/developer.github.com">developer.github.com</a>
> used under the
> <a href="https://creativecommons.org/licenses/by/4.0/">CC-BY-4.0</a>
> license.</a>

This means you can use the code and content in this repository except for
GitHub trademarks in your own projects.

When you contribute to this repository you are doing so under the above
licenses.
