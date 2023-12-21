# Mastering Email Validation by Shambo-Rambo

This regex /^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/ is all about making sure an email address has the right format. This tutorial aims to dissect each part of this regex to show you how it checks for a properly structured email.

## Summary

This regex is designed to match a standard email format, ensuring that it includes characters typical in email addresses, an "@" symbol, a domain name, and a domain suffix. Email validation is not just about conforming to a standard format; it's about ensuring data integrity, improving user experience, and securing applications from potential malicious inputs. By comprehensively understanding each component of this regex, developers can fine-tune their email validation process, tailor it to specific needs, and even use these principles for other types of pattern matching.

## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [Greedy and Lazy Match](#greedy-and-lazy-match)
- [OR Operator](#or-operator)
- [Character Classes](#character-classes)
- [Flags](#flags)
- [Grouping and Capturing](#grouping-and-capturing)
- [Bracket Expressions](#bracket-expressions)
- [Boundaries](#boundaries)
- [Back-references](#back-references)
- [Look-ahead and Look-behind](#look-ahead-and-look-behind)

## Regex Components

### Anchors
In regex, anchors are not characters but positions within the string. In email matching regex, the use of anchors ^ and $ is critical for defining the precise start and end of the string being evaluated.

'^': This anchor is placed at the beginning of the regex pattern. It ensures that the matching process starts at the very beginning of the string. In the context of email validation, this means that the email address must start exactly where the regex pattern starts, leaving no room for additional characters or spaces before the email address.

'$': Located at the end of the regex pattern, this anchor asserts that the matching process ends at the very end of the string. This is crucial for email validation as it ensures that the email address doesn't have any trailing characters or spaces after it.

Code Snippet:
/^...$/

Example:
Consider the regex /^abc$/:

This pattern will match a string that is exactly 'abc'.
It will not match ' abc', 'abc ', or 'abcc', as these strings have additional characters either before or after 'abc'.

### Quantifiers
In the email matching, quantifiers are the tools that tell you 'how many' of a certain character or pattern you're dealing with in a string. They're important for defining the scope of your pattern matching.

The Plus (+): This is straightforward. It looks for one or more occurrences of the pattern before it. For example, a+ will match 'a', 'aa', 'aaa', and so on.

The Asterisk (*): Similar to +, but it's more generous. It matches zero or more occurrences. So, a* will match an empty string, 'a', 'aa', 'aaa', etc.

The Question Mark (?): This one's a bit pickier. It matches zero or one occurrence. Using a? will match an empty string or 'a', but nothing more.

Curly Brackets ({}): These are your custom quantifiers. You can get specific by setting exact numbers or ranges. For example:
{n}: Matches exactly 'n' occurrences. a{3} matches 'aaa'.
{n,}: At least 'n' occurrences. a{2,} will match 'aa', 'aaa', 'aaaa', and so on.
{n,m}: From 'n' to 'm' occurrences. a{2,4} matches 'aa', 'aaa', or 'aaaa'.

Example
In the context of an email regex like /^[a-z0-9_\.-]+@[a-z0-9_\.-]+\.[a-z]{2,6}$/:

[a-z0-9_\.-]+: The plus (+) ensures that each section of the email (local part and domain) contains at least one character from the specified range.
\.[a-z]{2,6}: Curly brackets define that the domain extension must be between 2 to 6 characters long, accommodating common extensions like .com, .net, or .org.

### Greedy and Lazy Match

Greedy vs Lazy Matching: Quantifiers in regular expressions are inherently greedy, which implies they match the longest possible string. However, appending a ? to a quantifier alters its behavior to lazy matching, where it seeks the shortest possible match. For instance, in the string 'aaa', the expression a+? matches only the first 'a', whereas a+ matches the entire string 'aaa'.

Code Snippets
Greedy: a+ (Matches as many 'a's as possible)
Lazy: a+? (Matches as few 'a's as possible)

Example
Consider the string "aaa@example.com" and the regex pattern for an email local part:

Greedy Match: Using .+@ will match 'aaa@example'.
Lazy Match: Using .+?@ will match only 'a@', as it stops matching as soon as the condition is satisfied, which is at the first '@'.

### Character Classes
Character classes in regex are fundamental in defining a set of characters to be included in the match. In email validation regex, they are used to specify allowable characters in each segment of an email address, ensuring compliance with standard email formatting rules.

1. Username Character Class [a-z0-9_\.-]:

This part of the regex defines the valid characters for the username portion of the email.
It includes lowercase letters (a-z), digits (0-9), underscores (_), periods (.), and hyphens (-).
The inclusion of these characters is based on standard email address conventions.

2. Domain Name Character Class [\da-z\.-]:

This character class is for the domain part of the email.
It includes digits (\d), which is a shorthand character class equivalent to 0-9, lowercase letters (a-z), periods, and hyphens.
The domain part of an email typically consists of alphanumeric characters and may include hyphens and periods as separators.

3. Domain Extension Character Class [a-z\.]:

This character class specifies the allowable characters in the domain extension (such as .com, .org).
It's limited to lowercase letters and periods.
The quantifier {2,6} following this class restricts the length of the domain extension to between 2 and 6 characters, accommodating most common domain extensions.

Code Snippets
Username: [a-z0-9_\.-]+
Domain Name: [\da-z\.-]+
Domain Extension: [a-z\.]{2,6}

Examples
For an email address user@example.com:

The username user matches [a-z0-9_\.-]+ as it contains lowercase letters.
The domain example matches [\da-z\.-]+ as it is comprised of lowercase letters.
The domain extension .com matches [a-z\.]{2,6} since it's a lowercase letter sequence within the specified length range.


### Grouping and Capturing

In regular expressions, grouping and capturing are powerful features that allow for organizing and extracting parts of the string that match a specific pattern. In the context of an email matching regex, such as /^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/, grouping plays a crucial role in both structuring the regex and potentially capturing parts of the email address for later use.

Basic Grouping:

Parentheses () are used to create groups in regex.
In our email regex, there are three main groups:
The username ([a-z0-9_\.-]+)
The domain name ([\da-z\.-]+)
The domain extension ([a-z\.]{2,6})
These groups help organize the regex by dividing the email address into logical sections.
Capturing:

By default, every group in a regex is a capturing group. This means that the part of the string that matches each group is stored and can be accessed later. For example, in the regex above, the username, domain, and extension are each captured. This allows for easy extraction and manipulation of these parts of the email address if needed.

Non-Capturing Groups:

If you need the grouping functionality without capturing, you can use a non-capturing group by adding ?: at the start of the group. For example, (?:[a-z0-9_\.-]+).
This might be used in email matching if you want to group parts of the pattern for repetition or alternation purposes but don’t need to capture the content for later use.

Capturing for Practical Use:

In practical applications, capturing groups can be very useful. For instance, if you're validating email addresses and also want to separately analyze or store the username and domain, capturing makes this straightforward.
Grouping and capturing are integral to creating efficient and effective regex patterns, especially in complex strings like email addresses. They not only provide structure to the pattern but also offer the flexibility to access and manipulate specific parts of the matched strings.

Code Snippets
Basic Grouping: ([a-z0-9_\.-]+)
Capturing: ([a-z0-9_\.-]+)@([\da-z\.-]+)
Non-Capturing: (?:[a-z0-9_\.-]+)

Examples
For the email address user@example.com:

Basic Grouping: ([a-z0-9_\.-]+) will group the user part of the email.
Capturing: The first group ([a-z0-9_\.-]+) captures user, and the second group ([\da-z\.-]+) captures example.
Non-Capturing: (?:[a-z0-9_\.-]+)@ will group user@ but will not capture user.

### Bracket Expressions

In email matching regex like /^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/, bracket expressions play a pivotal role in specifying allowable characters in different parts of an email address.

Function of Bracket Expressions:

Bracket expressions are denoted by square brackets [].
They match any one character from the set of characters specified within the brackets.
For example, [a-z] matches any lowercase letter from 'a' to 'z'.
Usage in Email Regex:

In our email regex, several bracket expressions are used:
[a-z0-9_\.-]: This specifies the characters allowed in the username part of the email. It includes lowercase letters, digits, underscores, periods, and hyphens.
[\da-z\.-]: Used for the domain name, this combines the shorthand \d for digits with lowercase letters, periods, and hyphens.
Character Ranges:

Within bracket expressions, character ranges can be defined using a hyphen -.
For instance, a-z signifies all lowercase letters from 'a' to 'z', and 0-9 represents all digits.
Special Characters in Bracket Expressions:

Normally, characters like . or - have special meanings in regex. However, within bracket expressions, they lose their special status.
For instance, the period . in [a-z\.] is treated as a literal period rather than as a wildcard character.
Flexibility and Precision:

Bracket expressions provide the flexibility to include any specific set of characters needed for a particular pattern.
This is particularly useful in email regex patterns to ensure that the username, domain, and domain extension adhere to standard email formatting rules.
Bracket expressions are thus integral to the construction of regular expressions, especially in validating formats like email addresses. They offer a concise and precise way to specify exactly which characters are acceptable in each part of the email.

Code Snippets
Username: [a-z0-9_\.-]+
Domain Name: [\da-z\.-]+
Domain Extension: [a-z\.]{2,6}

Examples
For the email address user@example.com:

The username user matches [a-z0-9_\.-]+, including lowercase letters and digits.
The domain name example fits within [\da-z\.-]+, capturing lowercase letters and allowing periods.
The domain extension .com adheres to [a-z\.]{2,6}, meeting the criteria for lowercase letters and length.

## Author

This tutorial was made by [Simon Hamblin - Shambo Rambo] (https://github.com/shambo-rambo/)