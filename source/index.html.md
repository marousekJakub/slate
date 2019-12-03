---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - Maintained by Kuba Maroušek
  - <a href='mailto:kuba@ulmo.cz'>kuba@ulmo.cz</a>

includes:
  - errors

search: true
---

# Introduction

This is the description of the API of the ``dp-contracts-all:v5`` Docker image.

# Stemming

There are multiple endpoints on text stemming.

## Plain text stemming

```shell
$ curl "localhost:8000/text_stemmer?ngrams=1" \
     -d'"Plán starosty městské části Praha-Řeporyje Pavla Novotného postavit pomník vlasovcům vzbudil v Rusku pozornost."'
```

> returns

```json
["pl\u00e1n","starosta","m\u011bstsk\u00fd","\u010d\u00e1st","praha",
     "\u0159eporyj","pavel","novotn\u00fd","postavit","pomn\u00edk",
     "vlasovec","vzbudit","rusko","pozornost"]
```

This endpoint takes text sent in the body and stems it. It returns either stemmed single words, or ngrams up to the N equal to 3.

### HTTP Request

`POST http://localhost:8000/text_stemmer`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
ngrams | false | Number of words in every ngram, between 1 and 3.

## Contract stemming

```shell
$ cat smlouva.json
{
  "identifikator": {
    "idSmlouvy": "1701002",
    "idVerze": "9910603"
  },
  "schvalil": "",
  "ciziMena": null,
...
$ curl "localhost:8000/stemmer" -d@smlouva.json
```

> returns

```json
["dohoda","vyjad\u0159ovat_,h",null,"\u00fapln\u00fd",null,...]
```

Stems a given contract in its JSON form (available on the website).

### HTTP Request

`POST http://localhost:8000/stemmer`

## Semantic stemming

```shell
$ curl "localhost:8000/semantic_stemmer" -d'"Andrej Babiš je Slovák z STB."'
```

> returns

```json
[
     {"abbr":false,"name_type":"Giv","pos":"PROPN","word_orig":"Andrej","word_stemmed":"Andrej"},
     {"abbr":false,"name_type":"Sur","pos":"PROPN","word_orig":"Babi\u0161","word_stemmed":"Babi\u0161"},
     {"abbr":false,"name_type":"None","pos":"VERB","word_orig":"je","word_stemmed":"b\u00fdt"},
     {"abbr":false,"name_type":"Nat","pos":"PROPN","word_orig":"Slov\u00e1k","word_stemmed":"Slov\u00e1k"},
     {"abbr":true,"name_type":"Com","pos":"PROPN","word_orig":"STB","word_stemmed":"STB"}
]
```

Stems single words only (not ngrams) and returns a set of semantic attributes with each stemmed word.

### Attributes for each word

This is a list of attributes extracted from the output of UdPipe. See all possible attributes at <https://universaldependencies.org/u/feat/>.

Attribute | Meaning | Values
----------|---------|-------
word_orig | The original word. |
word_stemmed | The stemmed word. |
abbr | Whether the word is an abbreviation. | true / false
pos | Part of speech (slovní druh). Only a subset of all possible words is returned, based on their parts. | NOUN, ADJ (adjective), VERB, PROPN (property noun, a name of something), ADV (adverb)
name_type | Type of named entity. | None (is not a named entity), Geo (geographical), Prs (name of person, unsure whether name or surname), Giv (given name of person), Sur (surname), Nat (nationality), Com (company), Pro (product), Oth (other)

# Classification and finalization

In order to classify a document, it first must be stemmed through [Contract stemming](#contract-stemming) and its output sent to the classifier. The output of classifier is then sent to the finalizer.

## Classifier

TODO

## Finalizer

TODO
