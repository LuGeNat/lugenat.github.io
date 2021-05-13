---
layout: post
title: Fun with the Soundex Algorithm
categories:
  - Thoughts
excerpt_separator: <!--more-->
---

Today I did some really funny Python coding as I implemented the Soundex algorithm from scratch.

It is used for indexing or encoding names by how they sound. This is really cool, because often the same name is spelled differently such as 'Kristina' and 'Christina'.
This can be a pain in the neck when linking records from multiple data sources and trying to find records representing the same real world entity (e.g. person).

The Algorithm helps with different spellings of the same name and encodes 'Lucas Gerke' and 'Lukas Gehrke' both as 'l200 g620'. Nice, they should use that in the cafes for the correct spelling on coffee to-go cups..

Check out the algorithm and encode your own name! (Oh and sorry if your name is abort)

```
#!/usr/bin/env python3

# 'text' will be encoded
text = ''

while text != 'abort':

    print(
        'Type in a text to convert it into soundex encoding (Type "abort" to quit): ')
    text = input('')

    if text == 'abort':
        print('Programm terminated.')
    else:
        print(f'You entered {text}')
        # Soundex encoding

        # 1. Keep first letter and directly convert it into lower case
        first = text[0].lower()
        rest = text[1:]

        # 2. Remove all occurences of [a, e, i, o, u, y, h, w] inside the rest
        removal_list = ['a', 'e', 'i', 'o', 'u', 'y', 'h', 'w']
        rest_cleaned_list = [rest[i] for i in range(len(rest)) if rest[i] not in removal_list]
        rest_cleaned = ''.join(rest_cleaned_list)

        # 3. Replace all consonants from position two onwards with digits using
        #    these rules:
        #
        #    b, f, p, v → 1
        #    c, g, j, k, q, s, x, z → 2
        #    d, t → 3
        #    l → 4
        #    m, n → 5
        #    r → 6
        # 
        replacement_rules = {'1': ['b', 'f', 'p', 'v'],
                             '2': ['c', 'g', 'j', 'k', 'q', 's', 'x', 'z'],
                             '3': ['d', 't'],
                             '4': ['l'],
                             '5': ['m', 'n'],
                             '6': ['r']}
        rest_replaced = ''
        for i in range(len(rest_cleaned)):
            for j in replacement_rules.keys():
                if rest_cleaned[i] in replacement_rules[j]:
                    rest_replaced += j
                    break

        # 4. Only keep unique adjacent numbers
        rest_unique = ''
        for i in range(len(rest_replaced)):
            element = rest_replaced[i]
            try:
                next_element = rest_replaced[i+1]
                if element != next_element:
                    rest_unique += element
            except (IndexError):
                rest_unique += element
                break

        # 5. If length of a code is less than 4 add zeros, if longer truncate
        #    at length 4
        code = first + rest_unique
        if len(code) < 4:
            while len(code) < 4:
                code += '0'
        elif len(code) > 4:
            code = code[:4]

        # Done! Print encoded text
        print(f'The soundex code of {text} is: {code}')

```
