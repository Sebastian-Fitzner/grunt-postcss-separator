grunt-postcss-separator
============

This is a grunt wrapper for [`postcss-separator`](https://github.com/Sebastian-Fitzner/postcss-separator). 

Split up your Data-URI (or anything else) into a separate CSS file.

Written with [PostCSS](https://github.com/postcss).

## Installation

`npm install grunt-postcss-separator`

## Usage

Read `source.css`, process its content, and output processed CSS to `styles.css` and `data.css`.

If `source.css` has:

```css
a.top:hover, a.top:focus {
	background-image: url("data:image");
}

a.top {
	background-image: url("data:image");
}

caption, th, td {
	text-align: left;
	font-weight: normal;
	vertical-align: middle;
}

q, blockquote {
	quotes: none;
}

q:before, q:after, blockquote:before, blockquote:after {
	content: "";
}

a img {
	border: none;
}
```

You will get the following output:

```css
a.top:hover, a.top:focus {
	background-image: url("data:image");
}
a.top {
	background-image: url("data:image");
}
```

## Options

All options of [`postcss-separator`](https://github.com/Sebastian-Fitzner/postcss-separator) are available.
 
Furthermore you can use the option `keepOrigin`:

#### keepOrigin (only available in Grunt)
Type: `boolean`
Default value: false

Keep the origin file untouched when defined as `true`. When defined as `false` the origin css file will be cleaned up.


## Usage

You can enable this plugin in the `Gruntfile.js` of your project like that:

`grunt.loadNpmTasks('grunt-postcss-separator');`

### Example

Here are some grunt examples:

- `icons` task splits up your dataURIs (`data`)
- `prop` task splits up a specific property (`background-image`)
- `ie` task splits up all ie classes (`.lt-ie9`)
- `mediaQuery` task splits up all media queries which match the RegExp
- `print` task splits up all @media print


``` js
separator: {
	options: {
		keepOrigin: true,
		dataFile: true
	},
	icons: {
		options: {
			pattern: {
				matchValue: /data/, // The RegExp to match values with
				matchParent: true // Rules (eg. in @media blocks) include their parent node.
			}
		},
		files: {
			'tmp/icons.css': ['test/fixtures/source.css']
		}
	},
	prop: {
		options: {
			pattern: {
				matchValue: false, // The RegExp to match values with
				matchProp: /background-image/, // The RegExp to match values with
				matchParent: true // Rules (eg. in @media blocks) include their parent node.
			}
		},
		files: {
			'tmp/prop.css': ['test/fixtures/source.css']
		}
	},
	ie: {
		options: {
			pattern: {
				matchRule: /lt-ie9/, // The RegExp to match values with
				matchMedia: false, // The RegExp to match media queries with
				matchParent: true // Rules (eg. in @media blocks) include their parent node.
			}
		},
		files: {
			'tmp/ie.css': ['test/fixtures/source.css']
		}
	},
	mediaQuery: {
		options: {
			pattern: {
				matchValue: false, // The RegExp to match values with
				matchRule: false, // The RegExp to match values with
				matchMedia: /((min|max)-)?resolution\:\s*(\d+)?\.?(\d+)?dppx/, // The RegExp to match media queries with
				matchParent: false // Rules (eg. in @media blocks) include their parent node.
			}
		},
		files: {
			'tmp/image.css': ['test/fixtures/source.css']
		}
	},
	print: {
		options: {
			pattern: {
				matchValue: false, // The RegExp to match values with
				matchRule: false, // The RegExp to match values with
				matchMedia: false, // The RegExp to match media queries with
				matchParent: false, // Rules (eg. in @media blocks) include their parent node.
				matchAtRuleType: /print/ // Rules (eg. in @media blocks) include their parent node.
			}
		},
		files: {
			'tmp/print.css': ['source.css']
		}
	}
}
```

## License
Copyright (c) 2015 Sebastian Fitzner. Licensed under the MIT license.

## ToDos

- Add tests
