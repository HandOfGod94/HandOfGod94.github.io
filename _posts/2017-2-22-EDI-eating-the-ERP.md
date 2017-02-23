---
layout: post
title: EDI &#58; The Bane for ERP Big Data
published: true
---

EDI or Electronic Data Interchange was kind of formal specification
on how to transfer data between systems.

## History

For internet world we always had protocols such as HTTP, FTP build by orgs like 
IEEE, W3C etc. but apart from that we never had anything which can be used and 
shared between two or more systems. EDI was quite popular in both computing
as well as non computing industry and often adopted in Automobile and Electrical
industries. In these industries EDI helped in communicating between two or more
independent components of the system such as relay communication etc.


In tech industry it was highly used in warehouse and transportation management
systems where it generally have details like "shipTo","buyer","sender" etc.


## Where it all went wrong

Generally these standards were written as XSD schema as XML was becoming
quite popular in late 90's. People wrote 100's of XSD schemas to set up
standards for communication.

Then came BIG-DATA. Ever evolving sensors and IoT devices now continuously sends
data in small chunks but to a huge amount. In order to handle this specific problems
of sending semi-structured or unstructured data, new better faster data formats evolved
such as CSV, Json, YAML etc. This resulted in transfer/streaming information in reduced
size as compared to XML. Alongwith this evolved other infrastructure and technologies 
which we never had before such as Apache Flink, Spark, Hadoop and many more.

The problem is that standards were in XML and technologies were for JSON or CSV.
Unfortunately, the committee responsible for updating and maintaining standards also vanished.


## ERP Problem

For years ERP Software companies are struggling to catch up with the new gen tech. Why?
They have multi-billion dollar deals with customers in the past where they've implemented the old
legacy systems, which is now becoming harder and harder to keep up and upgrade.

With IoT gaining popularity, ERP companies wants to use new tech alongwith legacy systems, which
is getting harder and harder day by day

Let's take basic example:

GS1, a standard developed for transferring documents in mainly Warehouse and Transportation system 
was developed in XML. It is also used in generating barcode, so basically barcodes are decoded
as GS1 documents which then can be shared with other systems. 

ERP companies have built software around this standard. They wrote hundreds of XSLTs and custom XSDs
to make it work. But one point which was not given much importance at time of development was 
that **XML was never developed for efficiency**. It was developed with human readability in mind.
So its good to have configuration in XML and it's really really bad if you want to transfer large chunk of
data in XML. Two reasons why you should not transfer large amount of data in XML:

1. It'll take more space than other formats.
2. XML processing has an overhead which any system have to suffer at each stage in communication.
Suppose if XML is passed from sys1->sys2->sys3 then each stage has to parse and validate(optional) the XML.

The next-gen tech such as Spark,Flink etc. are good at handling CSV's and JSON, but not at handling XML.


## Possible Solutions

So what other options we have? Standards have specific advantage that you can validate your data. XSD schemas
were powerful tools which can specifies the format in which your data should be. With JSON or CSV we don't have
this ability. But we have other players in this market.

### Serialization Frameworks

With rise in big data, people gradually felt they the still need a structure in the data. They
came up with serialization frameworks.

There are many serialization frameworks available such as : Thrift, Avro, ProtocolBuffers.

What they basically do is create a serialized object in any language you are working and sends
the serialized object as data. It is binary format which reduces the size. It also increases the efficiency
with which you can process the data as now you directly have data in serialized form as an object.

> Major Drawback of serializing is everything stays in-memory. So you can't have large object which is often a case for 
> large dataset. But often, we find that the large dataset is nothing but small repetitive chunks in data 
> of a particular format.

It is still challenging to work with large amount of data. The options I provided are just some basic stuff
which still needs to explored further.
