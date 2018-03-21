# Flow Results Data Package

A container and data format for describing a collection of interactions or "responses" reported by end-users of a digital system using the Flow Data paradigm.  It provides for the open publication, exchange, and analysis of Flow-generated interactions across supporting platforms.

<table>
  <tr>
    <td>Authors</td>
    <td>Mark Boots (VOTO Mobile)<br>
Peter Lubell-Doughtie (Ona)<br>
Eduardo Jezierski (InSTEDD)<br>
Gustavo Giráldez (InSTEDD)<br>
Evan Wheeler (UNICEF)
  </tr>
  <tr>
    <td>Media Type</td>
    <td>TODO: once registered:
application/vnd.org.flowinterop.results+json</td>
  </tr>
  <tr>
    <td>Version</td>
    <td>1.0.0-rc.1</td>
  </tr>
  <tr>
    <td>Last updated</td>
    <td>2017-09-30</td>
  </tr>
  <tr>
    <td>Created</td>
    <td>2017-03-31</td>
  </tr>
  <tr>
    <td></td>
    <td></td>
  </tr>
</table>

# Table of Contents

   * [Language](#language)
   * [Introduction](#introduction)
      * [Scope](#scope)
   * [Terminology](#terminology)
   * [Example](#example)
   * [Specification](specification.md#specification)
      * [Components](specification.md#components)
      * [Descriptor](specification.md#descriptor)
      * [Resource](specification.md#resource)
      * [Resource Data (found at external path)](specification.md#resource-data-found-at-external-path)
      * [Question Types](specification.md#question-types)
         * [select_one](specification.md#select_one)
         * [select_many](specification.md#select_many)
         * [numeric](specification.md#numeric)
         * [open](specification.md#open)
         * [text](specification.md#text)
         * [image](specification.md#image)
         * [video](specification.md#video)
         * [audio](specification.md#audio)
         * [geo_point](specification.md#geo_point)
         * [datetime](specification.md#datetime)
         * [date](specification.md#date)
         * [time](specification.md#time)
   * [API Usage](api-specification.md)

# Language

The key words MUST, MUST NOT, REQUIRED, SHALL, SHALL NOT, SHOULD, SHOULD NOT, RECOMMENDED, MAY, and OPTIONAL in this document are to be interpreted as described in RFC 2119.

# Introduction

Within the field of ICT tools for humanitarian and development work (ICT4D), a range of software applications has emerged for digital data collection and mobile engagement with remote populations.  These applications employ a diversity of communication channels, such as text messaging, automated voice calls, unstructured supplementary service data (USSD) menus, web-based forms, and in-person surveys using mobile apps for digital data entry.  Despite differences, many of these applications are based on a similar paradigm of "Flow-based" data collection using logical decision trees.  However, it was not previously possible to exchange data collected using different applications and vendors unless ad-hoc or one-to-one data mapping software was created.

By defining an open specification for the exchange of data generated by Flow-like software applications, organizations can reduce ad-hoc software development effort; accelerate the creation of new interoperable analysis tools for visualization, dashboards, and decision making; speed response time to sharing data in crisis situations, and reduce risks associated with vendor lock-in.

## Scope

The purpose of this specification is to standardize the exchange of data between data collection and data analysis/visualization applications within the ICT4D sector.  The focus of the data covered by this specification is the "results" or "responses" recorded during interactions with end-users through digital channels.  

The specification is intended to be relevant to both file-based and API-based data exchange.  It is agnostic to the communication channel used for data collection.  

The specification is self-sufficient and can be used independently, but it is also designed to complement the Flow Definition specification describing logical flows of mobile engagement content.  It is also designed to be flexible enough to describe responses collected with non-Flow-based applications, such as the suite of tools based on the Open Data Kit (ODK) framework, and to create a bridge between established and evolving technologies.

# Terminology

**Contact**: A Contact is an end-user of a digital interactive system, providing input or "responses" to the system.   Contacts can be human beings interacting via a channel such as interactive voice response (IVR), SMS, USSD, social media messaging, and web browsers. Contacts might also be non-human entities (such as a waterpoint or school) or automated systems (programmable agents) that do not necessarily represent a human being.

**Response**: A Response is a single input given by a Contact when prompted during an interaction with a digital interactive system.  As an example, answering a multiple choice question asking, "Are you male or female?" by choosing the female option constitutes providing a Response.

**Question**: A Question is a prompt to the Contact for a Response.  When looking at results data, knowledge of the nature of the question can help to analyze and visualize it.  For example, numeric question responses might be graphed on a scatter plot, while multiple-choice question responses are naturally visualized on a bar chart or pie chart.  Often additional information about the question is necessary beyond simply the responses, such as the complete set of choices presented in a multiple choice question.  

Although the terminology of "Questions" is natural when using Flows for survey-like use-cases, it's important to clarify that in general, Questions might not be literal questions.  They might represent the output of a lookup, script, or logical analysis within a digital engagement system.  The terminology used varies across platforms: Variable, Result, and Column are other common terms. Within this specification, we refer to them uniformly for the sake of convenience as Questions.

(TODO: We agreed that Question isn't the best terminology, but I found that while drafting the spec this made it much more legible and understandable, compared to "Variable" or "Result" which are much less precise.)

# Example

A minimal example of a Flow Results data package, stored as files on disk, is given here:

There is a Descriptor file in JSON format, e.g. `flow-results-example-1.json`:

```
{
  "profile":"flow-results-package",
  "name":"flow-results-example-1",
  "flow_results_specification_version":"1.0.0-rc1",
  "created":"2017-06-30 15:35:27+00:00",
  "modified":"2017-06-30 15:38:05+00:00",
  "id":"b03ec84-77fd-4270-813b-0c698943f7ce",
  "title":"A nice title",
  "resources":[
    {
      "path":"data/flow-results-example-1-data.json",
      "name":"flow-results-example-1-data",
      "access_method":"file",
      "mediatype":"application/json",
      "encoding":"utf-8",
      "schema":{
        "language":"eng",
        "fields":[
          {
            "name":"timestamp",
            "title":"Timestamp",
            "type":"datetime"
          },
          {
            "name":"row_id",
            "title":"Row ID",
            "type":"string"
          },
          {
            "name":"contact_id",
            "title":"Contact ID",
            "type":"string"
          },
          {
            "name":"session_id",
            "title":"Session ID",
            "type":"string"
          },
          {
            "name":"question_id",
            "title":"Question ID",
            "type":"string"
          },
          {
            "name":"response",
            "title":"Response",
            "type":"any"
          },
          {
            "name":"response_metadata",
            "title":"Response Metadata",
            "type":"object"
          }
        ],
        "questions":{
          "ae54d3":{
            "type":"multiple_choice",
            "label":"Are you male or female?",
            "type_options":{
              "choices":[
                "male",
                "female",
                "not identified"
              ]
            }
          },
          "ae54d7":{
            "type":"multiple_choice",
            "label":"Favorite ice cream flavor?",
            "type_options":{
              "choices":[
                "chocolate",
                "vanilla",
                "strawberry"
              ]
            }
          },
          "ae54d8":{
            "type":"numeric",
            "label":"How much do you weigh, in lbs?",
            "type_options":{
              "range":[
                1,
                250
              ]
            }
          },
          "ae54da":{
            "type":"open",
            "label":"How are you feeling today?",
            "type_options":{

            }
          },
          "ae54db":{
            "type":"geo_point",
            "label":"Where are you?",
            "type_options":{

            }
          }
        }
      }
    }
  ]
}
```

Additionally, there is a data file containing the data described by this resource, in this example, `data/flow-results-example-1-data.json`. This file provides all individual Responses in the following JSON format:

```
[
  [ "2017-05-23T13:35:37.356-04:00", 20394823948, 923842093, 10499221, "ae54d3", "female", {"option_order": ["male","female"]} ],
  [ "2017-05-23T13:35:47.012-04:00", 20394823950, 923842093, 10499221, "ae54d7", "chocolate", {} ],
  [ "2017-05-24T15:15:37.981-04:00", 20394823952, 923842086, 10499224, "ae54d3", "male", {"option_order": ["male","female"]} ],
  [ "2017-05-23T15:16:12.005-04:00", 20394823953, 923842086, 10499224, "ae54d7", "vanilla", {} ],
  [ "2017-05-23T15:16:20.781-04:00", 20394823954, 923842086, 10499224, "ae54d8", 196, {} ],
  [ "2017-05-23T15:16:38.119-04:00", 20394823955, 923842086, 10499224, "ae54da", "I am feeling curious.", {"type": "text", "language": "eng"} ],
  [ "2017-05-23T17:25:12.722-04:00", 20394823956, 923842093, 10499227, "ae54da", "https://myexampleflowserver.org/resources/audio/20394823956.ogg", {"type": "audio", "language": "eng", "format": "audio/ogg"} ],
  [ "2017-05-23T17:25:47.214-04:00", 20394823957, 923842093, 10499227, "ae54db", "[35.678323, -108.25343]", {} ]
]
```

