# secret-scanning config

This is our detectors list to be used in all Secret scanning tools, The detectors are mainly regexes that match certain secret patterns like API keys with a certain prefix and length.
There are several other components aswell to whitelist false positives by path or string.

You can add filenames, extenstions and folders that you wish to exclude from the scan.

```
[allowlist]
description = "global allow lists"
paths = [
    '''gitleaks.toml''',
    '''(.*?)(jpg|gif|doc|docx|zip|xls|pdf|bin|svg|socket)$''',
    '''(go.mod|go.sum)$''',
    '''node_modules''',
    '''vendor''',
]

[rules.allowlist]
# note: stopwords targets the extracted secret, not the entire regex match
# like 'regexes' does
stopwords= []

# Keywords are used for pre-regex check filtering. Rules that contain
# keywords will perform a quick string compare check to make sure the
# keyword(s) are in the content being scanned. Ideally these values should
# either be part of the idenitifer or unique strings specific to the rule's regex
# (introduced in v8.6.0)
keywords = [
  "auth",
  "password",
  "token",
]

# A rule is a detector made specific to catch a certain secret type, make sure to test the regex and be bound to the specefic secret key.
[[rules]]
description = "Adafruit API Key"
id = "adafruit-api-key"
regex = '''(?i)(?:adafruit)(?:[0-9a-z\-_\t .]{0,20})(?:[\s|']|[\s|"]){0,3}(?:=|>|:=|\|\|:|<=|=>|:)(?:'|\"|\s|=|\x60){0,5}([a-z0-9_-]{32})(?:['|\"|\n|\r|\s|\x60]|$)'''
secretGroup = 1
keywords = [
    "adafruit",
]

# Int used to extract secret from regex match and used as the group that will have
# its entropy checked if `entropy` is set.
secretGroup = 1

# Float representing the minimum shannon entropy a regex group must have to be considered a secret.
entropy = 3.5

```

To propose changes to this config file, create a [Issue](https://github.com/contentful/secret-scanning/issues/new/choose)
<br> or reach out to [#team-security](https://contentful.slack.com/archives/C28GPAQDA) over Slack
