# XmlText field type for Ibexa Platform 3.3

[![License](https://img.shields.io/github/license/netgen/ibexa-xmltext-fieldtype.svg?style=flat-square)](LICENSE)

This is the XmlText field type for Ibexa Platform 3.3, based on the original [ezsystems/ezplatform-xmltext-fieldtype](https://github.com/ezsystems/ezplatform-xmltext-fieldtype). It was extracted from the eZ Publish / Platform 5.x as it has been suceeded by docbook based [RichText](https://github.com/ezsystems/ezplatform-richtext) field type.


### Branches

- `master` (3.x): compatible with Ibexa OSS / DXP 4.x
- `ibexa-3.3` (2.x): compatible with Ibexa Platform (eZ Platform) 3.3

### Support limitations

This bundle is **only** supported for the purpose of migrating content from XmlText to RichText field type.


## Installation

Run the following:

```
composer require --update-with-all-dependencies "netgen/ibexa-xmltext-fieldtype:dev-ibexa-3.3"
```

And lastly enable the bundle by adding `EzSystems\EzPlatformXmlTextFieldTypeBundle\EzSystemsEzPlatformXmlTextFieldTypeBundle::class => ['all' => true],` to `config/bundles.php`.

----

_Once you have migrated your content you can remove the bundle from both `config/bundles.php` and `composer.json`._


## Migrating from XmlText to RichText

**Warning: As of 1.6 this is now fully supported, but regardless of that always make a backup before using the migration tools.**

This package provides tools to migration existing XmlText fields to RichText, the enriched text format eZ Platform uses.
The tool comes as a Symfony command, `ezxmltext:convert-to-richtext`.

It will do two things:

- convert `ezxmltext` field definitions to `ezrichtext` field definitions
- convert `ezxmltext` fields (content) to `ezrichtext`

We recommend that you do a test run first using something like:

```
php bin/console ezxmltext:convert-to-richtext -v --concurrency=2 --dry-run
```

The `-v` flag will output logs to the console, making it easy to track the conversion work that is being done.
This is an example of a successful conversion log entry for one field:

```
[2016-02-03 15:25:52] app.INFO: Converted ezxmltext field #745 to richtext {"original":"<?xml version=\"1.0\" encoding=\"utf-8\"?>\n<section xmlns:image=\"http://ez.no/namespaces/ezpublish3/image/\" xmlns:xhtml=\"http://ez.no/namespaces/ezpublish3/xhtml/\" xmlns:custom=\"http://ez.no/namespaces/ezpublish3/custom/\"/>\n","converted":"<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<section xmlns=\"http://docbook.org/ns/docbook\" xmlns:xlink=\"http://www.w3.org/1999/xlink\" xmlns:ezxhtml=\"http://ez.no/xmlns/ezpublish/docbook/xhtml\" xmlns:ezcustom=\"http://ez.no/xmlns/ezpublish/docbook/custom\" version=\"5.0-variant ezpublish-1.0\"/>\n"}
```

It contains, in a JSON structure, the `original` (ezxmltext) value, and the `converted` (ezrichtext) value that has been
written to the database.

Once you are ready to convert, drop `-v` and `--dry-run`.
