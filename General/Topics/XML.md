## What is XML?
- XML stands for eXtensible Markup Language
- XML is a markup language much like HTML
- XML was designed to store and transport data
- XML was designed to be self-descriptive

Simple XML Example
```xml
<note>  
  <to>Tove</to>  
  <from>Jani</from>  
  <heading>Reminder</heading>  
  <body>Don't forget me this weekend!</body>  
</note>
```

> [!info]
> XML is just information wrapped in tags.
> XML has no predefined tags!

#### XML vs HTML
- XML was designed to carry data - with focus on what data is
- HTML was designed to display data - with focus on how data looks
- XML tags are not predefined like HTML tags are

## XML DTD

DTD stands for Document Type Definition.
A DTD defines the structure and the legal elements and attributes of an XML document.

XML
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE note SYSTEM "Note.dtd">  
<note>  
<to>Tove</to>  
<from>Jani</from>  
<heading>Reminder</heading>  
<body>Don't forget me this weekend!</body>  
</note>
```

Note.dtd (this is an External Document Type Definition or EDTD)
```xml
<!DOCTYPE note  
[  
	<!ELEMENT note (to,from,heading,body)>  
	<!ELEMENT to (#PCDATA)>  
	<!ELEMENT from (#PCDATA)>  
	<!ELEMENT heading (#PCDATA)>  
	<!ELEMENT body (#PCDATA)>  
]>
```

> [!info] \#PCDATA
> \#PCDATA means Parseable Character

> [!important] `SYSTEM "Note.dtd"`
> The SYSTEM "Note.dtd" portion of the XML code shown above, accesses file system and takes note.dtd from the same directory as the XML file

Instead of putting the External DTD in a file such as Note.dtd, you can hardcode the values instead of `SYSTEM "Note.dtd"` as such (Paste EDTD content directly instead of `SYSTEM "Note.dtd"`)

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE note [<!ELEMENT note (to,from,heading,body)>]>  
<note>  
<to>Tove</to>  
<from>Jani</from>  
<heading>Reminder</heading>  
<body>Don't forget me this weekend!</body>  
</note>
```

> [!important] `!ELEMENT` -> `!ENTITY`
> `!ELEMENT` can also be replaced with XML Entities 

## XML Entities

#### Internal Entity Declaration

Syntax:
```xml
<!ENTITY entity-name "entity-value">
```

Example:
```xml
DTD Example:  
  
<!ENTITY writer "Donald Duck.">  
<!ENTITY copyright "Copyright W3Schools.">  
  
XML example:  
  
<author>&writer;&copyright;</author>
```

#### External Entity Declaration

Syntax
```xml
<!ENTITY entity-name SYSTEM "URI/URL">
```

Example
```xml
DTD Example:  
  
<!ENTITY writer SYSTEM "https://www.w3schools.com/entities.dtd">  
<!ENTITY copyright SYSTEM "https://www.w3schools.com/entities.dtd">  
  
XML example:  
  
<author>&writer;&copyright;</author>
```

> [!info] XXE has access to `SYSTEM`
> External XML Entity has access to `SYSTEM` operand, unlike Internal XML Entity 

This feature can be abused to create [[XXE]] vulnerability