---
title: functions
sidebar_label: functions
---

# functions

Flowpipe supports the following Terraform-compatible HCL functions:

## Numeric Functions

| Function | Description |
|----------|-------------|
| [abs](https://www.terraform.io/docs/language/functions/abs.html) | Returns the absolute value of a number. |
| [ceil](https://www.terraform.io/docs/language/functions/ceil.html) | Returns the smallest integer value greater than or equal to a given number. |
| [floor](https://www.terraform.io/docs/language/functions/floor.html) | Returns the largest integer value less than or equal to a given number. |
| [log](https://www.terraform.io/docs/language/functions/log.html) | Returns the natural logarithm of a number. |
| [max](https://www.terraform.io/docs/language/functions/max.html) | Returns the maximum value from a list of numbers. |
| [min](https://www.terraform.io/docs/language/functions/min.html) | Returns the minimum value from a list of numbers. |
| [parseint](https://www.terraform.io/docs/language/functions/parseint.html) | Parses a string as an integer. |
| [pow](https://www.terraform.io/docs/language/functions/pow.html) | Returns the result of raising a number to the power of another. |
| [signum](https://www.terraform.io/docs/language/functions/signum.html) | Returns -1, 0, or 1 depending on the sign of a number. |
| [sum](https://www.terraform.io/docs/language/functions/sum.html) | Returns the sum of a list of numbers. |

## String Functions

| Function | Description |
|----------|-------------|
| [chomp](https://www.terraform.io/docs/language/functions/chomp.html) | Removes trailing newline characters from a string. |
| [endswith](https://www.terraform.io/docs/language/functions/endswith.html) | Checks if a string ends with a specified suffix. |
| [format](https://www.terraform.io/docs/language/functions/format.html) | Formats a string using printf-style formatting. |
| [formatlist](https://www.terraform.io/docs/language/functions/formatlist.html) | Formats a list of values into a string using printf-style formatting. |
| [indent](https://www.terraform.io/docs/language/functions/indent.html) | Indents a string by a specified number of spaces. |
| [join](https://www.terraform.io/docs/language/functions/join.html) | Joins elements of a list into a string with a delimiter. |
| [lower](https://www.terraform.io/docs/language/functions/lower.html) | Converts a string to lowercase. |
| [regex](https://www.terraform.io/docs/language/functions/regex.html) | Searches a string using a regular expression. |
| [regexall](https://www.terraform.io/docs/language/functions/regexall.html) | Searches a string using a regular expression and returns all matches. |
| [replace](https://www.terraform.io/docs/language/functions/replace.html) | Replaces occurrences of a substring with another string. |
| [split](https://www.terraform.io/docs/language/functions/split.html) | Splits a string into a list of substrings. |
| [startswith](https://www.terraform.io/docs/language/functions/startswith.html) | Checks if a string starts with a specified prefix. |
| [strcontains](https://www.terraform.io/docs/language/functions/strcontains.html) | Checks if a string contains another substring. |
| [strrev](https://www.terraform.io/docs/language/functions/strrev.html) | Reverses a string. |
| [substr](https://www.terraform.io/docs/language/functions/substr.html) | Returns a substring from a string. |
| [title](https://www.terraform.io/docs/language/functions/title.html) | Converts the first character of each word in a string to uppercase. |
| [trim](https://www.terraform.io/docs/language/functions/trim.html)               | Trim whitespace from a string                   |
| [trimprefix](https://www.terraform.io/docs/language/functions/trimprefix.html)   | Trim a prefix from a string                     |
| [trimspace](https://www.terraform.io/docs/language/functions/trimspace.html)     | Trim leading and trailing whitespace            |
| [trimsuffix](https://www.terraform.io/docs/language/functions/trimsuffix.html)   | Trim a suffix from a string                     |
| [upper](https://www.terraform.io/docs/language/functions/upper.html) | Converts a string to uppercase. |


## Collection Functions

| Function | Description |
|----------|-------------|
| [alltrue](https://www.terraform.io/docs/language/functions/alltrue.html) | Returns true if all elements in a list are true. |
| [anytrue](https://www.terraform.io/docs/language/functions/anytrue.html) | Returns true if any element in a list is true. |
| [chunklist](https://www.terraform.io/docs/language/functions/chunklist.html) | Splits a list into chunks of a specified size. |
| [coalesce](https://www.terraform.io/docs/language/functions/coalesce.html) | Returns the first non-null value in a list. |
| [coalescelist](https://www.terraform.io/docs/language/functions/coalescelist.html) | Returns the first non-empty list in a list of lists. |
| [compact](https://www.terraform.io/docs/language/functions/compact.html) | Removes null and empty string elements from a list. |
| [concat](https://www.terraform.io/docs/language/functions/concat.html) | Concatenates multiple lists into one list. |
| [contains](https://www.terraform.io/docs/language/functions/contains.html) | Checks if a value is in a list, set, or map. |
| [distinct](https://www.terraform.io/docs/language/functions/distinct.html) | Returns a new list with duplicate elements removed. |
| [element](https://www.terraform.io/docs/language/functions/element.html) | Returns the element at a specified index in a list. |
| [flatten](https://www.terraform.io/docs/language/functions/flatten.html) | Flattens a list of lists into a single list. |
| [index](https://www.terraform.io/docs/language/functions/index.html) | Returns the index of a value in a list. |
| [keys](https://www.terraform.io/docs/language/functions/keys.html) | Returns the keys of a map as a list. |
| [length](https://www.terraform.io/docs/language/functions/length.html) | Returns the length of a string, list, or map. |
| [list](https://www.terraform.io/docs/language/functions/list.html) | Creates a list from the arguments. |
| [lookup](https://www.terraform.io/docs/language/functions/lookup.html) | Looks up a value in a map using a key. |
| [map](https://www.terraform.io/docs/language/functions/map.html) | Creates a map from a list of key-value pairs. |
| [matchkeys](https://www.terraform.io/docs/language/functions/matchkeys.html) | Returns a map with only the specified keys. |
| [merge](https://www.terraform.io/docs/language/functions/merge.html) | Merges maps together. |
| [one](https://www.terraform.io/docs/language/functions/one.html) | Returns true if only one element in a list is true. |
| [range](https://www.terraform.io/docs/language/functions/range.html) | Generates a sequence of numbers. |
| [reverse](https://www.terraform.io/docs/language/functions/reverse.html) | Reverses the order of elements in a list. |
| [setintersection](https://www.terraform.io/docs/language/functions/setintersection.html) | Returns the intersection of two sets. |
| [setproduct](https://www.terraform.io/docs/language/functions/setproduct.html) | Returns the Cartesian product of two or more sets. |
| [setsubtract](https://www.terraform.io/docs/language/functions/setsubtract.html) | Returns the difference of two sets. |
| [setunion](https://www.terraform.io/docs/language/functions/setunion.html) | Returns the union of two or more sets. |
| [slice](https://www.terraform.io/docs/language/functions/slice.html) | Extracts a subsequence from a list. |
| [sort](https://www.terraform.io/docs/language/functions/sort.html) | Sorts a list of strings or numbers. |
| [sum](https://www.terraform.io/docs/language/functions/sum.html) | Returns the sum of a list of numbers. |
| [transpose](https://www.terraform.io/docs/language/functions/transpose.html)     | Transpose a list of lists                       |
| [values](https://www.terraform.io/docs/language/functions/values.html) | Returns the values of a map as a list. |
| [zipmap](https://www.terraform.io/docs/language/functions/zipmap.html) | Creates a map from two lists. |



## Encoding Functions

| Function | Description |
|----------|-------------|
| [base64decode](https://www.terraform.io/docs/language/functions/base64decode.html) | Decodes a base64-encoded string. |
| [base64encode](https://www.terraform.io/docs/language/functions/base64encode.html) | Encodes a string to base64. |
| [base64gzip](https://www.terraform.io/docs/language/functions/base64gzip.html) | Compresses and then base64-encodes a string. |
| [csvdecode](https://www.terraform.io/docs/language/functions/csvdecode.html) | Decodes a CSV string into a list of maps. |
| [jsondecode](https://www.terraform.io/docs/language/functions/jsondecode.html) | Decodes a JSON string into a map or list. |
| [jsonencode](https://www.terraform.io/docs/language/functions/jsonencode.html) | Encodes a value as a JSON string. |
| [textdecodebase64](https://www.terraform.io/docs/language/functions/textdecodebase64.html) | Decodes a base64-encoded string. |
| [textencodebase64](https://www.terraform.io/docs/language/functions/textencodebase64.html) | Encodes a string to base64. |
| [urlencode](https://www.terraform.io/docs/language/functions/urlencode.html) | URL-encodes a string. |
| [yamldecode](https://www.terraform.io/docs/language/functions/yamldecode.html) | Decodes a YAML string into a map or list. |
| [yamlencode](https://www.terraform.io/docs/language/functions/yamlencode.html) | Encodes a value as a YAML string. |


## Filesystem Functions

| Function | Description |
|----------|-------------|
| [abspath](https://www.terraform.io/docs/language/functions/abspath.html) | Returns the absolute path of a file or directory. |
| [basename](https://www.terraform.io/docs/language/functions/basename.html) | Returns the last element of a path. |
| [dirname](https://www.terraform.io/docs/language/functions/dirname.html) | Returns the directory part of a path. |
| [pathexpand](https://www.terraform.io/docs/language/functions/pathexpand.html) | Expands the tilde (~) in a file path. |
| [file](https://www.terraform.io/docs/language/functions/file.html) | Reads the contents of a file. |
| [fileexists](https://www.terraform.io/docs/language/functions/fileexists.html) | Returns true if a file exists. |
| [fileset](https://www.terraform.io/docs/language/functions/fileset.html) | Returns a set of files matching a glob pattern. |
| [filebase64](https://www.terraform.io/docs/language/functions/filebase64.html) | Reads the contents of a file and base64-encodes it. |




## Date and Time Functions

| Function | Description |
|----------|-------------|
| [formatdate](https://www.terraform.io/docs/language/functions/formatdate.html) | Formats a timestamp into a string. |
| [timeadd](https://www.terraform.io/docs/language/functions/timeadd.html) | Adds a duration to a timestamp. |
| [timecmp](https://www.terraform.io/docs/language/functions/timecmp.html) | Compares two timestamps. |
| [timestamp](https://www.terraform.io/docs/language/functions/timestamp.html) | Returns the current timestamp. |


## Hash and Crypto Functions

| Function | Description |
|----------|-------------|
| [base64sha256](https://www.terraform.io/docs/language/functions/base64sha256.html) | Computes the SHA-256 hash of a string and base64-encodes the result. |
| [base64sha512](https://www.terraform.io/docs/language/functions/base64sha512.html) | Computes the SHA-512 hash of a string and base64-encodes the result. |
| [bcrypt](https://www.terraform.io/docs/language/functions/bcrypt.html) | Hashes a string using the bcrypt algorithm. |
| [filebase64sha256](https://www.terraform.io/docs/language/functions/filebase64sha256.html) | Reads the contents of a file, calculates the SHA256 hash, and base64-encodes it. |
| [filebase64sha512](https://www.terraform.io/docs/language/functions/filebase64sha512.html) | Reads the contents of a file, calculates the SHA512 hash, and base64-encodes it. |
| [filemd5](https://www.terraform.io/docs/language/functions/filemd5.html) | Reads the contents of a file and calculates the MD5 hash. |
| [filesha1](https://www.terraform.io/docs/language/functions/filesha1.html) | Reads the contents of a file and calculates the SHA1 hash. |
| [filesha256](https://www.terraform.io/docs/language/functions/filesha256.html) | Reads the contents of a file and calculates the SHA256 hash. |
| [filesha512](https://www.terraform.io/docs/language/functions/filesha512.html) | Reads the contents of a file and calculates the SHA512 hash. |
| [md5](https://www.terraform.io/docs/language/functions/md5.html) | Computes the MD5 hash of a string. |
| [rsadecrypt](https://www.terraform.io/docs/language/functions/rsadecrypt.html) | Decrypts data using an RSA private key. |
| [sha1](https://www.terraform.io/docs/language/functions/sha1.html) | Computes the SHA-1 hash of a string. |
| [sha256](https://www.terraform.io/docs/language/functions/sha256.html) | Computes the SHA-256 hash of a string. |
| [sha512](https://www.terraform.io/docs/language/functions/sha512.html) | Computes the SHA-512 hash of a string. |
| [uuid](https://www.terraform.io/docs/language/functions/uuid.html) | Generates a random UUID. |
| [uuidv5](https://www.terraform.io/docs/language/functions/uuidv5.html) | Generates a UUID using a name and namespace. |


## IP Network Functions

| Function | Description |
|----------|-------------|
| [cidrhost](https://www.terraform.io/docs/language/functions/cidrhost.html) | Calculates the IP address of a host in a CIDR block. |
| [cidrnetmask](https://www.terraform.io/docs/language/functions/cidrnetmask.html) | Calculates the network mask of a CIDR block. |
| [cidrsubnet](https://www.terraform.io/docs/language/functions/cidrsubnet.html) | Calculates a subnet address within a CIDR block. |
| [cidrsubnets](https://www.terraform.io/docs/language/functions/cidrsubnets.html) | Splits a CIDR block into multiple subnets. |

## Type Conversion Functions

| Function | Description |
|----------|-------------|
| [can](https://www.terraform.io/docs/language/functions/can.html) | Checks if a value has a specific attribute or method. |
| [nonsensitive](https://www.terraform.io/docs/language/functions/nonsensitive.html) | Marks a value as non-sensitive, allowing it to be displayed in logs. |
| [sensitive](https://www.terraform.io/docs/language/functions/sensitive.html) | Marks a value as sensitive to avoid exposing sensitive information. |
| [tobool](https://www.terraform.io/docs/language/functions/tobool.html) | Converts a value to a boolean. |
| [tolist](https://www.terraform.io/docs/language/functions/tolist.html) | Converts a value to a list. |
| [tomap](https://www.terraform.io/docs/language/functions/tomap.html) | Converts a value to a map. |
| [tonumber](https://www.terraform.io/docs/language/functions/tonumber.html) | Converts a string to a number. |
| [toset](https://www.terraform.io/docs/language/functions/toset.html) | Converts a list to a set. |
| [tostring](https://www.terraform.io/docs/language/functions/tostring.html) | Converts a value to a string. |
| [try](https://www.terraform.io/docs/language/functions/try.html) | Attempts to evaluate an expression and returns a default value on failure. |

## Flowpipe Functions

| Function | Description |
|----------|-------------|
| [env](#env)                     | Gets the value of an environment variable.
| [error_message](#error_message) | Given a reference to a step, returns a string containing the first error message.
| [is_error](#is_error)           | Given a reference to a step, returns `true`` if there are any errors.


### env

`env` returns the value of an environment variable.  Be careful when using environment variables, as they are global variables.  When writing mod, you should generally use [variables](/docs/flowpipe-hcl/variable) or [pipeline parameters](/docs/flowpipe-hcl/pipeline#parameters) for passing data into the mod, though you may use `env` to set a default value.

```hcl
param "region" {
  default = env("AWS_REGION")
} 
```

### error_message

Given a reference to a step, `error_message` will return a string containing the first error message, if any. If there are no errors, it will return an empty string.

```hcl
step "http" "my_request" {
  url           = "https://myapi.local/subscribe"
  method        = "post"
  body          = jsonencode({
    name = param.subscriber
  })

  error {
    ignore = true
  }
}

output "error_message" {
  value = err_message(step.http.my_request)
}
```

### is_error
Given a reference to a step, `is_error` returns a boolean true if there are 1 or more errors, or false if there are no errors.

```hcl
step "http" "my_request" {
  url           = "https://myapi.local/subscribe"
  method        = "post"
  body          = jsonencode({
    name = param.subscriber
  })

  error {
    ignore = true
  }
}

step "email" "send_it" {
  to      = param.subscriber
  subject = "You have been subscribed"
  body    = step.http.my_request.response_body
  if      = !is_error(step.http.my_request)
}
```