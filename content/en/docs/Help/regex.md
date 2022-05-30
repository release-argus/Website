---
title: "RegEx"
linkTitle: "RegEx"
weight: 4
description: >
  Help with Regular Expressions
---

A RegEx (short for Regular Expression), is a string of text that allows you to match on specific patterns of text. For example, if you were given the text `"tag_name": "v1.2.3"`, a small RegEx could be used to extract the version number in semantic form ('1.2.3'), `[0-9]+`. (this would match on all cases of 1 or more of 0-9 in the target string)

With Argus, RegEx can be used in a few places
- `service.name.regex_content` - RegEx used on the content of the page @ `url` that needs to match before Argus considers it a valid version.
- `service.name.regex_version` - RegEx used on the version found that needs to match before Argus considers it a valid version.
- `service.url_commands[*].regex` - RegEx used in a `url_command` to extract the version from the page.
- `service.deployed_version.regex` - RegEx to get the `deployed_version` from the page @ `url`

| Character | Description | Example | Matches |
| --------- | ----------- | ------- | ------- |
| ^ | Matches beginning of line | ^any | allow,any
| $ | Matches end of line | y$ | friendly,yay
| . | Match any character | a.c | abc,a_c
| \| | OR operator | a1|b1 | a1,b1
| (...) | Capture anything matched | v(1) | captures 1
| [...] | Matches anything contained in the brackets | [abc] | a,b,c
| [^...] | Matches anything not contained in the brackets | [^abc] | d,e,f
| [a-z] | Matches any character between 'a' and 'z' (inclusive) | [a-z] | a,r,g
| [0-9] | Matches any number between '0' and '9' (inclusive) | [0-2] | 0,1,2
| {x} | match 'x' amount of times | (ab){2} | abab
| {x,} | match 'x' or more amount of times | (ab){2,} | abab, ababab
| {x,y} | match between 'x' and 'y'  times | (ab){1,2} | ab, abab
| * | Match 0+ of the item before it | ab*c | ac,abc,abbc
| + | Match 1+ of the item before it | ab+c | abc,abbc,abbc
| ? | Match 0/1 of the item before it | ab?c | ac,abc
| \\ | Escape the character after it | 1\\.2,dev\\* | 1.2, dev* (won't match 132 or de)

Sometimes the sites you query will have multiple matches for your regex, e.g.
```yaml
beta:
  version: "2.0.0-dev"
stable:
  version: "1.0.5"
```
If you wanted `stable.version`, you could use the `version: "([^"]+)"` RegEx. Though on its own, this would match both the '2.0.0-dev' and the '1.0.5'. Argus would take the first match by default. You can get around this by giving your `url_commands` an `index`. So using this RegEx, but with an index of '1' would give you the second match (as indexes start from 0).

An alternative would be to make the version number RegEx a bit stricter. Here, we have `-dev` in the beta release, so if it's like that on all betas, we could make the RegEx exclude those cases like so, `version: "([0-9.]+)"`. Now the RegEx is only allowing the version to be a sequence of numbers and full stops. This would give us '1.0.5'.

#### Helpful sites

You can test your regex on the sites below and see what it's matching as well as why

- https://regex101.com/
- https://regoio.herokuapp.com/
