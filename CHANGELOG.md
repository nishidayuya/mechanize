# Mechanize CHANGELOG

## next / unreleased

### Requirements

* Mechanize now requires Ruby 2.6 or newer.


## 2.8.5 / 2022-06-09

### Security

Fixes low-severity CVE-2022-31033, "Authorization header leak on port redirect." See [GHSA-64qm-hrgp-pgr9](https://github.com/sparklemotion/mechanize/security/advisories/GHSA-64qm-hrgp-pgr9) for more details.


## 2.8.4 / 2022-01-17

### Fix

* `Mechanize::CookieJar#load` calls `Psych.safe_load` when using Psych >= 3.1


## 2.8.3 / 2021-11-11

### Update

* Update the "Linux Firefox" user agent string to rev94 (#587) Thank you, @ncs1!


## 2.8.2 / 2021-08-06

### Dependencies

* Update dependency on Addressable from `~>2.7` to `~>2.8`. (#584) @yidingww


## 2.8.1 / 2021-05-09

### Fix

* Gracefully handle parsing errors that contain an invalid byte sequence. Previously, if libxml2 registered a parsing error that itself contained invalid encoding, an exception might be raised. (#553)


## 2.8.0 / 2021-04-01

### Requirements

* Mechanize now requires Ruby 2.5 or newer.
* Move from `ntlm-http` to `rubyntlm` gem. (#495, #574)

### New Features

* Page::Link#uri now handles non-ASCII `href`s. (#569) @terryyin
* FileConnection supports Windows drive letters (#483)
* Credential headers 'Authorization' and 'Cookie' are deleted on cross-origin redirects. (#538) @kyoshidajp
* ContentDispositionParser handles ISO8601 date headers, to be robust with websites that ignore RFC2183. (#554) @reitermarkus

### Bug fix

* POST headers 'Content-Length', 'Content-MD5', and 'Content-Type' are deleted in a case-insensitive manner on redirects. Previously these headers were treated as case-sensitive.


## 2.7.7 / 2021-02-01

* Security fixes for CVE-2021-21289

  Mechanize `>= v2.0`, `< v2.7.7` allows for OS commands to be injected into several classes'
  methods via implicit use of Ruby's `Kernel.open` method. Exploitation is possible only if
  untrusted input is used as a local filename and passed to any of these calls:

  - `Mechanize::CookieJar#load`: since v2.0 (see 208e3ed)
  - `Mechanize::CookieJar#save_as`: since v2.0 (see 5b776a4)
  - `Mechanize#download`: since v2.2 (see dc91667)
  - `Mechanize::Download#save` and `#save!` since v2.1 (see 98b2f51, bd62ff0)
  - `Mechanize::File#save` and `#save_as`: since v2.1 (see 2bf7519)
  - `Mechanize::FileResponse#read_body`: since v2.0 (see 01039f5)

  See https://github.com/sparklemotion/mechanize/security/advisories/GHSA-qrqm-fpv6-6r8g for more
  information.

  Also see #547, #548. Thank you, @kyoshidajp!

* New Features
  * Support for Ruby 3.0 by adding `webrick` as a runtime dependency. (#557) @pvalena

* Bug fix
  * Ignore input fields with blank names (#542, #536)


## 2.7.6

* New Features
  * Mechanize#set_proxy accepts an HTTP URL/URI. (#513)

* Bug fix
  * Fix element(s)_with(search: selector) methods not working for forms, form fields and frames. (#444)
  * Improve the filename parser for the `Content-Disposition` header. (#496, #517)
  * Accept `Content-Encoding: identity`. (#515)
  * Mechanize::Page#title no longer picks a title in an embeded SVG/RDF element. (#503)
  * Make Mechanize::Form#has_field? boolean. (#501)

## 2.7.5

* New Features
  * All 4xx responses and RedirectLimitReachedError when fetching robots.txt are treated as full allow just like Googlebot does.
  * Enable support for mime-types > 3.

* Bug fix
  * Don't cause infinite loop when `GET /robots.txt` redirects. (#457)
  * Fix basic authentication for a realm that contains uppercase characters. (#458, #459)
  * Fix encoding error when uploading a file which name is non-ASCII. (#333)

## 2.7.4

* New Features
  * Accept array-like and hash-like values as query/parameter value.
    A new utility method Mechanize::Util.each_parameter is added, and Mechanize::Util.build_query_string is enhanced
    for this feature.
  * Allow passing a `Form::FileUpload` instance to `#post`. #350 by Sam
    Rawlins.
  * Capture link when scheme is unsupported. #362 by Jon Rowe.
  * Pre-defined User-Agent stings are updated to those of more recent versions, and new aliases for IE 10/11 and Edge are added.
  * Support for mime-types 1.x is restored while keeping compatible with mime-types 2.x.
  * Mechanize::Page now responds to #xpath, #css, #at_xpath, #at_css, and #%.
  * element(s)_with methods now accept :xpath and :css options for doing xpath/css
    selector searching.
  * Pass URI information to Nokogiri where applicable. #405 @lulalala

* Bug fix
  * Don't raise an exception if a connection has set a {read,open}_timeout and
    a `file://` request is made. (#397)
  * Fix whitespace bug in WWW-Authenticate. #451, #450, by Rasmus Bergholdt
  * Don't allow redirect from a non-file URL to a file URL for security reasons. (#455)

## 2.7.3

* New Features
  * Allow net-http-persistent instance to be named. #324, John Weir.
  * #save and #save! return filename #340
  * Updated mime-types requirement to 2.x versions.  #348 by Jeff Nyman.

* Bug fix
  * Ensure Download#save! defaults back to original filename if
    none is provided (#300)

## 2.7.2

* Bug fix
  * API compatibility issues with Mechanize::CookieJar cookies has been
    addressed.  https://github.com/sparklemotion/http-cookie/issues/2 #326

## 2.7.1

* Bug fix
  * Ensure images with no "src" attribute still return correct URLs. #317
  * Fixes Mechanize::Parser#extract_filename where an empty string filename
    in the response caused unhandled exception. #318

## 2.7.0

* New Features
  * Mechanize::Agent#response_read will now raise a
    Mechanize::ResponseReadError instead of an EOFError and avoid losing
    requested content. #296.
  * Depend on http-cookie, add backwards compatible deprecations. #257 Akinori MUSHA.
  * Added `Download#save!` for overwriting existing files. #300 Sean Kim.

* Bug fix
  * Ensure page URLs with whitespace in them are escaped #313 @pacop.
  * Added a workaround for a bug of URI#+ that led to failure in resolving a relative path containing double slash like "/../http://.../". #304

## 2.6.0

* New Features
  * Mechanize#start and Mechanize#shutdown (Thanks, Damian Janowski!)
  * Added Mechanize::Agent#allowed_error_codes for setting an Array
    of status codes which should not raise an error. #248 Laurence Rowe.
  * Added `File.save!` for overwriting existing files #219.
  * DirectorySaver::save_to now accepts an option to decode filename. #262
  * element(s)_with methods now accept a :search option for doing xpath/css
    selector searching. #287 Philippe Bourgau
  * Added httponly option for Mechanize::Cookie #242 by Paolo Perego.
  * Added Mechanize::XmlFile as a default pluggable parser for handling
    XML responses. #289

* Minor enhancements
  * Added Mechanize::Download#save_as as an alias to #save. #246
  * Fix documentation for `Mechanize::Page` element matchers. #269
  * Added Mechanize::Form::Field#raw_value for fetching a fields value
    before it's sent through Mechanize::Util.html_unescape. #283
  * Added iPad and Android user agents.  #277 by sambit, #278 by seansay.

* Bug fix
  * Mechanize#cert and Mechanize#key now return the values set by #cert= and #key=. #244, #245 (Thanks, Robert Gogolok!)
  * Mechanize no longer submits disabled form fields.  #276 by Bogdan Gusiev, #279 by Ricardo Valeriano.
  * Mechanize::File#save now behaves like Mechanize::Download#save in
    that it will create the parent directory before saving. #272, #280 by Ryan Kowalick
  * Ensure `application/xml` is registered as an XML parser in
    `PluggableParser`, not just `text/xml`. #266 James Gregory
  * Mechanize now writes cookiestxt with a prefixed dot for wildcard domain
    handling. #295 by Mike Morearty.

## 2.5.2

* New Features
  * Mechanize::CookieJar#save_as takes a keyword option "session" to say
    that session cookies should be saved.  Based on #230 by Jim Jones.

* Minor enhancements
  * Added Mechanize#follow_redirect= as an alias to redirect_ok=.

* Bug fix
  * Fixed casing of the Mac Firefox user-agent alias to match Linux Firefox.
    In mechanize 3 the old "Mac FireFox" user-agent alias will be removed.
    Pull request #231 by Gavin Miller.
  * Mechanize now authenticates using the raw challenge, not a reconstructed
    one, to avoid dealing with quoting rules of RFC 2617.  Fixes failures in #231 due to net-http-digest_auth 1.2.1
  * Fixed Content-Disposition parameter parser to be case insensitive. #233
  * Fixed redirection counting in following meta refresh. #240

## 2.5.1

* Bug fix
  * Mechanize no longer copies POST requests during a redirect which was
    introduced by #215.  Pull request #229 by Godfrey Chan.

## 2.5

* Minor enhancements
  * Added Mechanize#ignore_bad_chunking for working around servers that don't
    terminate chunked transfer-encoding properly.  Enabling this may cause
    data loss.  Issue #116
  * Removed content-type check from Mechanize::Page allowing forced parsing
    of incorrect or missing content-types.  Issue #221 by GarthSnyder
* Bug fixes
  * Fixed typos in EXAMPLES and GUIDES.  Pull Request #213 by Erkan Yilmaz.
  * Fixed handling of a quoted content-disposition size.  Pull Request #220 by
    Jason Rust
  * Mechanize now ignores a missing gzip footer like browsers do.  Issue #224
    by afhbl
  * Mechanize handles saving of files with the same name better now.  Pull
    Request #223 by Godfrey Chan, Issue #219 by Jon Hart
  * Mechanize now sends headers across redirects.  Issue #215 by Chris Gahan
  * Mechanize now raises Mechanize::ResponseReadError when the server does not
    terminate chunked transfer-encoding properly.  Issue #116
  * Mechanize no longer raises an exception when multiple identical
    radiobuttons are checked.  Issue #214 by Matthias Guenther
  * Fixed documentation for pre_connect_hooks and post_connect_hooks.  Issue #226 by Robert Poor
  * Worked around ruby 1.8 run with -Ku and ISO-8859-1 encoded characters in
    URIs.  Issue #228 by Stanislav O.Pogrebnyak

## 2.4

* Security fix:

  Mechanize#auth and Mechanize#basic_auth allowed disclosure of passwords to
  malicious servers and have been deprecated.

  In prior versions of mechanize only one set of HTTP authentication
  credentials were allowed for all connections.  If a mechanize instance
  connected to more than one server then a malicious server detecting
  mechanize could ask for HTTP Basic authentication.  This would expose the
  username and password intended only for one server.

  Mechanize#auth and Mechanize#basic_auth now warn when used.

  To fix the warning switch to Mechanize#add_auth which requires the URI
  the credentials are intended for, the username and the password.
  Optionally an HTTP authentication realm or NTLM domain may be provided.

* Minor enhancement
  * Improved exception messages for 401 Unauthorized responses.  Mechanize now
    tells you if you were missing credentials, had an incorrect password, etc.

## 2.3 / 2012-02-20

* Minor enhancements
  * Add support for the Max-Age attribute in the Set-Cookie header.
  * Added Mechanize::Download#body for compatibility with Mechanize::File when
    using Mechanize#get_file with Mechanize::Image or other Download-based
    pluggable parser.  Issue #202 by angas
  * Mechanize#max_file_buffer may be set to nil to disable creation of
    Tempfiles.

* Bug fixes
  * Applied Mechanize#max_file_buffer to the Content-Encoding handlers as well
    to prevent extra Tempfiles for small gzip or deflate response
  * Increased the default Mechanize#max_file_buffer to 100,000 bytes.  This
    gives ~5MB of response bodies in memory with the default history setting
    of 50 pages (depending on GC behavior).
  * Ignore empty path/domain attributes.
  * Cookies with an empty Expires attribute value were stored as session
    cookies but cookies without the Expires attribute were not.  Issue #78

## 2.2.1 / 2012-02-13

* Bug fixes
  * Add missing file to the gem, ensure that missing files won't cause
    failures again.  Issue #201 by Alex
  * Fix minor grammar issue in README.  Issue #200 by Shane Becker.

## 2.2 / 2012-02-12

* API changes
  * MetaRefresh#href is not normalized to an absolute URL, but set to the
    original value and resolved later.  It is even set to nil when the
    Refresh URL is unspecified or empty.

* Minor enhancements
  * Expose ssl_version from net-http-persistent.  Patch by astera.
  * SSL parameters and proxy may now be set at any time.  Issue #194 by
    dsisnero.
  * Improved Mechanize::Page with #image_with and #images_with and
    Mechanize::Page::Image various img element attribute accessors, #caption, #extname, #mime_type and #fetch.  Pull request #173 by kitamomonga
  * Added MIME type parsing for content-types in Mechanize::PluggableParser
    for fine-grained parser choices.  Parsers will be chosen based on exact
    match, simplified type or media type in that order.  See
    Mechanize::PluggableParser#[]=.
  * Added Mechanize#download which downloads a response body to an IO-like or
    filename.
  * Added Mechanize::DirectorySaver which saves responses in a single
    directory.  Issue #187 by yoshie902a.
  * Added Mechanize::Page::Link#noreferrer?
  * The documentation for Mechanize::Page#search and #at now show that both
    XPath and CSS expressions are allowed.  Issue #199 by Shane Becker.

* Bug fixes
  * Fixed handling of a HEAD request with Accept-Encoding: gzip.  Issue #198
    by Oleg Dashevskii
  * Use #resolve for resolving a Location header value.  fixes #197
  * A Refresh value can have whitespaces around the semicolon and equal sign.
  * MetaRefresh#click no longer sends out Referer.
  * A link with an empty href is now resolved correctly where previously the
    query part was dropped.

## 2.1.1 / 2012-02-03

* Bug fixes
  * Set missing idle_timeout default.  Issue #196
  * Meta refresh URIs are now escaped (excluding %).  Issue #177
  * Fix charset name extraction.  Issue #180
  * A Referer URI sent on request no longer includes user information
    or fragment part.
  * Tempfiles for storing response bodies are unlinked upon creation to avoid
    possible lack of finalization.  Issue #183
  * The default maximum history size is now 50 pages to avoid filling up a
    disk with tempfiles accidentally.  Related to Issue #183
  * Errors in bodies with deflate and gzip responses now result in a
    Mechanize::Error instead of silently being ignored and causing future
    errors.  Issue #185
  * Mechanize now raises an UnauthorizedError instead of crashing when a 403
    response does not contain a www-authenticate header.  Issue #181
  * Mechanize gives a useful exception when attempting to click buttons across
    pages.  Issue #186
  * Added note to Mechanize#cert_store describing how to add certificates in
    case your system does not come with a default set.  Issue #179
  * Invalid content-disposition headers are now ignored.  Issue #191
  * Fix NTLM by recognizing the "Negotiation" challenge instead of endlessly
    looping.  Issue #192
  * Allow specification of the NTLM domain through Mechanize#auth.  Issue #193
  * Documented how to convert a Mechanize::ResponseReadError into a File or
    Page, along with a new method #force_parse.  Issue #176

## 2.1 / 2011-12-20

* Deprecations
  * Mechanize#get no longer accepts an options hash.
  * Mechanize::Util::to_native_charset has been removed.

* Minor enhancements
  * Mechanize now depends on net-http-persistent 2.3+.  This new version
    brings idle timeouts to help with the dreaded "too many connection resets"
    issue when POSTing to a closed connection.  Issue #123
  * SSL connections will be verified against the system certificate store by
    default.
  * Added Mechanize#retry_change_requests to allow mechanize to retry POST and
    other non-idempotent requests when you know it is safe to do so.  Issue #123
  * Mechanize can now stream files directly to disk without loading them into
    memory first through Mechanize::Download, a pluggable parser for
    downloading files.

    All responses larger than Mechanize#max_file_buffer are downloaded to a
    Tempfile.  For backwards compatibility Mechanize::File subclasses still
    load the response body into memory.

    To force all unknown content types to download to disk instead of memory
    set:

      agent.pluggable_parser.default = Mechanize::Download
  * Added Mechanize#content_encoding_hooks which allow handling of
    non-standard content encodings like "agzip".  Patch #125 by kitamomonga
  * Added dom_class to elements and the element matcher like dom_id.  Patch #156 by Dan Hansen.
  * Added support for the HTML5 keygen form element.  See
    http://dev.w3.org/html5/spec/Overview.html#the-keygen-element  Patch #157
    by Victor Costan.
  * Mechanize no longer follows meta refreshes that have no "url=" in the
    content attribute to avoid infinite loops.  To follow a meta refresh to
    the same page set Mechanize#follow_meta_refresh_self to true.  Issue #134
    by Jo Hund.
  * Updated 'Mac Safari' User-Agent alias to Safari 5.1.1.  'Mac Safari 4' can
    be used for the old 'Mac Safari' alias.
  * When given multiple HTTP authentication options mechanize now picks the
    strongest method.
  * Improvements to HTTP authorization:
    * mechanize raises Mechanize::UnathorizedError for 401 responses which is
      a sublcass of Mechanize::ResponseCodeError.
    * Added support for NTLM authentication, but this has not been tested.
  * Mechanize::Cookie.new accepts attributes in a hash.
  * Mechanize::CookieJar#<<(cookie) (alias: add!) is added.  Issue #139
  * Different mechanize instances may now have different loggers.  Issue #122
  * Mechanize now accepts a proxy port as a service name or number string.
    Issue #167

* Bug fixes
  * Mechanize now handles cookies just as most modern browsers do,
    roughly based on RFC 6265.
    * domain=.example.com (which is invalid) is considered identical to
      domain=example.com.
    * A cookie with domain=example.com is sent to host.sub.example.com
      as well as host.example.com and example.com.
    * A cookie with domain=TLD (no dots) is accepted and sent if the
      host name is TLD, and rejected otherwise.  To retain compatibility
      and convention, host/domain names starting with "local" are exempt
      from this rule.
    * A cookie with no domain attribute is only sent to the original
      host.
    * A cookie with an Effective TLD is rejected based on the public
      suffix list. (cf. http://publicsuffix.org/)
    * "Secure" cookies are not sent via non-https connection.
    * Subdomain match is not performed against an IP address.
    * It is recommended that you clear out existing cookie jars for
      regeneration because previously saved cookies may not have been
      parsed correctly.
  * Mechanize takes more care to avoid saving files with certain unsafe names.
    You should still take care not to use mechanize to save files directly
    into your home directory ($HOME).  Issue #163.
  * Mechanize#cookie_jar= works again.  Issue #126
  * The original Referer value persists on redirection.  Issue #150
  * Do not send a referer on a Refresh header based redirection.
  * Fixed encoding error in tests when LANG=C.  Patch #142 by jinschoi.
  * The order of items in a form submission now match the DOM order.  Patch #129 by kitamomonga
  * Fixed proxy example in EXAMPLE.  Issue #146 by NielsKSchjoedt

## 2.0.1 / 2011-06-28

Mechanize now uses minitest to avoid 1.9 vs 1.8 assertion availability in
test/unit

* Bug Fixes
  * Restored Mechanize#set_proxy.  Issue #117, #118, #119
  * Mechanize::CookieJar#load now lazy-loads YAML.  Issue #118
  * Mechanize#keep_alive_time no longer crashes but does nothing as
    net-http-persistent does not support HTTP/1.0 keep-alive extensions.

## 2.0 / 2011-06-27

Mechanize is now under the MIT license

* API changes
  * WWW::Mechanize has been removed.  Use Mechanize.
  * Pre connect hooks are now called with the agent and the request.  See
    Mechanize#pre_connect_hooks.
  * Post connect hooks are now called with the agent and the response.  See
    Mechanize#post_connect_hooks.
  * Mechanize::Chain is gone, as an internal API this should cause no problems.
  * Mechanize#fetch_page no longer accepts an options Hash.
  * Mechanize#put now accepts headers instead of an options Hash as the last
    argument
  * Mechanize#delete now accepts headers instead of an options Hash as the
    last argument
  * Mechanize#request_with_entity now accepts headers instead of an options
    Hash as the last argument
  * Mechanize no longer raises RuntimeError directly, Mechanize::Error or
    ArgumentError are raised instead.
  * The User-Agent header has changed.  It no longer includes the WWW- prefix
    and now includes the ruby version.  The URL has been updated as well.
  * Mechanize now requires ruby 1.8.7 or newer.
  * Hpricot support has been removed as webrobots requires nokogiri.
  * Mechanize#get no longer accepts the referer as the second argument.
  * Mechanize#get no longer allows the HTTP method to be changed (:verb
    option).
  * Mechanize::Page::Meta is now Mechanize::Page::MetaRefresh to accurately
    depict its responsibilities.
  * Mechanize::Page#meta is now Mechanize::Page#meta_refresh as it only
    contains meta elements with http-equiv of "refresh"
  * Mechanize::Page#charset is now Mechanize::Page::charset.  GH #112, patch
    by Godfrey Chan.

* Deprecations
  * Mechanize#get with an options hash is deprecated and will be removed after
    October, 2011.
  * Mechanize::Util::to_native_charset is deprecated as it is no longer used
    by Mechanize.

* New Features

  * Add header reference methods to Mechanize::File so that a reponse
    object gets compatible with Net::HTTPResponse.
  * Mechanize#click accepts a regexp or string to click a button/link in the
    current page. It works as expected when not passed a string or regexp.
  * Provide a way to only follow permanent redirects (301)
    automatically: <tt>agent.redirect_ok = :permanent</tt>  GH #73
  * Mechanize now supports HTML5 meta charset.  GH #113
  * Documented various Mechanize accessors.  GH #66
  * Mechanize now uses net-http-digest_auth.  GH #31
  * Mechanize now implements session cookies.  GH #78
  * Mechanize now implements deflate decoding.  GH #40
  * Mechanize now allows a certificate and key to be passed directly.  GH #71
  * Mechanize::Form::MultiSelectList now implements #option_with and #options_with.  GH #42
  * Add Mechanize::Page::Link#rel and #rel?(kind) to read and test the rel
    attribute.
  * Add Mechanize::Page#canonical_uri to read a </tt><link
    rel="canonical"></tt> tag.
  * Add support for Robots Exclusion Protocol (i.e. robots.txt) and
    nofollow/noindex in meta tags and the rel attribute.  Automatic
    exclusion can be turned on by setting:
      agent.robots = true
  * Manual robots.txt test can be performed with
    Mechanize#robots_allowed? and #robots_disallowed?.
  * Mechanize::Form now supports the accept-charset attribute.  GH #96
  * Mechanize::ResponseReadError is raised if there is an exception while
    reading the response body.  This allows recovery from broken HTTP servers
    (or connections).  GH #90
  * Mechanize#follow_meta_refresh set to :anywhere will follow meta refresh
    found outside of a document's head.  GH #99
  * Add support for HTML5's rel="noreferrer" attribute which indicates
    no "Referer" information should be sent when following the link.
  * A frame will now load its content when #content is called.  GH #111
  * Added Mechanize#default_encoding to provide a default for pages with no
    encoding specified.  GH #104
  * Added Mechanize#force_default_encoding which only uses
    Mechanize#default_encoding for parsing HTML.  GH #104

* Bug Fixes:

  * Fixed a bug where Referer is not sent when accessing a relative
    URI starting with "http".
  * Fix handling of Meta Refresh with relative paths.  GH #39
  * Mechanize::CookieJar now supports RFC 2109 correctly.  GH #85
  * Fixed typo in EXAMPLES.rdoc.  GH #74
  * The base element is now handled correctly for images.  GH #72
  * Image buttons with no name attribute are now included in the form's button
    list.  GH#56
  * Improved handling of non ASCII-7bit compatible characters in links (only
    an issue on ruby 1.8).  GH #36, GH #75
  * Loading cookies.txt is faster.  GH #38
  * Mechanize no longer sends cookies for a.b.example to axb.example.  GH #41
  * Mechanize no longer sends the button name as a form field for image
    buttons.  GH #45
  * Blank cookie values are now skipped.  GH #80
  * Mechanize now adds a '.' to cookie domains if no '.' was sent.  This is
    not allowed by RFC 2109 but does appear in RFC 2965.  GH #86
  * file URIs are now read in binary mode.  GH #83
  * Content-Encoding: x-gzip is now treated like gzip per RFC 2616.
  * Mechanize now unescapes URIs for meta refresh.  GH #68
  * Mechanize now has more robust HTML charset detection.  GH #43
  * Mechanize::Form::Textarea is now created from a textarea element.  GH #94
  * A meta content-type now overrides the HTTP content type.  GH #114
  * Mechanize::Page::Link#uri now handles both escaped and unescaped hrefs.
    GH #107

## 1.0.0

* New Features:

  * An optional verb may be passed to Mechanize#get GH #26
  * The WWW constant is deprecated. Switch to the top level constant Mechanize
  * SelectList#option_with and options_with for finding options

* Bug Fixes:

  * Rescue errors from bogus encodings
  * 7bit content-encoding support.  Thanks sporkmonger! GH #2
  * Fixed a bug with iconv conversion. Thanks awesomeman! GH #9
  * meta redirects outside the head are not followed. GH #13
  * Form submissions work with nil page encodings. GH #25
  * Fixing default values with serialized cookies. GH #3
  * Checkboxes and fields are sorted by page appearance before submitting. #11

## 0.9.3

* Bug Fixes:

  * Do not apply encoding if encoding equals 'none' Thanks Akinori MUSHA!
  * Fixed Page#encoding= when changing the value from or to nil.  Made
    it return the assigned value while at it. (Akinori MUSHA)
  * Custom request headers may be supplied WWW::Mechanize#request_headers
    RF #24516
  * HTML Parser may be set on a per instance level WWW::Mechanize#html_parser
    RF #24693
  * Fixed string encoding in ruby 1.9.  RF #2433
  * Rescuing Zlib::DataErrors (Thanks Kelley Reynolds)
  * Fixing a problem with frozen SSL objects.  RF #24950
  * Do not send a referer on meta refresh. RF #24945
  * Fixed a bug with double semi-colons in Content-Disposition headers
  * Properly handling cookies that specify a path.  RF #25259

## 0.9.2 / 2009/03/05

* New Features:
  * Mechanize#submit and Form#submit take arbitrary headers(thanks penguincoder)

* Bug Fixes:
  * Fixed a bug with bad cookie parsing
  * Form::RadioButton#click unchecks other buttons (RF #24159)
  * Fixed problems with Iconv (RF #24190, RF #24192, RF #24043)
  * POST parameters should be CGI escaped
  * Made Content-Type match case insensitive (Thanks Kelly Reynolds)
  * Non-string form parameters work

## 0.9.1 2009/02/23

* New Features:
  * Encoding may be specified for a page: Page#encoding=

* Bug Fixes:
  * m17n fixes. ありがとう konn!
  * Fixed a problem with base tags.  ありがとう Keisuke
  * HEAD requests do not record in the history
  * Default encoding to ISO-8859-1 instead of ASCII
  * Requests with URI instances should not be polluted RF #23472
  * Nonce count fixed for digest auth requests.  Thanks Adrian Slapa!
  * Fixed a referer issue with requests using a uri.  RF #23472
  * WAP content types will now be parsed
  * Rescued poorly formatted cookies.  Thanks Kelley Reynolds!

## 0.9.0

* Deprecations
  * WWW::Mechanize::List is gone!
  * Mechanize uses Nokogiri as it's HTML parser but you may switch to
    Hpricot by using WWW::Mechanize.html_parser = Hpricot

* Bug Fixes:
  * Nil check on page when base tag is used #23021

## 0.8.5

* Deprecations
  * WWW::Mechanize::List will be deprecated in 0.9.0, and warnings have
    been added to help you upgrade.

* Bug Fixes:
  * Stopped raising EOF exceptions on HEAD requests. ありがとう：HIRAKU Kuroda
  * Fixed exceptions when a logger is set and file:// requests are made.
  * Made Mechanize 1.9 compatible
  * Not setting the port in the host header for SSL sites.
  * Following refresh headers.  Thanks Tim Connor!
  * Cookie Jar handles cookie domains containing ports, like
    'mydomain.com:443' (Thanks Michal Ochman!)
  * Fixing strange uri escaping problems [#22604]
  * Making content-type determintation more robust.  (thanks Han Holl!)
  * Dealing with links that are query string only.  [#22402]
  * Nokogiri may be dropped in as a replacement.
      WWW::Mechanize.html_parser = Nokogiri::HTML
  * Making sure the correct page is added to the history on meta refresh.
    [#22708]
  * Mechanize#get requests no longer send a referer unless they are relative
    requests.

## 0.8.4

* Bug Fixes:
  * Setting the port number on the host header.
  * Fixing Authorization headers for picky servers

## 0.8.3

* Bug Fixes:
  * Making sure logger is set during SSL connections.

## 0.8.2

* Bug Fixes:
  * Doh!  I was accidentally setting headers twice.

## 0.8.1

* Bug Fixes:
  * Fixed problem with nil pointer when logger is set

## 0.8.0

* New Features:
  * Lifecycle hooks.  Mechanize#pre_connect_hooks, Mechanize#post_connect_hooks
  * file:/// urls are now supported
  * Added Mechanize::Page#link_with, frame_with for searching for links using
    +criteria+.
  * Implementing PUT, DELETE, and HEAD requests

* Bug Fixes:
  * Fixed an infinite loop when content-length and body length don't match.
  * Only setting headers once
  * Adding IIS authentication support

## 0.7.8

* Bug Fixes:
  * Fixed bug when receiving a 304 response (HTTPNotModified) on a page not
    cached in history.
  * #21428 Default to HTML parser for 'application/xhtml+xml' content-type.
  * Fixed an issue where redirects were resending posted data

## 0.7.7

* New Features:
  * Page#form_with takes a +criteria+ hash.
  * Page#form is changed to Page#form_with
  * Mechanize#get takes custom http headers.  Thanks Mike Dalessio!
  * Form#click_button submits a form defaulting to the current button.
  * Form#set_fields now takes a hash.  Thanks Tobi!
  * Mechanize#redirection_limit= for setting the max number of redirects.

* Bug Fixes:
  * Added more examples.  Thanks Robert Jackson.
  * #20480 Making sure the Host header is set.
  * #20672 Making sure cookies with weird semicolons work.
  * Fixed bug with percent signs in urls.
    http://d.hatena.ne.jp/kitamomonga/20080410/ruby_mechanize_percent_url_bug
  * #21132 Not checking for EOF errors on redirect
  * Fixed a weird gzipping error.
  * #21233 Smarter multipart boundry. Thanks Todd Willey!
  * #20097 Supporting meta tag cookies.

## 0.7.6

* New Features:
  * Added support for reading Mozilla cookie jars.  Thanks Chris Riddoch!
  * Moving text, password, hidden, int to default.  Thanks Tim Harper!
  * Mechanize#history_added callback for page loads. Thanks Tobi Reif!
  * Mechanize#scheme_handlers callbacks for handling unsupported schemes on
    links.

* Bug Fixes:
  * Ignoring scheme case
    http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=470642
  * Not encoding tildes in uris. Thanks Bruno.  [#19380]
  * Resetting request bodys when retrying form posts.  Thanks Bruno. [#19379]
  * Throwing away keep alive connections on EPIPE and ECONNRESET.
  * Duplicating request headers when retrying a 401. Thanks Hiroshi Ichikawa.
  * Simulating an EOF error when a response length is bad.  Thanks Tobias Gruetzmacher.
    http://rubyforge.org/tracker/index.php?func=detail&aid=19178&group_id=1453&atid=5711
  * Defaulting option tags to the inner text.
    http://rubyforge.org/tracker/index.php?func=detail&aid=19976&group_id=1453&atid=5709
  * Supporting blank strings for option values.
    http://rubyforge.org/tracker/index.php?func=detail&aid=19975&group_id=1453&atid=5709

## 0.7.5

* Fixed a bug when fetching files and not pages.  Thanks Mat Schaffer!

## 0.7.4

* doh!

## 0.7.3

* Pages are now yielded to a blocks given to WWW::Mechanize#get
* WWW::Mechanize#get now takes hash arguments for uri parameters.
* WWW::Mechanize#post takes an IO object as a parameter and posts correctly.
* Fixing a strange zlib inflate problem on windows

## 0.7.2

* Handling gzipped responses with no Content-Length header

## 0.7.1

* Added iPhone to the user agent aliases. [#17572]
* Fixed a bug with EOF errors in net/http.  [#17570]
* Handling 0 length gzipped responses. [#17471]

## 0.7.0

* Removed Ruby 1.8.2 support
* Changed parser to lazily parse links
* Lazily parsing document
* Adding verify_callback for SSL requests.  Thanks Mike Dalessio!
* Fixed a bug with Accept-Language header.  Thanks Bill Siggelkow.

## 0.6.11

* Detecting single quotes in meta redirects.
* Adding pretty inspect for ruby versions > 1.8.4 (Thanks Joel Kociolek)
  http://rubyforge.org/tracker/index.php?func=detail&aid=13150&group_id=1453&atid=5709
* Fixed bug with file name in multipart posts
  http://rubyforge.org/tracker/?func=detail&aid=15594&group_id=1453&atid=5709
* Posting forms relative to the originating page. Thanks Mortee.
* Added a FAQ
  http://rubyforge.org/tracker/?func=detail&aid=15772&group_id=1453&atid=5709

## 0.6.10

* Made digest authentication work with POSTs.
* Made sure page was HTML before following meta refreshes.
  http://rubyforge.org/tracker/index.php?func=detail&aid=12260&group_id=1453&atid=5709
* Made sure that URLS with a host and no path would default to '/' for history
  purposes.
  http://rubyforge.org/tracker/index.php?func=detail&aid=12368&group_id=1453&atid=5709
* Avoiding memory leaks with transact.  Thanks Tobias Gruetzmacher!
  http://rubyforge.org/tracker/index.php?func=detail&aid=12057&group_id=1453&atid=5711
* Fixing a problem with # signs in the file name.  Thanks Tobias Gruetzmacher!
  http://rubyforge.org/tracker/index.php?func=detail&aid=12510&group_id=1453&atid=5711
* Made sure that blank form values are submitted.
  http://rubyforge.org/tracker/index.php?func=detail&aid=12505&group_id=1453&atid=5709
* Mechanize now respects the base tag.  Thanks Stephan Dale.
  http://rubyforge.org/tracker/index.php?func=detail&aid=12468&group_id=1453&atid=5709
* Aliasing inspect to pretty_inspect.  Thanks Eric Promislow.
  http://rubyforge.org/pipermail/mechanize-users/2007-July/000157.html

## 0.6.9

* Updating UTF-8 support for urls
* Adding AREA tags to the links list.
  http://rubyforge.org/pipermail/mechanize-users/2007-May/000140.html
* WWW::Mechanize#follow_meta_refresh will allow you to automatically follow
  meta refresh tags. [#10032]
* Adding x-gzip to accepted content-encoding.  Thanks Simon Strandgaard
  http://rubyforge.org/tracker/index.php?func=detail&aid=11167&group_id=1453&atid=5711
* Added Digest Authentication support.  Thanks to Ryan Davis and Eric Hodel,
  you get a gold star!

## 0.6.8

* Keep alive can be shut off now with WWW::Mechanize#keep_alive
* Conditional requests can be shut off with WWW::Mechanize#conditional_requests
* Monkey patched Net::HTTP#keep_alive?
* [#9877] Moved last request time.  Thanks Max Stepanov
* Added WWW::Mechanize::File#save
* Defaulting file name to URI or Content-Disposition
* Updating compatability with hpricot
* Added more unit tests

## 0.6.7

* Fixed a bug with keep-alive requests
* [#9549] fixed problem with cookie paths

## 0.6.6

* Removing hpricot overrides
* Fixed a bug where alt text can be nil.  Thanks Yannick!
* Unparseable expiration dates in cookies are now treated as session cookies
* Caching connections
* Requests now default to keep alive
* [#9434] Fixed bug where html entities weren't decoded
* [#9150] Updated mechanize history to deal with redirects

## 0.6.5

* Copying headers to a hash to prevent memory leaks
* Speeding up page parsing
* Aliased fields to elements
* Adding If-Modified-Since header
* Added delete_field! to form.  Thanks to Sava Chankov
* Updated uri escaping to support high order characters.  Thanks to Henrik Nyh.
* Better handling relative URIs.  Thanks to Henrik Nyh
* Now handles pipes in URLs
  http://rubyforge.org/tracker/?func=detail&aid=7140&group_id=1453&atid=5709
* Now escaping html entities in form fields.
  http://rubyforge.org/tracker/?func=detail&aid=7563&group_id=1453&atid=5709
* Added MSIE 7.0 user agent string

## 0.6.4

* Adding the "redirect_ok" method to Mechanize to stop mechanize from
  following redirects.
	http://rubyforge.org/tracker/index.php?func=detail&aid=6571&group_id=1453&atid=5712
* Added protected method Mechanize#set_headers so that subclasses can set
  custom headers.
  http://rubyforge.org/tracker/?func=detail&aid=7208&group_id=1453&atid=5712
* Aliased Page#referer to Page#page
* Fixed a bug when clicking relative urls
  http://rubyforge.org/pipermail/mechanize-users/2006-November/000035.html
* Fixing a bug when bad version or max age is passed to Cookie::parse
  http://rubyforge.org/pipermail/mechanize-users/2006-November/000033.html
* Fixing a bug with response codes. [#6526]
* Fixed bug [#6548].  Input type of 'button' was not being added as a button.
* Fixed bug [#7139]. REXML parser calls hpricot parser by accident

## 0.6.3

* Added keys and values methods to Form
* Added has_value? to Form
* Added a has_field? method to Form
* The add_field! method on Form now creates a field for you on the form.
  Thanks to Mat Schaffer for the patch.
  http://rubyforge.org/pipermail/mechanize-users/2006-November/000025.html
* Fixed a bug when form actions have html ecoded entities in them.
  http://rubyforge.org/pipermail/mechanize-users/2006-October/000019.html
* Fixed a bug when links or frame sources have html encoded entities in the
  href or src.
* Fixed a bug where '#' symbols are encoded
  http://rubyforge.org/forum/message.php?msg_id=14747

## 0.6.2

* Added a yield to Page#form so that dealing with forms can be more DSL like.
* Added the parsed page to the ResponseCodeError so that the parsed results
  can be accessed even in the event of an error.
  http://rubyforge.org/pipermail/mechanize-users/2006-September/000007.html
* Updated documentation (Thanks to Paul Smith)

## 0.6.1

* Added a method to Form called "submit".  Now forms can be submitted by
  calling a method on the form.
* Added a click method to links
* Added an REXML pluggable parser for backwards compatability.  To use it,
  just do this:
   agent.pluggable_parser.html = WWW::Mechanize::REXMLPage
* Fixed a bug with referrers by adding a page attribute to forms and links.
* Fixed a bug where domain names were case sensitive.
  http://tenderlovemaking.com/2006/09/04/road-to-ruby-mechanize-060/#comment-53
* Fixed a bug with URI escaped links.
  http://rubyforge.org/pipermail/mechanize-users/2006-September/000002.html
* Fixed a bug when options in select lists don't have a value. Thanks Dan Higham
  [#5837] Code in lib/mechanize/form_elements.rb is incorrect.
* Fixed a bug with loading text in to links.
  http://rubyforge.org/pipermail/mechanize-users/2006-September/000000.html

## 0.6.0

* Changed main parser to use hpricot
* Made WWW::Mechanize::Page class searchable like hpricot
* Updated WWW::Mechanize#click to support hpricot links like this:
  @agent.click (page/"a").first
* Clicking a Frame is now possible:
  @agent.click (page/"frame").first
* Removed deprecated attr_finder
* Removed REXML helper methods since the main parser is now hpricot
* Overhauled cookie parser to use WEBrick::Cookie

## 0.5.4

* Added WWW::Mechanize#trasact for saving history state between in a
  transaction.  See the EXAMPLES file.  Thanks Johan Kiviniemi.
* Added support for gzip compressed pages
* Forms can now be accessed like a hash.  For example, to set the value
  of an input field named 'name' to "Aaron", you can do this:
   form['name'] = "Aaron"
  Or to get the value of a field named 'name', do this:
   puts form['name']
* File uploads will now read the file specified in FileUpload#file_name
* FileUpload can use an IO object in FileUpload#file_data
* Fixed a bug with saving files on windows
* Fixed a bug with the filename being set in forms

## 0.5.3

* Mechanize#click will now act on the first element of an array.  So if an
  array of links is passed to WWW::Mechanize#click, the first link is clicked.
  That means the syntax for clicking links is shortened and still supports
  selecting a link.  The following are equivalent:
   agent.click page.links.first
   agent.click page.links
* Fixed a bug with spaces in href's and get's
* Added a tick, untick, and click method to radio buttons so that
  radiobuttons can be "clicked"
* Added a tick, untick, and click method to check boxes so that
  checkboxes can be "clicked"
* Options on Select lists can now be "tick"ed, and "untick"ed.
* Fixed a potential bug conflicting with rails.  Thanks Eric Kolve
* Updated log4r support for a speed increase.  Thanks Yinon Bentor
* Added inspect methods and pretty printing

## 0.5.2

* Fixed a bug with input names that are nil
* Added a warning when using attr_finder because attr_finder will be deprecated
  in 0.6.0 in favor of method calls.  So this syntax:
    @agent.links(:text => 'foo')
  should be changed to this:
    @agent.links.text('foo')
* Added support for selecting multiple options in select tags that support
  multiple options.  See WWW::Mechanize::MultiSelectList.
* New select list methods have been added, select_all, select_none.
* Options for select lists can now be "clicked" which toggles their selection,
  they can be "selected" and "unselected".  See WWW::Mechanize::Option
* Added a method to set multiple fields at the same time,
  WWW::Mechanize::Form#set_fields.  Which can be used like so:
   form.set_fields( :foo => 'bar', :name => 'Aaron' )

## 0.5.1

* Fixed bug with file uploads
* Added performance tweaks to the cookie class

## 0.5.0

* Added pluggable parsers. (Thanks to Eric Kolve for the idea)
* Changed namespace so all classes are under WWW::Mechanize.
* Updating Forms so that fields can be used as accessors (Thanks Gregory Brown)
* Added WWW::Mechanize::File as default object used for unknown content types.
* Added 'save_as' method to Mechanize::File, so any page can be saved.
* Adding 'save_as' and 'load' to CookieJar so that cookies can be saved
  between sessions.
* Added WWW::Mechanize::FileSaver pluggable parser to automatically save files.
* Added WWW::Mechanize::Page#title for page titles
* Added OpenSSL certificate support (Thanks Mike Dalessio)
* Removed support for body filters in favor of pluggable parsers.
* Fixed cookie bug adding a '/' when the url is missing one (Thanks Nick Dainty)

## 0.4.7

* Fixed bug with no action in forms.  Thanks to Adam Wiggins
* Setting a default user-agent string
* Added house cleaning to the cookie jar so expired cookies don't stick around.
* Added new method WWW::Form#field to find the first field with a given name.
  (thanks to Gregory Brown)
* Added WWW::Mechanize#get_file for fetching non text/html files

## 0.4.6

* Added support for proxies
* Added a uri field to WWW::Link
* Added a error class WWW::Mechanize::ContentTypeError
* Added image alt text to link text
* Added an visited? method to WWW::Mechanize
* Added Array#value= which will set the first value to the argument.  That
  allows syntax as such:    form.fields.name('q').value = 'xyz'
  Before it was like this:  form.fields.name('q').first.value = 'xyz'

## 0.4.5

* Added support for multiple values of the same name
* Updated build_query_string to take an array of arrays (Thanks Michal Janeczek)
* Added WWW::Mechanize#body_filter= so that response bodies can be preprocessed
* Added WWW::Page#body_filter= so that response bodies can be preprocessed
* Added support for more date formats in the cookie parser
* Fixed a bug with empty select lists
* Fixing a problem with cookies not handling no spaces after semicolons

## 0.4.4

* Fixed error in method signature, basic_authetication is now basic_auth
* Fixed bug with encoding names in file uploads (Big thanks to Alex Young)
* Added options to the select list

## 0.4.3

* Added syntactic sugar for finding things
* Fixed bug with HttpOnly option in cookies
* Fixed a bug with cookie date parsing
* Defaulted dropdown lists to the first element
* Added unit tests

## 0.4.2

* Added support for iframes
* Made mechanize dependant on ruby-web rather than narf
* Added unit tests
* Fixed a bunch of warnings

## 0.4.1

* Added support for file uploading
* Added support for frames (Thanks Gabriel[mailto:leerbag@googlemail.com])
* Added more unit tests
* Fixed some bugs

## 0.4.0

* Added more unit tests
* Added a cookie jar with better cookie support, included expiration of cookies
  and general cookie security.
* Updated mechanize to use built in net/http if ruby version is new enough.
* Added support for meta refresh tags
* Defaulted form actions to 'GET'
* Fixed various bugs
* Added more unit tests
* Added a response code exception
* Thanks to Brian Ellin (brianellin@gmail.com) for:
  Added support for CA files, and support for 301 response codes

