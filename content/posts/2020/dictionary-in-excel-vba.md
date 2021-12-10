---
title: "Dictionary in Excel VBA"
date: 2020-01-15T00:00:00+05:30
draft: false
author: "Daniccan"
categories:
  - VBA
tags:
  - VBA
  - Macro
  - Excel
---

I recently worked on a project which involved creating a macro in Microsoft Excel. Macros can be easily created without any programming knowledge by [recording a macro](https://support.office.com/en-us/article/Quick-start-Create-a-macro-741130CA-080D-49F5-9471-1E5FB3D581A8). However, recording macros do not help with advanced or customized use cases. VBA is used in such cases where you can [code the macro](https://www.excel-easy.com/vba/create-a-macro.html) to suit your requirement.

## VBA and its Key-Value Pair Data Structure

Just like any other programming language, [VBA](https://www.tutorialspoint.com/vba/index.htm) has its own syntax of variable declarations, data types, conditional and looping statements etc. But, I was not able to find a particular data structure in VBA, a data structure that deals with key-value pairs. `Java` has the [Map Interface](https://docs.oracle.com/javase/8/docs/api/java/util/Map.html), `Python` has the [Dictionary](https://docs.python.org/2/tutorial/datastructures.html#dictionaries), `Ruby` has the [Hash](https://ruby-doc.org/core-2.7.0/Hash.html) and so on. Why doesn't VBA have one by default?

It turns out that VBA does have a key-value pair data structure called the [Dictionary](https://docs.microsoft.com/en-us/office/vba/Language/Reference/User-Interface-Help/dictionary-object). To use it in a project, it has to be enabled manually by adding an reference. The Dictionary in VBA contains `Keys` and `Items` as its values. The Keys and Items can be of any data type supported by VBA. 

## Creating a Dictionary

### Early Binding

This type of Dictionary creation requires adding an reference before usage. This also enables auto-complete of code, which allows us to view the procedures and properties of the Dictionary in the editor.

To add an reference, 
  - Select `Tools -> References` from the Visual Basic Menu.
  - Select `Microsoft Scripting Runtime` from the list of references and save.

Now, the Dictionary can be initialized as follows

```vb
Dim dict As Dictionary
Set dict = New Dictionary
```

### Late Binding

This type of Dictionary creation does not require adding an reference before usage. This does not have the auto-complete feature.

The Dictionary can be initialized as follows

```vb
Dim dict As Object
Set dict = CreateObject("Scripting.Dictionary")
```

## Add to Dictionary

An entry can be added to the Dictionary as follows

**Syntax**

```vb
dict.Add Key, Item
```

**Examples**

```vb
dict.Add "Name", "John"
dict.Add Key:="Age", Item:=25
```

Just like other programming langugaes, duplicate Keys are not allowed in VBA Dictionary too. If a duplicate Key is added to the Dictionary, a Run-time error is thrown.

## Read from Dictionary

An Item can be read from the Dictionary using its Key as follows

**Syntax**

```vb
dict.Item(Key)
```

**Examples**

```vb
dict.Item("Name") 'Returns "John"
```

The list of Keys and Items can be read from the Dictionary as follows

**Syntax**

```vb
dict.Keys 'Returns the list of Keys
dict.Items 'Returns the list of Items
```

A Key can be checked for existence in the Dictionary as follows

**Syntax**

```vb
dict.Exists(Key) 'Returns Boolean
```

**Examples**

```vb
dict.Exists("Name") 'Returns true
dict.Exists("Salary") 'Returns false
```

## Update a Dictionary

An Item can be updated in the Dictionary using its Key as follows

**Syntax**

```vb
Set dict.Item(Key) = Item
```

**Examples**

```vb
Set dict.Item("Age") = 26
```

## Remove from Dictionary

Item(s) can be removed from the Dictionary as follows

**Syntax**

```vb
dict.Remove(Key) 'Remove entry based on Key
dict.RemoveAll 'Remove all entries
```

## VBA Dictionary in Office for Mac

When you are using [Office for Mac](https://products.office.com/en-US/mac/microsoft-office-for-mac), the VBA references list does not contain the `Microsoft Scripting Runtime` option. How else do you create a Dictionary with early binding?

There are two open-source implementations of the Dictionary class that you can import into Excel for Mac. 

- [Patrick O'Beirne@sysmod's Implementation](http://www.sysmod.com/Dictionary.zip)
- [VBA-tools Implementation in GitHub](https://github.com/VBA-tools/VBA-Dictionary)

Once imported, any of the above implementations prove to be a good replacement of Dictionary class provided by the `Microsoft Scripting Runtime` reference. 

## Conclusion

Data types that deal with collection of objects are important for any programming language. While collections such as Arrays or Lists are used to deal with multiple entries of single objects, key-value pair data structures are equally necessary to pair objects together and for lookups.
