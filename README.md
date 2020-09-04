# Umbraco FindAndReplace

[![Build status](https://ci.appveyor.com/api/projects/status/iwm15s9u25pfd2r2/branch/master?svg=true)](https://ci.appveyor.com/project/Cogworks/findandreplace/branch/master)
[![NuGet release](https://img.shields.io/nuget/v/Cogworks.FindAndReplace.svg)](https://www.nuget.org/packages/Cogworks.FindAndReplace)
[![Our Umbraco project page](https://img.shields.io/badge/our-umbraco-orange.svg)](https://our.umbraco.org/projects/backoffice-extensions/find-and-replace/)

A simple and intuitive package which allows editors to find and replace content in Umbraco 7.

## Getting started

This package is supported on Umbraco 7.5+.

### Installation

FindAndReplace is available from Our Umbraco, NuGet, or as a manual download directly from GitHub.

#### Our Umbraco repository

You can find a downloadable package, along with a discussion forum for this package, on the [Our Umbraco](https://our.umbraco.org/projects/backoffice-extensions/find-and-replace/) site.

#### NuGet package repository

To [install from NuGet](https://www.nuget.org/packages/Cogworks.FindAndReplace/), run the following command in your instance of Visual Studio.

    PM> Install-Package Cogworks.FindAndReplace

## Usage

After installing the package, you'll be able to search for phrase and replace it with new one.

![FindAndReplace Menu Item](docs/img/menu-item.png?raw=true)

To use it you have to right click on content tree node and choose "Find And Replace" option.

![Meganav Property Editor](docs/img/replace-all.gif?raw=true)

You are able to replace each occursion exclusively or replace all of those using "Replace all" button.

## Enable Full Text search

For large Umbraco sites with lots of content - search and replace can be slow and cause SQL timeouts. Full text search can help out here. Currently only one language is supported here - so this won't work for multi lingual sites right now.

In your SQL database run the following:

```
select * from sys.fulltext_languages
```

This will give you a list of supported languages and IDs. English is ID 1033 - but can be replaced with your language of choice below.

To enable full text indexing of the Umbraco content - run the following SQL against the database.

```
CREATE FULLTEXT CATALOG UmbracoFullTextCatalogue;

CREATE FULLTEXT INDEX ON CmsPropertyData
(  
    dataNvarchar, dataNText                         
        Language 1033                 
)  
    KEY INDEX PK_cmsPropertyData  
    ON UmbracoFullTextCatalogue;
	
ALTER FULLTEXT INDEX ON CmsPropertyData  
   START FULL POPULATION;  

ALTER FULLTEXT INDEX ON CmsPropertyData SET CHANGE_TRACKING AUTO;  


CREATE FULLTEXT INDEX ON UmbracoNode
(  
    Path							 
        Language 1033                  
)  
KEY INDEX PK_structure  
ON UmbracoFullTextCatalogue;
```

Lastly add an apsetting "FindAndReplace:EnableFullTextSearch" in your Web.config with a value of "True"


### Contribution guidelines

To raise a new bug, create an issue on the GitHub repository. To fix a bug or add new features, fork the repository and send a pull request with your changes. Feel free to add ideas to the repository's issues list if you would to discuss anything related to the package.

### Who do I talk to?

This project is maintained by [Cogworks](http://www.thecogworks.com/) and contributors. If you have any questions about the project please contact us through the forum on Our Umbraco, on [Twitter](https://twitter.com/cogworks), or by raising an issue on GitHub.

## License

Copyright &copy; 2017 [The Cogworks Ltd](http://www.thecogworks.com/), and other contributors

Licensed under the MIT License.