# NAME

Mojolicious::Plugin::Gzip - Plugin to Gzip Mojolicious responses

# STATUS

<div>
    <a href="https://travis-ci.org/srchulo/Mojolicious-Plugin-Gzip"><img src="https://travis-ci.org/srchulo/Mojolicious-Plugin-Gzip.svg?branch=master"></a>
</div>

# SYNOPSIS

    # Mojolicious::Lite
    plugin 'Gzip';

    # With minimum size in bytes required before gzipping. Default is 860.
    plugin Gzip => {min_size => 1500};

    # Mojolicious
    $app->plugin('Gzip');

    # With minimum size in bytes required before gzipping. Default is 860.
    $app->plugin(Gzip => {min_size => 1500});

# WARNING

This module can be inefficient for large static files. It loads the entire file into memory and then gzips it, resulting in both the original file and
the gzipped one in memory at once. By default, [Mojolicious](https://metacpan.org/pod/Mojolicious) serves static files from disk, which is much more memory efficient. For static files, you may want
to compress them ahead of time or use something like a reverse proxy with [Nginx](https://www.nginx.com/) to serve your static files or a service like
[Cloudlfare](https://www.cloudflare.com/), which will compress files for you. For gzipping dynamic content, you should consider using ["compress" in Mojolicious::Renderer](https://metacpan.org/pod/Mojolicious::Renderer#compress)
instead.

# DESCRIPTION

[Mojolicious::Plugin::Gzip](https://metacpan.org/pod/Mojolicious::Plugin::Gzip) gzips all responses equal to or greater than a ["min\_size"](#min_size) by using the ["after\_dispatch" in Mojolicious](https://metacpan.org/pod/Mojolicious#after_dispatch) hook.
[Mojolicious::Plugin::Gzip](https://metacpan.org/pod/Mojolicious::Plugin::Gzip) will only gzip a response if all of these conditions are met:

- The ["accept\_encoding" in Mojo::Headers](https://metacpan.org/pod/Mojo::Headers#accept_encoding) header contains 'gzip'.
- The ["body\_size" in Mojo::Content](https://metacpan.org/pod/Mojo::Content#body_size) of the response is greater than or equal to ["min\_size"](#min_size).
- The ["code" in Mojo::Message::Response](https://metacpan.org/pod/Mojo::Message::Response#code) is 200.
- The ["content\_encoding" in Mojo::Headers](https://metacpan.org/pod/Mojo::Headers#content_encoding) for the response is not set.

[Mojolicious::Plugin::Gzip](https://metacpan.org/pod/Mojolicious::Plugin::Gzip) will do these things if those conditions are met:

- Set ["body" in Mojo::Message](https://metacpan.org/pod/Mojo::Message#body) to the gzipped version of the previous ["body" in Mojo::Message](https://metacpan.org/pod/Mojo::Message#body).
- Set ["body\_size" in Mojo::Message](https://metacpan.org/pod/Mojo::Message#body_size) to the size of the gzipped content.
- Set ["content\_encoding" in Mojo::Headers](https://metacpan.org/pod/Mojo::Headers#content_encoding) to "gzip".
- If ["etag" in Mojo::Headers](https://metacpan.org/pod/Mojo::Headers#etag) was set, append "-gzip" to the existing ["etag" in Mojo::Headers](https://metacpan.org/pod/Mojo::Headers#etag). This is done according to [RFC-7232](https://tools.ietf.org/html/rfc7232#section-2.3.3), which
states that ETags should be content-coding aware.
- Use ["append" in Mojo::Headers](https://metacpan.org/pod/Mojo::Headers#append) to append "Accept-Encoding" to the ["vary" in Mojo::Headers](https://metacpan.org/pod/Mojo::Headers#vary) header.

# OPTIONS

## min\_size

    # Mojolicious::Lite
    plugin 'Gzip' => {min_size => 1500};

    # Mojolicious
    $app->plugin(Gzip => {min_size => 1500});

Sets the minimum ["body\_size" in Mojo::Content](https://metacpan.org/pod/Mojo::Content#body_size) required before response content will be gzipped. If the ["body\_size" in Mojo::Content](https://metacpan.org/pod/Mojo::Content#body_size) is greater than or equal to ["min\_size"](#min_size), then it will be
gzipped. Default is 860.

# AUTHOR

Adam Hopkins <srchulo@cpan.org>

# COPYRIGHT

Copyright 2019- Adam Hopkins

# LICENSE

This library is free software; you can redistribute it and/or modify
it under the same terms as Perl itself.

# SEE ALSO

- [MojoX::Encode::Gzip](https://metacpan.org/pod/MojoX::Encode::Gzip)
- [Mojolicious](https://metacpan.org/pod/Mojolicious)
- [https://mojolicious.org](https://mojolicious.org)
