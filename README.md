A super fast, highly extensible markdown parser for PHP
=======================================================

[![Latest Stable Version](https://poser.pugx.org/cebe/markdown/v/stable.png)](https://packagist.org/packages/cebe/markdown)
[![Total Downloads](https://poser.pugx.org/cebe/markdown/downloads.png)](https://packagist.org/packages/cebe/markdown)
[![Build Status](https://secure.travis-ci.org/cebe/markdown.png)](http://travis-ci.org/cebe/markdown)
[![Tested against HHVM](http://hhvm.h4cc.de/badge/cebe/markdown.png)](http://hhvm.h4cc.de/package/cebe/markdown)
[![Code Coverage](https://scrutinizer-ci.com/g/cebe/markdown/badges/coverage.png?s=db6af342d55bea649307ef311fbd536abb9bab76)](https://scrutinizer-ci.com/g/cebe/markdown/)
<!-- [![Scrutinizer Quality Score](https://scrutinizer-ci.com/g/cebe/markdown/badges/quality-score.png?s=17448ca4d140429fd687c58ff747baeb6568d528)](https://scrutinizer-ci.com/g/cebe/markdown/) -->

What is this?
-------------

A set of [PHP][] classes, each representing a [Markdown][] flavor, and a command line tool
for converting markdown files to HTML files.

The implementation focus is to be **fast** (see [benchmark][]) and **extensible**. You are able to add additional language elements by
directly hooking into the parser - no (possibly error-prone) post- or pre-processing is needed to extend the language.
It is also [well tested][] to provide best rendering results also in edge cases where other parsers fail.

Currently the following markdown flavors are supported:

- **The original Markdown** according to <http://daringfireball.net/projects/markdown/syntax> ([try it!](http://markdown.cebe.cc/try?flavor=default)).
- **Github flavored Markdown** according to <https://help.github.com/articles/github-flavored-markdown> ([try it!](http://markdown.cebe.cc/try?flavor=gfm)).
- **Markdown Extra** according to <http://michelf.ca/projects/php-markdown/extra/> (currently not fully supported WIP see [#25][], [try it!](http://markdown.cebe.cc/try?flavor=extra))
- Any mixed Markdown flavor you like because of its highly extensible structure (See documentation below).

[#25]: https://github.com/cebe/markdown/issues/25 "issue #25"
[well tested]: https://travis-ci.org/cebe/markdown "PHPUnit tests on Travis-CI"

Future plans are to support:

- Smarty Pants <http://daringfireball.net/projects/smartypants/>
- ... (Feel free to [suggest](https://github.com/cebe/markdown/issues/new) further additions!)

### Who is using it?

- It powers the [API-docs and the definitive guide](http://www.yiiframework.com/doc-2.0/) for the [Yii Framework][] [2.0](https://github.com/yiisoft/yii2).

[Yii Framework]: http://www.yiiframework.com/ "The Yii PHP Framework"


Installation
------------

[PHP 5.4 or higher](http://www.php.net/downloads.php) is required to use it.
It will also run on facebook's [hhvm](http://hhvm.com/).

Installation is recommended to be done via [composer][] by adding the following to the `require` section in your `composer.json`:

```json
"cebe/markdown": "*"
```

Run `composer update` afterwards.

Alternatively you can clone this repository and use the classes directly.
In this case you have to include the `Parser.php` and `Markdown.php` files yourself.


Usage
-----

### In your PHP project

To parse your markdown you need only two lines of code. The first one is to choose the markdown flavor as
one of the following:

- Original Markdown: `$parser = new \cebe\markdown\Markdown();`
- Github Flavored Markdown: `$parser = new \cebe\markdown\GithubMarkdown();`
- Markdown Extra: `$parser = new \cebe\markdown\MarkdownExtra();`

The next step is to call the `parse()`-method for parsing the text using the full markdown language
or calling the `parseParagraph()`-method to parse only inline elements.

Here are some examples:

```php
// original markdown and parse full text
$parser = new \cebe\markdown\Markdown();
$parser->parse($markdown);

// use github markdown
$parser = new \cebe\markdown\GithubMarkdown();
$parser->parse($markdown);

// use markdown extra
$parser = new \cebe\markdown\MarkdownExtra();
$parser->parse($markdown);

// parse only inline elements (useful for one-line descriptions)
$parser = new \cebe\markdown\GithubMarkdown();
$parser->parseParagraph($markdown);
```

You may optionally set one of the following options on the parser object:

For all Markdown Flavors:

- `$parser->html5 = true` to enable HTML5 output instead of HTML4.
- `$parser->keepListStartNumber = true` to enable keeping the numbers of ordered lists as specified in the markdown.
  The default behavior is to always start from 1 and increment by one regardless of the number in markdown.

For GithubMarkdown:

- `$parser->enableNewlines = true` to convert all newlines to `<br/>`-tags. By default only newlines with two preceding spaces are converted to `<br/>`-tags. 

### The command line script

You can use it to render this readme:

    bin/markdown README.md > README.html

Using github flavored markdown:

    bin/markdown --flavor=gfm README.md > README.html

or convert the original markdown description to html using the unix pipe:

    curl http://daringfireball.net/projects/markdown/syntax.text | bin/markdown > md.html

Here is the full Help output you will see when running `bin/markdown --help`:

    PHP Markdown to HTML converter
    ------------------------------
    
    by Carsten Brandt <mail@cebe.cc>
    
    Usage:
        bin/markdown [--flavor=<flavor>] [--full] [file.md]
    
        --flavor  specifies the markdown flavor to use. If omitted the original markdown by John Gruber [1] will be used.
                  Available flavors:
    
                  gfm   - Github flavored markdown [2]
                  extra - Markdown Extra [3]

        --full    ouput a full HTML page with head and body. If not given, only the parsed markdown will be output.

        --help    shows this usage information.

        If no file is specified input will be read from STDIN.

    Examples:

        Render a file with original markdown:

            bin/markdown README.md > README.html

        Render a file using gihtub flavored markdown:

            bin/markdown --flavor=gfm README.md > README.html

        Convert the original markdown description to html using STDIN:

            curl http://daringfireball.net/projects/markdown/syntax.text | bin/markdown > md.html

    
    [1] http://daringfireball.net/projects/markdown/syntax
    [2] https://help.github.com/articles/github-flavored-markdown
    [3] http://michelf.ca/projects/php-markdown/extra/


Extensions
----------

Here are some extensions to this library:

- [Bogardo/markdown-codepen](https://github.com/Bogardo/markdown-codepen) - shortcode to embed codepens from http://codepen.io/ in markdown.
- [kartik-v/yii2-markdown](https://github.com/kartik-v/yii2-markdown) - Advanced Markdown editing and conversion utilities for Yii Framework 2.0.
- [cebe/markdown-latex](https://github.com/cebe/markdown-latex) - Convert Markdown to LaTeX and PDF
- ... [add yours!](https://github.com/cebe/markdown/edit/master/README.md#L98)


Extending the language
----------------------

Markdown consists of two types of language elements, I'll call them block and inline elements simlar to what you have in
HTML with `<div>` and `<span>`. Block elements are normally spreads over several lines and are separated by blank lines.
The most basic block element is a paragraph (`<p>`).
Inline elements are elements that are added inside of block elements i.e. inside of text.

This markdown parser allows you to extend the markdown language by changing existing elements behavior and also adding
new block and inline elements. You do this by extending from the parser class and adding/overriding class methods and
properties. For the different element types there are different ways to extend them as you will see in the following sections.

### Adding block elements

The markdown is parsed line by line to identify each non-empty line as one of the block element types.
This job is performed by the `indentifyLine()` method which takes the array of lines and the number of the current line
to identify as an argument. This method returns the name of the identified block element which will then be used to parse it.
In the following example we will implement support for [fenced code blocks][] which are part of the github flavored markdown.

[fenced code blocks]: https://help.github.com/articles/github-flavored-markdown#fenced-code-blocks
                      "Fenced code block feature of github flavored markdown"

```php
<?php

class MyMarkdown extends \cebe\markdown\Markdown
{
	protected function identifyLine($lines, $current)
	{
		// if a line starts with at least 3 backticks it is identified as a fenced code block
		if (strncmp($lines[$current], '```', 3) === 0) {
			return 'fencedCode';
		}
		return parent::identifyLine($lines, $current);
	}

	// ...
}
```

Parsing of a block element is done in two steps:

1. "consuming" all the lines belonging to it. In most cases this is iterating over the lines starting from the identified
   line until a blank line occurs. This step is implemented by a method named `consume{blockName}()` where `{blockName}`
   will be replaced by the name we returned in the `identifyLine()`-method. The consume method also takes the lines array
   and the number of the current line. It will return two arguments: an array representing the block element and the line
   number to parse next. In our example we will implement it like this:

   ```php
	protected function consumeFencedCode($lines, $current)
	{
		// create block array
		$block = [
			'type' => 'fencedCode',
			'content' => [],
		];
		$line = rtrim($lines[$current]);

		// detect language and fence length (can be more than 3 backticks)
		$fence = substr($line, 0, $pos = strrpos($line, '`') + 1);
		$language = substr($line, $pos);
		if (!empty($language)) {
			$block['language'] = $language;
		}

		// consume all lines until ```
		for($i = $current + 1, $count = count($lines); $i < $count; $i++) {
			if (rtrim($line = $lines[$i]) !== $fence) {
				$block['content'][] = $line;
			} else {
				// stop consuming when code block is over
				break;
			}
		}
		return [$block, $i];
	}
	```

2. "rendering" the element. After all blocks have been consumed, they are being rendered using the `render{blockName}()`
   method:

   ```php
	protected function renderFencedCode($block)
	{
		$class = isset($block['language']) ? ' class="language-' . $block['language'] . '"' : '';
		return "<pre><code$class>" . htmlspecialchars(implode("\n", $block['content']) . "\n", ENT_NOQUOTES, 'UTF-8') . '</code></pre>';
	}
   ```

   You may also add code highlighting here. In general it would also be possible to render ouput in a different language than
   HTML for example LaTeX.


### Adding inline elements

Adding inline elements is different from block elements as they are directly parsed in the text where they occur.
An inline element is identified by a marker that marks the beginning of an inline element (e.g. `[` will mark a possible
beginning of a link or `` ` `` will mark inline code).

Inline markers are declared in the `inlineMarkers()`-method which returns a map from marker to parser method. That method
will then be called when a marker is found in the text. As an argument it takes the text starting at the position of the marker.
The parser method will return an array containing the text to append to the parsed markup and an offset of text it has
parsed from the input markdown. All text up to this offset will be removed from the markdown before the next marker will be searched.

As an example, we will add support for the [strikethrough][] feature of github flavored markdown:

[strikethrough]: https://help.github.com/articles/github-flavored-markdown#strikethrough "Strikethrough feature of github flavored markdown"

```php
<?php

class MyMarkdown extends \cebe\markdown\Markdown
{
	protected function inlineMarkers()
	{
		$markers = [
			'~~'    => 'parseStrike',
		];
		// merge new markers with existing ones from parent class
		return array_merge(parent::inlineMarkers(), $markers);
	}

	protected function parseStrike($markdown)
	{
		// check whether the marker really represents a strikethrough (i.e. there is a closing ~~)
		if (preg_match('/^~~(.+?)~~/', $markdown, $matches)) {
			return [
			    // return the parsed tag with its content and call `parseInline()` to allow
			    // other inline markdown elements inside this tag
				'<del>' . $this->parseInline($matches[1]) . '</del>',
				// return the offset of the parsed text
				strlen($matches[0])
			];
		}
		// in case we did not find a closing ~~ we just return the marker and skip 2 characters
		return [$markdown[0] . $markdown[1], 2];
	}
}
```


Acknowledgements
----------------

I'd like to thank [@erusev][] for creating [Parsedown][] which heavily influenced this work and provided
the idea of the line based parsing approach.

[@erusev]: https://github.com/erusev "Emanuil Rusev"

FAQ
---

### Why another markdown parser?

While reviewing PHP markdown parsers for choosing one to use bundled with the [Yii framework 2.0][]
I found that most of the implementations use regex to replace patterns instead
of doing real parsing. This way extending them with new language elements is quite hard
as you have to come up with a complex regex, that matches your addition but does not mess
with other elements. Such additions are very common as you see on github which supports referencing
issues, users and commits in the comments.
A [real parser][] should use context aware methods that walk trough the text and
parse the tokens as they find them. The only implentation that I have found that uses
this approach is [Parsedown][] which also shows that this implementation is [much faster][benchmark]
than the regex way. Parsedown however is an implementation that focuses on speed and implements
its own flavor (mainly github flavored markdown) in one class and at the time of this writing was
not easily extensible.

Given the situation above I decided to start my own implementation using the parsing approach
from Parsedown and making it extensible creating a class for each markdown flavor that extend each
other in the way that also the markdown languages extend each other.
This allows you to choose between markdown language flavors and also provides a way to compose your
own flavor picking the best things from all.
I chose this approach as it is easier to implement and also more intuitive approach compared
to using callbacks to inject functionallity into the parser.


### Where do I report bugs or rendering issues?

Just [open an issue][] on github, post your markdown code and describe the problem. You may also attach screenshots of the rendered HTML result to describe your problem.


### Am I free to use this?

This library is open source and licensed under the [MIT License][]. This means that you can do whatever you want
with it as long as you mention my name and include the [license file][license]. Check the [license][] for details.

[MIT License]: http://opensource.org/licenses/MIT

Contact
-------

Feel free to contact me using [email](mailto:mail@cebe.cc) or [twitter](https://twitter.com/cebe_cc).


[PHP]: http://php.net/ "PHP is a popular general-purpose scripting language that is especially suited to web development."
[Markdown]: http://en.wikipedia.org/wiki/Markdown "Markdown on Wikipedia"
[composer]: https://getcomposer.org/ "The PHP package manager"
[Parsedown]: http://parsedown.org/ "The Parsedown PHP Markdown parser"
[benchmark]: https://github.com/kzykhys/Markbench#readme "kzykhys/Markbench on github"
[Yii framework 2.0]: https://github.com/yiisoft/yii2
[real parser]: http://en.wikipedia.org/wiki/Parsing#Types_of_parser
[open an issue]: https://github.com/cebe/markdown/issues/new
[license]: https://github.com/cebe/markdown/blob/master/LICENSE
[UIweb]: https://github.com/krmarien/Gollum 
 ----
Tương tự: https://github.com/wolfie/php-markdown/tree/extra
