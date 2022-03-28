# location

## NGINX location directive syntax

The NGINX location block can be placed inside a server block or inside another location block with some restrictions.

The syntax for constructing a location block is:

```
location [modifier] [URI] {
  ...
  ...
}
```


The modifier in the location block is optional. Having a modifier in the location block will allow NGINX to treat a URL differently. Few most common modifiers are:

- none: If no modifiers are present in a location block then the requested URI will be matched against the beginning of the requested URI.
- =: The equal sign is used to match a location block exactly against a requested URI.
- ~: The tilde sign is used for case-sensitive regular expression match against a requested URI.
- ~*: The tilde followed by asterisk sign is used for case insensitive regular expression match against a requested URI.
- ^~: The carat followed by tilde sign is used to perform longest nonregular expression match against the requested URI. If the requested URI hits such a location block, no further matching will takes place.
-

From: https://www.journaldev.com/26342/nginx-location-directive